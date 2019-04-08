---
title: Afficher un EmptyView lorsque les données sont non disponible
description: CollectionView, une vue vide peut être spécifiée qui fournit des commentaires à l’utilisateur lorsque aucune donnée n’est disponible pour l’affichage. La vue vide peut être une chaîne, une vue ou plusieurs vues.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: a430387bba83887045e5687c99d9295d4be373e4
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329978"
---
# <a name="display-an-emptyview-when-data-is-unavailable"></a>Afficher un EmptyView lorsque les données sont non disponible

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version d’évaluation et ne dispose pas de certaines de ses fonctionnalités planifiée. En outre, l’API peut changer que l’implémentation est terminée.

`CollectionView` définit les propriétés suivantes qui peuvent être utilisées pour fournir des commentaires utilisateur lorsqu’il n’existe aucune donnée à afficher :

- `EmptyView`, de type `object`, la chaîne, la liaison ou la vue qui sera affiché lorsque la `ItemsSource` propriété est `null`, ou lorsque la collection spécifiée par la `ItemsSource` propriété est `null` ou vide. La valeur par défaut est `null`.
- `EmptyViewTemplate`, de type [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), le modèle à utiliser pour formater le texte spécifié `EmptyView`. La valeur par défaut est `null`.

Ces propriétés sont secondées par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données.

Les principaux scénarios d’utilisation pour le paramètre de la `EmptyView` propriété Affichez les commentaires des utilisateurs lorsqu’une opération de filtrage sur une `CollectionView` génère sans données et affichage des commentaires des utilisateurs tandis que les données sont récupérées à partir d’un service web.

> [!NOTE]
> Le `EmptyView` propriété peut être définie sur une vue qui inclut le contenu interactif si nécessaire.

Pour plus d’informations sur les modèles de données, consultez [Modèles de données Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Affiche une chaîne lorsque les données ne sont pas disponibles

Le `EmptyView` propriété peut être définie sur une chaîne, qui sera affiché lorsque la `ItemsSource` propriété est `null`, ou lorsque la collection spécifiée par la `ItemsSource` propriété est `null` ou vide. Le XAML suivant montre un exemple de ce scénario :

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

Le résultat est que, étant donné que les données liées de collection est `null`, la chaîne définie en tant que le `EmptyView` valeur de propriété est affichée :

[![Capture d’écran de liste verticale CollectionView avec une vue vide de texte, sur iOS et Android](emptyview-images/null-itemssource.png "CollectionView de liste verticale avec affichage de texte vide")](emptyview-images/null-itemssource-large.png#lightbox "CollectionView de liste verticale avec texte vide Vue")

## <a name="display-views-when-data-is-unavailable"></a>Afficher les vues lorsque les données ne sont pas disponibles

Le `EmptyView` propriété peut être définie à une vue, qui sera affiché lorsque la `ItemsSource` propriété est `null`, ou lorsque la collection spécifiée par la `ItemsSource` propriété est `null` ou vide. Cela peut être une vue unique, ou une vue qui contient plusieurs vues enfants. L’exemple XAML suivant montre le `EmptyView` propriété définie sur une vue qui contient plusieurs vues enfants :

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

Le code C# équivalent est :

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Lorsque le [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) exécute le `FilterCommand`, la collection affichée par le `CollectionView` est filtré pour le terme de recherche stockées dans le [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) propriété. Si l’opération de filtrage ne renvoie aucune donnée, le [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) définir en tant que le `EmptyView` valeur de propriété est affichée :

[![Capture d’écran de liste verticale CollectionView avec vue vide personnalisée, sur iOS et Android](emptyview-images/filter-multiple-views.png "CollectionView de liste verticale avec vue vide personnalisée")](emptyview-images/filter-multiple-views-large.png#lightbox "CollectionView de liste verticale personnalisé vue vide")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Afficher un type personnalisé basé sur un modèle lorsque les données ne sont pas disponibles

Le `EmptyView` propriété peut être définie à un type personnalisé, dont le modèle est affiché lorsque le `ItemsSource` propriété est `null`, ou lorsque la collection spécifiée par la `ItemsSource` propriété est `null` ou vide. Le `EmptyViewTemplate` propriété peut être définie sur une [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) qui définit l’apparence de la `EmptyView`. Le XAML suivant montre un exemple de ce scénario :

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

Le code C# équivalent est :

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

Le `FilterData` type définit un `Filter` propriété et une [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty):

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

Le `EmptyView` propriété est définie sur une `FilterData` objet et le `Filter` lie les données de propriété pour le [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) propriété. Lorsque le [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) exécute le `FilterCommand`, la collection affichée par le `CollectionView` est filtré pour le terme de recherche stockées dans le `Filter` propriété. Si l’opération de filtrage ne renvoie aucune donnée, le [ `Label` ](xref:Xamarin.Forms.Label) définies dans le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), qui est défini comme le `EmptyViewTemplate` valeur de propriété, s’affiche :

[![Capture d’écran de liste verticale CollectionView avec un modèle de vue vide, sur iOS et Android](emptyview-images/emptyviewtemplate.png "CollectionView de liste verticale avec un modèle de vue vide")](emptyview-images/emptyviewtemplate-large.png#lightbox "CollectionView de liste verticale avec modèle de vue vide")

> [!NOTE]
> Lors de l’affichage d’un type personnalisé basé sur un modèle lorsque les données ne sont pas disponibles, le `EmptyViewTemplate` propriété peut être définie sur une vue qui contient plusieurs vues enfants.

## <a name="choose-an-emptyview-at-runtime"></a>Choisissez un EmptyView lors de l’exécution

Les vues qui apparaîtront en tant qu’un `EmptyView` lorsque les données ne sont pas disponibles, peut être défini comme [ `ContentView` ](xref:Xamarin.Forms.ContentView) des objets dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Le `EmptyView` propriété peut ensuite être définie sur un spécifique `ContentView`, selon une logique métier, lors de l’exécution. L’exemple XAML suivant montre un exemple de ce scénario :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

Ce XAML définit deux [ `ContentView` ](xref:Xamarin.Forms.ContentView) objets de niveau de la page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), avec le [ `Switch` ](xref:Xamarin.Forms.Switch) objet qui contrôle `ContentView` objet doit être défini comme le `EmptyView` valeur de propriété. Lorsque le [ `Switch` ](xref:Xamarin.Forms.Switch) est activé ou désactivé, le `OnEmptyViewSwitchToggled` Gestionnaire d’événements s’exécute le `ToggleEmptyView` méthode :

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

Le `ToggleEmptyView` méthode indique le `EmptyView` propriété de la `collectionView` objet à un des deux [ `ContentView` ](xref:Xamarin.Forms.ContentView) objets stockés dans le [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), en fonction de la valeur de la [ `Switch.IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) propriété. Lorsque le [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) exécute le `FilterCommand`, la collection affichée par le `CollectionView` est filtré pour le terme de recherche stockées dans le [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) propriété. Si l’opération de filtrage ne renvoie aucune donnée, le `ContentView` objet défini en tant que le `EmptyView` propriété s’affiche :

[![Capture d’écran d’une liste verticale CollectionView avec des vues vides échangées, sur iOS et Android](emptyview-images/swap.png "CollectionView de liste verticale avec des vues vides échangées")](emptyview-images/swap-large.png#lightbox "CollectionView de liste verticale avec échanger des vues vides")

Pour plus d’informations sur les dictionnaires de ressources, consultez [Xamarin.Forms les dictionnaires de ressources](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Modèles de données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Dictionnaires de ressources de Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
