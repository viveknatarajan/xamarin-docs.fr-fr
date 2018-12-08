---
title: Styles implicites dans Xamarin.Forms
description: Un style implicite est celui qui est utilisé par tous les contrôles de la même TargetType, sans nécessiter de chaque contrôle pour faire référence au style.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c30e6c0060407720e0324a3327607100db3960d6
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056147"
---
# <a name="implicit-styles-in-xamarinforms"></a>Styles implicites dans Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Un style implicite est celui qui est utilisé par tous les contrôles de la même TargetType, sans nécessiter de chaque contrôle pour faire référence au style._

## <a name="creating-an-implicit-style-in-xaml"></a>Création d’un Style implicite dans XAML

Pour déclarer un [ `Style` ](xref:Xamarin.Forms.Style) au niveau de la page, un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) doit être ajouté à la page et ensuite un ou plusieurs `Style` déclarations peuvent être incluses dans le `ResourceDictionary`. Un `Style` a *implicite* en ne spécifiant un `x:Key` attribut. Le style sera ensuite être appliqué à des éléments visuels qui correspondent à la `TargetType` exactement, mais pas aux éléments qui sont dérivés de la `TargetType` valeur.

Le code suivant montre l’exemple un *implicite* style déclaré dans XAML dans une page `ResourceDictionary`et appliquée à la page [ `Entry` ](xref:Xamarin.Forms.Entry) instances :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Le [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) définit un seul *implicite* style est appliqué à la page [ `Entry` ](xref:Xamarin.Forms.Entry) instances. Le `Style` est utilisé pour afficher le texte en bleu sur un arrière-plan jaune, lors de la définition également les autres options d’apparence. Le `Style` est ajoutée à la page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) sans spécifier un `x:Key` attribut. Par conséquent, le `Style` est appliquée à tous le `Entry` instances implicitement qu’ils correspondent le [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) propriété de la `Style` exactement. Toutefois, le `Style` n’est pas appliqué à la `CustomEntry` instance, ce qui est une sous-classe `Entry`. Cela provoque l’affichage indiqué dans les captures d’écran suivante :

[![](implicit-images/implicit-styles.png "Exemple de Styles implicites")](implicit-images/implicit-styles-large.png#lightbox "exemple de Styles implicites")

En outre, la quatrième [ `Entry` ](xref:Xamarin.Forms.Entry) remplace le [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) et [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) propriétés du style implicite à différents `Color`valeurs.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Création d’un Style implicite sur le contrôle de niveau

Outre la création de *implicite* styles au niveau de la page, ils peuvent également être créés au niveau du contrôle, comme indiqué dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Dans cet exemple, le *implicite* [ `Style` ](xref:Xamarin.Forms.Style) est affectée à la [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) collection de la [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)contrôle. Le *implicite* style peut ensuite être appliqué au contrôle et ses enfants.

Pour plus d’informations sur la création de styles dans une application [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), consultez [Styles globaux](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Création d’un Style implicite en C&#35;

[`Style`](xref:Xamarin.Forms.Style) instances peuvent être ajoutées à une page [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) collection en c# en créant un nouveau [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)et puis en ajoutant le `Style` instances pour le `ResourceDictionary`, comme illustré dans la exemple de code suivant :

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Le constructeur définit un seul *implicite* style est appliqué à la page [ `Entry` ](xref:Xamarin.Forms.Entry) instances. Le `Style` est utilisé pour afficher le texte en bleu sur un arrière-plan jaune, lors de la définition également les autres options d’apparence. Le `Style` est ajoutée à la page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) sans spécifier un `key` chaîne. Par conséquent, le `Style` est appliquée à tous le `Entry` instances implicitement qu’ils correspondent le [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) propriété de la `Style` exactement. Toutefois, le `Style` n’est pas appliqué à la `CustomEntry` instance, ce qui est une sous-classe `Entry`.

## <a name="summary"></a>Récapitulatif

Un *implicite* style est celui qui est utilisé par tous les éléments visuels du même [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), sans nécessiter de chaque contrôle pour faire référence au style. Un `Style` a *implicite* en ne spécifiant un `x:Key` attribut. Au lieu de cela, le `x:Key` attribut devient automatiquement la valeur de la [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) propriété.



## <a name="related-links"></a>Liens associés

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styles de base (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Utilisation des Styles (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
