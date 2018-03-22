---
title: Sandboxing une application Xamarin.Mac
description: "Cet article traite des sandboxing une application Xamarin.Mac version sur l’App Store. Elle traite tous les éléments dans un bac à sable, tels que les répertoires de conteneur, droits, déterminé par l’utilisateur des autorisations, séparation de privilège et mise en œuvre du noyau."
ms.topic: article
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 71f076bb0dc7d552a041d48a6ce531dcff444de6
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="sandboxing-a-xamarinmac-app"></a>Sandboxing une application Xamarin.Mac

_Cet article traite des sandboxing une application Xamarin.Mac version sur l’App Store. Elle traite tous les éléments dans un bac à sable, tels que les répertoires de conteneur, droits, déterminé par l’utilisateur des autorisations, séparation de privilège et mise en œuvre du noyau._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez la possibilité de même sandbox une application comme vous le feriez lorsque vous travaillez avec Objective-C ou Swift.

[![Un exemple de l’application en cours d’exécution](sandboxing-images/intro01.png "un exemple de l’application en cours d’exécution")](sandboxing-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de bac à sable dans une application Xamarin.Mac et tous les éléments nécessaires à la mise en sandbox : répertoires de conteneur, droits, déterminé par l’utilisateur des autorisations, privilège la séparation et mise en œuvre du noyau. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` attributs utilisé pour associer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

## <a name="about-the-app-sandbox"></a>À propos de l’application bac à sable

Le bac à sable application fournit fort défense contre les dommages peuvent être provoqué par une application malveillante en cours d’exécution sur un Mac en limitant l’accès aux ressources du système d’une application.

Une application non sandbox a tous les droits de l’utilisateur qui exécute l’application et peut accéder ou faire tout ce que l’utilisateur peut. Si l’application contient des failles de sécurité (ou n’importe quelle infrastructure qu’il utilise), un pirate peut potentiellement exploiter ces faiblesses et permet de prendre le contrôle de l’adresse Mac elle s’exécute sur l’application.

En limitant l’accès aux ressources sur chaque application, une application bac à sable fournit une ligne de défense contre les risques de vol ou de corruption malveillantes sur la partie d’une application en cours d’exécution sur l’ordinateur de l’utilisateur.

Le bac à sable d’application est une technologie de contrôle d’accès intégrée macOS (appliquée au niveau du noyau) qui fournit une stratégie de double :

1. Application bac à sable permet au développeur décrire _comment_ une application va interagir avec le système d’exploitation et, de cette façon, cette autorisation est accordée uniquement les droits d’accès qui sont requis pour effectuer le travail et pas davantage.
2. Le bac à sable application permet à l’utilisateur à accéder en toute transparence accorder l’accès au système via l’ouvrir et enregistrer des boîtes de dialogue, faites glisser les opérations et les interactions utilisateur communs.

### <a name="preparing-to-implement-the-app-sandbox"></a>Préparation à l’implémentation d’application bac à sable

Les éléments de l’application bac à sable qui seront abordés en détail dans l’article sont les suivantes :

- Répertoires de conteneur
- Droits d’accès
- Déterminé par l’utilisateur des autorisations
- Séparation de privilège
- Mise en œuvre du noyau

Une fois que vous avez compris ces détails, vous serez en mesure de créer un plan pour l’adoption d’application bac à sable dans votre application Xamarin.Mac.

Tout d’abord, vous devez déterminer si votre application est un bon candidat pour le bac à sable (la plupart des applications sont). Ensuite, vous devez résoudre les incompatibilités de l’API et déterminer les éléments d’application bac à sable vous avez besoin. Enfin, envisager l’utilisation de séparation de privilège vous optimisez au niveau de protection de l’application.

Lors de l’adoption d’application bac à sable, certains emplacements de système de fichiers que votre application utilise sera différentes. En particulier, votre application aura un répertoire conteneur qui sera utilisé pour les fichiers de prise en charge d’application, des bases de données, des caches et tous les autres fichiers qui ne sont pas des documents utilisateur. MacOS et Xcode fournissent la prise en charge pour migrer ces types de fichiers à partir de leurs emplacements hérités au conteneur.

## <a name="sandboxing-quick-start"></a>Démarrage rapide de bac à sable

Dans cette section, nous allons créer une application Xamarin.Mac simple qui utilise un affichage Web (qui nécessite une connexion de réseau est limitée sous sandboxing, sauf si spécifiquement demandées) en tant qu’un exemple de prise en main de l’application Sandbox.

Nous allons vérifier que l’application est réellement bac à sable et savoir comment résoudre les erreurs d’application Sandbox.

### <a name="creating-the-xamarinmac-project"></a>Création du projet Xamarin.Mac

Nous allons, procédez comme suit pour créer notre exemple de projet :

1. Démarrez Visual Studio pour Mac et cliquez sur le **nouvelle Solution...** compte.
2. À partir de la **nouveau projet** boîte de dialogue, sélectionnez **Mac** > **application** > **/Cocoa application**: 

    [![Création d’une nouvelle application/Cocoa](sandboxing-images/sample01.png "création d’une nouvelle application/Cocoa")](sandboxing-images/sample01-large.png#lightbox)
3. Cliquez sur le **suivant** bouton, entrez `MacSandbox` pour le nom du projet et cliquez sur le **créer** bouton : 

    [![Entrer le nom de l’application](sandboxing-images/sample02.png "entrer le nom de l’application")](sandboxing-images/sample02-large.png#lightbox)
4. Dans le **Solution remplissage**, double-cliquez sur le **Main.storyboard** fichier à ouvrir pour le modifier dans Xcode : 

    [![Modification de la table de montage séquentiel principal](sandboxing-images/sample03.png "modification le storyboard principal")](sandboxing-images/sample03-large.png#lightbox)
5. Faites glisser un **affichage Web** sur la fenêtre, redimensionner pour remplir la zone de contenu et la définir pour augmenter et réduire la fenêtre : 

    [![Ajout d’une vue web](sandboxing-images/sample04.png "Ajout d’un affichage web")](sandboxing-images/sample04-large.png#lightbox)
6. Créer une sortie pour l’affichage web appelé `webView`: 

    [![Création d’un nouveau magasin](sandboxing-images/sample05.png "création d’un nouveau magasin")](sandboxing-images/sample05-large.png#lightbox)
7. Revenez à Visual Studio pour Mac et double-cliquez sur le **ViewController.cs** de fichiers dans le **Solution remplissage** à ouvrir pour le modifier.
8. Ajoutez le code suivant à l’aide d’instruction : `using WebKit;`
9. Rendre le `ViewDidLoad` détaillée de la méthode comme suit : 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Enregistrez les modifications apportées.

Vous exécutez l’application et vous assurer que le site Web Apple s’affiche dans la fenêtre comme suit :

[![Afficher un exemple d’application exécution](sandboxing-images/sample06.png "montrant un exemple d’application exécution")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Signature et l’application de configuration

Avant que nous pouvons activer le bac à sable d’application, nous devons d’abord configurer et connecter notre application Xamarin.Mac.

Permettent d’effectuer les opérations suivantes :

1. Ouvrez une session sur le portail des développeurs Apple : 

    [![Connectez le portail des développeurs Apple](sandboxing-images/sign01.png "journalisation dans le portail des développeurs Apple")](sandboxing-images/sign01-large.png#lightbox)
2. Sélectionnez **certificats, les identificateurs et les profils**: 

    [![Sélection de certificats, d’identificateurs et de profils](sandboxing-images/sign02.png "Sélection de certificats, d’identificateurs et de profils")](sandboxing-images/sign02-large.png#lightbox)
3. Sous **les applications Mac**, sélectionnez **identificateurs**: 

    [![Sélection d’identificateurs](sandboxing-images/sign03.png "en sélectionnant des identificateurs")](sandboxing-images/sign03-large.png#lightbox)
4. Créer un nouvel ID de l’application : 

    [![Création d’un nouvel ID d’application](sandboxing-images/sign04.png "création d’un nouvel ID d’application")](sandboxing-images/sign04-large.png#lightbox)
5. Sous **profils de configuration**, sélectionnez **développement**: 

    [![Sélection de développement](sandboxing-images/sign05.png "en sélectionnant le développement")](sandboxing-images/sign05-large.png#lightbox)
6. Créer un nouveau profil et sélectionnez **le développement d’applications Mac**: 

    [![Création d’un profil](sandboxing-images/sign06.png "création d’un profil")](sandboxing-images/sign06-large.png#lightbox)
7. Sélectionnez l’ID d’application que nous avons créés ci-dessus : 

    [![Sélection de l’ID d’application](sandboxing-images/sign07.png "en sélectionnant l’ID d’application")](sandboxing-images/sign07-large.png#lightbox)
8. Sélectionnez les développeurs pour ce profil : 

    [![Les développeurs d’ajout](sandboxing-images/sign08.png "aux développeurs d’ajout")](sandboxing-images/sign08-large.png#lightbox)
9. Sélectionnez les ordinateurs pour ce profil : 

    [![Sélectionnez les ordinateurs autorisés](sandboxing-images/sign09.png "en sélectionnant les ordinateurs autorisés")](sandboxing-images/sign09-large.png#lightbox)
10. Nommez le profil : 

    [![Attribution d’un nom du profil](sandboxing-images/sign10.png "donnant le profil d’un nom")](sandboxing-images/sign10-large.png#lightbox)
11. Cliquez sur le **fait** bouton.

> [!IMPORTANT]
> Dans certains cas, vous devrez télécharger le nouveau profil de configuration à partir du portail des développeurs d’Apple directement et double-cliquez dessus pour l’installer. Vous devrez peut-être également arrêter et redémarrer Visual Studio pour Mac avant d’être en mesure d’accéder au nouveau profil.

Ensuite, nous avons besoin charger le nouvel ID d’application et le profil sur l’ordinateur de développement. Nous allons effectuer les opérations suivantes :

1. Démarrez Xcode et sélectionnez **préférences** à partir de la **Xcode** menu : 

    ![Modification des comptes dans Xcode](sandboxing-images/sign11.png "modification des comptes dans Xcode")
2. Cliquez sur le **afficher les détails...**  bouton : 

    ![Cliquez sur le bouton Afficher les détails](sandboxing-images/sign12.png "en cliquant sur le bouton Afficher les détails")
3. Cliquez sur le **Actualiser** bouton (dans le coin inférieur gauche).
4. Cliquez sur le **fait** bouton.

Ensuite, nous devons sélectionner l’ID d’application et un profil de configuration nouvelle dans notre projet Xamarin.Mac. Nous allons effectuer les opérations suivantes :

1. Dans le **Solution remplissage**, double-cliquez sur le **Info.plist** fichier à ouvrir pour le modifier.
2. Vérifiez que le **identificateur de lot** correspond à notre ID d’application créées précédemment (exemple : `com.appracatappra.MacSandbox`) : 

    [![Modification de l’identificateur de lot](sandboxing-images/sign13.png "modification de l’identificateur de lot")](sandboxing-images/sign13-large.png#lightbox)
3. Ensuite, double-cliquez sur le **Entitlements.plist** de fichier et vérifiez que notre **iCloud magasin clé-valeur** et **iCloud conteneurs** correspondent tous nos ID d’application créées précédemment (exemple : `com.appracatappra.MacSandbox`): 

    [![Modification du fichier Entitlements.plist](sandboxing-images/sign17.png "modifiant le fichier Entitlements.plist")](sandboxing-images/sign17-large.png#lightbox)
3. Enregistrez les modifications apportées.
4. Dans le **remplissage de la Solution**, double-cliquez sur le fichier projet pour ouvrir ses Options de modification :  

    ![Editign les options de la solution](sandboxing-images/sign14.png "Editign les options de la solution")
5. Sélectionnez **Mac signature**, puis vérifiez **se connecter à l’offre groupée d’application** et **signer le package de programme d’installation**. Sous **profil de configuration**, sélectionnez celui que nous avons créés ci-dessus : 

    ![Définir le profil de configuration](sandboxing-images/sign15.png "définissant le profil de configuration")
6. Cliquez sur le **fait** bouton.

> [!IMPORTANT]
> Vous devrez peut-être quitter et redémarrer Visual Studio pour Mac pour qu’il reconnaît l’ID d’application et un profil de configuration qui a été installée par Xcode nouvelle.

#### <a name="troubleshooting-provisioning-issues"></a>Résolution des problèmes de configuration

À ce stade vous devez tenter d’exécuter l’application et vous assurer que tout est signé et correctement configuré. Si l’application s’exécute toujours comme avant, tout est correct. En cas de défaillance, vous pouvez obtenir une boîte de dialogue, tel que le suivant :

[![Un exemple de boîte de dialogue de problème de configuration](sandboxing-images/sign16.png "un exemple de boîte de dialogue de problème de configuration")](sandboxing-images/sign16-large.png#lightbox)

Voici les causes les plus courantes de configuration et de problèmes de signature :

- L’ID d’offre groupée ne correspond pas à l’ID d’application du profil sélectionné.
- L’ID de développeur ne correspond pas à l’ID de développeur du profil sélectionné.
- L’UUID testée sur Mac n’est pas inscrit dans le cadre du profil sélectionné.

Dans le cas d’un problème, corrigez le problème sur le portail des développeurs Apple, d’actualiser les profils dans Xcode et effectuer un nettoyage de build dans Visual Studio pour Mac.

### <a name="enable-the-app-sandbox"></a>Activer le bac à sable d’application

Vous activez le bac à sable d’application en sélectionnant une case à cocher dans les options de projets. Effectuez ce qui suit :

1. Dans le **Solution remplissage**, double-cliquez sur le **Entitlements.plist** fichier à ouvrir pour le modifier.
2. Vérifiez les deux **activation des droits** et **permettre le Sandboxing de l’application**: 

    [![Droits de modification et l’activation de sandboxing](sandboxing-images/sign17.png "droits de modification et l’activation de bac à sable")](sandboxing-images/sign17-large.png#lightbox)
3. Enregistrez les modifications apportées.

À ce stade, vous avez activé le bac à sable d’application, mais vous n’avez pas fourni l’accès au réseau requis pour l’affichage Web. Si vous exécutez l’application maintenant, vous devez obtenir une fenêtre vide :

[![Affichant l’accès web bloqués](sandboxing-images/sample08.png "montrant l’accès web bloqués")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Vérification de l’application bac à sable

À l’exception de la ressource est un comportement de blocage, il existe trois méthodes principales pour indiquer si une application Xamarin.Mac a été correctement sable :

1. Dans le Finder, vérifiez le contenu de la `~/Library/Containers/` dossier - si l’application est bac à sable, il y aura un dossier nommé comme identificateur de lot de votre application (exemple : `com.appracatappra.MacSandbox`) : 

    [![Ouverture d’offre groupée de l’application](sandboxing-images/sample09.png "ouverture d’offre groupée de l’application")](sandboxing-images/sample09-large.png#lightbox)
2. Le système considère l’application comme dans le moniteur d’activité :
    - Lancer le moniteur d’activité (sous `/Applications/Utilities`). 
    - Choisissez **vue** > **colonnes** et vérifiez que le **Sandbox** élément de menu est activé.
    - Assurez-vous que la colonne de bac à sable lit `Yes` pour votre application : 

    [![La vérification de l’application dans le moniteur d’activité](sandboxing-images/sample10.png "la vérification de l’application dans le moniteur d’activité")](sandboxing-images/sample10-large.png#lightbox)
3. Vérifiez que l’application binaire est sandbox :
    - Démarrez l’application Terminal.
    - Accédez aux applications `bin` active.
    - Exécutez cette commande : `codesign -dvvv --entitlements :- executable_path` (où `executable_path` est le chemin d’accès à votre application) : 

    [![La vérification de l’application sur la ligne de commande](sandboxing-images/sample11.png "la vérification de l’application sur la ligne de commande")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Débogage d’une application bac à sable

Le débogueur se connecte à des applications de Xamarin.Mac via TCP, ce qui signifie que par défaut lorsque vous activez le sandboxing, il est impossible de se connecter à l’application, donc si vous essayez d’exécuter l’application sans les autorisations appropriées activées, vous obtenez une erreur *« Impossible de se connecter à le débogueur »*. 

[![Définir les options requises](sandboxing-images/debug01.png "définissant les options requises")](sandboxing-images/debug01-large.png#lightbox)

Le **autoriser les connexions réseau sortantes (Client)** autorisation est celui requis pour le débogueur, l’activation de celle-ci permet normalement le débogage. Étant donné que vous ne pouvez pas déboguer sans lui, nous avons mis à jour le `CompileEntitlements` cibles pour `msbuild` pour ajouter automatiquement cette autorisation pour les droits pour les versions seulement n’importe quelle application est sable pour le débogage. Versions Release doivent utiliser les droits que ceux spécifiés dans le fichier de droits, non modifié.

### <a name="resolving-an-app-sandbox-violation"></a>Résoudre une violation d’application Sandbox

Une Violation de bac à sable d’application se produit si un bac à sable Xamarin.Mac application a essayé d’accéder à une ressource qui comporte ne pas explicitement autorisés. Par exemple, notre affichage Web n’est plus en cours en mesure d’afficher le site Web Apple.

La source la plus commune de Violations de bac à sable d’application se produit lorsque les paramètres de droits spécifiés dans Visual Studio pour Mac ne correspondent pas la configuration requise de l’application. Là encore, sauvegarder à notre exemple, la connexion réseau manquant droits qui empêchent l’affichage Web de travail.

#### <a name="discovering-app-sandbox-violations"></a>Détection des violations d’application Sandbox

Si vous soupçonnez une Violation de bac à sable d’application se produit dans votre application Xamarin.Mac, le moyen le plus rapide pour détecter le problème est à l’aide de la **Console** application.

Effectuez ce qui suit :

1. Compilez l’application en question et exécutez-le à partir de Visual Studio pour Mac.
2. Ouvrez le **Console** application (à partir de `/Applications/Utilties/`).
3. Sélectionnez **tous les Messages** dans la barre latérale et entrez `sandbox` dans la recherche : 

    [![Un exemple d’un problème de sandboxing dans la console](sandboxing-images/resolve01.png "un exemple d’un problème de sandboxing dans la console")](sandboxing-images/resolve01-large.png#lightbox)

Pour notre exemple d’application ci-dessus, vous pouvez voir que le noyau bloque le `network-outbound` le trafic en raison de l’application bac à sable, étant donné que nous n’avons pas demandé ce droit.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Résolution des violations aux droits d’application Sandbox

Maintenant que nous avons vu comment rechercher des Violations de bac à sable d’application, nous allons voir comment ils peuvent être résolus en ajustant les droits de notre application.

Effectuez ce qui suit :

1. Dans le **Solution remplissage**, double-cliquez sur le **Entitlements.plist** fichier à ouvrir pour le modifier.
2. Sous le **droits** section, vérifiez le **autoriser les connexions réseau sortantes (Client)** case à cocher : 

    [![Modifier les droits](sandboxing-images/sign17.png "modifier les droits")](sandboxing-images/sign17-large.png#lightbox)
3. Enregistrez les modifications apportées à l’application.

Si nous effectuez les étapes ci-dessus pour notre exemple d’application, puis générer et exécuter, le contenu web sera désormais affichera comme prévu.

## <a name="the-app-sandbox-in-depth"></a>Application bac à sable approfondie

Les mécanismes de contrôle d’accès fournis par le bac à sable d’application sont peu et faciles à comprendre. Toutefois, que le bac à sable application sera adoptée par chaque application de façon est unique et en fonction des besoins de l’application.

Compte tenu de votre meilleur effort pour protéger votre application Xamarin.Mac ne soient exploitées par du code malveillant, est nécessaire uniquement une seule vulnérabilité dans soit l’application (ou une des bibliothèques ou des infrastructures qu’elle consomme) pour contrôler les interactions de l’application avec le système.

Le bac à sable d’application a été conçu pour empêcher la prise en charge (ou limiter les dommages que permettre provoquer) en vous permettant de spécifier des interactions prévue de l’application avec le système. Le système accorde uniquement l’accès à la ressource que votre application a besoin pour effectuer son travail et rien de plus.

Lors de la conception pour le bac à sable d’application, vous concevez pour un scénario pessimiste. Si l’application compromise par du code malveillant, il est limité à l’accès aux fichiers et aux ressources dans un sandbox de l’application uniquement.

### <a name="entitlements-and-system-resource-access"></a>Droits et l’accès aux ressources système

Comme nous l’avons vu, une application de Xamarin.Mac qui n’a pas été bac à sable est accordée les droits d’accès complets et l’accès de l’utilisateur qui exécute l’application. Si elles sont compromises par du code malveillant, une application non protégée peut agir comme un agent pour le comportement dangereux, avec une échelle comprises risque potentiel de nuire à l’entreprise.

En activant le bac à sable d’application, vous supprimez tous sauf un ensemble minimal de privilèges, que vous pouvez puis réactivez chaque besoin uniquement à l’aide de droits d’accès de votre application Xamarin.Mac. 

Modifier les ressources de bac à sable d’application de votre application en modifiant son **Entitlements.plist** de fichiers et la vérification ou en sélectionnant les droits requis dans les zones de liste déroulante des éditeurs :

[![Modifier les droits](sandboxing-images/sign17.png "modifier les droits")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Accès au système de fichiers et les répertoires de conteneur

Lorsque votre application Xamarin.Mac adopte le bac à sable d’application, il a accès aux emplacements suivants :

- **Le répertoire de conteneur d’application** -lors de la première exécution, le système d’exploitation crée une spéciale _répertoire conteneur_ lorsque toutes ses ressources accédez que seulement il peut accéder. L’application aura un accès complet en lecture/écriture à ce répertoire.
- **Application conteneur répertoires** -votre application peut être autorisée à accéder à un ou plusieurs _des conteneurs de groupe_ qui sont partagés entre les applications dans le même groupe.
- **Les fichiers spécifiés par l’utilisateur** -votre application obtient automatiquement un accès aux fichiers qui sont explicitement ouvert ou déplacé et déposé sur l’application par l’utilisateur.
- **Les éléments liés aux** -avec les droits appropriés, votre application peut avoir accès à un fichier portant le même nom mais une autre extension. Par exemple, un document qui a été enregistré à la fois comme un `.txt` fichier et un `.pdf`.
- **Répertoires temporaires, les répertoires de l’outil de ligne de commande et des emplacements spécifiques de lecture** -votre application a plusieurs degrés d’accès aux fichiers dans d’autres emplacements bien définis comme spécifié par le système.

#### <a name="the-app-container-directory"></a>Le répertoire de conteneur d’application

Répertoire de conteneur d’application d’une application Xamarin.Mac présente les caractéristiques suivantes :

- Il se trouve dans un emplacement masqué dans le répertoire de base de l’utilisateur (généralement `~Library/Containers`) et est accessible avec la `NSHomeDirectory` (fonction) (voir ci-dessous) dans votre application. Comme il est dans le répertoire de base, chaque utilisateur aura son propre conteneur pour l’application.
- L’application dispose d’un accès en lecture/écriture illimité pour le répertoire conteneur et tous ses sous-répertoires et les fichiers qu’il contient.
- La plupart des API de recherche de chemin d’accès de macOS sont relatif au conteneur de l’application. Par exemple, le conteneur possède sa propre **bibliothèque** (accessibles via `NSLibraryDirectory`), **prise en charge de l’Application** et **préférences** dans les sous-répertoires.
- macOS établit et applique la connexion entre et application et son conteneur via la signature de code. Même est une autre application tente d’usurper l’identité de l’application à l’aide de son **identificateur de lot**, il ne peut pas accéder au conteneur en raison de la signature de code.
- Le conteneur n’est pas pour les fichiers générés par l’utilisateur. Il est à la place pour les fichiers que votre application utilise telles que les bases de données, de caches ou d’autres types de données spécifiques.
- Pour _bibliothèque d’images_ types d’applications (par exemple, application de photos d’Apple), le contenu de l’utilisateur passe dans le conteneur.

> [!IMPORTANT]
> Malheureusement, Xamarin.Mac ne contient aucune couverture d’API de 100 % encore (contrairement à Xamarin.iOS), par conséquent la `NSHomeDirectory` API n’a pas été mappé dans la version actuelle de Xamarin.Mac.

En tant que solution de contournement temporaire, vous pouvez utiliser le code suivant :

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>Le répertoire de conteneur de groupe application

En commençant par Mac macOS 10.7.5 (et version ultérieure), une application peut utiliser le `com.apple.security.application-groups` droit pour accéder à un conteneur partagé qui sont commun à toutes les applications dans le groupe. Vous pouvez utiliser ce conteneur partagé pour le contenu en regard de non-utilisateur telles que la base de données ou d’autres types de fichiers de prise en charge (par exemple, des caches).

Les conteneurs de groupe sont automatiquement ajoutés à bac à sable conteneur chaque application (si elles font partie d’un groupe) et sont stockés à `~/Library/Group Containers/<application-group-id>`. L’ID de groupe _doit_ commencent par votre ID d’équipe de développement et d’une période, par exemple :

```plist
<team-id>.com.company.<group-name>
```

Pour plus d’informations, consultez le site d’Apple [Ajout d’une Application à un groupe d’applications](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) dans [référence de clé droit](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Accès au système Powerbox et le fichier en dehors du conteneur d’application

Une application de Xamarin.Mac sandbox peut accéder aux emplacements de système de fichiers en dehors de son conteneur comme suit :

- À l’initiative spécifique de l’utilisateur (au moyen d’ouvrir et enregistrer des boîtes de dialogue ou autres méthodes telles que la fonction glisser- déposer).
- À l’aide de droits pour les emplacements de système de fichiers spécifique (tel que `/bin` ou `/usr/lib`).
* Lorsque l’emplacement de système de fichiers est dans certains répertoires sont world lisible (par exemple, le partage).

_Powerbox_ est une technologie de sécurité qui interagit avec l’utilisateur de développer des droits d’accès de votre application Xamarin.Mac bac à sable fichier macOS. Powerbox n’a aucune API, mais il est activé en toute transparence lorsque l’application appelle une `NSOpenPanel` ou `NSSavePanel`. L’accès Powerbox est activé via les droits que vous définissez pour votre application Xamarin.Mac.

Quand une application bac à sable affiche Open ou boîte de dialogue Enregistrer, la fenêtre est présentée par Powerbox (et pas AppKit) et par conséquent a accès à aucun fichier ou le répertoire de l’utilisateur ayant accès à.

Lorsqu’un utilisateur sélectionne un fichier ou répertoire à partir de l’ouvrir ou boîte de dialogue Enregistrer (ou fait glisser soit sur l’icône de l’application), Powerbox ajoute le chemin d’accès associé à sandbox de l’application.

En outre, le système autorise automatiquement les éléments suivants pour une application bac à sable :

- Connexion à une méthode d’entrée du système.
- Appeler des services sélectionnés par l’utilisateur à partir de la **Services** menu (uniquement pour les services marqués en tant que _sécurisés pour les applications de bac à sable_ par le fournisseur de services).
- Choisissez des fichiers ouverts par l’utilisateur à partir de la **ouvrir récente** menu.
- Utilisez le copier et coller entre les autres applications.
- Lire des fichiers à partir des emplacements de lecture suivantes :
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Lire et écrire des fichiers dans les répertoires créés par `NSTemporaryDirectory`.

Par défaut, les fichiers ouverts ou enregistrés par une application Xamarin.Mac bac à sable restent accessibles jusqu'à l’arrêt de l’application (à moins que le fichier était encore ouvert lors de l’application se ferme). Fichiers ouverts va être restaurés automatiquement à sandbox de l’application via la fonctionnalité de reprise macOS lors du prochain que démarrage de l’application.

Pour assurer la persistance pour les fichiers situés en dehors du conteneur d’une application Xamarin.Mac, utiliser des signets de portée à la sécurité (voir ci-dessous).

#### <a name="related-items"></a>Articles connexes

Le bac à sable application permet à une application accéder aux éléments associés qui ont le même nom de fichier, mais différentes extensions. La fonctionnalité comporte deux parties : (a) une liste d’extensions associées dans l’application `Info.plst` fichier, b) du code pour indiquer le bac à sable à ce que l’application va faire de ces fichiers.

Il existe deux scénarios où cela est logique :

1. L’application doit être en mesure d’enregistrer une version différente du fichier (avec une nouvelle extension). Par exemple, exporter un `.txt` de fichiers à un `.pdf` fichier. Pour gérer cette situation, vous devez utiliser un `NSFileCoordinator` pour accéder au fichier. Vous devez appeler la `WillMove(fromURL, toURL)` méthode tout d’abord, déplacez le fichier vers la nouvelle extension, puis appelez `ItemMoved(fromURL, toURL)`.
2. L’application doit ouvrir un fichier avec une extension de principal et plusieurs fichiers de prise en charge avec différentes extensions. Par exemple, une vidéo et un fichier de sous-titre. Utilisez un `NSFilePresenter` pour accéder au fichier secondaire. Fournissez le fichier principal pour le `PrimaryPresentedItemURL` propriété et le fichier secondaire pour le `PresentedItemURL` propriété. Lorsque le fichier principal est ouvert, appelez le `AddFilePresenter` méthode de la `NSFileCoordinator` classe pour enregistrer le fichier secondaire.

Dans des deux cas, l’application **Info.plist** fichier doit déclarer les Types de documents pouvant être ouvertes par l’application. Pour tout type de fichier, ajoutez le `NSIsRelatedItemType` (avec la valeur `YES`) à son entrée dans le `CFBundleDocumentTypes` tableau.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Ouvrir et enregistrer le comportement de la boîte de dialogue avec les applications de bac à sable

Les limites suivantes sont placés sur le `NSOpenPanel` et `NSSavePanel` lorsque vous les appelez à partir d’une application Xamarin.Mac sandbox :

- Vous ne pouvez pas appeler par programmation la **OK** bouton.
- Vous ne pouvez pas modifier par programme la sélection d’un utilisateur dans un `NSOpenSavePanelDelegate`.

En outre, les modifications d’héritage suivantes sont en place :

- **Application non sandbox** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Étendue de sécurité des signets et accès de ressources persistants

Comme indiqué ci-dessus, une application de bac à sable Xamarin.Mac peut accéder à un fichier ou une ressource en dehors de son conteneur par le biais d’interaction directe de l’utilisateur (comme indiqué par PowerBox). Toutefois, l’accès à ces ressources ne persistent pas d’automatiquement lancement de l’application ou le système redémarre.

À l’aide de _Security-Scoped signets_, une application de bac à sable Xamarin.Mac permettre conserver l’intention de l’utilisateur et conserver l’accès au compte tenu des ressources après une application redémarrer.

#### <a name="security-scoped-bookmark-types"></a>Types de sécurité à portée de signet

Lorsque vous travaillez avec des signets de Security-Scoped et de l’accès aux ressources persistant, il existe deux cas d’utilisation sistine :

- **Un signet App-Scoped fournit un accès permanent à un fichier spécifié par l’utilisateur ou un dossier.** 

    Par exemple, si l’application Xamarin.Mac bac à sable permet d’utiliser pour ouvrir un document externe pour la modification (à l’aide un `NSOpenPanel`), l’application peut créer un signet App-Scoped afin qu’il puisse accéder à l’avenir le même fichier.
- **Un signet Document-Scoped fournit un accès permanent de document spécifique dans un fichier secondaire.** 

Par exemple, une application d’édition vidéo qui crée un fichier projet qui a accès aux images individuelles, clips vidéo et les fichiers audio qui seront ultérieurement combinées en une séquence unique.

Lorsque l’utilisateur importe un fichier de ressources dans le projet (via un `NSOpenPanel`), l’application crée un signet Document-Scoped pour l’élément qui est stocké dans le projet, afin que le fichier est toujours accessible à l’application.

Un signet Document-Scoped peuvent être résolu par n’importe quelle application capable d’ouvrir les données de signet et le document lui-même. Il prend en charge la portabilité, ce qui permet d’envoyer les fichiers de projet à un autre utilisateur et avoir tous les signets fonctionnent aussi bien pour les.

> [!IMPORTANT]
> Un signet Document-Scoped pouvez _uniquement_ pointe vers un seul fichier et non un dossier et que ce fichier ne peut pas être dans un emplacement utilisé par le système (tel que `/private` ou `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>À l’aide de signets de portée à la sécurité

À l’aide de des types de Security-Scoped signet, vous devez effectuer les étapes suivantes :

1. **Définir les droits appropriés dans l’application de Xamarin.Mac que doit utiliser des signets de Security-Scoped** -For App-Scoped signets, définissez la `com.apple.security.files.bookmarks.app-scope` clé droit à `true`. Pour les signets Document-Scoped, définissez le `com.apple.security.files.bookmarks.document-scope` clé droit à `true`.
2. **Créer un signet Security-Scoped** -vous devez effectuer cette opération pour tout fichier ou dossier fournies par l’utilisateur a accès à (via `NSOpenPanel` par exemple), que l’application devez un accès permanent aux. Utilisez le `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` méthode de la `NSUrl` classe pour créer le signet.
3. **Résoudre le signet Security-Scoped** - lorsque l’application doit accéder à la ressource à nouveau (par exemple, après le redémarrage) il doit résoudre le signet vers une URL de l’étendue de sécurité. Utilisez le `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` méthode de la `NSUrl` classe pour résoudre le signet.
4. **Notifier explicitement le système que vous souhaitez accéder au fichier à partir de l’URL Security-Scoped** : cette étape doit être effectuée immédiatement après l’obtention de l’URL Security-Scoped ci-dessus, ou lorsque vous souhaitez ultérieurement récupérer l’accès à la ressource après avoir Abandon de l’accès à celui-ci. Appelez le `StartAccessingSecurityScopedResource ()` méthode de la `NSUrl` classe pour pouvoir accéder à une URL Security-Scoped.
5. **Notifier explicitement le système que vous avez terminé l’accès au fichier à partir de l’URL Security-Scoped** -dès que possible, vous devez informer le système lors de l’application n’a plus besoin l’accès au fichier (par exemple, si l’utilisateur la ferme). Appelez le `StopAccessingSecurityScopedResource ()` méthode de la `NSUrl` classe pour arrêter l’accès à une URL Security-Scoped.

Une fois que vous avez annulé l’accès à une ressource, vous devez revenir à l’étape 4 pour rétablir l’accès. Si l’application Xamarin.Mac est redémarrée, vous devez revenir à l’étape 3 et de résoudre le signet.

> [!IMPORTANT]
> Libérer l’accès aux ressources Security-Scoped URL provoquera une application Xamarin.Mac une fuite des ressources du noyau. Par conséquent, l’application ne seront plus en mesure d’ajouter des emplacements de système de fichiers à son conteneur jusqu'à ce qu’il est redémarré.

### <a name="the-app-sandbox-and-code-signing"></a>Le bac à sable d’application et la signature du code

Après avoir activé le bac à sable d’application et que vous activer les conditions requises pour votre application Xamarin.Mac (via des droits), vous devez coder signe le projet pour le bac à sable entrent en vigueur. Vous devez effectuer, car les droits requis pour application bac à sable sont liées à la signature de l’application de la signature de code. 

macOS applique un lien entre conteneur d’une application et sa signature de code, de cette façon, qu'aucune autre application ne peut accéder à ce conteneur, même si les usurpation de l’ID d’offre groupée d’applications Ce mécanisme fonctionne comme suit :

1. Lorsque le système crée un conteneur de l’application, il définit un _liste de contrôle d’accès_ (ACL) sur ce conteneur. L’entrée de contrôle d’accès initial dans la liste contient l’application _désigné une exigence_ (DR), qui décrit comment les futures versions de l’application peut être reconnu (lorsqu’il a été mis à niveau).
2. Chaque fois qu’une application avec le même ID d’offre groupée démarre, le système vérifie que la signature de code de l’application correspondant aux critères désigné spécifiée dans une des entrées dans l’ACL du conteneur. Si le système ne trouve pas de correspondance, il empêche le lancement de l’application.

Code de signature fonctionne comme suit :

1. Avant de créer le projet Xamarin.Mac, obtenir un certificat de développement, un certificat de Distribution et un certificat de code de développeur à partir du portail des développeurs Apple.
2. Lorsque le magasin d’applications Mac distribue l’application Xamarin.Mac, il est signé avec une signature de code Apple.

Lorsque le test et de débogage, vous allez utiliser une version de l’application Xamarin.Mac que vous vous êtes (qui sera utilisé pour créer le conteneur d’application). Ultérieurement, si vous souhaitez tester ou d’installer la version à partir de l’Apple App Store, il sera signé avec la signature Apple et ne pourront pas lancer (puisque qu’il n’a pas la même signature de code en tant que le conteneur d’application d’origine). Dans ce cas, vous obtenez un rapport d’incidents semblable au suivant :

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Pour résoudre ce problème, vous devrez ajuster l’entrée ACL pour pointer vers la version signée Apple de l’application.

Pour plus d’informations sur la création et de télécharger les profils de configuration requis pour le bac à sable, veuillez consulter le [signature et l’application de configuration](#Signing_and_Provisioning_the_App) section ci-dessus.

#### <a name="adjusting-the-acl-entry"></a>Ajustement de l’entrée d’ACL

Pour autoriser la version signée Apple de l’application Xamarin.Mac à exécuter, procédez comme suit :

1. Ouvrez l’application Terminal (dans `/Applications/Utilities`).
2. Ouvrez une fenêtre de recherche vers la version signée Apple de l’application Xamarin.Mac.
3. Type `asctl container acl add -file ` dans la fenêtre de Terminal Server.
4. Faites glisser d’icône de l’application Xamarin.Mac à partir de la fenêtre de recherche et déposez-la sur la fenêtre de Terminal.
5. Le chemin d’accès complet au fichier sera ajouté à la commande dans Terminal Server.
6. Appuyez sur **entrée** pour exécuter la commande.

L’ACL du conteneur contient désormais les exigences de code désigné pour les deux versions de l’application Xamarin.Mac et macOS autorise désormais des versions à exécuter.

#### <a name="display-a-list-of-acl-code-requirements"></a>Afficher une liste d’exigences de code ACL

Vous pouvez afficher une liste des exigences de code dans ACL d’un conteneur en procédant comme suit :

1. Ouvrez l’application Terminal (dans `/Applications/Utilities`).
2. Tapez `asctl container acl list -bundle <container-name>`.
3. Appuyez sur **entrée** pour exécuter la commande.

Le `<container-name>` est généralement l’identificateur de lot pour l’application Xamarin.Mac.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Conception d’une application Xamarin.Mac pour le bac à sable d’application

Il existe un flux de travail courant qui doit être suivie lors de la conception d’une application Xamarin.Mac pour le bac à sable d’application. Ceci dit, les détails d’implémentation dans une application de bac à sable vont être unique à la fonctionnalité de l’application donnée.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Six étapes pour l’adoption d’application bac à sable

Conception d’une application Xamarin.Mac pour le bac à sable application généralement se compose des étapes suivantes :

1. Déterminer si l’application est adaptée à sandboxing.
2. Concevoir une stratégie de développement et de distribution.
3. Résoudre les incompatibilités de l’API.
4. Appliquer les droits requis de bac à sable l’application au projet Xamarin.Mac.
5. Ajoutez la séparation de privilège à l’aide de XPC.
6. Implémenter une stratégie de migration.

> [!IMPORTANT]
> Vous devez non seulement le sandbox du fichier exécutable principal dans le lot d’applications pour vous, mais également toutes helper inclus application ou un outil dans ce groupe. Cela est requis pour toutes les applications distribuées à partir de l’ordinateur Mac App Store et, si possible, doit être effectuée pour toute autre forme de distribution d’applications.

Pour obtenir la liste de tous les fichiers binaires exécutables dans un Xamarin.Mac d’offre groupée, tapez la commande suivante dans le Terminal :

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Où `[Your-App-Bundle]` est le nom et le chemin d’accès au groupe de l’application.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Déterminer si une application Xamarin.Mac est appropriée pour le bac à sable

La plupart des applications Xamarin.Mac sont totalement compatibles avec le bac à sable d’application et par conséquent, approprié pour le bac à sable. Si l’application requiert un comportement qui n’autorise pas le bac à sable d’application, vous devez envisager une approche alternative.

Si votre application nécessite un des comportements suivants, il est incompatible avec le bac à sable d’application :

- **Services d’autorisation** -avec l’application bac à sable, vous ne fonctionne pas avec les fonctions décrites dans [référence C de Services d’autorisation](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **API d’accessibilité** -vous ne pouvez pas sandbox des applications d’assistance tels que les lecteurs d’écran ou les applications qui contrôlent les autres applications.
- **Envoyer des événements Apple aux applications arbitraires** -si l’application requiert l’envoi d’événements Apple à une application inconnue, arbitraire, elle ne peut pas être sandbox. Pour une liste d’applications appelées, l’application peut toujours être bac à sable et les droits doit inclure la liste d’applications appelé.
- **Envoyer des dictionnaires des informations utilisateur dans les Notifications de Distributed à d’autres tâches** -avec l’application bac à sable, vous ne pouvez pas inclure un `userInfo` dictionnaire lors de la validation à un `NSDistributedNotificationCenter` objet pour les autres tâches de messagerie.
- **Charger les Extensions du noyau** -le chargement des Extensions du noyau est interdite par le bac à sable d’application.
- **Simulation d’entrée utilisateur dans ouvrir et enregistrer les boîtes de dialogue** - par programmation manipulation ouvrir ou enregistrer des boîtes de dialogue pour simuler ou de modifier l’entrée d’utilisateur est interdite par le bac à sable d’application.
- **Accès ou la définition des préférences sur d’autres applications** -manipuler les paramètres d’autres applications est interdite par le bac à sable d’application.
- **Configuration des paramètres réseau** -manipuler les paramètres de réseau est interdite par le bac à sable d’application.
- **Arrêt d’autres applications** -l’application Sandbox interdit à l’aide de `NSRunningApplication` pour mettre fin à d’autres applications.

### <a name="resolving-api-incompatibilities"></a>Résolution des incompatibilités de l’API

Lorsque vous concevez une application Xamarin.Mac pour le bac à sable d’application, vous pouvez rencontrer des incompatibilités avec l’utilisation de certaines API macOS.

Voici quelques problèmes courants et des éléments que vous pouvez effectuer pour résoudre ces problèmes :

- **Ouverture, l’enregistrement et le suivi de Documents** : Si vous gérez des documents à l’aide de n’importe quelle technologie autre que `NSDocument`, vous devez basculer vers elle en raison de la prise en charge intégrée pour le bac à sable d’application. `NSDocument` fonctionne avec PowerBox et fournit la prise en charge de conservation des documents dans votre bac à sable si l’utilisateur les déplace dans le Finder automatiquement.
- **Conserver l’accès aux ressources de système de fichiers** - si le Xamarin.Mac application dépend d’un accès permanent aux ressources en dehors de son conteneur, utiliser des signets de portée à la sécurité de conserver l’accès.
- **Créer un élément de connexion pour une application** -avec l’application bac à sable, Impossible de créer un élément à l’aide du compte de connexion `LSSharedFileList` ni vous pouvez manipuler l’état des services de lancement à l’aide de `LSRegisterURL`. Utilisez le `SMLoginItemSetEnabled` comme décrit dans les pommes [Ajout d’éléments de la connexion à l’aide de l’infrastructure de gestion de Service](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) documentation.
- **Accès aux données utilisateur** : Si vous utilisez des fonctions POSIX, tel que `getpwuid` pour obtenir le répertoire de base à partir des services d’annuaire, envisagez d’utiliser les symboles/Cocoa ou de base telles que `NSHomeDirectory`.
- **Les préférences d’autres applications de l’accès à** - parce que l’application bac à sable dirige le chemin d’accès de recherche API au conteneur de l’application, modification des préférences a lieu dans le conteneur et accéder à un autre préférences d’applications dans ne pas autorisé. 
- **À l’aide de la vidéo incorporée HTML5 dans des vues Web** -si l’application Xamarin.Mac utilise WebKit pour lire les vidéos incorporées HTML5, vous devez également lier l’application par rapport à l’infrastructure AV Foundation. Le bac à sable application empêche CoreMedia play ces vidéos sinon.

### <a name="applying-required-app-sandbox-entitlements"></a>Application de droits d’application Sandbox requis

Vous devez modifier les droits pour n’importe quelle application Xamarin.Mac que vous souhaitez exécuter dans le bac à sable d’application et vérifiez la **activer application bac à sable** case à cocher.

Basé sur les fonctionnalités de l’application, vous devrez peut-être activer d’autres droits accéder aux ressources ou des fonctionnalités du système d’exploitation. Application bac à sable fonctionne meilleures lorsque vous réduisez les droits que vous demander au minimum requis pour exécuter votre application Activez donc uniquement de façon aléatoire de droits.

Pour déterminer les droits d’une application Xamarin.Mac requiert, procédez comme suit :

1. Activer le bac à sable d’application et exécuter l’application Xamarin.Mac.
2. Exécuter les fonctionnalités de l’application.
3. Ouvrez l’application Console (disponible dans `/Applications/Utilities`) et recherchez `sandboxd` violations dans le **tous les Messages** journal.
4. Pour chaque `sandboxd` violation, résolvez le problème à l’aide du conteneur d’application plutôt que d’autres emplacements de système de fichiers ou appliquer des droits de l’application Sandbox pour activer l’accès aux fonctions du système d’exploitation restreintes.
5. Exécuter à nouveau et testez de nouveau toutes les fonctionnalités d’application Xamarin.Mac.
6. Répétez ces étapes jusqu'à ce que tous les `sandboxd` violations ont été résolues.

### <a name="add-privilege-separation-using-xpc"></a>Ajouter la séparation de privilège à l’aide de XPC

Lorsque vous développez une application Xamarin.Mac pour le bac à sable d’application, examinez les comportements de l’application dans les conditions d’accès et de privilèges, puis prendre en compte la séparation des opérations à haut risque dans leurs propres services XPC.

Pour plus d’informations, consultez Apple [création de Services XPC](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) et [démons et Guide de programmation de Services](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Implémenter une stratégie de migration

Si vous lancez une nouvelle version d’une application Xamarin.Mac qui n’était pas précédemment bac à sable sandbox, vous devez vous assurer que les utilisateurs actuels de mise à niveau continue. 

 Pour plus d’informations sur l’implémentation d’un manifeste de migration de conteneur, lisez d’Apple [migration d’une application à un bac à sable](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) documentation.

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée de sandboxing, une application Xamarin.Mac. Tout d’abord, nous avons créé une application Xamarin.Mac simplement pour afficher les principes fondamentaux de l’application Sandbox. Ensuite, nous vous avons montré comment résoudre les violations de bac à sable. Ensuite, nous avons pris en profondeur examiner le bac à sable d’application, et enfin, nous avons examiné la conception d’une application Xamarin.Mac pour le bac à sable d’application.



## <a name="related-links"></a>Liens associés

- [Publication dans l’App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [À propos de l’application Sandbox](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Problèmes courants de l’application bac à sable](https://developer.apple.com/library/content/qa/qa1773/_index.html)
