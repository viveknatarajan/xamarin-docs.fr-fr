---
title: Styles explicites
description: Un style explicite est celle qui est appliquées de façon sélective aux contrôles en définissant leurs propriétés de Style.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 9f9e87ae0fd9d609cef56123e9052d85941bda51
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848185"
---
# <a name="explicit-styles"></a>Styles explicites

_Un style explicite est celle qui est appliquées de façon sélective aux contrôles en définissant leurs propriétés de Style._

## <a name="creating-an-explicit-style-in-xaml"></a>Création d’un Style explicite en XAML

Pour déclarer un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) au niveau de la page, un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) doit être ajouté à la page et ensuite un ou plusieurs `Style` déclarations peuvent être incluses dans le `ResourceDictionary`. A `Style` a *explicite* grâce à sa déclaration une `x:Key` attribut, ce qui lui donne une clé Description dans la `ResourceDictionary`. *Explicite* styles doivent ensuite être appliqués à des éléments visuels spécifiques en définissant leurs [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés.

Le code suivant montre d’exemple *explicite* styles déclarés en XAML dans une page `ResourceDictionary` et appliqué à la page [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Le [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) définit trois *explicite* styles sont appliqués à la page [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances. Chaque `Style` est utilisé pour afficher le texte dans une couleur différente, lors de la définition également à la police des options de mise en page de taille et horizontaux et verticaux. Chaque `Style` est appliqué à un autre `Label` en définissant son [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés à l’aide de la `StaticResource` extension de balisage. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

[![](explicit-images/explicit-styles.png "Exemple de Styles explicite")](explicit-images/explicit-styles-large.png#lightbox "exemple de Styles explicite")

En outre, la dernière [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) a un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) appliqué, mais remplace également le [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) propriété à un autre `Color`valeur.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Création d’un Style explicite sur le contrôle de niveau

Outre la création *explicite* styles au niveau de la page, ils peuvent également être créés au niveau du contrôle, comme indiqué dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Dans cet exemple, le *explicite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances sont affectés à la [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) collection de la [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) contrôle. Les styles peuvent ensuite être appliqués au contrôle et ses enfants.

Pour plus d’informations sur la création d’une application styles [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), consultez [Styles globaux](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Création d’un Style explicite en C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances peuvent être ajoutées à une page [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) collection en c#, en créant un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)et puis en ajoutant le `Style` instances à le `ResourceDictionary`, comme illustré dans le exemple de code suivant :

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Le constructeur définit trois *explicite* styles sont appliqués à la page [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances. Chaque *explicite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) est ajouté à la [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) à l’aide de la [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) (méthode), en spécifiant un `key` chaîne pour faire référence à la `Style` instance. Chaque `Style` est appliqué à un autre `Label` en définissant leurs [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés.

Toutefois, il n’existe aucun avantage un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ici. Au lieu de cela, [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances peuvent être assignées directement à la [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés des éléments visuels requis et le `ResourceDictionary` peut être supprimé, comme indiqué dans l’exemple suivant exemple de code :

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Le constructeur définit trois *explicite* styles sont appliqués à la page [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances. Chaque `Style` est utilisé pour afficher le texte dans une couleur différente, lors de la définition également à la police des options de mise en page de taille et horizontaux et verticaux. Chaque `Style` est appliqué à un autre `Label` en définissant son [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés. En outre, la dernière `Label` a un `Style` appliqué, mais remplace également le `TextColor` à une autre propriété `Color` valeur.

## <a name="summary"></a>Récapitulatif

A [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) a *explicite* grâce à sa déclaration un `x:Key` d’attribut et puis sélectivement son application aux contrôles en définissant leur [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés.



## <a name="related-links"></a>Liens associés

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styles de base (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Utilisation de Styles (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
