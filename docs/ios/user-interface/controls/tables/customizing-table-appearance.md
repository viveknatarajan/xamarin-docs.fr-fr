---
title: "Personnaliser l’apparence de la Table"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4a3f8ca8f4502b9585536815aef81f66cacd214f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-tables-appearance"></a>Personnaliser l’apparence de la Table

La façon la plus simple pour modifier l’apparence d’une table est d’utiliser un style de cellule différente. Vous pouvez modifier le style de cellule est utilisé lors de la création de chaque cellule dans le `UITableViewSource`de `GetCell` (méthode).

## <a name="cell-styles"></a>Styles de cellules

Il existe quatre styles intégrés :

-  **Par défaut** – prend en charge un `UIImageView`.
-  **Sous-titre** – prend en charge un `UIImageView` et le sous-titre.
-  **Valeur1** – droite aligné sous-titre, prend en charge un `UIImageView`.
-  **Value2** : titre est aligné à droite et le sous-titre est aligné à gauche (mais pas d’image).


Ces captures d’écran montrent comment chaque style s’affiche :

 [![](customizing-table-appearance-images/image7.png "Ces captures d’écran montrent comment chaque style s’affiche")](customizing-table-appearance-images/image7.png#lightbox)

L’exemple **CellDefaultTable** contient le code pour générer ces écrans. Le style de cellule est défini dans le `UITableViewCell` constructeur, comme suit :

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Prise en charge des propriétés](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) de la cellule de style peut être défini :

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Accessoires

Cellules peuvent avoir les accessoires suivants ajoutées à droite de la vue :

-   **Coche** – peut être utilisé pour indiquer la sélection multiple dans une table.
-   **DetailButton** – répond à horodater indépendamment du reste de la cellule, ce qui lui permet d’effectuer une autre fonction pour toucher la cellule elle-même (telles que l’ouverture d’un menu contextuel ou une nouvelle fenêtre qui n’est pas dans le cadre d’un `UINavigationController` pile).
-   **DisclosureIndicator** : normalement utilisé pour indiquer que le toucher la cellule s’ouvre une autre vue.
-   **DetailDisclosureButton** – une combinaison de la `DetailButton` et `DisclosureIndicator`.


Voici à quoi ils ressemblent :

 [![](customizing-table-appearance-images/image8.png "Exemple accessoires")](customizing-table-appearance-images/image8.png#lightbox)

Pour afficher l’un de ces accessoires que vous pouvez définir le `Accessory` propriété dans la `GetCell` méthode :

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Lorsque le `DetailButton` ou `DetailDisclosureButton` sont affichés, vous devez également substituer la `AccessoryButtonTapped` pour effectuer une action lorsqu’il est affecté.

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

Les séparateurs de cellule sont les cellules de tableau utilisés pour séparer la table. Les propriétés sont définies sur la Table.

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

Le séparateur peut également avoir une marge :

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Créer des dispositions de cellule personnalisé

Pour modifier le style visuel d’une table, que vous devez fournir des cellules personnalisées pour pouvoir les afficher. La cellule personnalisée peut avoir des dispositions différentes couleurs et de contrôle.

L’exemple CellCustomTable implémente un `UITableViewCell` sous-classe qui définit une disposition personnalisée de `UILabel`s et un `UIImage` avec différentes polices et couleurs. Les cellules qui en résulte se présenter comme suit :

 [![](customizing-table-appearance-images/image9.png "Dispositions de cellule personnalisé")](customizing-table-appearance-images/image9.png#lightbox)

La classe de cellule personnalisée se compose de trois méthodes :

-   **Constructeur** : crée des contrôles d’interface utilisateur et définit les propriétés de style personnalisé (par exemple). type de police, la taille et couleurs).
-   **UpdateCell** : une méthode pour `UITableView.GetCell` à utiliser pour définir les propriétés de la cellule.
-   **LayoutSubviews** : définir l’emplacement des contrôles d’interface utilisateur. Dans l’exemple, chaque cellule a la même disposition, mais une cellule plus complexe (en particulier celles ayant des tailles différentes) peut-être les positions de mise en page différente selon le contenu affiché.


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

Le `GetCell` méthode de le `UITableViewSource` doit être modifié pour créer la cellule personnalisée :

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
