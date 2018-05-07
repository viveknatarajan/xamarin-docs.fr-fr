---
title: Héritage de style
description: Styles peuvent hériter d’autres styles pour réduire la duplication et permettre la réutilisation.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: ffddc14145a3b187c03a6b2ba5ac6f8d6414468c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="style-inheritance"></a>Héritage de style

_Styles peuvent hériter d’autres styles pour réduire la duplication et permettre la réutilisation._

## <a name="style-inheritance-in-xaml"></a>Héritage de style en XAML

L’héritage de style est effectuée en affectant la [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) à une propriété [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/). Dans XAML, cela est accompli en définissant le `BasedOn` propriété un `StaticResource` extension de balisage qui fait référence à un précédemment créé `Style`. Dans c#, cela est accompli en définissant le `BasedOn` propriété un `Style` instance.

Les styles qui héritent d’un style de base peuvent inclure [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) instances nouvelles propriétés, ou les utiliser pour remplacer les styles du style de base. En outre, les styles qui héritent d’un style de base doivent cibler le même type, ou un type qui dérive du type ciblé par le style de base. Par exemple, si la cible d’un style de base [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) instances, les styles sont basés sur le style de base peuvent cibler `View` instances ou les types dérivés de la `View` class, telle que [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) et [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instances.

Le code suivant montre comment *explicite* d’héritage de style dans une page XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Le `baseStyle` cibles [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) instances et définit les [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés. Le `baseStyle` n’est pas définie directement sur tous les contrôles. Au lieu de cela, `labelStyle` et `buttonStyle` héritent de celui-ci, définition des valeurs de propriété pouvant être liée supplémentaires. Le `labelStyle` et `buttonStyle` sont ensuite appliquées à la [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances et [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance, en définissant leurs [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Un style implicite peut être dérivé d’un style explicite, mais un style explicite ne peut pas être dérivé d’un style implicite.

### <a name="respecting-the-inheritance-chain"></a>En respectant la chaîne d’héritage

Un style peut uniquement hériter de styles au même niveau ou ci-dessus, dans la hiérarchie. Cela signifie que :

- Une ressource de niveau application ne peut hériter que d’autres ressources de niveau application.
- Une ressource de niveau page peut hériter des ressources de niveau application et d’autres ressources de niveau page.
- Une ressource de niveau de contrôle peut hériter d’autres ressources de niveau de contrôle des ressources de niveau application et des ressources de niveau page.

Cette chaîne d’héritage est illustrée dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Dans cet exemple, `labelStyle` et `buttonStyle` est contrôle des ressources de niveau, alors que `baseStyle` est une ressource de niveau page. Toutefois, pendant `labelStyle` et `buttonStyle` héritent de `baseStyle`, il n’est pas possible pour `baseStyle` hériter `labelStyle` ou `buttonStyle`, en raison de leurs emplacements respectifs dans la hiérarchie.

## <a name="style-inheritance-in-c35"></a>Héritage de style c&#35;

La page c# équivalente, où [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) attribué directement à des instances du [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés des contrôles requis, est illustré dans l’exemple de code suivant :

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value = Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

Le `baseStyle` cibles [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) instances et définit les [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés. Le `baseStyle` n’est pas définie directement sur tous les contrôles. Au lieu de cela, `labelStyle` et `buttonStyle` héritent de celui-ci, définition des valeurs de propriété pouvant être liée supplémentaires. Le `labelStyle` et `buttonStyle` sont ensuite appliquées à la [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances et [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance, en définissant leurs [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés.

## <a name="summary"></a>Récapitulatif

Styles peuvent hériter d’autres styles pour réduire la duplication et permettre la réutilisation. L’héritage de style est effectuée en affectant la [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) à une propriété [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/).


## <a name="related-links"></a>Liens associés

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styles de base (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Utilisation de Styles (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
