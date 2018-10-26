---
title: Personnaliser l’apparence d’une Table dans Xamarin.iOS
description: Ce document décrit comment personnaliser l’apparence d’un tableau dans Xamarin.iOS. Il aborde les styles de cellules, les accessoires, les séparateurs de cellule et les dispositions de cellule personnalisé.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0282b4b2194411d503ef7eb54b0337272e2be3ed
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121319"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Personnaliser l’apparence d’une Table dans Xamarin.iOS

Pour modifier l’apparence d’une table, le plus simple consiste à utiliser un style de cellule différentes. Vous pouvez modifier le style de cellule utilisé lors de la création de chaque cellule dans le `UITableViewSource`de `GetCell` (méthode).

## <a name="cell-styles"></a>Styles de cellules

Il existe quatre styles intégrés :

-  **Par défaut** – prend en charge un `UIImageView`.
-  **Sous-titre** – prend en charge un `UIImageView` et le sous-titre.
-  **Value1** – droit sous-titre aligné, prend en charge un `UIImageView`.
-  **Value2** : titre est aligné à droite et le sous-titre est aligné à gauche (mais pas d’image).


Ces captures d’écran montrent la façon dont chaque style s’affiche :

 [![](customizing-table-appearance-images/image7.png "Ces captures d’écran montrent la façon dont chaque style s’affiche")](customizing-table-appearance-images/image7.png#lightbox)

L’exemple **CellDefaultTable** contient le code pour produire ces écrans. Le style de cellule est défini le `UITableViewCell` constructeur, comme suit :

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Propriétés prises en charge](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) de la cellule de style peut être défini :

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Accessoires

Cellules peuvent avoir les accessoires suivants ajoutés à droite de la vue :

-   **Coche** – peut être utilisé pour indiquer la sélection multiple dans une table.
-   **DetailButton** – répond à toucher indépendamment du reste de la cellule, ce qui lui permet d’effectuer une autre fonction pour toucher la cellule lui-même (telles que l’ouverture d’une fenêtre contextuelle ou une nouvelle fenêtre qui n’est pas dans le cadre d’un `UINavigationController` stack).
-   **DisclosureIndicator** : normalement utilisés pour indiquer que le toucher la cellule s’ouvre une autre vue.
-   **DetailDisclosureButton** – une combinaison de la `DetailButton` et `DisclosureIndicator`.


Voici à quoi ils ressemblent :

 [![](customizing-table-appearance-images/image8.png "Exemple accessoires")](customizing-table-appearance-images/image8.png#lightbox)

Pour afficher l’un de ces accessoires que vous pouvez définir le `Accessory` propriété dans le `GetCell` méthode :

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Lorsque le `DetailButton` ou `DetailDisclosureButton` sont affichés, vous devez également substituer la `AccessoryButtonTapped` d’effectuer une action lorsqu’il est touché.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

L’exemple **CellAccessoryTable** montre un exemple d’utilisation des accessoires.

## <a name="cell-separators"></a>Séparateurs de cellule

Séparateurs de cellule sont les cellules du tableau utilisés pour séparer la table. Les propriétés sont définies sur la Table.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Il est également possible d’ajouter un effet de flou ou dynamisme de notre place pour le séparateur :

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Le séparateur peut également avoir une incrustation :

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Création de dispositions de cellule personnalisée

Pour modifier le style visuel d’une table, que vous devez fournir des cellules personnalisés pour pouvoir les afficher. La cellule personnalisée peut avoir différentes dispositions de couleurs et de contrôle.

L’exemple CellCustomTable implémente un `UITableViewCell` sous-classe qui définit une disposition personnalisée de `UILabel`s et un `UIImage` avec des polices et couleurs. Les cellules qui en résulte se présenter comme suit :

 [![](customizing-table-appearance-images/image9.png "Dispositions de cellule personnalisée")](customizing-table-appearance-images/image9.png#lightbox)

La classe de cellule personnalisée se compose de seulement trois méthodes :

-   **Constructeur** : crée des contrôles d’interface utilisateur et définit les propriétés de style personnalisé (par exemple). type de police, la taille et couleurs).
-   **UpdateCell** – une méthode pour `UITableView.GetCell` à utiliser pour définir les propriétés de la cellule.
-   **LayoutSubviews** : définir l’emplacement des contrôles d’interface utilisateur. Dans l’exemple, chaque cellule a la même disposition, mais une cellule plus complexe (en particulier celles ayant différentes tailles) devrez peut-être les positions de mise en page différente selon le contenu affiché.


L’exemple de code complet dans **CellCustomTable > CustomVegeCell.cs** suit :

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

Le `GetCell` méthode de la `UITableViewSource` doit être modifié pour créer la cellule personnalisée :

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>Liens associés

- [WorkingWithTables (exemple)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
