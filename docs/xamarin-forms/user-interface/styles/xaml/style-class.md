---
title: Classes de Style de Xamarin.Forms
description: Classes de style de Xamarin.Forms permettent plusieurs styles à appliquer à un contrôle, sans avoir recours à l’héritage de style.
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: dd749a4a78adbab5317f1ae5ca6334caa009b9b3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277802"
---
# <a name="xamarinforms-style-classes"></a>Classes de Style de Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Classes de style de Xamarin.Forms permettent plusieurs styles à appliquer à un contrôle, sans avoir recours à l’héritage de style._

## <a name="create-style-classes"></a>Créer des classes de style

Une classe de style peut être créée en définissant le [ `Class` ](xref:Xamarin.Forms.Style.Class) propriété sur un [ `Style` ](xref:Xamarin.Forms.Style) à un `string` qui représente le nom de classe. L’avantage de cette offre, sur la définition d’un style explicite à l’aide de la `x:Key` d’attribut, est que plusieurs classes de style peuvent être appliqués à un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

> [!IMPORTANT]
> Plusieurs styles peuvent partager le même nom de classe, sous réserve qu’ils ciblent différents types. Ainsi, plusieurs classes de style identique, à différents types de cible.

L’exemple suivant illustre trois [ `BoxView` ](xref:Xamarin.Forms.BoxView) classes, de style et un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) classe de style :

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

Le `Separator`, `Rounded`, et `Circle` chaque jeu de classes de style [ `BoxView` ](xref:Xamarin.Forms.BoxView) propriétés aux valeurs spécifiques.

Le `Rotated` style classe a un [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) de [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), ce qui signifie qu’elle peut uniquement être appliqué à `VisualElement` instances. Toutefois, son [ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) propriété est définie sur `true`, ce qui garantit qu’il peut être appliqué à tous les contrôles qui dérivent de `VisualElement`, tel que [ `BoxView` ](xref:Xamarin.Forms.BoxView). Pour plus d’informations sur l’application d’un style à un type dérivé, consultez [appliquer un style aux types dérivés](implicit.md#apply-a-style-to-derived-types).

Le code C# équivalent est :

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>Consommer des classes de style

Classes de style peuvent être consommées en définissant le [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) propriété du contrôle, qui est de type `IList<string>`, à une liste des noms de classe de style. Les classes de style sont appliquées, fournie par le type du contrôle correspond à la [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) des classes de style.

L’exemple suivant illustre trois [ `BoxView` ](xref:Xamarin.Forms.BoxView) instances, la valeur des classes de style différent :

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

Dans cet exemple, la première [ `BoxView` ](xref:Xamarin.Forms.BoxView) est un style à un séparateur de ligne, lors de la troisième `BoxView` est circulaire. La seconde `BoxView` a deux classes de style appliqué, qui donnent les angles de l’informatique arrondi et faire pivoter de 45 degrés :

![](style-class-images/boxviews.png "BoxViews stylé avec les classes de style")

> [!IMPORTANT]
> Plusieurs classes de style peuvent être appliqués à un contrôle, car la [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) propriété est de type `IList<string>`. Lorsque cela se produit, les classes de style sont appliquées dans l’ordre croissant d’ordre de la liste. Par conséquent, lorsque plusieurs classes de style définissent des propriétés identiques, la propriété dans la classe de style qui se trouve dans la position la plus élevée de la liste sont prioritaires.

Le code C# équivalent est :

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>Liens connexes

- [Styles de base (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
