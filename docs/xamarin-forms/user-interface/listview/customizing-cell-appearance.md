---
title: Apparence de la cellule
description: "Explorer les options de présentation des données tout en tirant parti de la commodité de ListView."
ms.topic: article
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 726c65746b1282223c403b08c54c400ea32a6324
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="cell-appearance"></a>Apparence de la cellule

ListView présente des listes permettant le défilement, ce qui peuvent être personnalisés à l’aide de `ViewCell`s. `ViewCells` peut être utilisé pour afficher du texte et des images, qui indique un état de la valeur true/false et reçoit l’entrée d’utilisateur.

Il existe deux approches pour obtenir l’aspect souhaité à partir de cellules de ListView :

- **[Personnaliser des cellules intégrés](#Built_in_Cells)**  &ndash; implémentation simplifiée et meilleures performances au détriment de la personnalisation.
- **[Création de cellules personnalisés](#customcells)**  &ndash; plus contrôler le résultat final, mais peuvent avoir des problèmes de performances si ne pas correctement implémenté.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Intégré dans les cellules
Xamarin.Forms est fourni avec les cellules intégrés qui fonctionnent pour de nombreuses applications simples :

- **TextCell** &ndash; pour afficher du texte
- **ImageCell** &ndash; pour afficher une image avec du texte.

Deux autres cellules, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) et [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) sont disponibles, mais elles ne sont pas utilisées avec `ListView`. Consultez [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) pour plus d’informations sur ces cellules.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) est une cellule pour afficher du texte, éventuellement avec une deuxième ligne sous forme de texte de détail.

TextCells sont rendus sous forme de contrôles natifs à l’exécution, donc les performances sont très bonnes par rapport à un personnalisé `ViewCell`. TextCells sont personnalisables, ce qui vous permet de définir :

- `Text` &ndash; texte qui s’affiche sur la première ligne, dans la grande taille de police.
- `Detail` &ndash; texte qui s’affiche en dessous de la première ligne, dans une police plus petite.
- `TextColor` &ndash; la couleur du texte.
- `DetailColor` &ndash; la couleur du texte de détail

![](customizing-cell-appearance-images/text-cell-default.png "Exemple de TextCell par défaut")

![](customizing-cell-appearance-images/text-cell-custom.png "Exemple de TextCell personnalisés")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), tels que `TextCell`, peut être utilisé pour l’affichage de texte et texte détaillé secondaire, et il offre de bonnes performances à l’aide des contrôles natifs de chaque plateforme. `ImageCell` diffère de `TextCell` il affiche une image à gauche du texte.

`ImageCell` est utile lorsque vous avez besoin afficher une liste de données avec un aspect visuel, telle qu’une liste de contacts ou de films. ImageCells sont personnalisables, ce qui vous permet de définir :

- `Text` &ndash; le texte qui est affiché sur la première ligne, dans la grande taille de police
- `Detail` &ndash; le texte qui est affiché sous la première ligne, dans une police plus petite
- `TextColor` &ndash; la couleur du texte
- `DetailColor` &ndash; la couleur du texte de détail
- `ImageSource` &ndash; l’image à afficher en regard du texte

Notez que si vous ciblez Windows Phone 8.1, `ImageCell` n’évoluent pas les images par défaut. En outre, notez que Windows Phone 8.1 est la seule plateforme qui détaillent texte est présenté dans la même couleur et la police sous forme de texte principal par défaut. Windows Phone 8.0 restitue `ImageCell` comme indiqué ci-dessous :

![](customizing-cell-appearance-images/image-cell-default.png "Exemple de ImageCell par défaut")

![](customizing-cell-appearance-images/image-cell-custom.png "Exemple de ImageCell personnalisé")

<a name="customcells" />

## <a name="custom-cells"></a>Cellules personnalisées
Lorsque les cellules intégrés ne fournissent pas la disposition requise, cellules personnalisés implémenté la disposition requise. Par exemple, vous souhaiterez présenter une cellule avec deux étiquettes qui ont le même poids. A `LabelCell` est insuffisante car la `LabelCell` a une étiquette qui est plus petite.

Toutes les cellules personnalisés doivent dériver de [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), la même classe de base que tous les de la cellule intégrée types d’utilisation.

La plupart des personnalisations de cellule ajouter des données en lecture seule supplémentaires (par exemple, des légendes supplémentaires, images ou d’autres informations d’affichage). Si des boutons ou autres contrôles qui peuvent se concentrer sont ajoutés, la cellule lui-même n’est peut-être pas interactif sur Android. Un moyen de passer outre cette limitation, voir ci-dessous.

Xamarin.Forms 2 a introduit un nouveau [comportement de mise en cache](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) sur la `ListView` contrôle qui peut être définie pour améliorer les performances de défilement pour certains types de cellules personnalisés.

Il s’agit d’un exemple d’une cellule personnalisée :

![](customizing-cell-appearance-images/custom-cell.png "Exemple de cellule personnalisé")

### <a name="xaml"></a>XAML
Le code XAML pour créer la disposition ci-dessus est ci-dessous :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Le code XAML ci-dessus effectue beaucoup. Nous allons décomposer :

- La cellule personnalisée est imbriquée dans une `DataTemplate`, qui se trouve dans `ListView.ItemTemplate`. Ce processus est le même que l’utilisation de toute autre cellule.
- `ViewCell` est le type de la cellule personnalisé. L’enfant de la `DataTemplate` élément doit être d’ou dérivés du type `ViewCell`.
- Notez qu’à l’intérieur du `ViewCell`, mise en page est gérée par un `StackLayout`. Cette disposition permet de personnaliser la couleur d’arrière-plan. Notez que n’importe quelle propriété de `StackLayout` c'est-à-dire pouvant être lié peut être lié à l’intérieur d’une cellule personnalisée, mais qui n’est pas indiqué.

### <a name="cnum"></a>C&num;

La spécification d’une cellule personnalisée en c# est un peu plus détaillée que l’équivalent XAML. Étudions cela d’un peu plus près :

Tout d’abord, définissez une classe de cellule personnalisée avec `ViewCell` comme classe de base :

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

Dans le constructeur de la page avec le `ListView`, défini de ListView `ItemTemplate` à une nouvelle propriété `DataTemplate`:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

Notez que le constructeur de `DataTemplate` prend un type. L’opérateur typeof Obtient le type CLR pour `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Changements de contexte de liaison

Lors de la liaison à un type de cellule personnalisé [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instances, les contrôles d’interface utilisateur affichant le `BindableProperty` valeurs doivent utiliser le [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) remplacement pour définir les données à afficher dans chaque cellule, plutôt que le constructeur de cellule, comme illustré dans l’exemple de code suivant :

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

Le [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) remplacement sera appelé lorsque le [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) se déclenche des événements, en réponse à la valeur de la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) la modification de propriété. Par conséquent, lorsque le `BindingContext` change, les contrôles d’interface utilisateur affichant le [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) valeurs doivent définir leurs données. Notez que la `BindingContext` doit être vérifiée pour un `null` valeur, comme cela peut être défini par Xamarin.Forms pour le garbage collection, ce qui à son tour entraîne le `OnBindingContextChanged` remplacer l’appel.

Vous pouvez également lier les contrôles d’interface utilisateur du [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instances pour afficher leurs valeurs, ce qui supprime la nécessité de remplacer le `OnBindingContextChanged` (méthode).

> [!NOTE]
> **Remarque**: lors de la substitution `OnBindingContextChanged`, vérifiez que la classe de base `OnBindingContextChanged` méthode est appelée afin que les délégués inscrits reçoivent le `BindingContextChanged` événement.

En XAML, le type de cellule personnalisé de liaison de données peut être obtenue comme indiqué dans l’exemple de code suivant :

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Ceci lie le `Name`, `Age`, et `Location` propriétés pouvant être liées dans le `CustomCell` de l’instance, à la `Name`, `Age`, et `Location` propriétés de chaque objet dans la collection sous-jacente.

La liaison équivalente en c# est indiquée dans l’exemple de code suivant :

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

Sur iOS et Android, si le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) recyclage d’éléments et la cellule personnalisée utilise un convertisseur personnalisé, le convertisseur personnalisé doit implémenter correctement la notification de modification de propriété. Lorsque les cellules sont réutilisés leurs valeurs de propriété change lorsque le contexte de liaison est mise à jour à celle d’une cellule disponible, avec `PropertyChanged` événements déclenchés. Pour plus d’informations, consultez [personnalisation un ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Pour plus d’informations sur le recyclage de cellule, consultez [stratégie de mise en cache](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

### <a name="enabling-row-selection-on-android"></a>L’activation de la sélection de ligne sur Android

Pour permettre la sélection de ligne pour les cellules qui incluent également d’entrée d’éléments tels que des boutons, une simple [ `custom renderer` ](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) est requis. Dans le code commun, créez une sous-classe de `Button` afin qu’un convertisseur personnalisé peut être ajouté dans les projets de plateforme :

```csharp
public class ListButton : Button { }
```

L’implémentation de convertisseur pour Android définit simplement le `Focusable` propriété qui permet à la ligne à être sélectionnés, ainsi que des boutons interactifs à l’hôte. Ce code est ajouté au projet d’application Android :

```csharp
[assembly: ExportRenderer (typeof (ListButton), typeof (ListButtonRenderer))]
// ...
public class ListButtonRenderer : ButtonRenderer {
    protected override void OnElementChanged (ElementChangedEventArgs<ListButton> e) {
        base.OnElementChanged (e);
        Control.Focusable = false;
    }
}
```

Comme indiqué ci-dessus, seules Android requiert la `ButtonRenderer` à implémenter. iOS et les plateformes Windows Phone autorisent le bouton sur lequel cliquer sans avoir à implémenter un convertisseur personnalisé.


## <a name="related-links"></a>Liens associés

- [Intégré dans les cellules (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Cellules personnalisés (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Liaison de contexte modifié (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
