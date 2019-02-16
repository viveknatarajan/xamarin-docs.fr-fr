---
title: watchOS contrôles d’Interface utilisateur dans Xamarin
description: Ce document décrit les différents contrôles qui sont disponibles pour une utilisation dans les interfaces utilisateur de watchOS. Elle fournit une description des étiquettes, boutons, commutateurs, curseurs, images, les séparateurs, mappages et bien plus encore.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2016
ms.openlocfilehash: c8ef76f24b017f5e3e6bec9d39534f3626e79147
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321101"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>watchOS contrôles d’Interface utilisateur dans Xamarin

Le [ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) exemple illustre les différents contrôles watchOS. Animation de l’application est indiquée ici (cliquez pour effectuer un zoom) :

[![](images/storyboard-sml.png "Disposition de watchOS exemple")](images/storyboard.png#lightbox)

Les noms de tous les contrôles de programmation est préfixé avec `WKInterface` (par exemple). `WKInterfaceLabel`, `WKInterfaceButton`).

|Contrôle|Description|Capture d'écran|
|---|---|---|
|Etiquette|Utilisez `SetText` et d’autres propriétés pour contrôler l’apparence du texte dans un contrôle label. `NSAttributedString` est également pris en charge.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Bouton|Créez et définissez les propriétés dans le storyboard. CTRL + faire glisser pour ajouter un `Action` pour implémenter un gestionnaire pour laquelle il est activé.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Basculer|Utilisez `SetOn` pour contrôler l’état du commutateur.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Curseur|Beaucoup de styles différents est possibles.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Image|Utilisez `myImage.SetImage("MyWatchImage")` pour charger des images sur la surveillance, ou `WKInterfaceDevice.CurrentDevice.AddCachedImage` à les mettre en cache pour une utilisation répétée sur la surveillance.<br />[Documentation sur le contrôle image](~/ios/watchos/user-interface/image.md)<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Séparateur|Utiliser les séparateurs pour aider à créer attrayante regardez des interfaces utilisateur.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Carte|L’image de carte est statiquement affiché sur l’observation mais vous pouvez contrôler de nombreux aspects de son apparence, y compris l’ajout des codes confidentiels.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Movie & InlineMove|Films peuvent soit ouvert sur leur propre ou inline<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Regrouper|Utiliser des groupes pour aider à créer attrayante regardez des interfaces utilisateur.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Table|Une version simplifiée des tables sur iOS. Implémentez `DidSelectRow` pour répondre à la sélection de l’utilisateur (ou utilisez un segue).<br />[Documentation sur le contrôle table](~/ios/watchos/user-interface/table.md)<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Appareil|`WKInterfaceDevice.CurrentDevice` inclut des propriétés telles que `ScreenBounds`, `ScreenScale`, et `PreferredContentSizeCategory`.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|Définir le menu de presse de force dans la table de montage séquentiel et implémentez les actions pour chaque bouton dans le code.<br />[Documentation sur le contrôle (Force Touch) menu](~/ios/watchos/user-interface/menu.md)<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Entrée de texte|Utilisez `PresentTextInputController` et `WKTextInputMode` énumération.<br />[Documentation d’entrée de texte](~/ios/watchos/user-interface/text-input.md)<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Couronne numérique|Le Crown numérique peut être utilisé pour un sélecteur de lecteur, ou sa rotation peut être suivie dans le code.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Mouvements|Il existe quatre types de reconnaissance de mouvement qui peuvent être ajoutées à une scène : TAP, effectuez un balayage, panoramique et LongPress.<br />[Code de catalogue](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Regardez la référence d’API de Kit](xref:WatchKit)
