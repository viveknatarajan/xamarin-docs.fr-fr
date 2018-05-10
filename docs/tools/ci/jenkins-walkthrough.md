---
title: À l’aide de Jenkins avec Xamarin
description: Ce guide illustre comment configurer Jenkins comme un serveur d’intégration continue et automatiser la compilation des applications mobiles créées avec Xamarin. Il décrit comment installer Jenkins sur OS X, configurez-le et paramétrer des tâches pour compiler des applications de Xamarin.iOS et Xamarin.Android lorsque des modifications sont validées dans le système de gestion de code source.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: 1052507bfbf06e264f9e9da89be1e0f35fa70ce1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="using-jenkins-with-xamarin"></a>À l’aide de Jenkins avec Xamarin

_Ce guide illustre comment configurer Jenkins comme un serveur d’intégration continue et automatiser la compilation des applications mobiles créées avec Xamarin. Il décrit comment installer Jenkins sur OS X, configurez-le et paramétrer des tâches pour compiler des applications de Xamarin.iOS et Xamarin.Android lorsque des modifications sont validées dans le système de gestion de code source._

[Introduction à l’intégration continue avec Xamarin](~/tools/ci/intro-to-ci.md) présente l’intégration continue en tant qu’une pratique de développement de logiciels utiles permettant d’avertir du code défectueux ou incompatible. L’élément de configuration permet aux développeurs les problèmes et résoudre les problèmes qu’ils surviennent et conserve le logiciel dans un état approprié pour le déploiement. Cette procédure pas à pas explique comment utiliser le contenu à partir de ces deux documents ensemble.

Ce guide montre comment installer Jenkins sur un ordinateur dédié exécutant OS X et les configurer pour s’exécuter automatiquement au démarrage de l’ordinateur. Une fois Jenkins est installé, nous allons installer le plug-ins supplémentaires pour prendre en charge MS Build. Jenkins prend en charge Git prêtes à l’emploi. Si TFS est utilisé pour le contrôle de code source, un plug-in et ligne de commande des utilitaires supplémentaires doivent également être installé.

Une fois Jenkins est configuré et des plug-ins nécessaires ont été installés, nous allons créer un ou plusieurs travaux pour compiler les projets Xamarin.Android et Xamarin.iOS. Une tâche est une collection d’étapes et les métadonnées requises pour exécuter des tâches. Généralement, un travail est constitué des éléments suivants :

-  **Source de gestion du Code (SCM)** – il s’agit d’une entrée de métadonnées dans les fichiers de configuration Jenkins qui contient des informations sur comment se connecter à un contrôle de code source et des fichiers à récupérer.
-  **Déclencheurs** – déclencheurs sont utilisés pour démarrer une tâche en fonction de certaines actions, telles que lorsqu’un développeur valide les modifications dans le référentiel de code source.
-  **Instructions de génération** – il s’agit d’un plug-in ou un script qui compile le code source et produit un fichier binaire qui peut être installé sur les périphériques mobiles.
-  **Générer des Actions facultatives** : cela peut inclure l’exécution de tests unitaires, effectuer l’analyse statique sur le code, signature de code, ou à partir d’une autre tâche pour effectuer d’autres de créer le travail associé.
-  **Notifications** – un travail peut envoyer un type de notification sur l’état d’une build.
-  **Sécurité** : bien que facultative, il est fortement recommandé que les fonctionnalités de sécurité Jenkins soit également activée.


Ce guide vous guide dans la configuration d’un serveur Jenkins couvrant chacun de ces points. À la fin de celle-ci, nous devrions obtenir une bonne compréhension des comment installer et configurer Jenkins pour créer des extensions IPA et de APK pour nos projets mobiles Xamarin.

## <a name="requirements"></a>Spécifications

Le serveur de builds idéale est un ordinateur autonome dédié pour le seul but de génération et de test éventuellement de l’application. Un ordinateur dédié garantit que les artefacts qui peuvent être requises pour d’autres rôles (telle que celle d’un serveur web) ne contaminent pas la build. Par exemple, si le serveur de builds agit également comme un serveur web, le serveur web peut nécessiter une version en conflit d’une bibliothèque commune. En raison de ce conflit le serveur web peut ne pas fonctionne correctement ou Jenkins peut créer des builds qui ne fonctionnent pas lors du déploiement pour les utilisateurs.

Le serveur de builds pour les applications mobiles Xamarin est configuré de façon similaire de station de travail du développeur. Il a un compte d’utilisateur dans les Jenkins, Visual Studio pour Mac, et Xamarin.iOS et Xamarin.Android seront installés. Tous le certificats, la configuration des profils et les magasins de clés de signature de code doivent être également installés. *En général, compte d’utilisateur du serveur de build est séparé de vos comptes de développeur : veillez à installer et configurer tous les logiciels, les clés et les certificats lorsque vous êtes connecté avec le compte d’utilisateur de serveur build.*

Le diagramme suivant illustre tous ces éléments sur un serveur de build Jenkins classique :

 [![](jenkins-walkthrough-images/image1.png "Ce diagramme illustre tous ces éléments sur un serveur de build Jenkins classique")](jenkins-walkthrough-images/image1.png#lightbox)

les applications iOS peuvent uniquement être générées et connectées sur un ordinateur exécutant Mac OS X. Un Mini Mac est une option économique raisonnable, mais n’importe quel ordinateur capable d’exécuter OS X 10.10 (Yosemite) ou version ultérieure est suffisante.

Si TFS est utilisé pour le contrôle de code source, vous souhaitez installer [Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785), disponible à partir de Microsoft. Team Explorer Everywhere fournit un accès inter-plateformes à TFS au Terminal dans OS X.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>L’installation de Jenkins

La première tâche à l’aide de Jenkins consiste à installer. Il existe trois manières d’exécuter Jenkins sur OS x :

-  En tant que démon, en cours d’exécution en arrière-plan.
-  À l’intérieur d’un conteneur de servlet tel que Tomcat, Jetty ou JBoss.
-  En tant que processus normal en cours d’exécution sous un compte d’utilisateur.


Les applications d’intégration continue plus traditionnelles exécutent en arrière-plan, sous la forme d’un démon (sur OS X ou \*nix) ou en tant que service (sous Windows). Cela est approprié pour les scénarios où il n’existe aucune interaction de l’interface graphique utilisateur requis et où le programme d’installation de l’environnement de génération peut être effectuée facilement. Les applications mobiles requièrent également keystores et la signature des certificats qui peuvent être problématiques pour l’accès lorsque Jenkins s’exécute comme un démon. En raison de ces problèmes, ce document se concentre sur le troisième scénario : en cours d’exécution Jenkins sous un compte d’utilisateur sur le serveur de builds.

Jenkins.App est un moyen pratique pour installer Jenkins. Il s’agit d’un wrapper AppleScript qui simplifie le démarrage et l’arrêt d’un serveur Jenkins. Au lieu d’exécuter dans un interpréteur de commandes bash, Jenkins s’exécute comme une application avec une icône dans la station d’accueil, comme indiqué dans la capture d’écran suivante :

 [![](jenkins-walkthrough-images/image2.png "Au lieu d’exécuter dans un interpréteur de commandes bash, Jenkins s’exécute comme une application avec une icône dans la station d’accueil, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image2.png#lightbox)

Démarrage ou arrêt Jenkins est aussi simple que le démarrage ou arrêt Jenkins.App.

Pour installer Jenkins.App, téléchargez la dernière version à partir de la page de téléchargement du projet, dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image3.png "Application, page, illustrée dans cette capture d’écran de téléchargement de la dernière version des projets de téléchargement")](jenkins-walkthrough-images/image3.png#lightbox)

Extrayez le fichier zip dans le `/Applications` dossier sur votre serveur de builds, début il comme toute autre application OS X.
La première fois que vous démarrez Jenkins.App, il présente une boîte de dialogue qui vous informe qu’il télécharge Jenkins :

 [![](jenkins-walkthrough-images/image4.png "Application, il présente une boîte de dialogue qui vous informe qu’il télécharge Jenkins")](jenkins-walkthrough-images/image4.png#lightbox)

Une fois Jenkins.App son téléchargement, il affiche une autre boîte de dialogue vous demandant si vous souhaitez personnaliser le démarrage de Jenkins, comme indiqué dans la capture d’écran suivante :

 [![](jenkins-walkthrough-images/image5.png "Application a terminé son téléchargement, il affiche une autre boîte de dialogue vous demandant si vous souhaitez personnaliser le démarrage de Jenkins, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image5.png#lightbox)

Personnalisation de Jenkins est facultatif et n’a pas à effectuer chaque fois que l’application est lancée : les paramètres par défaut pour Jenkins fonctionnent pour la plupart des situations.

S’il est nécessaire de personnaliser Jenkins, cliquez sur le **modifier les valeurs par défaut** bouton. Cela vous présente deux boîtes de dialogue consécutives : une qui demande les paramètres de ligne de commande Java et une autre qui demande les paramètres de ligne de commande Jenkins. Les deux captures d’écran suivantes illustrent ces deux boîtes de dialogue :

 [![](jenkins-walkthrough-images/image6.png "Cette capture d’écran montre les boîtes de dialogue")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "Cette capture d’écran montre les boîtes de dialogue")](jenkins-walkthrough-images/image7.png#lightbox)

Une fois Jenkins est en cours d’exécution, vous voudrez définie comme un élément de connexion afin qu’il démarre chaque fois que les connexions utilisateur dans l’ordinateur. Cela en cliquant sur l’icône de Jenkins dans sa station d’accueil et en choisissant **Options... Ouvrir à la connexion**, comme illustré dans la capture d’écran suivante :

 [![](jenkins-walkthrough-images/image8.png "Vous pouvez faire cela en cliquant sur l’icône de Jenkins dans sa station d’accueil et en choisissant donnéesOpen lors de la connexion, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image8.png#lightbox)

Cela entraîne Jenkins.App lancer automatiquement chaque fois que l’utilisateur se connecte, mais pas lorsque l’ordinateur démarre. Il est possible de spécifier un compte d’utilisateur du système d’exploitation X utilisera automatiquement avec lequel se connecter au moment du démarrage. Ouvrez le **Préférences système**et sélectionnez le **utilisateur et groupes** icône comme indiqué dans cette capture d’écran :

 [![](jenkins-walkthrough-images/image9.png "Ouvrir les préférences système, puis sélectionnez l’icône de groupes d’utilisateurs comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image9.png#lightbox)

Cliquez sur le **Options de connexion** bouton, puis choisissez le compte du système d’exploitation X utilisera pour la connexion au moment du démarrage.

À ce stade Jenkins a été installé. Toutefois, si vous souhaitez créer des applications mobiles Xamarin, nous devons installer certains plug-ins.


### <a name="installing-plugins"></a>L’installation de plug-ins

Une fois le programme d’installation Jenkins.App terminée, elle démarre Jenkins et lancer le navigateur web avec l’URL http://localhost:8080, comme illustré dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image10.png "8080, comme indiqué dans cette capture d’écran")](jenkins-walkthrough-images/image10.png#lightbox)

Dans cette page, sélectionnez **Jenkins > Jenkins de gérer > Gérer les plug-ins** à partir du menu dans le coin supérieur gauche, comme illustré dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image11.png "À partir de cette page, sélectionnez Jenkins gérer Jenkins gérer les plug-ins dans le menu dans le coin supérieur gauche")](jenkins-walkthrough-images/image11.png#lightbox)

Cette action affiche la **Gestionnaire de plug-in de Jenkins** page. Si vous cliquez sur l’onglet disponible, vous verrez une liste des plug-ins de plus de 600 qui peut être téléchargé et installé. Cela est illustré dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image12.png "Si vous cliquez sur l’onglet disponible, vous verrez une liste des plug-ins de plus de 600 qui peut être téléchargé et installé")](jenkins-walkthrough-images/image12.png#lightbox)

Faire défiler toutes les 600 les plug-ins à trouver que quelques peuvent être fastidieuses et sujette aux erreurs. Jenkins fournit un filtre de champ de recherche dans le coin supérieur droit de l’interface. Utilisation de ce champ de filtre pour rechercher simplifiera la localisation et installé un ou tous les plug-ins suivants :

-  **Le plug-in de Jenkins MSBuild** – ce plug-in vous permet de générer de Visual Studio et Visual Studio pour les projets (.csproj) et les solutions de Mac (.sln).
-  **Plug-in d’environnement injecteur** – il s’agit d’un plug-in facultatif mais utile qui permet de définir des variables d’environnement au niveau de la tâche et au niveau de build. Il offre également une protection supplémentaire pour les variables telles que les mots de passe utilisés pour le code de signer l’application. Il est parfois abrégée en tant que le *EnvInject Plugin* .
-  **Team Foundation Server Plugin** – il s’agit d’un plug-in facultatif qui est uniquement requis si vous utilisez Team Foundation Server ou Team Foundation Services pour le contrôle de code source.

Jenkins prend en charge Git sans les plug-ins supplémentaires.

Après l’installation de tous les plug-ins, que vous souhaitez redémarrer Jenkins et configurer les paramètres globaux pour chaque plug-in. Les paramètres globaux pour un plug-in sont accessibles en sélectionnant **Jenkins > Gérer les Jenkins > configurer le système** à partir de l’angle supérieur gauche, comme illustré dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image13.png "Les paramètres globaux pour un plug-in sont accessibles en sélectionnant Jenkins / gérer les Jenkins / de la configuration du système à partir de l’angle supérieur gauche main angle")](jenkins-walkthrough-images/image13.png#lightbox)

Lorsque vous sélectionnez cette option, vous serez dirigé vers le **configuration du système [Jenkins]** page. Cette page contient des sections pour configurer Jenkins lui-même et pour définir une partie des valeurs globales de plug-in.  La capture d’écran ci-dessous illustre un exemple de cette page :

 [![](jenkins-walkthrough-images/image14.png "Cette capture d’écran illustre un exemple de cette page")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>Configuration du plug-in de MSBuild

Le plug-in de MSBuild doit être configuré pour utiliser **/Library/Frameworks/Mono.framework/Commands/xbuild** compilation Visual Studio pour les fichiers solution et projet Mac. Faites défiler la **configuration du système [Jenkins]** page jusqu'à ce que le **MSBuild d’ajouter** bouton s’affiche, comme illustré dans la capture d’écran ci-dessous :

 [![](jenkins-walkthrough-images/image15.png "Faites défiler la page Configurer les Jenkins système jusqu'à ce que le bouton Ajouter un MSBuild s’affiche")](jenkins-walkthrough-images/image15.png#lightbox)

Cliquez sur ce bouton, puis remplissez le **nom** et **chemin d’accès** à **MSBuild** champs sur le formulaire qui s’affiche. Le nom de votre **MSBuild** doit être un nom significatif, lors de la **chemin d’accès à MSBuild** doit être le chemin d’accès `xbuild`, qui est généralement **Library/infrastructures / Mono.framework/Commands/xbuild**. Une fois que nous enregistrer les modifications en cliquant sur l’enregistrement ou le bouton Appliquer en bas de la page, Jenkins sera en mesure d’utiliser `xbuild` pour compiler vos solutions.

#### <a name="configuring-the-tfs-plugin"></a>Configuration du plug-in TFS

Cette section est obligatoire si vous envisagez d’utiliser TFS pour votre contrôle de code source.

Dans l’ordre d’une station de travail du système d’exploitation X interagir avec un serveur TFS, Team Explorer Everywhere doit être installé sur la station de travail. Team Explorer Everywhere est un ensemble d’outils à partir de Microsoft et qui inclut un client de ligne de commande multiplateforme pour accéder à TFS. Team Explorer Everywhere peut être téléchargé à partir de Microsoft et installé en trois étapes :

1. Décompressez le fichier d’archive dans un répertoire qui est accessible au compte d’utilisateur. Par exemple, vous pouvez décompresser le fichier à **~/tee**.
2. Configurer le chemin d’accès système ou d’interpréteur de commandes pour inclure le dossier qui contient les fichiers qui ont été décompressés à l’étape ci-dessus. Par exemple :

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Pour confirmer que Team Explorer Everywhere est installé, ouvrez une session Terminal Server et exécutez le `tf` commande. Si tf est configuré correctement, vous verrez la sortie suivante dans votre session Terminal Server :

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

Une fois que le client de ligne de commande pour TFS est installé, Jenkins doit être configuré avec le chemin complet vers le `tf` client de ligne de commande. Faites défiler la **configuration du système [Jenkins]** page jusqu'à ce que vous trouviez la section de Team Foundation Server, comme indiqué dans la capture d’écran suivante :

 [![](jenkins-walkthrough-images/image17.png "Faites défiler la page Configurer les Jenkins système jusqu'à ce que vous recherchez la section de Team Foundation Server")](jenkins-walkthrough-images/image17.png#lightbox)

Entrez le chemin complet vers le `tf` de commandes, puis cliquez sur le **enregistrer** bouton.

### <a name="configure-jenkins-security"></a>Configurer la sécurité de Jenkins

Lors de la première installation, Jenkins la sécurité est désactivée, il est donc possible pour tous les utilisateurs de configurer et exécuter n’importe quel type de tâche anonymement. Cette section explique comment configurer la sécurité à l’aide de la base de données utilisateur Jenkins pour configurer l’authentification et l’autorisation.

Paramètres de sécurité sont accessibles en sélectionnant **Jenkins > Gérer les Jenkins > configurer la sécurité globale**, comme indiqué dans cette capture d’écran :

 [![](jenkins-walkthrough-images/image18.png "Paramètres de sécurité sont accessibles en sélectionnant Jenkins / gérer les Jenkins / configurer la sécurité globale")](jenkins-walkthrough-images/image18.png#lightbox)

Sur le **configurer la sécurité globale** page, vérifiez le **activer la sécurité** case à cocher et **le contrôle d’accès** formulaire doit apparaître, semblable à la capture d’écran suivante :

 [![](jenkins-walkthrough-images/image19.png "Dans la page Configurer la sécurité globale, vérifiez la sécurité d’activer la case à cocher et l’écran de contrôle d’accès doivent apparaître, comme dans cette capture d’écran")](jenkins-walkthrough-images/image19.png#lightbox)

Activer/désactiver la case pour **base de données utilisateur des Jenkins** dans le **Section de domaine de sécurité**et vérifiez que **permettre aux utilisateurs de s’inscrire** est également activée, comme illustré dans le capture d’écran suivante :

 [![](jenkins-walkthrough-images/image20.png "Activer/désactiver la case d’option de base de données de Jenkins propre utilisateur dans la Section de domaine de sécurité et assurez-vous qu’Autoriser les utilisateurs à inscrire est également cochée.")](jenkins-walkthrough-images/image20.png#lightbox)

Enfin, redémarrez Jenkins et créer un nouveau compte. Le premier compte créé est le compte racine, et ce compte est automatiquement promu à un administrateur. Revenez à la **configurer la sécurité globale** page et marquer le **sécurité basée sur une matrice** case d’option. Le compte racine doit disposer un accès complet et le compte anonyme censé donner l’accès en lecture seule, comme indiqué dans la capture d’écran suivante :

 [![](jenkins-walkthrough-images/image21.png "Le compte racine doit disposer un accès complet et le compte anonyme doit disposer de l’accès en lecture seule")](jenkins-walkthrough-images/image21.png#lightbox)

Une fois que ces paramètres sont enregistrés et Jenkins est redémarré, la sécurité est activée.


#### <a name="disabling-security"></a>Désactivation de la sécurité

En cas de mot de passe oublié ou le verrouillage de Jenkins à l’échelle, il est possible de désactiver la sécurité en suivant ces étapes :

1. Arrêter Jenkins. Si vous utilisez Jenkins.app, vous pouvez pour cela en cliquant sur l’icône Jenkins.App dans sa station d’accueil et en sélectionnant Quitter dans le menu qui s’affiche :

    ![](jenkins-walkthrough-images/image19.png "Icône de l’application de la station d’accueil et en sélectionnant Quitter dans le menu qui s’affiche")
2. Ouvrez le fichier **~/.jenkins/config.xml** dans un éditeur de texte.
3. Modifiez la valeur de la `<usesecurity></usesecurity>` élément `true` à `false`.
4. Supprimer le `<authorizationstrategy></authorizationstrategy>` et `<securityrealm></securityrealm>` les éléments à partir du fichier.
5. Redémarrez Jenkins.

## <a name="setting-up-a-job"></a>Configuration d’un projet

Au niveau supérieur, Jenkins organise tous les différentes tâches nécessaires à la génération du logiciel dans un *travail*. Une tâche a également les métadonnées associées, qui fournit des informations sur la build, telles que comment obtenir le code source, la fréquence à laquelle la build doit s’exécuter, toutes les variables spéciales qui sont nécessaires pour la génération et comment avertir les développeurs si la build échoue.

Travaux sont créés en sélectionnant **Jenkins > Nouveau travail** à partir du menu dans le coin supérieur droit, comme indiqué dans la capture d’écran suivante :


![](jenkins-walkthrough-images/image22.png "Travaux sont créés en sélectionnant Jenkins nouvelle tâche à partir du menu dans le coin supérieur droit")

Cette action affiche la **nouvelle tâche [Jenkins]** page. Entrez un nom pour le travail, puis sélectionnez le **générer un projet de logiciel de forme libre** case d’option. La capture d’écran suivante montre un exemple :

![](jenkins-walkthrough-images/image23.png "Entrez un nom pour le travail, puis sélectionnez la Build une case de projet de logiciel de forme libre")

En cliquant sur le **OK** bouton affiche la page de configuration pour le travail. Cela doit ressembler à la capture d’écran suivante :

![](jenkins-walkthrough-images/image24.png "Cela doit ressembler à cette capture d’écran")

Jenkins organise les tâches dans un répertoire sur le disque dur situé dans le chemin d’accès suivant : **~/.jenkins/jobs/[JOB nom]**

Ce dossier contient tous les fichiers et les artefacts spécifiques de la tâche telles que les journaux, les fichiers de configuration et le code source qui doit être compilé.

Une fois que le travail initial a été créé, il doit être configuré avec un ou plusieurs des opérations suivantes :

 - Le système de gestion de code source doit être spécifié.
 - Un ou plusieurs *des actions de génération* doit être ajouté au projet. Voici les étapes ou les tâches requises pour générer l’application.
 - Le travail doit être affecté à un *déclencheur de build* – un ensemble d’instructions pour informer la fréquence à laquelle les Jenkins pour récupérer le code et générez le projet final.

### <a name="configuring-source-code-control"></a>Configuration du contrôle de Code Source

La première tâche est de Jenkins est de récupérer le code source depuis le système de gestion de code source. Jenkins prend en charge de nombreux les systèmes de gestion de code source populaires disponibles aujourd'hui. Cette section décrit deux systèmes courants, Git et Team Foundation Server. Chacun de ces systèmes de gestion du code source est décrite plus en détail dans les sections ci-dessous.

#### <a name="using-git-for-source-code-control"></a>À l’aide de Git pour le contrôle de Code Source

Si vous utilisez TFS pour le contrôle de code source, [ignorer](#Using_TFS_for_Source_Code_Management) cette section et passez à la section suivante à l’aide de TFS.

Jenkins prend en charge Git prêtes à l’emploi : aucun plug-ins supplémentaires ne sont nécessaires. Pour utiliser Git, cliquez sur le **Git** case d’option et entrez l’URL pour le référentiel Git, comme indiqué dans la capture d’écran suivante :

![](jenkins-walkthrough-images/image25.png "Pour utiliser Git, cliquez sur le bouton radio Git et entrez l’URL pour le référentiel Git")

Une fois que les modifications sont enregistrées, Git configuration est terminée.

#### <a name="using-tfs-for-source-code-management"></a>À l’aide de TFS pour la gestion du Code Source

Cette section s’applique uniquement aux utilisateurs TFS.

Cliquez sur le **Team Foundation Server** case d’option et la section de configuration de TFS doivent apparaître, semblables à ce qui est dans la capture d’écran suivante :


![](jenkins-walkthrough-images/image26.png "Cliquez sur le bouton radio de Team Foundation Server et de la section de configuration de TFS doit apparaître.")

Fournissez les informations nécessaires pour TFS. La capture d’écran suivante montre un exemple de formulaire rempli :

![](jenkins-walkthrough-images/image27.png "Cette capture d’écran montre un exemple de la forme achevée")

#### <a name="testing-the-source-code-control-configuration"></a>Test de la Configuration de contrôle de Code Source

Une fois que le contrôle de code source approprié a été configuré, cliquez sur **enregistrer** pour enregistrer les modifications. Cela vous renvoie à la page d’accueil de la tâche, qui ressemble à la capture d’écran suivante :

![](jenkins-walkthrough-images/image28.png "Cela vous renvoie à la page d’accueil de la tâche, qui ressemble à cette capture d’écran")

Le plus simple pour vérifier que le contrôle de code source est correctement configuré est déclencher une build manuellement, même si aucune action de build spécifiée. Pour démarrer une build manuellement, la page d’accueil de la tâche a un **générer maintenant** lien dans le menu de gauche, comme illustré dans la capture d’écran ci-dessous :

![](jenkins-walkthrough-images/image29.png "Pour démarrer une build manuellement, la page d’accueil de la tâche a un lien Créer maintenant dans le menu situé à gauche")

Quand une build a été démarrée, la boîte de dialogue Générer un historique affiche un cercle bleu clignote, une barre de progression, le numéro de build et l’heure de début de la build, semblable à la capture d’écran suivante :

![](jenkins-walkthrough-images/image30.png "Quand une build a été démarrée, la boîte de dialogue Générer un historique affiche un cercle clignotant bleu, une barre de progression, le numéro de build et l’heure de début de la build")

Si la tâche réussit, un cercle bleu s’affichera. Si le travail échoue, un cercle rouge s’affichera.

Pour vous aider à résoudre les problèmes qui peuvent se produire dans le cadre de la build, Jenkins capture toute la sortie de console pour le travail. Pour afficher la sortie de console, cliquez sur la tâche dans **générer un historique**, puis, dans le **la sortie de Console** lien dans le menu de gauche. La capture d’écran suivante affiche le **la sortie de Console** lien, ainsi que certaines de la sortie d’une tâche réussie :

![](jenkins-walkthrough-images/image31.png "Cette capture d’écran montre le lien de sortie de la Console, ainsi que certaines des résultats une fois la tâche")

#### <a name="location-of-build-artifacts"></a>Emplacement des artefacts de Build

Jenkins récupère la totalité du code source dans un dossier spécial appelé un *espace de travail*. Ce répertoire se trouve dans le dossier à l’emplacement suivant :

    ~/.jenkins/jobs/[JOB NAME]/workspace

Le chemin d’accès à l’espace de travail sera être stockée dans une variable d’environnement nommée `$WORKSPACE`.

Il est possible de parcourir le dossier de l’espace de travail de Jenkins en accédant à la page de lancement d’une tâche, puis en cliquant sur le **espace de travail** lien dans le menu de gauche. La capture d’écran suivante montre un exemple de l’espace de travail pour une tâche nommée **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Cette capture d’écran montre un exemple de l’espace de travail pour une tâche nommée HelloWorld")

### <a name="build-triggers"></a>Créer des déclencheurs

Il existe plusieurs stratégies différentes de l’initialisation des builds dans Jenkins : ils sont appelés *les déclencheurs de build*. Un déclencheur de build Jenkins permet de décider du moment auquel démarrer un travail et générez le projet. Deux des déclencheurs de build plus courantes sont :

- **Générer régulièrement** – ce déclencheur entraîne Jenkins démarrer un travail à des intervalles spécifiés, tels que toutes les deux heures ou à minuit les jours de semaine. La build démarre indépendamment de si il modifications ont été apportées dans le référentiel de code source.
- **Interrogation SCM** – ce déclencheur interrogera de contrôle de code source sur une base régulière. Si des modifications ont été validées dans le référentiel de code source, Jenkins démarre une nouvelle build.

Un déclencheur courants d’interrogation SCM est, car elle fournit des commentaires rapides lorsqu’un développeur valide les modifications qui provoquent l’arrêt de la génération. Cela est utile pour créer des alertes équipes que du code validé récemment pose des problèmes et permet de résoudre le problème, tandis que les modifications sont toujours actualisées à l’esprit les développeurs.

Builds périodiques sont souvent utilisés pour créer une version de l’application qui peut être distribuée aux testeurs. Par exemple, une build périodique peut être planifiée pour le vendredi soir afin que les membres de l’équipe de questions et réponses peuvent de tester le travail de la semaine précédente.


### <a name="compiling-a-xamarinios-applications"></a>La compilation d’une application Xamarin.iOS
Projets de Xamarin.iOS peuvent être compilés à la ligne de commande à l’aide de `xbuild` ou `msbuild`. La commande d’interpréteur de commandes s’exécute dans le contexte du compte d’utilisateur qui est en cours d’exécution Jenkins. Il est important que le compte d’utilisateur a accès pour le profil de configuration afin que l’application peut être empaquetée correctement pour la distribution. Il est possible d’ajouter cette commande d’interpréteur de commandes à la page de configuration du travail.

Faites défiler jusqu'à la **générer** section. Cliquez sur le **étape de génération ajouter** sélectionnez **exécuter shell**, comme illustré par la capture d’écran suivante :

![](jenkins-walkthrough-images/image33.png "Cliquez sur le bouton d’étape de génération ajouter et sélectionnez Exécuter shell")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Génération d’un projet Xamarin.Android

Génération d’un projet Xamarin.Android est très similaire dans son concept à la génération d’un projet Xamarin.iOS. Pour créer un fichier APK à partir d’un projet Xamarin.Android, Jenkins doit être configuré pour effectuer les deux étapes suivantes :

 - Compilez le projet en utilisant le plug-in de MSBuild
 - Signe et zip alignent APK avec un magasin de clés de version valide.

Ces deux étapes seront abordées plus en détail dans les deux sections suivantes.

### <a name="creating-the-apk"></a>Création APK

Cliquez sur le **étape de génération ajouter** bouton, puis sélectionnez **générer un projet Visual Studio ou une solution à l’aide de MSBuild**, comme illustré dans la capture d’écran ci-dessous :

![](jenkins-walkthrough-images/image36.png "Création du fichier APK cliquez sur le bouton Ajouter build et sélectionner la Build d’un projet Visual Studio ou une solution à l’aide de MSBuild")

Une fois que l’étape de génération est ajouté au projet, renseignez les champs de formulaire qui s’affichent. La capture d’écran suivante est un exemple de formulaire rempli :

![](jenkins-walkthrough-images/image37.png "Une fois que l’étape de génération est ajouté au projet, renseignez les champs de formulaire qui s’affichent")


Cette étape de génération exécutera `xbuild` dans les **$WORKSPACE** dossier. Le fichier de génération MSBuild est défini sur le **Xamarin.Android.csproj** fichier. Le **Arguments de ligne de commande** spécifier une version finale de la cible de **PackageForAndroid**. Le produit de cette étape sera un APK que, à l’emplacement suivant :

    $WORKSPACE/[PROJECT NAME]/bin/Release

La capture d’écran suivante montre un exemple de cette APK :

![](jenkins-walkthrough-images/image38.png "Cette capture d’écran montre un exemple de cette APK")

Cette APK n’est pas prêt pour le déploiement, car il n’a pas été signé avec un magasin de clés privée et zip doit être alignée.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>Signature et Zipaligning APK de version

Signature et zipaligning APK sont techniquement deux tâches distinctes qui sont effectuées par les deux outils de ligne de commande distincte du SDK Android. Toutefois, il convient de les exécuter dans une build action. Pour plus d’informations sur la signature et zipaligning un APK, consultez la documentation de Xamarin sur la préparation d’une application Android pour la mise en production.

Ces deux commandes nécessitent des paramètres de ligne de commande qui peuvent varier d’un projet. En outre, certains de ces paramètres de ligne de commande sont des mots de passe ne doivent pas apparaître dans la sortie de console lors de la build est en cours d’exécution. Nous allons stocker certains de ces paramètres de ligne de commande dans les variables d’environnement. Les variables d’environnement requises pour la signature et/ou zip alignement sont décrites dans le tableau ci-dessous :

|Variable d’environnement|Description|
|--- |--- |
|KEYSTORE_FILE|Il s’agit du chemin d’accès pour le magasin de clés pour signer l’APK|
|KEYSTORE_ALIAS|La clé dans le magasin de clés qui permet de signer l’APK.|
|INPUT_APK|APK créé par `xbuild`.|
|SIGNED_APK|Signé APK généré par `jarsigner`.|
|FINAL_APK|Voici le fichier zip aligné APK généré par `zipalign`.|
|STORE_PASS|Il s’agit du mot de passe est utilisé pour accéder au contenu de la combinaison de touches pour passion le fichier.|

Comme décrit dans la section Configuration requise, vous peuvent définir ces variables d’environnement lors de la build à l’aide du plug-in EnvInject. La tâche doit avoir une nouvelle build étape ajoutée basé sur les variables d’environnement insertion, comme indiqué dans la capture d’écran suivante :

![](jenkins-walkthrough-images/image39.png "La tâche doit avoir une nouvelle build étape ajoutée basé sur les variables d’environnement insertion")

Dans le **propriétés contenu** forment le champ qui apparaît, l’environnement de variables sont ajoutées, un par ligne, dans le format suivant :

    ENVIRONMENT_VARIABLE_NAME = value

La capture d’écran suivante montre les variables d’environnement qui sont requis pour signer l’APK :

![](jenkins-walkthrough-images/image40.png "Cette capture d’écran montre les variables d’environnement qui sont requis pour signer l’APK")

Notez que certaines variables d’environnement pour les fichiers APK reposent sur la `WORKSPACE` variable d’environnement.

La variable d’environnement finale est le mot de passe pour accéder au contenu de la combinaison de touches : `STORE_PASS`. Les mots de passe sont les valeurs sensibles qui doivent être masquées ou omis dans les fichiers journaux. Le plug-in EnvInject peut être configuré pour protéger ces valeurs afin qu’ils n’apparaissent pas dans les journaux.

Immédiatement avant le **générer** section de la configuration de la tâche est un **environnement Build** section. Lorsque le **injecter des mots de passe** case à cocher est activée, une forme champs à afficher. Ces champs de formulaire sont utilisés pour capturer le nom et la valeur de la variable d’environnement. La capture d’écran suivante est un exemple d’ajout du `STORE_PASS` variable d’environnement :

![](jenkins-walkthrough-images/image41.png "Cette capture d’écran est un exemple d’ajout de la variable d’environnement STOREPASS")

Une fois que les variables d’environnement aient été initialisés, l’étape suivante consiste à ajouter une étape de génération pour la signature et le code postal alignement APK. Immédiatement après l’étape de génération pour insérer les variables d’environnement sera un autre **exécuter shell** build de commande qui s’exécutera `jarsigner` et `zipalign`. Chaque commande occupe une seule ligne, comme indiqué dans l’extrait de code suivant :


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

La capture d’écran suivante montre un exemple de la façon d’entrer le `jarsigner` et `zipalign` des commandes dans l’étape :

![](jenkins-walkthrough-images/image42.png "Cette capture d’écran montre un exemple de comment entrer les commandes jarsigner et zipalign dans l’étape")

Une fois que toutes les actions de génération sont en place, il est conseillé de déclencher une build manuelle pour vérifier que tout fonctionne. Si la build échoue, le **la sortie de Console** doivent être vérifiées pour plus d’informations sur ce qui a provoqué l’échec de la build.

### <a name="submitting-tests-to-test-cloud"></a>Envoi de Tests Test Cloud

Les tests automatisés peuvent être soumises au Cloud de Test à l’aide des commandes shell. Pour plus d’informations sur la configuration d’une série de tests dans Xamarin Test Cloud, nous avons guides d’utilisation de [Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/) ou [Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).


## <a name="summary"></a>Récapitulatif

Dans ce guide, nous introduites Jenkins comme un serveur de builds sur Mac OS X et qu’il est configuré pour compiler et préparer des applications mobiles Xamarin mise en production. Nous avons installé Jenkins sur un ordinateur Mac OS X, ainsi que plusieurs plug-ins pour prendre en charge le processus de génération. Nous avons créé et configuré un travail qui extrait de code à partir de Git ou TFS, puis le compiler ce code dans une application de prêt de version Nous avons exploré également deux façons de planifier le moment de l’exécution des travaux.

## <a name="related-links"></a>Liens associés

- [Intégration continue](https://developer.xamarin.com~/testcloud/ci/)
- [Envoi de Tests à Xamarin Test Cloud](https://developer.xamarin.com~/testcloud/submitting/)
- [Pourquoi est Jenkins pas pris en charge par Xamarin ?](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
