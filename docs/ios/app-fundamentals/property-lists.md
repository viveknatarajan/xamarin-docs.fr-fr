---
title: Utilisation des listes de propriétés dans Xamarin.iOS
description: Ce document présente Visual Studio pour l’éditeur de liste (.plist) Mac propriété graphique et avancées pour travailler avec Info.plist et Entitlements.plist. Il illustre la définition d’icônes et images de lancement des applications iOS à partir de Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b102f268fd457ca42f3d64b5766a2b3824e3849d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242327"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Utilisation des listes de propriétés dans Xamarin.iOS

_Ce document présente Visual Studio pour l’éditeur de liste (.plist) Mac propriété graphique et avancées pour travailler avec Info.plist et Entitlements.plist. Il illustre la définition d’icônes et images de lancement des applications iOS à partir de Visual Studio pour Mac._

Visual Studio pour Mac propose un éditeur graphique .plist qui facilite la modification des propriétés de l’application et de fonctionnalités plus facilitées. Visual Studio pour Mac a deux .plists - `Info.plist` pour la modification des propriétés de l’application et des icônes, et `Entitlements.plist` pour gérer des fonctionnalités de l’application. Ce guide présente le Info.plists et fournit une vue d’ensemble de leur utilisation dans Visual Studio pour Mac. Pour plus d’informations sur le fichier Entitlements.plist, consultez le [utilisation de droits](~/ios/deploy-test/provisioning/entitlements.md) guide.

## <a name="infoplist"></a>Info.plist

La liste des propriétés d’informations ( `Info.plist`) est un fichier iOS requis qui fournit des informations sur la configuration de votre application dans le système. Personnalisée du Visual Studio pour Mac `Info.plist` trois panneaux contrôlé par les onglets en bas à gauche de la fenêtre Éditeur de fonctionnalités de l’éditeur :

 [![](property-lists-images/tabs.png "Les onglets de l’éditeur Info.plist en bas à gauche de la fenêtre d’éditeur")](property-lists-images/tabs.png#lightbox)

Chaque panneau de contrôle des propriétés différentes, comme indiqué ci-dessous :

-  **Panneau application** -une interface graphique pour définir courantes des propriétés de l’application ainsi que des icônes et des lancement images ; spécifiez l’intégration des cartes et des modes de backgrounding.
-  **Advanced panneau** -le panneau de configuration avancée est l’endroit pour spécifier les types de documents pris en charge, UTI et les types d’URL.
-  **Panneau de configuration de source** -le panneau source de contrôle des propriétés moins courantes, ainsi que des propriétés personnalisées pour l’application.


Les trois sections suivantes examiner les fonctionnalités de chaque panneau plus en détail.

## <a name="application-panel"></a>Panneau de l’application

Visual Studio pour Mac propose une interface graphique pour la modification courantes `Info.plist` entrées pour une application :

1.  Propriétés des applications
1.  Types d’appareils pris en charge
1.  Orientations de prise en charge pour chaque type d’appareil
1.  Style et la couleur de la barre d’état
1.  Icônes et écrans de démarrage
1.  Cartes et les Modes d’arrière-plan


Ceux-ci sont décrits plus en détail dans les sections suivantes.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>Cible d’Application iOS

Cette section contient des informations importantes qui décrit votre application.
Le **identificateur** stockées ici doit correspondre à l’identificateur de Bundle qui est entré dans iTunes Connect (pour les applications du Windows Store de l’application), ainsi que dans la liste des ID d’application portail d’approvisionnement iOS et les certificats de développement et de Distribution.

 [![](property-lists-images/image24.png "Cible d’Application iOS")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Déploiement de l’appareil

 [![](property-lists-images/deployment.png "Déploiement de l’appareil")](property-lists-images/deployment.png#lightbox)

L’appareil **déploiement** sections d’informations sont affichées de manière sélective, selon la sélection dans le **appareils** liste déroulante dans le **Application cible** section ci-dessus. Le **Interface principale** liste déroulante est définie sur **MainStoryboard** dans des applications pilotées par table de montage séquentiel. Si l’interface utilisateur est écrite entièrement en code puis cela peut être vide.

### <a name="supported-device-orientations"></a>Orientations de l’appareil pris en charge

 **Prise en charge des Orientations de l’appareil** détermine comment l’application répond à la rotation de l’appareil. Il est très courant pour les applications iPhone/iPad prendre en charge uniquement **Portrait**, ou tous les éléments mais **envers**. En règle générale, toutes les applications iPad, à l’exception des jeux doivent prendre en charge toutes les orientations.

### <a name="status-bar-styles"></a>Styles de barre d’état

Le **Styles de barre d’état** section est une interface graphique pour la modification d’une application `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Styles de barre d’état")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Icônes, d’Images de lancement et de conception graphique iTunes

Vous trouverez des informations sur l’utilisation des icônes, des images et des illustrations dans votre fichier Info.plist dans le [utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) guide.




### <a name="maps-integration-and-background-modes"></a>Intégration des cartes et des Modes d’arrière-plan

Le `Info.plist` contient des sections spéciales pour spécifier l’intégration des cartes et des modes de backgrounding. Vous choisissez les options que vous souhaitez prendre en charge ajoutera les propriétés requises à votre application pour vous.

 [![](property-lists-images/maps.png "Intégration des cartes")](property-lists-images/maps.png#lightbox)

Pour plus d’informations sur l’utilisation des mappages, reportez-vous à la Xamarin [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) guide.

 [![](property-lists-images/bging.png "Modes d’arrière-plan")](property-lists-images/bging.png#lightbox)

Pour plus d’informations sur les Modes d’arrière-plan, reportez-vous à la Xamarin [Backgrounding dans iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) guide.

## <a name="advanced-panel"></a>Panneau de configuration avancée

Le panneau avancées contrôle les types de document et les schémas d’URL qui prend en charge de l’application.

 [![](property-lists-images/image34.png "Panneau de configuration avancée")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Types de documents

Pour les applications qui prennent en charge l’ouverture des types de fichiers spécifiques, iOS fournit le `CFBundleDocumentTypes` clé. Si nous voulons que notre application pour prendre en charge certains types de fichiers connus - par exemple un fichier PDF - nous serait ajouter la valeur PDF à la clé. Cette section fournit un moyen pratique d’entrer les données qui est stocké dans le `CFBundleDocumentTypes` clé dans le `Info.plist` fichier.

Reportez-vous à la documentation sur [enregistrer le fichier de Types de votre application prend en charge](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) pour plus d’informations sur la façon de configurer ces valeurs.

## <a name="utis"></a>UTI

Parfois, une application doit prendre en charge l’ouverture d’un type de fichier personnalisé. Par exemple, nous souhaiterons peut-être ouvrir des fichiers de l’image avec une extension personnalisée *.xam*. Pour spécifier un type de fichier personnalisé, nous allons créer un UTI - identificateur de Type universel - personnalisé à l’aide du `UIExportedTypeDeclarations` clé. La capture d’écran ci-dessous illustre comment créer un UTI personnalisé pour l’extension .xam :

 [![](property-lists-images/uti.png "Éditeur d’UTI")](property-lists-images/uti.png#lightbox)

UTI de type simplement comme exporté spécifier UTI personnalisé spécifique à votre application, le *importé UTI de type* ( `UIImportedTypeDeclarations` clé) spécifier les types personnalisés pris en charge, mais ne pas détenus par votre application.

Pour plus d’informations sur l’utilisation des UTI personnalisés, consultez d’Apple [inscription fichier Types votre application prend en charge](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) guide.

## <a name="custom-urls"></a>URL personnalisées

Un nom de schéma d’URL (également appelé protocole) est la première partie de l’URL. Par exemple, `http://` et `https://` sont les schémas d’URL courantes. Vous avez la possibilité de créer un schéma d’URL personnalisé pour votre application. Schémas d’URL personnalisés sont utilisés pour communiquer et envoyer des données dans les deux sens avec d’autres applications. La capture d’écran suivante illustre la création d’un nouveau schéma d’URL personnalisé appelé `monkeys://`:

 [![](property-lists-images/url.png "URL personnalisées")](property-lists-images/url.png#lightbox)



Pour plus d’informations sur l’implémentation de schémas d’URL personnalisées, consultez d’Apple [section d’implémentation de schémas d’URL personnalisée de ce guide](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Panneau de la source

Le **Source** onglet de la `Info.plist` fichier autorise des valeurs personnalisées être ajouté ou modifié. Visual Studio pour Mac fournit une liste de propriétés les plus courantes :

 [![](property-lists-images/image31.png "Ajout d’une nouvelle propriété dans une liste déroulante")](property-lists-images/image31.png#lightbox)

Pour les propriétés connues Visual Studio pour Mac sera une liste de valeurs valides, comme illustré par la capture d’écran suivante :

 [![](property-lists-images/image32.png "Sélectionnez une valeur dans une liste de valeurs connus")](property-lists-images/image32.png#lightbox)

Visual Studio pour Mac détecte également le Type de propriété, comme indiqué :

 [![](property-lists-images/image33.png "Les types de propriétés disponibles")](property-lists-images/image33.png#lightbox)

Passez en revue d’Apple [ressources connexes d’application](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) des liens pour plus d’informations sur les propriétés facultatives.

 <a name="Entitlements" />

## <a name="summary"></a>Récapitulatif

Cet article a montré à l’aide les éditeurs .plist graphique et Avancé pour modifier les configurations d’application courants, ainsi que pour spécifier les icônes et images de lancement. Il a également introduit le `Entitlements.plist` pour l’ajout et la gestion des fonctionnalités de l’application.


## <a name="related-links"></a>Liens associés

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [Ressources liées à l’application](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Inscrit le fichier de Types de votre application prend en charge](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Implémentation de schémas d’URL personnalisée](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Référence de Format de catalogue Asset](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
