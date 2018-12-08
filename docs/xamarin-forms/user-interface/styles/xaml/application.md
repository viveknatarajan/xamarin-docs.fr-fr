---
title: Styles globaux dans Xamarin.Forms
description: Styles peuvent être rendues disponibles dans le monde entier en les ajoutant au dictionnaire de ressources de l’application. Cela permet d’éviter la duplication des styles de pages ou des contrôles.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: b663a7f2a4a67a9c3e18ed474d9935227fe34294
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059559"
---
# <a name="global-styles-in-xamarinforms"></a>Styles globaux dans Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Styles peuvent être rendues disponibles dans le monde entier en les ajoutant au dictionnaire de ressources de l’application. Cela permet d’éviter la duplication des styles de pages ou des contrôles._

## <a name="creating-a-global-style-in-xaml"></a>Création d’un Style Global dans XAML

Par défaut, toutes les applications Xamarin.Forms créées à partir d’un modèle utilisent la **application** classe pour implémenter le [ `Application` ](xref:Xamarin.Forms.Application) sous-classe. Pour déclarer un [ `Style` ](xref:Xamarin.Forms.Style) au niveau de l’application, dans l’application [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) à l’aide de XAML, la valeur par défaut **application** classe doit être remplacée par un XAML **Application** classe et code-behind associé. Pour plus d’informations, consultez [utilisation de la classe App](~/xamarin-forms/app-fundamentals/application-class.md).

Le code suivant montre l’exemple un [ `Style` ](xref:Xamarin.Forms.Style) déclaré au niveau de l’application :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Cela [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) définit un seul *explicite* style, `buttonStyle`, qui servira à définir l’apparence de [ `Button` ](xref:Xamarin.Forms.Button) instances. Toutefois, les styles globaux peuvent être *explicite* ou *implicite*.

L’exemple de code suivant montre une application de page XAML le `buttonStyle` à la page [ `Button` ](xref:Xamarin.Forms.Button) instances :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Cela provoque l’affichage indiqué dans les captures d’écran suivante :

[![](application-images/application-styles-1.png "Exemple de Styles globaux")](application-images/application-styles-1-large.png#lightbox "exemple de Styles globaux")

Pour plus d’informations sur la création de styles dans une page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), consultez [Styles explicites](~/xamarin-forms/user-interface/styles/explicit.md) et [Styles implicites](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Substitution de Styles

Styles de niveau inférieurs dans la hiérarchie d’affichage sont prioritaires sur ceux définis plus haut. Par exemple, si un [ `Style` ](xref:Xamarin.Forms.Style) qui définit [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) à `Red` au niveau de l’application niveau sera remplacé par un style de niveau page définit `Button.TextColor` à `Green`. De même, un style de niveau page sera remplacé par un style de niveau de contrôle. En outre, si `Button.TextColor` est définie directement sur une propriété de contrôle, il sera prioritaire sur tous les styles. Cette priorité est illustrée dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

La version d’origine `buttonStyle`, définis au niveau de l’application, est remplacé par le `buttonStyle` instance définie au niveau de la page. En outre, le style de niveau page est remplacé par le niveau de contrôle `buttonStyle`. Par conséquent, le [ `Button` ](xref:Xamarin.Forms.Button) instances sont affichées avec le texte en bleu, comme indiqué dans les captures d’écran suivante :

[![](application-images/application-styles-2.png "Exemple de Styles de substitution")](application-images/application-styles-2-large.png#lightbox "exemple de Styles de substitution")

## <a name="creating-a-global-style-in-c35"></a>Création d’un Style Global en C&#35;

[`Style`](xref:Xamarin.Forms.Style) instances peuvent être ajoutées à l’application [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) collection en c# en créant un nouveau [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)et puis en ajoutant le `Style` instances pour le `ResourceDictionary`, en tant que indiqué dans l’exemple de code suivant :

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Le constructeur définit un seul *explicite* style pour l’application à [ `Button` ](xref:Xamarin.Forms.Button) instances tout au long de l’application. *Explicite* [ `Style` ](xref:Xamarin.Forms.Style) instances sont ajoutées à la [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) à l’aide de la [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) méthode, en spécifiant un `key`chaîne pour faire référence à la `Style` instance. Le `Style` instance peut ensuite être appliquée à tous les contrôles du type correct dans l’application. Toutefois, les styles globaux peuvent être *explicite* ou *implicite*.

L’exemple de code suivant montre un c# page appliquant le `buttonStyle` à la page [ `Button` ](xref:Xamarin.Forms.Button) instances :

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

Le `buttonStyle` est appliqué à la [ `Button` ](xref:Xamarin.Forms.Button) instances en définissant leurs [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) propriétés et contrôle l’apparence de la `Button` instances.

## <a name="summary"></a>Récapitulatif

Styles peuvent être rendues disponibles dans le monde entier en les ajoutant à l’application [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Cela permet d’éviter la duplication des styles de pages ou des contrôles.



## <a name="related-links"></a>Liens associés

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styles de base (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Utilisation des Styles (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
