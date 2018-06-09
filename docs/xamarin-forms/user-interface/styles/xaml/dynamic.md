---
title: Styles dynamiques dans Xamarin.Forms
description: Cet article explique comment une application Xamarin.Forms peut répondre aux modifications de style dynamiquement au moment de l’exécution à l’aide de ressources dynamiques.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 0f82e0cfde29921ea768000f17b93d04f8ad307e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245219"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Styles dynamiques dans Xamarin.Forms

_Styles de ne pas répondre aux modifications apportées aux propriétés et restent inchangées pendant la durée d’une application. Par exemple, après avoir attribué un Style à un élément visuel, si une des instances de l’accesseur Set est modifiée, supprimé, ou une nouvelle instance de méthode Setter ajouté, les modifications ne seront pas appliquées à l’élément visuel. Toutefois, les applications peuvent répondre aux modifications de style dynamiquement au moment de l’exécution à l’aide de ressources dynamiques._

Le `DynamicResource` extension de balisage est similaire à la `StaticResource` une extension de balisage dans que les deux utilisent une clé de dictionnaire pour extraire une valeur d’un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Toutefois, pendant la `StaticResource` effectue une recherche dans le dictionnaire unique, la `DynamicResource` conserve un lien vers la clé du dictionnaire. Par conséquent, si l’entrée de dictionnaire associée à la clé est remplacée, la modification est appliquée à l’élément visuel. Ainsi, les modifications du runtime style doit être faite dans une application.

L’exemple de code suivant montre *dynamique* styles dans une page XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Le [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) instances utiliser le `DynamicResource` extension de balisage pour faire référence à un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) nommé `searchBarStyle`, qui n’est pas défini dans le code XAML. Toutefois, étant donné que la [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés de la `SearchBar` instances sont définies à l’aide d’un `DynamicResource`, la clé de dictionnaire manquante ne produit pas une exception est levée.

Au lieu de cela, dans le fichier code-behind, le constructeur crée un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) entrée avec la clé `searchBarStyle`, comme illustré dans l’exemple de code suivant :

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

Lorsque le `OnButtonClicked` Gestionnaire d’événements est exécuté, `searchBarStyle` basculent entre `blueSearchBarStyle` et `greenSearchBarStyle`. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

[![](dynamic-images/dynamic-style-blue.png "Bleu Dynamic Style exemple")](dynamic-images/dynamic-style-blue-large.png#lightbox "bleu Dynamic Style exemple")
[![](dynamic-images/dynamic-style-green.png "vert Dynamic Style exemple") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Vert Dynamic Style exemple")

L’exemple de code suivant illustre la page équivalente dans c# :

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

En c#, le [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) instances utiliser le [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) méthode pour faire référence à `searchBarStyle`. Le `OnButtonClicked` code gestionnaire d’événements est identique à l’exemple de code XAML et lors de l’exécution, `searchBarStyle` basculent entre `blueSearchBarStyle` et `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Héritage de Style dynamique

Dériver un style à partir d’un style dynamique ne peut pas être obtenue à l’aide de la [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) propriété. Au lieu de cela, le [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) classe inclut la [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) propriété, qui peut être définie sur une clé de dictionnaire dont la valeur peut changer dynamiquement.

L’exemple de code suivant montre *dynamique* d’héritage de style dans une page XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Le [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) instances utiliser le `StaticResource` extension de balisage pour faire référence à un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) nommé `tealSearchBarStyle`. Cela `Style` définit des propriétés supplémentaires et utilise le [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) propriété pour faire référence à `searchBarStyle`. Le `DynamicResource` extension de balisage n’est pas nécessaire car `tealSearchBarStyle` ne change pas, à l’exception de la `Style` il dérive. Par conséquent, `tealSearchBarStyle` conserve un lien vers `searchBarStyle` et est modifié lorsque le style de base est modifiée.

Dans le fichier code-behind, le constructeur crée un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) entrée avec la clé `searchBarStyle`, selon l’exemple précédent qui a démontré styles dynamiques. Lorsque le `OnButtonClicked` Gestionnaire d’événements est exécuté, `searchBarStyle` basculent entre `blueSearchBarStyle` et `greenSearchBarStyle`. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

[![](dynamic-images/dynamic-style-inheritance-blue.png "Exemple d’héritage de Style dynamiques de bleu")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "bleu exemple d’héritage de Style dynamiques")
[![](dynamic-images/dynamic-style-inheritance-green.png "vert Dynamic Style Exemple d’héritage")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "vert Style dynamique un exemple d’héritage")

L’exemple de code suivant illustre la page équivalente dans c# :

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

Le `tealSearchBarStyle` est affectée directement à la [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété de la [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) instances. Cela `Style` définit des propriétés supplémentaires et utilise le [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) propriété pour faire référence à `searchBarStyle`. Le [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) méthode n’est pas requise ici car `tealSearchBarStyle` ne change pas, à l’exception de la `Style` il dérive. Par conséquent, `tealSearchBarStyle` conserve un lien vers `searchBarStyle` et est modifié lorsque le style de base est modifiée.

## <a name="summary"></a>Récapitulatif

Styles de ne pas répondre aux modifications apportées aux propriétés et restent inchangées pendant la durée d’une application. Toutefois, les applications peuvent répondre aux modifications de style dynamiquement au moment de l’exécution à l’aide de ressources dynamiques. En outre, *dynamique* styles peuvent être dérivés avec la [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) propriété.



## <a name="related-links"></a>Liens associés

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styles dynamiques (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Utilisation de Styles (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
