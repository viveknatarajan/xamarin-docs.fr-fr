---
title: watchOS Interface utilisateur
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 34063be728f8a13bbe5d68440d9aceba417c5627
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-user-interface"></a>watchOS Interface utilisateur

Le [ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) exemple illustre les différents contrôles watchOS. Plan conceptuel de l’application est illustrée ici (cliquez pour effectuer un zoom avant) :

[ ![](images/storyboard-sml.png "Disposition de watchOS exemple")](images/storyboard.png)

Les noms de tous les contrôles de programmation est préfixé avec `WKInterface` (par exemple). `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>Control</strong>
      </th>
      <th>
        <strong>Description</strong>
      </th>
      <th>
        <strong>Screenshot</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
Ajouter des contrôles </td>
      <td valign="top">
Utilisez <code>SetText</code> et d’autres propriétés pour contrôler l’apparence du texte dans un contrôle label. <code>NSAttributedString</code> est également pris en charge.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Bouton </td>
      <td valign="top">
Créer et définir les propriétés dans la table de montage séquentiel. <kbd>CTRL + glisser</kbd> pour ajouter un <code>Action</code> pour implémenter un gestionnaire pour laquelle il est activé.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Basculer </td>
      <td valign="top">
Utilisez <code>SetOn</code> pour contrôler l’état du commutateur.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Curseur </td>
      <td valign="top">
De nombreux styles différents sont possibles.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Image </td>
      <td valign="top">
Utilisez <code>myImage.SetImage("MyWatchImage")</code> pour charger des images sur la surveillance, ou <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> pour les mettre en cache pour une utilisation répétée sur la surveillance.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">Documentation sur le contrôle image</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Séparateur </td>
      <td valign="top">
Utiliser les séparateurs pour aider à créer attrayante regarder des interfaces utilisateur.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Carte </td>
      <td valign="top">
L’image de carte est statiquement affichée sur la surveillance, mais vous pouvez contrôler de nombreux aspects de son apparence, y compris l’ajout des codes confidentiels.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
FILM & InlineMove </td>
      <td valign="top">
Films peuvent ouvrir sur leurs propres ou inline <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Regrouper </td>
      <td valign="top">
Utiliser des groupes pour aider à créer attrayante regarder des interfaces utilisateur.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Table </td>
      <td valign="top">
Une version simplifiée de tables sur iOS.
Implémentez <code>DidSelectRow</code> pour répondre à la sélection de l’utilisateur (ou utilisez un segue).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">Documentation sur le contrôle table</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Appareil </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> contient des propriétés telles que <code>ScreenBounds</code>, <code>ScreenScale</code>, et <code>PreferredContentSizeCategory</code>.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">Menu</a>
      </td>
      <td valign="top">
Définir le menu de presse de force dans le plan conceptuel et implémenter les actions pour chaque bouton dans le code.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">Documentation du menu contrôle (Force Touch)</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Entrée de texte </td>
      <td valign="top">
Utilisez <code>PresentTextInputController</code> et <code>WKTextInputMode</code> énumération.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">Documentation d’entrée de texte</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Couronne numérique </td>
      <td valign="top">
La couronne numérique peut servir à un sélecteur de lecteur, ou sa rotation peut être suivie dans le code.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Mouvements </td>
      <td valign="top">
Il existe quatre types de reconnaissance de mouvement qui peut être ajouté à une scène : Tap, faites défiler, panoramique et LongPress.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">Code de catalogue</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Référence des API Kit espion](https://developer.xamarin.com/api/namespace/WatchKit/)
