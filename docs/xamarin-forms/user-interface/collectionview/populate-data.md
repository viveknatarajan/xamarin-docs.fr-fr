---
title: Xamarin.Forms CollectionView de données
description: Un CollectionView est rempli avec des données en définissant sa propriété ItemsSource à toute collection qui implémente IEnumerable.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 68e28fcbe6a64834d3b594f7f639a1cdd990370d
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970579"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.Forms CollectionView de données

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) définit les propriétés suivantes qui définissent les données à afficher et son apparence :

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), de type `IEnumerable`, spécifie la collection d’éléments à afficher, et a la valeur par défaut `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate), de type [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), spécifie le modèle à appliquer à chaque élément dans la collection d’éléments à afficher.

Ces propriétés sont secondées par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données.

## <a name="populate-a-collectionview-with-data"></a>Remplir un CollectionView de données

Un [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) est rempli avec des données en définissant son [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) propriété à toute collection qui implémente `IEnumerable`. Éléments peuvent être ajoutés dans XAML en initialisant le `ItemsSource` propriété à partir d’un tableau de chaînes :

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

Le code C# équivalent est :

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
> Si le [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) est nécessaire pour actualiser les comme éléments sont ajoutés, supprimés ou modifiés dans la collection sous-jacente, la collection sous-jacente doit être un `IEnumerable` collection qui envoie la propriété notifications de modifications, telles que `ObservableCollection`.

Par défaut, [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) affiche les éléments dans une liste verticale, comme indiqué dans les captures d’écran suivante :

[![Capture d’écran de CollectionView contenant des éléments de texte, sur iOS et Android](populate-data-images/text.png "des éléments de texte d’un CollectionView de")](populate-data-images/text-large.png#lightbox "des éléments de texte d’un CollectionView de")

Pour plus d’informations sur la façon de modifier le [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) mise en page, consultez [spécifier une disposition](layout.md). Pour plus d’informations sur la façon de définir l’apparence de chaque élément dans le `CollectionView`, consultez [définissent l’apparence de l’élément](#define-item-appearance).

### <a name="data-binding"></a>Liaison de données

[`CollectionView`](xref:Xamarin.Forms.CollectionView) peut être rempli de données à l’aide de la liaison de données à lier son [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) propriété un `IEnumerable` collection. Dans XAML, cela est possible avec le `Binding` extension de balisage :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Dans cet exemple, le [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) lie les données de propriété pour le `Monkeys` propriété du modèle de vue connectée.

> [!NOTE]
> Liaisons compilées peuvent être activées pour améliorer les performances de liaison de données dans les applications Xamarin.Forms. Pour plus d’informations, consultez [liaisons compilé](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Pour plus d’informations sur la liaison de données, consultez la page [Liaison de données Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Définir l’apparence de l’élément

L’apparence de chaque élément dans le [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) peut être définie en configurant le [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) propriété à un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

Le code C# équivalent est :

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

## <a name="choose-item-appearance-at-runtime"></a>Choisissez l’apparence de l’élément lors de l’exécution

L’apparence de chaque élément dans le [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) peuvent être choisies lors de l’exécution, selon la valeur de l’élément, en définissant le [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) propriété à un [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)objet :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Le [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) propriété est définie sur une `MonkeyDataTemplateSelector` objet. L’exemple suivant montre la `MonkeyDataTemplateSelector` classe :

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

Le `MonkeyDataTemplateSelector` classe définit `AmericanMonkey` et `OtherMonkey` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) propriétés qui sont définies sur les différents modèles de données. Le `OnSelectTemplate` remplacer retourne le `AmericanMonkey` modèle, qui affiche le nom de monkey et l’emplacement dans bleu-vert, lorsque le nom monkey contient « America ». Lorsque le nom monkey ne contient pas « America », le `OnSelectTemplate` remplacer retourne le `OtherMonkey` modèle, qui affiche le nom de monkey et l’emplacement dans silver :

[![Capture d’écran de CollectionView runtime élément sélection du modèle, sur iOS et Android](populate-data-images/datatemplateselector.png "sélection du modèle d’élément Runtime d’un CollectionView de")](populate-data-images/datatemplateselector-large.png#lightbox "sélection du modèle d’élément Runtime dans un CollectionView")

Pour plus d’informations sur les sélecteurs de modèle de données, consultez [créer un Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Liaison de données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Modèles de données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Créer un Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
