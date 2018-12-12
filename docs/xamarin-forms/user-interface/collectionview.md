---
title: CollectionView de Xamarin.Forms
description: Le CollectionView est une vue flexible et plus performant pour la présentation des listes de données à l’aide des spécifications de mise en page différente.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 46ab8169b31624e3862cf14f477bcd6cf8c8f3a3
ms.sourcegitcommit: 408b78dd6eded4696469e316af7922a5991f2211
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53246306"
---
# <a name="xamarinforms-collectionview"></a>CollectionView de Xamarin.Forms

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` est une vue pour la présentation des listes de données à l’aide de spécifications de mise en page différente. Il vise à fournir la plus flexible et alternative performante à `ListView`. Bien que le `CollectionView` et `ListView` API sont similaires, il existe des différences notables :

- `CollectionView` n’a aucun concept de cellules. Au lieu de cela, les modèles de données sont utilisés pour définir l’apparence de chaque élément de données dans la liste.
- `CollectionView` réduit la surface d’API de `ListView`. De nombreuses propriétés et événements de `ListView` ne sont pas présents dans `CollectionView`.
- `CollectionView` a un modèle de disposition flexible qui permet d’être présentés verticalement ou horizontalement, dans une liste ou d’une grille de données.

Un `CollectionView` est consommée en définissant son `ItemsSource` propriété à toute collection qui implémente `IEnumerable`et son `ItemTemplate` propriété un `DataTemplate` qui définit chaque apparence de l’élément de liste. En outre, sa `ItemsLayout` propriété doit être définie sur une `ItemsLayout` (classe), pour définir la spécification de mise en page pour obtenir la liste.

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version préliminaire et ne dispose pas d’une grande partie de sa fonctionnalité planifiée. En outre, l’API change l’implémentation est terminée.

`CollectionView` est disponible dans Xamarin.Forms 4.0-pre1. Toutefois, il est actuellement en phase expérimentale et peut uniquement être utilisé en ajoutant la ligne suivante de code à votre `AppDelegate` classe sur iOS ou à votre `MainActivity` classe sur Android, avant d’appeler `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

## <a name="populating-a-collectionview-with-data"></a>Remplissage d’un CollectionView de données

Un `CollectionView` est rempli avec des données en définissant son `ItemSource` propriété à toute collection qui implémente `IEnumerable`. Éléments peuvent être ajoutés dans XAML en initialisant le `ItemsSource` propriété à partir d’un tableau d’éléments :

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
> Notez que le `x:Array` élément requiert une `Type` attribut indiquant le type des éléments dans le tableau.

En outre, un `CollectionView` peut être rempli de données à l’aide de la liaison de données à lier son `ItemsSource` propriété un `IEnumerable` collection. Dans XAML, cela est possible avec le `Binding` extension de balisage :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Dans cet exemple, le `ItemsSource` lie les données de propriété pour le `Monkeys` propriété du modèle de vue connectée, qui retourne un `IList<Monkey>` collection. Le code suivant montre l’exemple le `Monkey` (classe), qui contient les quatre propriétés :

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

## <a name="providing-a-view-for-the-empty-state"></a>En fournissant une vue pour l’état vide

Lorsque le `CollectionView` ne contient aucune donnée à afficher, un message approprié peut être affiché à l’utilisateur. Cela s’effectue en définissant le `CollectionView.EmptyView` propriété à un `object` qui s’affichera lorsque la collection spécifiée par la `ItemSource` propriété est vide :

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}">
    <CollectionView.EmptyView>
        <Label Text="No items to display" />
    </CollectionView.EmptyView>
</CollectionView>
```

Le `EmptyView` propriété peut être définie sur une `string`, une liaison ou une vue et par conséquent, peuvent inclure du contenu interactif si nécessaire.

En outre, le `EmptyViewTemplate` propriété peut être définie sur une `DataTemplate` qui sera utilisé pour mettre en forme le `EmptyView`.

## <a name="defining-list-item-appearance"></a>Définir l’apparence de l’élément de liste

L’apparence des données pour chaque élément dans le `CollectionView` peut être définie en configurant le `CollectionView.ItemTemplate` propriété un `DataTemplate`:

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

Dans cet exemple, le `DataTemplate` contient un `Grid` avec un `Image`et deux `Label` instances, que tous les lient aux propriétés de la `Monkey` objet.

Pour plus d’informations sur les modèles de données, consultez [modèles de données](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="specifying-a-layout"></a>Spécification d’une disposition

Par défaut, un `CollectionView` affiche ses éléments dans une liste verticale. Toutefois, chacun des spécifications de disposition suivantes peut être utilisé :

- Liste verticale : une liste de colonne unique qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés.
- Liste horizontale : une liste de ligne unique qui se développe horizontalement comme de nouveaux éléments sont ajoutés.
- Quadrillage vertical – une grille de plusieurs colonne qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés.
- Grille horizontale – une grille à plusieurs ligne qui se développe horizontalement comme de nouveaux éléments sont ajoutés.

Ces mises en page peuvent être spécifiés en définissant le `CollectionView.ItemsLayout` propriété à un `ItemsLayout` (classe), avec la `ListItemsLayout` classe fournissant une disposition de la liste et le `GridItemsLayout` classe fournissant une disposition en grille.

Le `ItemsLayout` classe définit la `Orientation` propriété, de type `ItemsLayoutOrientation`. Le `ItemsLayoutOrientation` énumération définit `Vertical` et `Horizontal` les valeurs de membre.

Le `ListItemsLayout` classe hérite de la `ItemsLayout` classe et définit statique `VerticalList` et `HorizontalList` membres. Ces membres peuvent être utilisés pour créer des listes verticales ou horizontales, respectivement. Vous pouvez également un `ListItemsLayout` instance peut être créée en spécifiant un `ItemsLayoutOrientation` membre d’énumération en tant qu’argument.

Le `GridItemsLayout` classe hérite de la `ItemsLayout` classe et définit un `Span` propriété, de type `int`, qui représente le nombre de colonnes ou lignes à afficher dans la grille. La valeur par défaut de la `Span` propriété est 1, et sa valeur doit toujours être supérieure ou égale à 1.

### <a name="vertical-list"></a>Liste verticale

Par défaut, un `CollectionView` affiche ses éléments dans une disposition Liste verticale. Par conséquent, il n’est pas nécessaire de définir le `ItemsLayout` propriété à utiliser cette disposition :

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

Ou bien, pouvez également le faire en définissant le `ItemsLayout` propriété à une instance de la `ListItemsLayout` classe, en spécifiant le `Vertical` `ItemsLayoutOrientation` membre d’énumération en tant qu’argument :

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

Il en résulte dans une liste de colonne unique, qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés :

[![Disposition Liste verticale de CollectionView](collectionview-images/vertical-list.png "mise en page de la liste verticale CollectionView")](collectionview-images/vertical-list-large.png#lightbox)

### <a name="horizontal-list"></a>Liste horizontale

Un `CollectionView` peut afficher ses éléments dans une liste horizontale en définissant son `ItemsLayout` propriété statique `ListItemsLayout.HorizontalList` membre :

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

Ou bien, pouvez également le faire en définissant le `ItemsLayout` propriété à une instance de la `ListItemsLayout` classe, en spécifiant le `Horizontal` `ItemsLayoutOrientation` membre d’énumération en tant qu’argument :

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

Il en résulte dans une liste de ligne unique, ce qui se développe horizontalement comme de nouveaux éléments sont ajoutés :

[![Disposition de liste horizontale CollectionView](collectionview-images/horizontal-list.png "mise en page de liste horizontale CollectionView")](collectionview-images/horizontal-list-large.png#lightbox)

### <a name="vertical-grid"></a>Grille verticale

Un `CollectionView` peut afficher ses éléments dans une grille verticale en définissant son `ItemsLayout` propriété à un `GridItemsLayout` dont la propriété d’instance `Orientation` propriété est définie sur `Vertical`:

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

Par défaut, un vertical `GridItemsLayout` affichera les éléments dans une seule colonne. Toutefois, cet exemple définit le `GridItemsLayout.Span` propriété à 2. Il en résulte dans une grille à deux colonnes, qui s’agrandit verticalement comme de nouveaux éléments sont ajoutés :

[![Présentation de la grille verticale CollectionView](collectionview-images/vertical-grid.png "mise en page de grille verticale CollectionView")](collectionview-images/vertical-grid-large.png#lightbox)

### <a name="horizontal-grid"></a>Grille horizontale

Un `CollectionView` peut afficher ses éléments dans une grille horizontale en définissant son `ItemsLayout` propriété à un `GridItemsLayout` dont la propriété d’instance `Orientation` propriété est définie sur `Horizontal`:

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

Par défaut, un horizontal `GridItemsLayout` affichera les éléments dans une seule ligne. Toutefois, cet exemple définit le `GridItemsLayout.Span` propriété à 4. Il en résulte dans une grille de quatre lignes, ce qui se développe horizontalement comme de nouveaux éléments sont ajoutés :

[![Présentation de la grille horizontale CollectionView](collectionview-images/horizontal-grid.png "mise en page de grille horizontale CollectionView")](collectionview-images/horizontal-grid-large.png#lightbox)

## <a name="scrolling"></a>Le défilement

`CollectionView` peut défiler des éléments demandés dans l’affichage avec le `ScrollTo` (méthode). Cette méthode fait défiler l’élément à l’index spécifié dans la vue :

```csharp
_collectionView.ScrollTo(12);
```

Vous pouvez également une surcharge de cette méthode peut être utilisée pour faire défiler l’élément spécifié dans la vue :

```csharp
var viewModel = BindingContext as MonkeysViewModel;
var monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
_collectionView.ScrollTo(monkey);
```

Les deux surcharges permettent de spécifier qui indiquent le groupe dans l’élément, la position exacte de l’élément après que le défilement a terminé (démarrage), center, fin et si vous souhaitez animer le défilement des arguments supplémentaires.

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
