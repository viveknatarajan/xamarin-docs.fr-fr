---
title: Installation et configuration
description: "Configuration pour le développement watchOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: f7e511d7f0a933ab7f29369e5e5f0aa46607c8f8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="installation"></a>Installation

watchOS 4 requiert macOS Sierra (10.12) avec Xcode 9.

watchOS 1 requis à l’origine OS X Yosemite (10.10) avec Xcode 7.

> [!WARNING]
> [mises à jour watchOS 1 ne seront pas acceptés après le 1 avril 2018](https://developer.apple.com/news/?id=11162017a). Mises à jour ultérieures doivent utiliser watchOS 2 SDK ou une version ultérieure ; génération avec la watchOS 4 SDK est recommandé.

## <a name="project-structure"></a>Structure de projet

Une application de surveillance se compose de trois projets :

- **Projet d’application iPhone Xamarin.iOS** -il s’agit d’un projet iPhone normal, il peut être un des modèles de Xamarin.iOS. L’application de surveillance et son extension est fournies à l’intérieur de ce projet principal.

- **Projet d’Extension espion** -contient le code (par exemple, les classes de contrôleur) pour l’application de surveillance.

- **Projet d’application espion** -il contient le fichier de plan conceptuel de l’Interface utilisateur à toutes les ressources de l’interface utilisateur pour l’application de surveillance.

Le [exemple espion Kit Catalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) solution ressemble à ceci dans Xamarin.Studio :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "La solution dans Visual Studio")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "La solution dans Visual Studio")

-----

Téléchargez et exécutez le [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) exemple pour commencer.
Écrans de l’exemple se trouve sur le [contrôles](~/ios/watchos/user-interface/index.md) page.


## <a name="creating-a-new-project"></a>Création d'un projet

Vous ne pouvez pas créer une « espion Solution »... au lieu de cela, vous pouvez ajouter une application de surveillance pour une application iOS existante. Suivez ces étapes pour créer une application de surveillance :

1. Si vous n’avez pas un projet existant, vous devez tout d’abord choisir **fichier > Nouvelle Solution** et créer une application iOS (par exemple, un **unique afficher l’application**) :

    [ ![](installation-images/cycle8-2-sml.png "Choisissez Fichier > Nouvelle Solution et créer une application iOS")](installation-images/cycle8-2.png)

2. Une fois que l’application iOS est créée (ou si vous envisagez d’utiliser votre application iOS existante), avec le bouton droit sur la solution et choisissez **Ajouter > Ajouter un nouveau projet...** . Dans le **nouveau projet** sélectionnez **watchOS > applications > WatchKit application**:

    [ ![](installation-images/cycle8-6-sml.png "Sélectionnez watchOS > applications > WatchKit application")](installation-images/cycle8-6.png)

3. L’écran suivant vous permet de choisir le projet d’application iOS doit inclure l’application de surveillance :

    [ ![](installation-images/cycle8-7-sml.png "Choisissez le projet d’application iOS doit inclure l’application de surveillance")](installation-images/cycle8-7.png)

4. Enfin, choisissez l’emplacement où enregistrer le projet (et éventuellement a activé le contrôle de code source) :

    [ ![](installation-images/cycle8-8-sml.png "Choisissez l’emplacement où enregistrer le projet")](installation-images/cycle8-8.png)

5. Visual Studio pour Mac configure automatiquement [références de projet et **Info.plist** paramètres](~/ios/watchos/get-started/project-references.md) pour vous.

## <a name="creating-the-watch-user-interface"></a>Création de l’Interface utilisateur de surveillance

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>À l’aide du concepteur Xamarin iOS

Double-cliquez sur l’application de surveillance **Interface.storyboard** à modifier à l’aide du Concepteur d’iOS. Vous pouvez faire glisser contrôleurs d’interface et contrôles d’interface utilisateur sur le plan conceptuel à partir de la **boîte à outils** et les configurer à l’aide de la **propriétés** remplissage :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "Le plan conceptuel dans le Concepteur")](installation-images/iosdesigner.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "Le plan conceptuel dans le Concepteur")](installation-images/iosdesigner-vs.png)

-----

Vous devez donner à chaque nouveau contrôleur d’interface une **classe** en la sélectionnant et en entrant ensuite le nom de la **propriétés** remplissage (Cela créera les fichiers code-behind c# requis automatiquement) :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "Donnez à chaque nouveau contrôleur d’interface à une classe")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "Donnez à chaque nouveau contrôleur d’interface à une classe")

-----

Créer est parfait pour par **Ctrl + glisser** à partir d’un contrôleur de bouton, d’une table ou d’interface sur un autre contrôleur d’interface.


### <a name="using-xcode-on-the-mac"></a>À l’aide de Xcode sur Mac

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Vous pouvez continuer à utiliser Xcode pour générer votre interface utilisateur en cliquant sur le fichier Interface.storyboard et en sélectionnant **ouvrir avec > Xcode Interface Builder**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Les utilisateurs de Visual Studio peuvent également utiliser Xcode pour créer leur interface utilisateur par basculement pour utiliser l’hôte de Build Mac directement.
Ouvrez votre solution dans Visual Studio pour Mac avec le bouton droit sur le fichier Interface.storyboard et sélectionnez **ouvrir avec > Xcode Interface Builder**:

-----

![](installation-images/openwith-xcode.png "Ouvrez le Interface.storyboard dans Xcode Interface Builder")

Si vous utilisez Xcode, vous devez suivre les mêmes étapes pour les applications de surveillance que pour normal [iOS application storyboards](~/ios/user-interface/storyboards/index.md) (telles que la création de points de vente et les actions par **Ctrl + déplacement** dans le **.h**fichier d’en-tête).

Lorsque vous enregistrez la table de montage séquentiel dans le Générateur de Interface Xcode il ajoute automatiquement les prises de courant et les actions que vous créez pour le langage c# **. designer.cs** fichiers dans le projet d’extension de surveillance.


### <a name="adding-additional-screens-in-xcode"></a>L’ajout d’autres écrans dans Xcode

Lorsque vous ajoutez des écrans supplémentaires (au-delà de ce qui est inclus dans le modèle par défaut) à votre plan conceptuel à l’aide de Xcode Interface Builder **vous devez ajouter manuellement les fichiers de code c#** pour chaque nouveau contrôleur d’interface.

Reportez-vous à la [avancé des instructions sur l’ajout de nouveaux contrôleurs de l’interface à une table de montage séquentiel](~/ios/watchos/troubleshooting.md#add).

*Xamarin iOS concepteur effectue cette opération automatiquement, aucune étape manuelle est nécessaire.*


## <a name="building"></a>Génération

Un projet qui inclut une application espion génère comme les autres projets iOS. Le processus de génération entraîne une application iPhone (.app) qui contient une extension de surveillance (.appex), qui contient l’application sans code espion (.app).


## <a name="launching"></a>Lancement du

Vous pouvez lancer des applications de surveillance dans le simulateur à l’aide de Visual Studio pour Mac et Visual Studio (démarre sur l’hôte de Build Mac).

Il existe deux modes pour le lancement d’une application WatchKit :

 - mode d’application normal (la valeur par défaut), et
- [Notifications](~/ios/watchos/platform/notifications.md) (ce qui nécessite une charge utile de notification test au format JSON).

### <a name="xcode-8-support"></a>Prise en charge Xcode 8

Une fois Xcode 8 (ou version ultérieure) est installé, les simulateurs Apple Watch sont distincts d’iOS simulateurs (contrairement aux [Xcode 6](#xcode6), où elles apparaissaient comme un *affichage externe*).
Lorsque vous sélectionnez le projet d’application de surveillance et que le projet de démarrage, la liste des simulateurs affichera les *iOS simulateurs* sélectionnables (comme indiqué ci-dessous).

[ ![](installation-images/xs-xcode8-watchos3-sml.png "Sélectionner le type de simulateur")](installation-images/xs-xcode8-watchos3.png)

Lorsque vous démarrez le débogage, *deux* simulateurs doit démarrer - simulateur iOS *et* le simulateur d’Apple Watch. Utilisez **commande + MAJ + H** pour accéder à l’image de menu et d’horloge espion ; et utiliser le **matériel** menu pour définir le **Force Touch la sollicitation de la**. Le défilement sur la souris ou du pavé tactile simule à l’aide de la couronne numérique.

#### <a name="troubleshooting"></a>Résolution des problèmes

L’erreur suivante s’affiche dans le **sortie de l’Application** si vous essayez de vous lancer dans un simulateur qui n’a pas un espion apparié :

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Reportez-vous à [les forums d’Apple](https://forums.developer.apple.com/thread/7783) pour obtenir des instructions sur la configuration des simulateurs, si les valeurs par défaut ne fonctionnent pas.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 et watchOS 1

Vous devez apporter la *projet d’extension espion* le **projet de démarrage** avant d’exécuter ou déboguer l’application. Vous ne pouvez pas « start » l’application de surveillance proprement dit, et si vous choisissez l’application iOS puis démarrera normalement dans le simulateur iOS.

Par défaut, une application de surveillance démarre en mode normal **application** mode (mode Aperçu pas ou les Notifications) à partir de Visual Studio pour Mac **exécuter** ou **déboguer** commandes.

Lorsque vous utilisez Xcode 6, uniquement les iPhone 5, 5 s les iPhone, iPhone 6 et iPhone 6 Plus peut activer l’affichage externe pour une **Apple Watch - 38mm** ou **Apple Watch - 42mm** où les applications de surveillance seront affiché.

**Remarque :** souvenez-vous que l’écran de surveillance n’apparaît pas automatiquement dans le simulateur iOS lors de l’utilisation de Xcode 6.
Utilisez le **matériel > affiche externe** menu pour afficher l’écran de surveillance.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Lors du lancement du Mode de Notification

Reportez-vous à la [page Notifications](~/ios/watchos/platform/notifications.md) pour plus d’informations comment gérer les notifications dans le code.


Visual Studio pour Mac peut démarrer l’application de surveillance avec une notification _modes de démarrage_ pour les notifications :



Avec le bouton droit sur le projet d’application espion et choisissez **exécuter avec > Configuration personnalisée...** :


[![](installation-images/runwith-customparams-sml.png "Une Configuration personnalisée en cours d’exécution")](installation-images/runwith-customparams.png)


Cette opération ouvre le **les paramètres personnalisés** fenêtre dans laquelle vous pouvez sélectionner **Notification** (et fournir une charge utile JSON), puis appuyez sur **exécuter** pour démarrer l’application de surveillance dans le simulateur :


[![](installation-images/runwith-execargs-sml.png "Définition de la Notification et la charge utile")](installation-images/runwith-execargs.png)



## <a name="debugging"></a>Débogage

Le débogage est pris en charge dans Visual Studio pour Mac et Visual Studio.
N’oubliez pas de fournir un fichier JSON de notification lors du débogage en mode de notifications. Cette capture d’écran montre un point d’arrêt de débogage en cours d’accès dans une application de surveillance :

![](installation-images/debug-sml.png "Cette capture d’écran montre un point d’arrêt de débogage en cours d’accès dans une application de surveillance")

Après avoir suivi les instructions de lancement, vous se retrouve avec votre application espion en cours d’exécution le **iOS Simulator (Watch)**.
Pour le mode de notification, vous pouvez sélectionner **Déboguer > Ouvrir le journal système** (**CMD + /**) et utiliser `Console.WriteLine` dans votre code.

### <a name="debugging-lifecycle-event-handlers"></a>Débogage des gestionnaires d’événements de cycle de vie

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

Les fichiers de modèle watchOS (tel que `InterfaceController`, `ExtensionDelegate`, `NotificationController`, et `ComplicationController`) sont fournis avec leurs méthodes de cycle de vie requis déjà implémentés. Ajouter `Console.WriteLine` appels et lire le **sortie de l’Application** pour mieux comprendre le cycle de vie des événements.



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Première vidéo de l’application de surveillance](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Conseils de WatchKit d’Apple](https://developer.apple.com/watchkit/tips/)
