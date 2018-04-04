---
title: Affichage des alertes
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b15afa55f170ce86fd66e17209c6db172b1db740
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="displaying-alerts"></a>Affichage des alertes

À compter d’iOS 8, UIAlertController a terminé UIActionSheet remplacé et UIAlertView qui sont désormais déconseillées.

Contrairement aux classes qui sont des sous-classes de UIView, qu'il est remplacé, UIAlertController est une sous-classe de UIViewController.

Utilisez `UIAlertControllerStyle` pour indiquer le type d’alerte à afficher. Ces types d’alertes sont :

- **UIAlertControllerStyleActionSheet**
    * Préliminaire iOS 8 cela aurait un UIActionSheet
- **UIAlertControllerStyleAlert**
    * Préliminaire iOS 8 cela aurait été UIAlertView 

Il existe trois étapes nécessaires à prendre lors de la création d’un contrôleur d’alerte :

- Créer et configurer l’alerte avec r :
    * titre
    * message
    * preferredStyle
    
- (Facultatif) Ajouter un champ de texte
- Ajoutez les actions requises
- Présenter le contrôleur d’affichage

L’alerte la plus simple contient un seul bouton, comme indiqué dans cette capture d’écran :

 ![Avec un bouton d’alerte](alerts-images/alert1.png)

Le code pour afficher une alerte simple est comme suit :

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

Affiche une alerte avec de nombreuses options, est effectuée de façon similaire, mais ajouter deux actions. Par exemple, la capture d’écran suivante montre une alerte avec deux boutons :

 ![ Alerte contenant deux boutons](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Les alertes peuvent également afficher une feuille de l’action semblable à la capture d’écran ci-dessous :

 ![Alerte de feuille d’action](alerts-images/alert3.png)

Boutons sont ajoutés à l’alerte avec la `AddAction` méthode :

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
- [Contrôleur de l’alerte](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
