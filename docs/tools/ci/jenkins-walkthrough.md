---
title: Utilisation de Jenkins avec Xamarin
description: Ce document décrit comment utiliser Jenkins pour l’intégration continue avec les applications Xamarin. Il explique comment installer, configurer et utiliser Jenkins.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: 7f66c97ce4b7880d32dfd87aec0691a26a08cfd2
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669425"
---
# <a name="using-jenkins-with-xamarin"></a>Utilisation de Jenkins avec Xamarin

_Ce guide explique comment configurer Jenkins en tant qu’un serveur d’intégration continue et d’automatiser la compilation des applications mobiles créées avec Xamarin. Il décrit comment installer Jenkins sur OS X, configurez-le et paramétrer des tâches pour compiler des applications Xamarin.iOS et Xamarin.Android lorsque les modifications sont validées dans le système de gestion de code source._

[Introduction à l’intégration continue avec Xamarin](~/tools/ci/intro-to-ci.md) introduit l’intégration continue comme une pratique de développement de logiciels utiles qui fournit l’avertissement en amont du code rompu ou incompatible. CI permet aux développeurs de résoudre les problèmes et les problèmes lorsqu’ils surviennent et conserve le logiciel dans un état approprié pour le déploiement. Cette procédure pas à pas explique comment utiliser le contenu à partir de ces deux documents ensemble.

Ce guide montre comment installer Jenkins sur un ordinateur dédié exécutant OS X et les configurer pour s’exécuter automatiquement au démarrage de l’ordinateur. Une fois Jenkins installé, nous allons installer le plug-ins supplémentaires pour prendre en charge MS Build. Jenkins prend en charge Git prêts à l’emploi. Si TFS est utilisé pour le contrôle de code source, un plug-in et ligne de commande des utilitaires supplémentaires doivent également être installés.

Une fois que Jenkins est configurée et plug-ins nécessaires ont été installés, nous allons créer un ou plusieurs travaux pour compiler les projets Xamarin.Android et Xamarin.iOS. Un travail est une collection d’étapes et les métadonnées nécessaires pour exécuter des tâches. Généralement, un travail est constitué des éléments suivants :

- **Source Code Management (SCM)** – il s’agit d’une entrée de métadonnées dans les fichiers de configuration de Jenkins qui contient des informations sur comment se connecter à un contrôle de code source et des fichiers à récupérer.
- **Déclencheurs** – déclencheurs sont utilisés pour démarrer un travail en fonction de certaines actions, telles que quand un développeur valide des modifications dans le référentiel de code source.
- **Instructions de génération** – il s’agit d’un plug-in ou un script qui va de compiler le code source et de générer un fichier binaire qui peut être installé sur les appareils mobiles.
- **Les Actions de génération facultatives** : cela peut inclure l’exécution de tests unitaires, effectuer une analyse statique sur la signature du code, code, ou depuis un autre travail pour effectuer d’autres de créer le travail associé.
- **Notifications** – un travail peut envoyer une quelconque de notification sur l’état d’une build.
- **Sécurité** : bien que facultatif, il est fortement recommandé que les fonctionnalités de sécurité Jenkins soit également activée.

Ce guide vous guide configurer un serveur Jenkins couvrant chacune de ces points. À la fin de celui-ci, nous devrions avoir une bonne compréhension de comment installer et configurer Jenkins pour créer des extensions IPA et le fichier APK pour nos projets mobiles Xamarin.

## <a name="requirements"></a>Spécifications

Le serveur de builds idéale est un ordinateur autonome dédié à l’unique but de génération et test éventuellement l’application. Un ordinateur dédié permet de s’assurer que les artefacts pouvant être nécessaires pour d’autres rôles (tel que celui d’un serveur web) ne pas polluer la build. Par exemple, si le serveur de builds joue également un serveur web, le serveur web peut nécessiter une version en conflit de certains bibliothèque commune. En raison de ce conflit le serveur web peut ne pas fonctionne correctement ou Jenkins peut créer des builds qui ne fonctionnent pas lors du déploiement sur les utilisateurs.

Le serveur de builds pour les applications mobiles Xamarin très semblable à la station de travail d’un développeur est configuré. Il dispose d’un compte d’utilisateur dans les Jenkins, Visual Studio pour Mac, et Xamarin.iOS et Xamarin.Android seront installés. Tout le code de signature des certificats, l’approvisionnement de profils et les magasins de clés doit être également installés. *En général, compte d’utilisateur du serveur de builds est distinct de vos comptes de développeur - veillez à installer et configurer tous les logiciels, les clés et les certificats lorsque vous êtes connecté avec le compte d’utilisateur de serveur build.*

Le diagramme suivant illustre tous ces éléments sur un serveur de builds Jenkins classique :

[![](jenkins-walkthrough-images/image1.png "Ce diagramme illustre tous ces éléments sur un serveur de builds Jenkins classique")](jenkins-walkthrough-images/image1.png#lightbox)

les applications iOS peuvent uniquement être générées et connectées sur un ordinateur exécutant macOS. Un Mac Mini est une option à moindre coût raisonnable, mais n’importe quel ordinateur capable d’exécuter OS X 10.10 (Yosemite) ou version ultérieure est suffisant.

Si TFS est utilisé pour le contrôle de code source, vous souhaitez installer [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/). Team Explorer Everywhere fournit l’accès inter-plateformes à TFS sur un Terminal dans macOS.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>L’installation Jenkins

La première tâche à l’aide de Jenkins consiste à installer. Il existe trois façons d’exécuter Jenkins sur OS x :

- En tant que démon, en cours d’exécution en arrière-plan.
- À l’intérieur d’un conteneur de servlet tel que Tomcat, Jetty ou JBoss.
- En tant que processus normal en cours d’exécution sous un compte d’utilisateur.

La plupart des applications d’intégration continue s’exécuter en arrière-plan, soit en tant que démon (sur OS X ou \*nix) ou en tant que service (sur Windows). Cela convient pour les scénarios où il n’existe aucune interaction de l’interface graphique utilisateur requis, et où le programme d’installation de l’environnement de génération peut être effectuée facilement. Les applications mobiles nécessitent également keystores certificats et signature qui peuvent être problématiques pour l’accès lorsque Jenkins est exécuté en tant que démon. En raison de ces problèmes, ce document se concentre sur le troisième scénario – en cours d’exécution Jenkins sous un compte d’utilisateur sur le serveur de builds.

Jenkins.App est un moyen pratique pour installer Jenkins. Il s’agit d’un wrapper AppleScript qui simplifie le démarrage et l’arrêt d’un serveur Jenkins. Au lieu d’exécuter dans un interpréteur de commandes bash, Jenkins s’exécute comme une application avec une icône dans le Dock, comme illustré dans la capture d’écran suivante :

[![](jenkins-walkthrough-images/image2.png "Au lieu d’exécuter dans un interpréteur de commandes bash, Jenkins s’exécute comme une application avec une icône dans le Dock, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image2.png#lightbox)

Démarrage ou arrêt Jenkins est aussi simple que le démarrage ou arrêt Jenkins.App.

Pour installer Jenkins.App, téléchargez la dernière version à partir de la page de téléchargement du projet, illustrée dans la capture d’écran ci-dessous :

[![](jenkins-walkthrough-images/image3.png "Application, page, illustrée dans cette capture d’écran de téléchargement de la dernière version à partir des projets de téléchargement")](jenkins-walkthrough-images/image3.png#lightbox)

Extrayez le fichier zip dans le `/Applications` dossier sur votre serveur de builds, début il comme toute autre application OS X.
La première fois que vous démarrez Jenkins.App, il présente une boîte de dialogue vous informant qu’il téléchargera Jenkins :

[![](jenkins-walkthrough-images/image4.png "Application, il présente une boîte de dialogue vous informant qu’il téléchargera Jenkins")](jenkins-walkthrough-images/image4.png#lightbox)

Une fois Jenkins.App a terminé son téléchargement, il affiche une autre boîte de dialogue vous demandant si vous souhaitez personnaliser le démarrage de Jenkins, comme illustré dans la capture d’écran suivante :

[![](jenkins-walkthrough-images/image5.png "Application a terminé son téléchargement, il affiche une autre boîte de dialogue vous demandant si vous souhaitez personnaliser le démarrage de Jenkins, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image5.png#lightbox)

Personnalisation de Jenkins est facultatif et ne devra pas être effectuées à chaque fois que l’application est lancée : les paramètres par défaut pour Jenkins fonctionnent pour la plupart des situations.

S’il est nécessaire de personnaliser Jenkins, cliquez sur le **modifier les valeurs par défaut** bouton. Cela présente deux boîtes de dialogue consécutives : une qui demande les paramètres de ligne de commande Java et une autre qui demande les paramètres de ligne de commande de Jenkins. Les deux captures d’écran suivantes montrent ces deux boîtes de dialogue :

[![](jenkins-walkthrough-images/image6.png "Cette capture d’écran montre les boîtes de dialogue")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "Cette capture d’écran montre les boîtes de dialogue")](jenkins-walkthrough-images/image7.png#lightbox)

Une fois que Jenkins est exécuté, vous souhaiterez définissez-le comme un élément de connexion afin qu’il démarre chaque fois que l’utilisateur se connecte en à l’ordinateur. Cela en cliquant sur l’icône de Jenkins dans l’écran d’ancrage et en choisissant **Options... > Ouvrir lors de la connexion**, comme illustré dans la capture d’écran suivante :

[![](jenkins-walkthrough-images/image8.png "Faire cela en cliquant sur l’icône de Jenkins dans l’écran d’ancrage et en choisissant donnéesOpen lors de la connexion, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image8.png#lightbox)

Cela entraîne Jenkins.App lancer automatiquement chaque fois que l’utilisateur se connecte, mais pas lorsque l’ordinateur démarre. Il est possible de spécifier un compte d’utilisateur du système d’exploitation X utilisera automatiquement avec lequel se connecter au moment du démarrage. Ouvrir le **Préférences système**, puis sélectionnez le **utilisateurs et groupes** icône comme illustré dans cette capture d’écran :

[![](jenkins-walkthrough-images/image9.png "Ouvrez les préférences système, puis sélectionnez l’icône de groupes d’utilisateurs comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image9.png#lightbox)

Cliquez sur le **Options de connexion** bouton, puis choisissez le compte du système d’exploitation X utilisera pour la connexion au moment du démarrage.

À ce stade Jenkins a été installé. Toutefois, si vous souhaitez créer des applications mobiles Xamarin, nous devrons installer certains plug-ins.

### <a name="installing-plugins"></a>L’installation de plug-ins

Une fois le programme d’installation Jenkins.App terminée, il démarre Jenkins et lancer le navigateur web avec l’URL http://localhost:8080, comme illustré dans la capture d’écran ci-dessous :

[![](jenkins-walkthrough-images/image10.png "8080, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image10.png#lightbox)

Dans cette page, sélectionnez **Jenkins > Gérer Jenkins > Gérer les plug-ins** dans le menu dans le coin supérieur gauche, comme illustré dans la capture d’écran ci-dessous :

[![](jenkins-walkthrough-images/image11.png "À partir de cette page, sélectionnez Jenkins gérer Jenkins gérer les plug-ins dans le menu dans le coin supérieur gauche")](jenkins-walkthrough-images/image11.png#lightbox)

Ceci affichera la **le Gestionnaire de plug-in Jenkins** page. Si vous cliquez sur l’onglet disponible, vous verrez une liste de plus de 600 plug-ins qui peuvent être téléchargés et installés. Cela est illustré dans la capture d’écran ci-dessous :

[![](jenkins-walkthrough-images/image12.png "Si vous cliquez sur l’onglet disponible, vous verrez une liste de plus de 600 plug-ins qui peuvent être téléchargés et installés")](jenkins-walkthrough-images/image12.png#lightbox)

Parcourez tous les plug-ins à trouver que quelques peuvent être fastidieuses et sujette aux erreurs de 600. Jenkins fournit un champ de recherche de filtre dans le coin supérieur droit de l’interface. À l’aide de ce champ de filtre pour rechercher simplifiera la localisation et installé un ou tous les plug-ins suivants :

- **Le plug-in de Jenkins MSBuild** – ce plug-in permet de générer de Visual Studio et Visual Studio pour Mac solutions (.sln) et des projets (.csproj).
- **Le plug-in d’environnement injecteur** – il s’agit d’un plug-in, facultatif mais utile qui rend possible de définir des variables d’environnement à la tâche et niveau de build. Il offre également une protection supplémentaire pour les variables telles que les mots de passe utilisés pour le code signer l’application. Il est parfois abrégée en tant que le *EnvInject Plugin* .
- **L’équipe de plug-in de Server Foundation** – il s’agit d’un plug-in facultatif qui est uniquement requis si vous utilisez Team Foundation Server ou Team Foundation Services pour le contrôle de code source.

Jenkins prend en charge Git sans plug-ins supplémentaires.

Après avoir installé tous les plug-ins, vous souhaitez redémarrer Jenkins et configurez les paramètres globaux pour chaque plug-in. Les paramètres globaux pour un plug-in accessibles en sélectionnant **Jenkins > Gérer Jenkins > configurer le système** à partir de l’angle supérieur gauche, comme illustré dans la capture d’écran ci-dessous :

[![](jenkins-walkthrough-images/image13.png "Vous trouverez les paramètres globaux pour un plug-in en sélectionnant Jenkins / gérer Jenkins / configurer le système à partir de l’angle supérieur gauche main supérieur")](jenkins-walkthrough-images/image13.png#lightbox)

Lorsque vous sélectionnez cette option de menu, vous êtes redirigé vers la **configurer le système [Jenkins]** page. Cette page contient des sections pour configurer Jenkins lui-même et pour définir certaines des valeurs globales de plug-in.  La capture d’écran ci-dessous illustre un exemple de cette page :

[![](jenkins-walkthrough-images/image14.png "Cette capture d’écran illustre un exemple de cette page")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>Configurez le plug-in de MSBuild

Le plug-in de MSBuild doit être configuré pour utiliser **/Library/Frameworks/Mono.framework/Commands/xbuild** compiler Visual Studio pour les fichiers solution et projet Mac. Faites défiler la **configurer le système [Jenkins]** page jusqu'à ce que le **MSBuild ajouter** bouton s’affiche, comme illustré dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image15.png "Faites défiler la page Configurer les Jenkins système jusqu'à ce qu’apparaisse le bouton Ajouter de MSBuild")](jenkins-walkthrough-images/image15.png#lightbox)

Cliquez sur ce bouton, puis remplissez le **nom** et **chemin d’accès** à **MSBuild** champs sur le formulaire qui s’affiche. Le nom de votre **MSBuild** installation doit être un nom significatif, lors de la **chemin d’accès à MSBuild** doit être le chemin d’accès à `xbuild`, ce qui est généralement **Library/Frameworks / Mono.framework/Commands/xbuild**. Une fois que nous enregistrons les modifications en cliquant sur l’enregistrement ou le bouton Appliquer en bas de la page, Jenkins sera en mesure d’utiliser `xbuild` pour compiler vos solutions.

#### <a name="configuring-the-tfs-plugin"></a>Configurez le plug-in TFS

Cette section est obligatoire si vous envisagez d’utiliser TFS pour votre contrôle de code source.

Dans l’ordre pour une station de travail macOS interagir avec un serveur TFS, [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) doit être installé sur la station de travail. Team Explorer Everywhere est un ensemble d’outils de Microsoft qui inclut un client de ligne de commande multiplateforme pour accéder à TFS. Team Explorer Everywhere peuvent être téléchargé à partir de Microsoft et installé en trois étapes :

1. Décompressez le fichier d’archive dans un répertoire qui est accessible au compte d’utilisateur. Par exemple, vous pouvez décompresser le fichier à **~/tee**.
2. Configurer l’interpréteur de commandes ou votre chemin système afin d’inclure le dossier qui contient les fichiers qui ont été décompressés à l’étape ci-dessus. Par exemple :

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Pour confirmer que Team Explorer Everywhere est installé, ouvrez une session de terminal et exécutez le `tf` commande. Si tf est correctement configuré, vous verrez la sortie suivante dans votre session terminal :

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

Une fois que le client de ligne de commande pour TFS est installé, Jenkins doit être configuré avec le chemin complet vers le `tf` client de ligne de commande. Faites défiler la **configurer le système [Jenkins]** page jusqu'à ce que vous trouviez la section de Team Foundation Server, comme illustré dans la capture d’écran suivante :

[![](jenkins-walkthrough-images/image17.png "Faites défiler la page Configurer les Jenkins système jusqu'à ce que vous trouviez la section de Team Foundation Server")](jenkins-walkthrough-images/image17.png#lightbox)

Entrez le chemin complet vers le `tf` commande, puis cliquez sur le **enregistrer** bouton.

### <a name="configure-jenkins-security"></a>Configurer la sécurité de Jenkins

Lors de la première installation, Jenkins a la sécurité est désactivée, il est donc possible pour tous les utilisateurs à configurer et exécuter n’importe quel type de travail anonymement. Cette section explique comment configurer la sécurité à l’aide de la base de données utilisateur Jenkins pour configurer l’authentification et l’autorisation.

Vous trouverez les paramètres de sécurité en sélectionnant **Jenkins > Gérer Jenkins > configurer la sécurité globale**, comme illustré dans cette capture d’écran :

[![](jenkins-walkthrough-images/image18.png "Vous trouverez les paramètres de sécurité en sélectionnant Jenkins / gérer Jenkins / configurer la sécurité globale")](jenkins-walkthrough-images/image18.png#lightbox)

Sur le **configurer la sécurité globale** page, vérifiez le **activer la sécurité** case à cocher et le **contrôle d’accès** formulaire doit apparaître, semblable à la capture d’écran suivante :

[![](jenkins-walkthrough-images/image19.png "Dans la page Configurer la sécurité globale, vérifiez la sécurité d’activer la case à cocher et le formulaire de contrôle d’accès doivent apparaître, semblables à cette capture d’écran")](jenkins-walkthrough-images/image19.png#lightbox)

Activer/désactiver le bouton radio pour **base de données utilisateur des Jenkins** dans le **sécurité domaine Section**et vérifiez que **permettre aux utilisateurs de s’inscrire** est également activée, comme illustré dans la capture d’écran suivante :

[![](jenkins-walkthrough-images/image20.png "Activer/désactiver la case d’option de base de données de Jenkins propre utilisateur dans la Section de domaine de sécurité et vérifiez qu’Autoriser les utilisateurs à s’inscrire est également cochée")](jenkins-walkthrough-images/image20.png#lightbox)

Enfin, redémarrez Jenkins et créer un nouveau compte. Le premier compte créé est le compte racine, et ce compte est automatiquement promu à un administrateur. Revenez à la **configurer la sécurité globale** page et la marquer comme étant le **sécurité basée sur une matrice** case d’option. Le compte racine doit disposer d’un accès complet et que le compte anonyme doit être un accès en lecture seule, comme indiqué dans la capture d’écran suivante :

[![](jenkins-walkthrough-images/image21.png "Le compte racine doit disposer d’un accès complet et le compte anonyme censé donner l’accès en lecture seule")](jenkins-walkthrough-images/image21.png#lightbox)

Une fois que ces paramètres sont enregistrés et Jenkins est redémarré, la sécurité est activée.

#### <a name="disabling-security"></a>Désactivation de la sécurité

En cas d’oubli du mot de passe ou le verrouillage de Jenkins à l’échelle, il est possible de désactiver la sécurité en procédant comme suit :

1. Arrêter Jenkins. Si vous utilisez Jenkins.app, nous vous conseillons de vous faire en cliquant sur l’icône Jenkins.App dans le Dock et en sélectionnant Quitter dans le menu qui s’affiche :

    ![Icône d’application dans l’écran d’ancrage et sélectionner Quitter dans le menu qui s’affiche](jenkins-walkthrough-images/image19.png)

2. Ouvrez le fichier **~/.jenkins/config.xml** dans un éditeur de texte.
3. Modifiez la valeur de la `<usesecurity></usesecurity>` élément `true` à `false`.
4. Supprimer le `<authorizationstrategy></authorizationstrategy>` et `<securityrealm></securityrealm>` éléments à partir du fichier.
5. Redémarrez Jenkins.

## <a name="setting-up-a-job"></a>Configuration d’un projet

Au niveau supérieur, Jenkins organise l’ensemble des différentes tâches nécessaires pour générer des logiciels dans un *travail*. Un travail a également les métadonnées associées, fournissant des informations sur la build comme comment obtenir le code source, la fréquence à laquelle la build doit s’exécuter, toutes les variables spéciales qui sont nécessaires pour la création et la procédure avertir les développeurs si la build échoue.

Travaux sont créés en sélectionnant **Jenkins > Nouveau travail** à partir du menu dans le coin supérieur droit, comme illustré dans la capture d’écran suivante :

![](jenkins-walkthrough-images/image22.png "Travaux sont créés en sélectionnant Nouveau travail de Jenkins à partir du menu dans le coin supérieur droit")

Ceci affichera la **nouveau travail [Jenkins]** page. Entrez un nom pour le travail, puis sélectionnez le **générer un projet de logiciel de forme libre** case d’option. La capture d’écran suivante montre un exemple de ceci :

![](jenkins-walkthrough-images/image23.png "Entrez un nom pour le travail, puis sélectionnez la Build un bouton radio du projet logiciel de forme libre")

En cliquant sur le **OK** bouton présente la page de configuration pour le travail. Cela doit ressembler à la capture d’écran suivante :

![](jenkins-walkthrough-images/image24.png "Cela doit ressembler à cette capture d’écran")

Jenkins organise les tâches dans un répertoire sur le disque dur situé dans le chemin d’accès suivant : **~/.jenkins/jobs/[JOB nom]**

Ce dossier contient tous les fichiers et les artefacts spécifiques à la tâche telles que les journaux, fichiers de configuration et le code source qui doit être compilé.

Une fois que le travail initial a été créé, il doit être configuré avec un ou plusieurs des opérations suivantes :

- Le système de gestion de code source doit être spécifié.
- Un ou plusieurs *actions de génération* doit être ajouté au projet. Voici les étapes ou les tâches requises pour générer l’application.
- Le travail doit être assigné un *déclencheur de build* – un ensemble d’instructions pour informer la fréquence à laquelle Jenkins pour récupérer le code et générez le projet final.

### <a name="configuring-source-code-control"></a>Configuration du contrôle de Code Source

La première tâche est Jenkins est de récupérer le code source à partir du système de gestion de code source. Jenkins prend en charge plusieurs les systèmes de gestion de code source populaires disponibles aujourd'hui. Cette section traite des deux systèmes courants, Git et Team Foundation Server. Chacune de ces systèmes de gestion de code source est abordée plus en détail dans les sections ci-dessous.

#### <a name="using-git-for-source-code-control"></a>À l’aide de Git pour le contrôle de Code Source

Si vous utilisez TFS pour le contrôle de code source, [ignorer](#Using-TFS-for-Source-Code-Management) cette section et passez à la section suivante à l’aide de TFS.

Jenkins prend en charge Git prêts à l’emploi : aucun plug-ins supplémentaires ne sont nécessaires. Pour utiliser Git, cliquez sur le **Git** case d’option et entrez l’URL pour le dépôt Git, comme indiqué dans la capture d’écran suivante :

![](jenkins-walkthrough-images/image25.png "Pour utiliser Git, cliquez sur le bouton radio de Git et entrez l’URL pour le dépôt Git")

Une fois que les modifications sont enregistrées, la configuration de Git est terminée.

#### <a name="using-tfs-for-source-code-management"></a>À l’aide de TFS pour gérer le Code Source

Cette section s’applique uniquement aux utilisateurs TFS.

Cliquez sur le **Team Foundation Server** case d’option et la section de configuration TFS doivent apparaître, semblables à ce qui est dans la capture d’écran suivante :

![](jenkins-walkthrough-images/image26.png "Cliquez sur le bouton radio de Team Foundation Server et de la section de configuration TFS doit apparaître.")

Fournir les informations nécessaires pour TFS. La capture d’écran suivante montre un exemple du formulaire terminé :

![](jenkins-walkthrough-images/image27.png "Cette capture d’écran montre un exemple de formulaire rempli")

#### <a name="testing-the-source-code-control-configuration"></a>Test de la Configuration de contrôle de Code Source

Une fois que le contrôle de code source approprié a été configuré, cliquez sur **enregistrer** pour enregistrer les modifications. Vous permet de retourner à la page d’accueil pour le travail, qui doit ressembler à la capture d’écran suivante :

![](jenkins-walkthrough-images/image28.png "Vous permet de retourner à la page d’accueil pour le travail, qui doit ressembler à cette capture d’écran")

Le plus simple pour vérifier que le contrôle de code source est configuré correctement consiste à déclencher une build manuellement, même si aucune action de génération spécifié. Pour démarrer manuellement une build, la page d’accueil de la tâche a un **générer maintenant** lien dans le menu de gauche, comme illustré dans la capture d’écran ci-dessous :

![](jenkins-walkthrough-images/image29.png "Pour démarrer manuellement une build, la page d’accueil de la tâche a un lien générer maintenant dans le menu sur le côté gauche")

Quand une build a été démarrée, la boîte de dialogue de l’historique de Build affiche un cercle bleu clignotant, une barre de progression, le numéro de build et l’heure à laquelle la build a démarré, similaire à la capture d’écran suivante :

![](jenkins-walkthrough-images/image30.png "Quand une build a été démarrée, la boîte de dialogue de l’historique de Build affiche un cercle bleu de clignotement, une barre de progression, le numéro de build et l’heure de début de la build")

Si la tâche réussit, un cercle bleu s’affichera. Si le travail échoue, un cercle rouge s’affichera.

Pour vous aider à résoudre des problèmes qui peuvent se produire dans le cadre de la build, Jenkins capture toute la sortie de console pour le travail. Pour afficher la sortie de console, cliquez sur la tâche dans **l’historique de Build**, puis, dans le **sortie de la Console** lien dans le menu de gauche. La capture d’écran suivante montre le **sortie de la Console** lien, mais aussi partie de la sortie à partir d’une tâche réussie :

![](jenkins-walkthrough-images/image31.png "Cette capture d’écran montre le lien de sortie de la Console, mais aussi partie de la sortie à partir d’une tâche réussie")

#### <a name="location-of-build-artifacts"></a>Emplacement des artefacts de Build

Jenkins récupérera le code source complet dans un dossier spécial appelé un *espace de travail*. Vous trouverez ce répertoire à l’intérieur du dossier à l’emplacement suivant :

    ```
    ~/.jenkins/jobs/[JOB NAME]/workspace
    ```

Le chemin d’accès à l’espace de travail sera être stockée dans une variable d’environnement nommée `$WORKSPACE`.

Il est possible de parcourir le dossier de l’espace de travail dans Jenkins en accédant à la page d’accueil pour un travail, puis en cliquant sur le **espace de travail** lien dans le menu de gauche. La capture d’écran suivante montre un exemple de l’espace de travail pour une tâche nommée **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Cette capture d’écran montre un exemple de l’espace de travail pour une tâche nommée HelloWorld")

### <a name="build-triggers"></a>Déclencheurs de build

Il existe plusieurs stratégies différentes de l’initialisation des builds dans Jenkins : ils sont appelés *déclencheurs de build*. Un déclencheur de build Jenkins permet de décider du moment auquel démarrer un travail et générez le projet. Deux des déclencheurs de build plus courantes sont :

- **Générer régulièrement** – ce déclencheur provoque Jenkins démarrer un travail à des intervalles spécifiés, tels que toutes les deux heures ou à minuit en semaine. Démarre la build ait ou non des modifications ont été apportées dans le référentiel de code source.
- **Interrogation SCM** – ce déclencheur interroge le contrôle de code source sur une base régulière. Si toutes les modifications ont été validées dans le référentiel de code source, Jenkins démarre une nouvelle build.

L’interrogation SCM d’est un déclencheur populaire, car il fournit un commentaire rapide lorsqu’un développeur à valider les modifications qui provoquent l’arrêt de la build. Cela est utile pour les alertes d’équipes que du code validé récemment pose des problèmes et permet aux développeurs de résoudre le problème, tandis que les modifications sont toujours actualisées à l’esprit.

Builds périodiques sont souvent utilisés pour créer une version de l’application qui peut être distribuée aux testeurs. Par exemple, une build périodique peut être programmée pour le vendredi soir, afin que les membres de l’équipe d’assurance qualité peuvent tester le travail de la semaine précédente.

### <a name="compiling-a-xamarinios-applications"></a>Compilation d’une application Xamarin.iOS
Les projets Xamarin.iOS peuvent être compilés à la ligne de commande à l’aide `xbuild` ou `msbuild`. La commande shell s’exécutera dans le contexte du compte d’utilisateur qui est en cours d’exécution Jenkins. Il est important que le compte d’utilisateur a accès au profil d’approvisionnement afin que l’application peut être empaquetée correctement pour la distribution. Il est possible d’ajouter cette commande shell à la page configuration du travail.

Faites défiler jusqu'à la **Build** section. Cliquez sur le **ajouter une étape de build** bouton et sélectionnez **exécuter un script shell**, comme illustré par la capture d’écran suivante :

![](jenkins-walkthrough-images/image33.png "Cliquez sur le bouton d’étape de build ajouter et sélectionnez Execute shell")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Génération d’un projet Xamarin.Android

Génération d’un projet Xamarin.Android est très similaire dans son concept à la création d’un projet Xamarin.iOS. Pour créer un APK à partir d’un projet Xamarin.Android, Jenkins doit être configuré pour effectuer les deux étapes suivantes :

- Compilez le projet en utilisant le plug-in de MSBuild
- Connexion et zip alignent l’APK avec un magasin de clés de version valide.

Ces deux étapes seront abordées plus en détail dans les deux sections suivantes.

### <a name="creating-the-apk"></a>Création de l’APK

Cliquez sur le **ajouter une étape de build** bouton, puis sélectionnez **générer un projet Visual Studio ou une solution à l’aide de MSBuild**, comme illustré dans la capture d’écran ci-dessous :

![](jenkins-walkthrough-images/image36.png "Créer le fichier APK cliquez sur le bouton Ajouter build et sélectionner la Build, un projet Visual Studio ou une solution à l’aide de MSBuild")

Une fois que l’étape de génération est ajouté au projet, renseignez les champs de formulaire qui s’affichent. La capture d’écran suivante est un exemple du formulaire terminé :

![](jenkins-walkthrough-images/image37.png "Une fois que l’étape de génération est ajouté au projet, renseignez les champs de formulaire qui s’affichent")

Cette étape de build exécutera `xbuild` dans le **$WORKSPACE** dossier. Le fichier de Build MSBuild est défini comme le **Xamarin.Android.csproj** fichier. Le **Arguments de ligne de commande** spécifier une version Release de la cible **PackageForAndroid**. Le produit de cette étape sera un APK qui, à l’emplacement suivant :

    ```
    $WORKSPACE/[PROJECT NAME]/bin/Release
    ```

La capture d’écran suivante montre un exemple de ce fichier APK :

![](jenkins-walkthrough-images/image38.png "Cette capture d’écran montre un exemple de ce fichier APK")

Ce fichier APK n’est pas prêt pour le déploiement, car il n’a pas été signé avec un magasin de clés privée et zip doit être alignée.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>Signature et Zipaligning l’APK de mise en production

Signature et zipaligning l’APK sont techniquement deux tâches distinctes qui sont effectuées par les deux outils de ligne de commande séparée du Kit Android SDK. Toutefois, il est pratique de les exécuter dans une build action. Pour plus d’informations sur la connexion et zipaligning un APK, consultez la documentation Xamarin sur la préparation d’une application Android pour la mise en production.

Ces deux commandes nécessitent des paramètres de ligne de commande qui peuvent varier d’un projet. En outre, certains de ces paramètres de ligne de commande sont les mots de passe ne doivent pas apparaître dans la sortie de console lors de l’exécution de la build. Nous allons stocker certains de ces paramètres de ligne de commande dans les variables d’environnement. Les variables d’environnement requises pour la signature et/ou alignement zip sont décrits dans le tableau ci-dessous :

|Variable d’environnement|Description|
|--- |--- |
|KEYSTORE_FILE|Il s’agit du chemin d’accès au magasin de clés pour signer l’APK|
|KEYSTORE_ALIAS|La clé dans le magasin de clés qui sera utilisé pour signer l’APK.|
|INPUT_APK|Le fichier APK qui est créé par `xbuild`.|
|SIGNED_APK|Le fichier APK signé produites par `jarsigner`.|
|FINAL_APK|C’est le fichier zip aligné APK qui est généré par `zipalign`.|
|STORE_PASS|Il s’agit du mot de passe est utilisé pour accéder au contenu du magasin de clés pour authentifier le fichier.|

Comme décrit dans la section Configuration requise, vous peuvent définir ces variables d’environnement lors de la génération à l’aide du plug-in EnvInject. La tâche doit comprendre une nouvelle build étape ajoutée basé sur les variables d’environnement Inject, comme illustré dans la capture d’écran suivante :

![](jenkins-walkthrough-images/image39.png "La tâche doit comprendre une nouvelle build étape ajoutée basé sur les variables d’environnement Inject")

Dans le **propriétés contenu** forment le champ qui apparaît, l’environnement de variables sont ajoutés, une par ligne, au format suivant :

    ```
    ENVIRONMENT_VARIABLE_NAME = value
    ```

La capture d’écran suivante montre les variables d’environnement qui sont nécessaires pour signer l’APK :

![](jenkins-walkthrough-images/image40.png "Cette capture d’écran montre les variables d’environnement qui sont nécessaires pour signer l’APK")

Notez que certaines variables d’environnement pour les fichiers APK reposent sur le `WORKSPACE` variable d’environnement.

La variable d’environnement final est le mot de passe pour accéder au contenu de combinaison de touches : `STORE_PASS`. Les mots de passe sont des valeurs sensibles qui doivent être masquées ou omis dans les fichiers journaux. Le plug-in EnvInject peut être configuré pour protéger ces valeurs afin qu’ils ne s’affichent pas dans les journaux.

Immédiatement avant le **Build** section de la configuration du travail est un **environnement Build** section. Lorsque le **injecter des mots de passe** case à cocher est activée ou désactivée, une forme champs apparaissent. Ces champs de formulaire sont utilisés pour capturer le nom et la valeur de la variable d’environnement. La capture d’écran suivante est un exemple d’ajout du `STORE_PASS` variable d’environnement :

![](jenkins-walkthrough-images/image41.png "Cette capture d’écran est un exemple d’ajout de la variable d’environnement STOREPASS")

Une fois que les variables d’environnement ont été initialisés, l’étape suivante consiste à ajouter une étape de génération pour la signature et code postal alignement de l’APK. Immédiatement après l’étape de build pour insérer les variables d’environnement sera un autre **exécuter un script shell** build de commande exécutera `jarsigner` et `zipalign`. Chaque commande prendra une ligne, comme indiqué dans l’extrait suivant :

    ```
    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK
    ```

La capture d’écran suivante montre un exemple de la façon d’entrer le `jarsigner` et `zipalign` commandes dans l’étape :

![](jenkins-walkthrough-images/image42.png "Cette capture d’écran montre un exemple montrant comment entrer les commandes jarsigner et zipalign dans l’étape")

Une fois que toutes les actions de génération sont en place, il est conseillé de déclencher une build manuelle pour vérifier que tout fonctionne. Si la build échoue, le **sortie de la Console** doivent être vérifiées pour plus d’informations sur ce qui a provoqué l’échec de la build.

### <a name="submitting-tests-to-test-cloud"></a>Envoi de Tests vers Test Cloud

Les tests automatisés peuvent être envoyées vers Test Cloud à l’aide des commandes d’environnement. Pour plus d’informations sur la configuration d’une série de tests dans Xamarin Test Cloud, consultez ce guide pour l’utilisation de [Xamarin.UITest](/appcenter/test-cloud/preparing-for-upload/uitest/).

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous introduit Jenkins comme un serveur de builds sur macOS et configuré pour compiler et préparer des applications mobiles Xamarin pour la mise en production. Nous avons installé Jenkins sur un ordinateur macOS, ainsi que plusieurs plug-ins pour prendre en charge le processus de génération. Nous avons créé et configuré un travail qui va extraire le code à partir de TFS ou Git, puis compiler ce code dans une application de prêt de mise en production. Nous avons également examiné de deux façons de planifier le moment où les travaux doit-elle être exécutés.

## <a name="related-links"></a>Liens associés

- [Intégration continue](~/tools/ci/index.md)
- [Test App Center](https://docs.microsoft.com/appcenter/test-cloud/)
