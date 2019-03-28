---
title: Remplir Xamarin.Forms CollectionView avec des données
description: Un CollectionView est rempli avec des données en définissant sa propriété ItemsSource à toute collection qui implémente IEnumerable.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 57012202d981b96dba42f3017a19f2e32e4982ec
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507186"
---
# <a name="populate-xamarinforms-collectionview-with-data"></a>Remplir Xamarin.Forms CollectionView avec des données

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version d’évaluation et ne dispose pas de certaines de ses fonctionnalités planifiée. En outre, l’API peut changer que l’implémentation est terminée.

`CollectionView` définit les propriétés suivantes qui définissent les données à afficher et son apparence :

- `ItemsSource`, de type `IEnumerable`, spécifie la collection d’éléments à afficher, et a la valeur par défaut `null`.
- `ItemTemplate`, de type [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), spécifie le modèle à appliquer à chaque élément dans la collection d’éléments à afficher.

Ces propriétés sont secondées par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données.

## <a name="populate-a-collectionview-with-data"></a>Remplir un CollectionView de données

Un `CollectionView` est rempli avec des données en définissant son `ItemsSource` propriété à toute collection qui implémente `IEnumerable`. Éléments peuvent être ajoutés dans XAML en initialisant le `ItemsSource` propriété à partir d’un tableau de chaînes :

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> Notez que l’élément `x:Array` nécessite un attribut `Type` qui indique le type des éléments du tableau.

Le code c# équivalent est :

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> Si le `CollectionView` est nécessaire pour actualiser les comme éléments sont ajoutés, supprimés ou modifiés dans la collection sous-jacente, la collection sous-jacente doit être un `IEnumerable` collection qui envoie la propriété notifications de changement, tel que `ObservableCollection`.

Par défaut, `CollectionView` affiche les éléments dans une liste verticale, comme indiqué dans les captures d’écran suivante :

[![Capture d’écran de CollectionView contenant des éléments de texte, sur iOS et Android](populate-data-images/text.png "des éléments de texte d’un CollectionView de")](populate-data-images/text-large.png#lightbox "des éléments de texte d’un CollectionView de")

Pour plus d’informations sur la façon de modifier le `CollectionView` mise en page, consultez [spécifier une disposition](layout.md). Pour plus d’informations sur la façon de définir l’apparence de chaque élément dans le `CollectionView`, consultez [définissent l’apparence de l’élément](#define-item-appearance).

### <a name="data-binding"></a>Liaison de données

`CollectionView` peut être rempli de données à l’aide de la liaison de données à lier son `ItemsSource` propriété un `IEnumerable` collection. Dans XAML, cela est possible avec le `Binding` extension de balisage :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Le code c# équivalent est :

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Dans cet exemple, le `ItemsSource` lie les données de propriété pour le `Monkeys` propriété du modèle de vue connectée.

> [!NOTE]
> Liaisons compilées peuvent être activées pour améliorer les performances de liaison de données dans les applications Xamarin.Forms. Pour plus d’informations, consultez [liaisons compilé](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Pour plus d’informations sur la liaison de données, consultez la page [Liaison de données Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Définir l’apparence de l’élément

L’apparence de chaque élément dans le `CollectionView` peut être définie en configurant le `CollectionView.ItemTemplate` propriété à un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

Le code c# équivalent est :

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

Les éléments spécifiés dans le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) définissent l’apparence de chaque élément dans la liste. Dans l’exemple, la disposition au sein du `DataTemplate` est géré par un [ `Grid` ](xref:Xamarin.Forms.Grid). Le `Grid` contient un [ `Image` ](xref:Xamarin.Forms.Image) objet et deux [ `Label` ](xref:Xamarin.Forms.Label) objets, que tous les lient aux propriétés de la `Monkey` classe :

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Les captures d’écran suivantes affichent le résultat de la création de modèles de chaque élément dans la liste :

[![Capture d’écran de CollectionView où chaque élément est basé sur un modèle, sur iOS et Android](populate-data-images/datatemplate.png "éléments basé sur un modèle d’un CollectionView de")](populate-data-images/datatemplate-large.png#lightbox "éléments basé sur un modèle d’un CollectionView de")

Pour plus d’informations sur les modèles de données, consultez [Modèles de données Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Liaison de données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Modèles de données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
