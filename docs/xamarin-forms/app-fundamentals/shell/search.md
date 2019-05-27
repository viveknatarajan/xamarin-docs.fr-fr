---
title: Recherche dans Xamarin.Forms Shell
description: Les applications Xamarin.Forms Shell peuvent utiliser la fonctionnalité de recherche intégrée fournie par une zone de recherche qui est peut être ajoutée en haut de chaque page.
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: e97dbb993cb108245636ae459a572e18b13d6817
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005195"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Xamarin.Forms Shell inclut une fonctionnalité de recherche intégrée, fournie par la classe `SearchHandler`. Une fonction de recherche peut être ajoutée à une page en définissant la propriété jointe `Shell.SearchHandler` sur un objet `SearchHandler` sous-classé. Une zone de recherche est alors ajoutée en haut de la page :

[![Capture d’écran d’un élément SearchHandler Shell sur iOS et Android](search-images/searchhandler.png "SearchHandler Shell")](search-images/searchhandler-large.png#lightbox "SearchHandler Shell")

Lorsqu’une requête est entrée dans la zone de recherche, la zone des suggestions de recherche peut être remplie de données :

[![Capture d’écran des résultats d’une recherche dans un SearchHandler Shell sur iOS et Android](search-images/search-suggestions.png "Résultats d’une recherche dans un SearchHandler Shell")](search-images/search-suggestions-large.png#lightbox "Résultats d’une recherche dans un SearchHandler Shell")

Puis, lorsqu’un résultat de recherche est sélectionné, l’application peut réagir de façon appropriée, par exemple en accédant à une autre page.

## <a name="searchhandler-class"></a>Classe de SearchHandler

La classe `SearchHandler` définit les propriétés suivantes, qui contrôlent son apparence et son comportement :

- `ClearIcon`, de type [`ImageSource`](xref:Xamarin.Forms.ImageSource) : icône affichée pour effacer le contenu de la zone de recherche.
- `ClearIconHelpText`, de type `string` : texte d’aide accessible pour l’icône d’effacement.
- `ClearIconName`, de type `string` : nom de l’icône d’effacement pour une utilisation avec des lecteurs d’écran.
- `ClearPlaceholderCommand`, de type `ICommand` : exécutée lorsque vous activez l’option `ClearPlaceholderIcon`.
- `ClearPlaceholderCommandParameter`, de type `object` : paramètre passé à la commande `ClearPlaceholderCommand`.
- `ClearPlaceholderEnabled`, de type `bool` : détermine si la commande `ClearPlaceholderCommand` peut être exécutée. La valeur par défaut est `true`.
- `ClearPlaceholderHelpText`, de type `string` : texte d’aide accessible pour l’icône de l’espace réservé pour l’effacement.
- `ClearPlaceholderIcon`, de type [`ImageSource`](xref:Xamarin.Forms.ImageSource) : icône d’espace réservé pour l’effacement, affichée lorsque la zone de recherche est vide.
- `ClearPlaceholderName`, de type `string` : nom de l’icône de l’espace réservé pour l’effacement, à utiliser avec des lecteurs d’écran.
- `Command`, de type `ICommand` : exécutée lorsque la requête de recherche est confirmée.
- `CommandParameter`, de type `object` : paramètre passé à la commande `Command`.
- `DisplayMemberName`, de type `string` : représente le nom ou le chemin d’accès de la propriété, affiché pour chaque élément de données de la collection `ItemsSource`.
- `IsSearchEnabled`, de type `bool` : représente l’état activé de la zone de recherche. La valeur par défaut est `true`.
- `ItemsSource`, de type `IEnumerable` : spécifie la collection d’éléments à afficher dans la zone de suggestion, et dont la valeur par défaut est `null`.
- `ItemTemplate`, de type [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) : spécifie le modèle à appliquer à chaque élément de la collection d’éléments à afficher dans la zone de suggestion.
- `Placeholder`, de type `string` : texte à afficher lorsque la zone de recherche est vide.
- `Query`, de type `string` : texte saisi par l’utilisateur dans la zone de recherche.
- `QueryIcon`, de type [`ImageSource`](xref:Xamarin.Forms.ImageSource) : icône utilisée pour indiquer à l’utilisateur que la recherche est disponible.
- `QueryIconHelpText`, de type `string` : texte d’aide accessible pour l’icône de la requête.
- `QueryIconName`, de type `string` : nom de l’icône de la requête à utiliser avec des lecteurs d’écran.
- `SearchBoxVisibility`, de type `SearchBoxVisibility` : visibilité de la zone de recherche. Par défaut, la zone de recherche est visible et complètement développée.
- `SelectedItem`, de type `object` : élément sélectionné dans les résultats de la recherche. Cette propriété est en lecture seule et sa la valeur par défaut est `null`.
- `ShowsResults`, de type `bool` : indique si les résultats de la recherche doivent être attendus dans la zone de suggestion, lors de la saisie du texte. La valeur par défaut est `false`.

Toutes ces propriétés sont soutenues par des objets [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), ce qui signifie qu’elles peuvent être des cibles de liaisons de données.

En outre, la classe `SearchHandler` fournit les méthodes substituables suivantes :

- `OnClearPlaceholderClicked` : appelée chaque fois que l’option `ClearPlaceholderIcon` est activée.
- `OnItemSelected` : appelée chaque fois qu’un résultat de recherche est sélectionné par l’utilisateur.
- `OnQueryChanged` : appelée lorsque les propriétés `Query` changent.
- `OnQueryConfirmed` : appelée chaque fois que l’utilisateur appuie sur entrée ou confirme sa requête dans la zone de recherche.

Lorsqu’un utilisateur saisit une requête dans la zone de recherche, la propriété `Query` est mise à jour, et la méthode `OnQueryChanged` est exécutée à chaque mise à jour. Cette méthode peut être utilisée pour mettre à jour la zone de suggestions qui s’affiche sous la zone de recherche. Lorsqu’un utilisateur sélectionne un résultat dans la zone des suggestions, la méthode `OnItemSelected` est exécutée.

## <a name="create-a-searchhandler"></a>Créer un SearchHandler

Une fonctionnalité de recherche peut être ajoutée à une application Shell en sous-classant la classe `SearchHandler` et en remplaçant les méthodes `OnQueryChanged` et `OnItemSelected` :

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

La substitution `OnQueryChanged` a deux arguments : `oldValue`, qui contient la requête de recherche précédente, et `newValue`, qui contient la requête de recherche actuelle. La zone des suggestions de recherche peut être mise à jour en définissant la propriété `SearchHandler.ItemsSource` sur une collection `IEnumerable` contenant les éléments qui correspondent à la requête de recherche actuelle.

Lorsqu’un résultat de recherche est sélectionné par l’utilisateur, la substitution `OnItemSelected` est exécutée et la propriété `SelectedItem` est définie. Dans cet exemple, la méthode accède à une autre page qui affiche des données concernant l’élément `Animal` sélectionné. Pour plus d’informations sur la navigation, consultez [Navigation dans Xamarin.Forms Shell](navigation.md).

> [!NOTE]
> Des propriétés `SearchHandler` supplémentaires peuvent être définies pour contrôler l’apparence de la zone de recherche.

## <a name="consume-a-searchhandler"></a>Consommer un SearchHandler

L’élément `SearchHandler` sous-classé peut être consommé en définissant la propriété jointe `Shell.SearchHandler` sur un objet du type sous-classé :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Le code C# équivalent est :

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

La méthode `MonkeySearchHandler.OnQueryChanged` retourne un `List` des objets `Animal`. La propriété `DisplayMemberName` est définie sur la propriété `Name` de chaque objet `Animal`, et par conséquent, les données affichées dans la zone de suggestions correspondront à chaque nom d’animal.

La propriété `ShowsResults` est définie sur `true`, de sorte que les suggestions de recherche sont affichées lorsque l’utilisateur saisit une requête de recherche :

[![Capture d’écran des résultats de la recherche dans un SearchHandler Shell sur iOS et Android](search-images/search-results.png "Résultats de la recherche dans un SearchHandler Shell")](search-images/search-results-large.png#lightbox "Résultats de la recherche dans un SearchHandler Shell")

À chaque changement de la requête de recherche, la zone de suggestions est mise à jour :

[![Capture d’écran des résultats de la recherche dans un SearchHandler Shell sur iOS et Android](search-images/search-results-change.png "Résultats de la recherche dans un SearchHandler Shell")](search-images/search-results-change-large.png#lightbox "Résultats de la recherche dans un SearchHandler Shell")

Lorsqu’un résultat de recherche est sélectionné, la page `MonkeyDetailPage` s’affiche, et les données relatives au singe sélectionné apparaissent :

[![Capture d’écran des détails du singe sur iOS et Android](search-images/detailpage.png "Détails du singe")](search-images/detailpage-large.png#lightbox "Détails du singe")

## <a name="define-search-results-item-appearance"></a>Définir l’apparence d’un élément des résultats de la recherche

Outre l’affichage des données `string` dans les résultats de la recherche, l’apparence de chaque élément des résultats de la recherche peut être définie en configurant la propriété `SearchHandler.ItemTemplate` sur un [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Le code C# équivalent est :

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

Les éléments spécifiés dans le [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) définissent l’apparence de chaque élément dans la zone des suggestions. Dans cet exemple, la disposition du `DataTemplate` est gérée par un élément [`Grid`](xref:Xamarin.Forms.Grid). L’élément `Grid` contient un objet [`Image`](xref:Xamarin.Forms.Image) ainsi qu’un objet [`Label`](xref:Xamarin.Forms.Label), tous deux liés aux propriétés de chaque objet `Monkey`.

Les captures d’écran suivantes montrent le résultat de la création d’un modèle pour chaque élément de la zone de suggestions :

[![Capture d’écran d’un modèle de résultat de recherche dans un SearchHandler Shell sur iOS et Android](search-images/search-results-template.png "Modèle de résultat de recherche dans un SearchHandler Shell")](search-images/search-results-template-large.png#lightbox "Modèle de résultat de recherche dans un SearchHandler Shell")

Pour plus d’informations sur les modèles de données, consultez [Modèles de données Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="searchbox-visibility"></a>Visibilité de la zone de recherche

Lors de l’ajout d’une zone de recherche en haut d’une page, par défaut la zone de recherche est visible et complètement développée. Toutefois, vous pouvez changer ce comportement en définissant la propriété `SearchHandler.SearchBoxVisibility` sur un des membres d’énumération `SearchBoxVisibility` :

- `Hidden` : la zone de recherche n’est pas visible ou accessible.
- `Collapsible` : la zone de recherche est masquée jusqu’à ce que l’utilisateur effectue une action pour l’afficher.
- `Expanded` : la zone de recherche est visible et complètement développée.

L’exemple suivant montre comment masquer la zone de recherche :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="related-links"></a>Liens connexes

- [Xaminals (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Navigation dans Xamarin.Forms Shell](navigation.md)
