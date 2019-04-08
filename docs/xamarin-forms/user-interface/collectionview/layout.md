---
title: Spécifier la disposition de CollectionView Xamarin.Forms
description: Par défaut, un CollectionView affiche ses éléments dans une liste verticale. Toutefois, grilles et répertorie vertical et horizontal peuvent être spécifiés.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 8ed365ed41ac31c66d41f1a32a7a16929cdc6770
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329837"
---
# <a name="specify-xamarinforms-collectionview-layout"></a>Spécifier la disposition de CollectionView Xamarin.Forms

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version d’évaluation et ne dispose pas de certaines de ses fonctionnalités planifiée. En outre, l’API peut changer que l’implémentation est terminée.

`CollectionView` définit les propriétés suivantes qui contrôlent la mise en page :

- `ItemsLayout`, de type `IItemsLayout`, spécifie la disposition à utiliser.
- `ItemSizingStrategy`, de type `ItemSizingStrategy`, spécifie la stratégie de mesures élément à utiliser.

Ces propriétés sont secondées par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données.

Par défaut, un `CollectionView` affiche ses éléments dans une liste verticale. Toutefois, chacun des dispositions suivantes peut être utilisé :

- Liste verticale : une liste de colonne unique qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés.
- Liste horizontale : une liste de ligne unique qui se développe horizontalement comme de nouveaux éléments sont ajoutés.
- Quadrillage vertical – une grille de plusieurs colonne qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés.
- Grille horizontale – une grille à plusieurs ligne qui se développe horizontalement comme de nouveaux éléments sont ajoutés.

Ces mises en page peuvent être spécifiés en définissant le `ItemsLayout` propriété de classe qui dérive de la `ItemsLayout` classe. Cette classe définit les propriétés suivantes :

- `Orientation`, de type `ItemsLayoutOrientation`, spécifie la direction dans laquelle le `CollectionView` s’étend autant que les éléments sont ajoutés.
- `SnapPointsAlignment`, de type `SnapPointsAlignment`, spécifie comment les points d’ancrage sont alignées avec les éléments.
- `SnapPointsType`, de type `SnapPointsType`, spécifie le comportement des points d’ancrage lors du défilement.

Ces propriétés sont secondées par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données. Pour plus d’informations sur les points d’ancrage, consultez [points d’alignement](scrolling.md#snap-points) dans le [défiler un élément dans l’affichage](scrolling.md) guide.

Le `ItemsLayoutOrientation` énumération définit les membres suivants :

- `Vertical` Indique que le `CollectionView` étendre verticalement comme les éléments sont ajoutés.
- `Horizontal` Indique que le `CollectionView` se développe horizontalement, comme les éléments sont ajoutés.

Le `ListItemsLayout` classe hérite de la `ItemsLayout` classe et définit statique `VerticalList` et `HorizontalList` membres. Ces membres peuvent être utilisés pour créer des listes verticales ou horizontales, respectivement. Vous pouvez également un `ListItemsLayout` objet peut être créé, en spécifiant un `ItemsLayoutOrientation` membre d’énumération en tant qu’argument.

Le `GridItemsLayout` classe hérite de la `ItemsLayout` classe et définit un `Span` propriété, de type `int`, qui représente le nombre de colonnes ou lignes à afficher dans la grille. La valeur par défaut de la `Span` propriété est 1, et sa valeur doit toujours être supérieure ou égale à 1.

> [!NOTE]
> `CollectionView` utilise les moteurs de présentation natif pour effectuer la mise en page.

## <a name="vertical-list"></a>Liste verticale

Par défaut, `CollectionView` affiche ses éléments dans une disposition Liste verticale. Par conséquent, il n’est pas nécessaire de définir le `ItemsLayout` propriété à utiliser cette disposition :

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
</CollectionView>
```

Toutefois, par souci d’exhaustivité, un `CollectionView` peut être définie pour afficher ses éléments dans une liste verticale en définissant son `ItemsLayout` à la méthode statique `ListItemsLayout.VerticalList` membre :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

Ou bien, pouvez également le faire en définissant le `ItemsLayout` propriété vers un objet de la `ListItemsLayout` classe, en spécifiant le `Vertical` `ItemsLayoutOrientation` membre d’énumération en tant qu’argument :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.VerticalList
};
```

Il en résulte dans une liste de colonne unique, qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés :

[![Capture d’écran d’une disposition de liste verticale CollectionView, sur iOS et Android](layout-images/vertical-list.png "mise en page de la liste verticale CollectionView")](layout-images/vertical-list-large.png#lightbox "mise en page de la liste verticale CollectionView")

## <a name="horizontal-list"></a>Liste horizontale

`CollectionView` peut afficher ses éléments dans une liste horizontale en définissant son `ItemsLayout` propriété statique `ListItemsLayout.HorizontalList` membre :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.HorizontalList}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Ou bien, pouvez également le faire en définissant le `ItemsLayout` propriété à un `ListItemsLayout` de l’objet, en spécifiant le `Horizontal` `ItemsLayoutOrientation` membre d’énumération en tant qu’argument :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Horizontal</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.HorizontalList
};
```

Il en résulte dans une liste de ligne unique, ce qui se développe horizontalement comme de nouveaux éléments sont ajoutés :

[![Capture d’écran d’une disposition de liste horizontale CollectionView, sur iOS et Android](layout-images/horizontal-list.png "mise en page de liste horizontale CollectionView")](layout-images/horizontal-list-large.png#lightbox "mise en page de liste horizontale CollectionView")

## <a name="vertical-grid"></a>Grille verticale

`CollectionView` peut afficher ses éléments dans une grille verticale en définissant son `ItemsLayout` propriété à un `GridItemsLayout` de l’objet dont la propriété `Orientation` propriété est définie sur `Vertical`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

Par défaut, un vertical `GridItemsLayout` affichera les éléments dans une seule colonne. Toutefois, cet exemple définit le `GridItemsLayout.Span` propriété à 2. Il en résulte dans une grille à deux colonnes, qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés :

[![Capture d’écran d’une disposition de grille verticale CollectionView, sur iOS et Android](layout-images/vertical-grid.png "mise en page de grille verticale CollectionView")](layout-images/vertical-grid-large.png#lightbox "mise en page de grille verticale CollectionView")

## <a name="horizontal-grid"></a>Grille horizontale

`CollectionView` peut afficher ses éléments dans une grille horizontale en définissant son `ItemsLayout` propriété à un `GridItemsLayout` de l’objet dont la propriété `Orientation` propriété est définie sur `Horizontal`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

Par défaut, un horizontal `GridItemsLayout` affichera les éléments dans une seule ligne. Toutefois, cet exemple définit le `GridItemsLayout.Span` propriété à 4. Il en résulte dans une grille de quatre lignes, ce qui se développe horizontalement comme de nouveaux éléments sont ajoutés :

[![Capture d’écran d’une disposition de grille horizontale CollectionView, sur iOS et Android](layout-images/horizontal-grid.png "mise en page de grille horizontale CollectionView")](layout-images/horizontal-grid-large.png#lightbox "mise en page de grille horizontale CollectionView")

## <a name="right-to-left-layout"></a>Disposition de droite à gauche

`CollectionView` pouvez disposer son contenu dans un sens de déroulement de droite à gauche en définissant son [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft). Toutefois, le `FlowDirection` propriété doit être définie dans l’idéal, sur une disposition de page ou racine, ce qui entraîne tous les éléments dans la page, ou la disposition de la racine, pour répondre à la direction du flux :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

La valeur par défaut [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) pour un élément avec un parent est [ `MatchParent` ](xref:Xamarin.Forms.FlowDirection.MatchParent). Par conséquent, le `CollectionView` hérite le `FlowDirection` valeur de propriété à partir de la [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), qui à son tour hérite le `FlowDirection` valeur de propriété à partir de la [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) . Il en résulte dans la disposition de droite à gauche indiquée dans les captures d’écran suivante :

[![Capture d’écran d’une disposition de droite à gauche de liste verticale CollectionView, sur iOS et Android](layout-images/vertical-list-rtl.png "disposition Liste verticale de droite à gauche de CollectionView")](layout-images/vertical-list-rtl-large.png#lightbox "vertical de droite à gauche CollectionView disposition de liste")

Pour plus d’informations sur la direction du flux, consultez [localisation de droite à gauche](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="item-sizing"></a>Dimensionnement de l’élément

Par défaut, chaque élément dans un `CollectionView` individuellement mesuré et en taille réelle, à condition que les éléments d’interface utilisateur dans le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) ne spécifiez des tailles fixes. Ce comportement, ce qui peut être modifié, est spécifié par le `CollectionView.ItemSizingStrategy` valeur de propriété. Cette valeur de propriété peut être définie à une de la `ItemSizingStrategy` membres de l’énumération :

- `MeasureAllItems` : chaque élément est mesurée individuellement. Valeur par défaut.
- `MeasureFirstItem` : seul le premier élément est mesuré, avec tous les articles suivants étant données la même taille que le premier élément.

> [!IMPORTANT]
> Le `MeasureFirstItem` stratégie de dimensionnement doit être utilisé dans les situations où la taille de l’élément est destinée à être uniforme sur tous les éléments et entraîne des performances accrues.

L’exemple de code suivant montre le paramètre le `ItemSizingStrategy` propriété :

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Localisation de droite à gauche](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Faire défiler un élément dans la vue](scrolling.md)
