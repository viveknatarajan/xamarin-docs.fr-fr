---
title: Utilisation des listes de propriétés
description: Ce document présente Visual Studio pour l’éditeur de liste (.plist) de Mac propriété graphique et avancées pour travailler avec Info.plist et Entitlements.plist. Il illustre la définition d’icônes et les images de démarrage pour les applications iOS à partir de Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f6ee7a606243f5d21d827546b528ca5d9d3f0281
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-property-lists"></a>Utilisation des listes de propriétés

_Ce document présente Visual Studio pour l’éditeur de liste (.plist) de Mac propriété graphique et avancées pour travailler avec Info.plist et Entitlements.plist. Il illustre la définition d’icônes et les images de démarrage pour les applications iOS à partir de Visual Studio pour Mac._

Visual Studio pour Mac propose un éditeur graphique .plist qui facilite la modification des propriétés de l’application et de fonctionnalités plus faciles. Visual Studio pour Mac a deux .plists - `Info.plist` pour la modification des propriétés de l’application et les icônes et `Entitlements.plist` pour gérer des fonctionnalités de l’application. Ce guide présente le Info.plists et fournit une vue d’ensemble de les utiliser dans Visual Studio pour Mac. Pour plus d’informations sur Entitlements.plist, consultez la [utilisation des droits](~/ios/deploy-test/provisioning/entitlements.md) guide.

## <a name="infoplist"></a>Info.plist

La liste des propriétés d’informations ( `Info.plist`) est un fichier requis iOS qui fournit des informations sur la configuration de votre application dans le système. Personnalisée du Visual Studio pour Mac `Info.plist` trois panneaux contrôlées par des tabulations en bas à gauche de la fenêtre Éditeur de fonctionnalités de l’éditeur :

 [![](property-lists-images/tabs.png "Les onglets de l’éditeur Info.plist en bas à gauche de la fenêtre de l’éditeur")](property-lists-images/tabs.png#lightbox)

Chaque panneau contrôles différentes propriétés, comme indiqué ci-dessous :

-  **Panneau d’application** -une interface graphique pour définir courantes propriétés de l’application ainsi que les icônes et lancez images ; spécifiez maps intégration et backgrounding modes.
-  **Advanced panneau** -le panneau de configuration avancée est le lieu de spécifier les types de document pris en charge, UTIs et les types d’URL.
-  **Panneau de configuration de source** -le panneau de configuration source contrôle propriétés moins courantes, ainsi que des propriétés personnalisées pour l’application.


Les trois sections étudier les fonctionnalités de chaque panneau plus en détail.

## <a name="application-panel"></a>Panneau d’application

Visual Studio pour Mac propose une interface graphique pour la modification commune `Info.plist` entrées pour une application :

1.  Propriétés des applications
1.  Types d’appareils pris en charge
1.  Orientations de prise en charge pour chaque type d’appareil
1.  Style et la couleur de la barre d’état
1.  Icônes et écrans de démarrage
1.  Cartes et les Modes d’arrière-plan


Ceux-ci sont décrits plus en détail dans les sections suivantes.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>e/s d’Application cible

Cette section contient des informations importantes qui décrit votre application.
Le **identificateur** stockées ici doit correspondre à l’identificateur de lot est entré dans iTunes Connect (pour les applications App Store) et également dans la liste des ID des applications de portail de configuration iOS et le développement et la Distribution des certificats.

 [![](property-lists-images/image24.png "e/s d’Application cible")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Déploiement de périphérique

 [![](property-lists-images/deployment.png "Déploiement de périphérique")](property-lists-images/deployment.png#lightbox)

L’appareil **déploiement** sections d’informations sont affichées de manière sélective, selon la sélection dans le **périphériques** liste déroulante dans le **Application cible** section ci-dessus. Le **Interface principale** liste déroulante est définie sur **MainStoryboard** dans des applications pilotées par table de montage séquentiel. Si l’interface utilisateur est écrite entièrement en code puis cela peut être vide.

### <a name="supported-device-orientations"></a>Orientations de périphérique pris en charge

 **Prise en charge d’appareil Orientations** détermine comment l’application répond à la rotation du périphérique. Il est très courant pour les applications iPhone/iPad prendre en charge uniquement **Portrait**, ou tous les éléments mais **envers**. En règle générale, toutes les applications iPad, à l’exception des jeux doivent prendre en charge toutes les orientations.

### <a name="status-bar-styles"></a>Styles de barre d’état

Le **Styles de barre d’état** section est une interface graphique pour la modification d’une application `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Styles de barre d’état")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Icônes, les Images de lancement et iTunes illustration

Vous trouverez des informations sur l’utilisation des icônes, des images et des illustrations dans votre fichier Info.plist dans le [utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) guide.




### <a name="maps-integration-and-background-modes"></a>Intégration de cartes et les Modes d’arrière-plan

Le `Info.plist` contient des sections particulières pour spécifier les maps intégration et backgrounding des modes. Vous choisissez les options que vous souhaitez prendre en charge ajoutera les propriétés requises pour votre application pour vous.

 [![](property-lists-images/maps.png "Intégration de mappages")](property-lists-images/maps.png#lightbox)

Pour plus d’informations sur l’utilisation des mappages, reportez-vous à la Xamarin [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) guide.

 [![](property-lists-images/bging.png "Modes d’arrière-plan")](property-lists-images/bging.png#lightbox)

Pour plus d’informations sur les Modes d’arrière-plan, reportez-vous à la Xamarin [Backgrounding dans iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) guide.

## <a name="advanced-panel"></a>Panneau de configuration avancée

Le panneau de configuration avancée contrôle les types de document et les schémas d’URL qui prend en charge de l’application.

 [![](property-lists-images/image34.png "Panneau de configuration avancée")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Types de documents

Pour les applications qui prennent en charge l’ouverture des types de fichiers spécifiques, les e/s fournit le `CFBundleDocumentTypes` clé. Si nous voulons que notre application pour prendre en charge certains types de fichiers connus - par exemple un fichier PDF - nous devez ajouter la valeur PDF à la clé. Cette section fournit un moyen pratique pour entrer les données qui est stocké dans le `CFBundleDocumentTypes` clé dans le `Info.plist` fichier.

Reportez-vous à la documentation sur [enregistrer le fichier de Types de votre application prend en charge](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) pour plus d’informations sur la façon de configurer ces valeurs.

## <a name="utis"></a>UTIs

Parfois, une application doit prendre en charge l’ouverture d’un type de fichier personnalisé. Par exemple, nous pouvons adopter ouvrir des fichiers image avec une extension personnalisée *.xam*. Pour spécifier un type de fichier personnalisé, nous allons créer un utilitaire - identificateur de Type universel - personnalisée à l’aide du `UIExportedTypeDeclarations` clé. La capture d’écran ci-dessous illustre comment créer un utilitaire personnalisé pour l’extension .xam :

 [![](property-lists-images/uti.png "Éditeur de UTIs")](property-lists-images/uti.png#lightbox)

Type simplement comme exporté UTIs spécifier UTIs personnalisés spécifiques à votre application, le *type UTIs importé* ( `UIImportedTypeDeclarations` clé) spécifier les types personnalisés pris en charge mais non détenus par votre application.

Pour plus d’informations sur l’utilisation de UTIs personnalisés, reportez-vous à Apple [enregistrement fichier Types votre application prend en charge](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) guide.

## <a name="custom-urls"></a>URL personnalisées

Un nom de schéma d’URL (également appelé protocole) est la première partie de l’URL. Par exemple, `http://` et `https://` sont les schémas d’URL courantes. Vous avez la possibilité de créer un modèle d’URL personnalisé pour votre application. Les schémas d’URL personnalisés sont utilisés pour communiquer et envoyer des données dans les deux sens avec d’autres applications. La capture d’écran suivante illustre la création d’un nouveau schéma d’URL personnalisé appelé `monkeys://`:

 [![](property-lists-images/url.png "URL personnalisées")](property-lists-images/url.png#lightbox)



Pour plus d’informations sur l’implémentation des modèles d’URL personnalisées, reportez-vous à Apple [section d’implémentation de schémas d’URL personnalisée de ce guide](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Panneau de configuration de source

Le **Source** onglet de le `Info.plist` fichier autorise des valeurs personnalisées être ajoutés ou modifiés. Visual Studio pour Mac fournit une liste de propriétés les plus courantes :

 [![](property-lists-images/image31.png "Ajout d’une nouvelle propriété à partir d’une liste déroulante")](property-lists-images/image31.png#lightbox)

Pour les propriétés connues Visual Studio pour Mac est une liste de valeurs valides, comme illustré par la capture d’écran suivante :

 [![](property-lists-images/image32.png "Sélectionnez une valeur dans une liste de valeurs connus")](property-lists-images/image32.png#lightbox)

Visual Studio pour Mac détecte également le Type de propriété, comme indiqué :

 [![](property-lists-images/image33.png "Les types de propriétés disponibles")](property-lists-images/image33.png#lightbox)

Passez en revue d’Apple [ressources connexes d’application](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) des liens pour plus d’informations sur les propriétés facultatives.

 <a name="Entitlements" />

## <a name="summary"></a>Récapitulatif

Cet article ne présente à l’aide des éditeurs graphiques et avancées .plist pour modifier configurations courantes d’application ainsi que pour spécifier les icônes et lancer des images. Il a également introduit le `Entitlements.plist` pour l’ajout et la gestion des fonctionnalités de l’application.


## <a name="related-links"></a>Liens associés

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [Application les ressources associées.](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Inscrit le fichier de votre application prend en charge les Types](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Schémas d’URL personnalisée](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [À propos des catalogues d’actif](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
