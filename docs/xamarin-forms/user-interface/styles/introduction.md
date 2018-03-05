---
title: Introduction aux Styles
description: "Les styles permettent l’apparence des éléments visuels à personnaliser. Les styles sont définis pour un type spécifique et contient des valeurs pour les propriétés disponibles sur ce type."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 6b4d63637eae7c1719f3f6e525327d416ddff59d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-styles"></a>Introduction aux Styles

_Les styles permettent l’apparence des éléments visuels à personnaliser. Les styles sont définis pour un type spécifique et contient des valeurs pour les propriétés disponibles sur ce type._

Xamarin.Forms applications contiennent souvent plusieurs contrôles qui ont une apparence identiques. Par exemple, une application peut avoir plusieurs [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances qui ont les mêmes options de police et les options de mise en page, comme indiqué dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

L’exemple de code suivant montre la page équivalente créée en c# :

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Chaque [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance possède des valeurs de propriété identiques pour contrôler l’apparence du texte affiché par le `Label`. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

[![](introduction-images/no-styles.png "Étiquette d’apparence sans Styles")](introduction-images/no-styles-large.png "apparence sans style d’étiquette")

Définir l’apparence de chaque contrôle individuel peut être répétitive et sujet aux erreurs. Au lieu de cela, vous pouvez créé un style qui définit l’apparence, puis appliquée pour les contrôles requis.

## <a name="creating-a-style"></a>Création d’un Style

Le [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) classe regroupe une collection de valeurs de propriété dans un objet qui peut ensuite être appliqué à plusieurs instances de l’élément visuel. Cela permet de réduire un balisage répétitif et permet une apparence des applications à modifier plus facilement.

Bien que les styles sont conçus principalement pour les applications basées sur XAML, ils peuvent également être créés en c# :

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances créées en XAML sont généralement définies dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) qui est assignée à la [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) collection d’un contrôle, page, ou à la [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) collecte de l’application.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances créées en c# sont généralement définies dans la classe de la page ou dans une classe qui peut être accessible globalement.

Choix de l’emplacement définir un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) impacts où il peut être utilisé :

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances définies au niveau du contrôle peuvent uniquement être appliqués au contrôle et à ses enfants.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instances définies au niveau de la page peuvent uniquement être appliqués à la page et à ses enfants.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) les instances définies au niveau de l’application peuvent être appliquées dans l’ensemble de l’application.

Chaque [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance contient une collection d’un ou plusieurs [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) avec chacun des objets, `Setter` ayant une [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) et un [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). Le `Property` est le nom de la propriété pouvant être lié de l’élément style est appliqué, et le `Value` est la valeur qui est appliquée à la propriété.

Chaque [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance peut être *explicite*, ou *implicite*:

- Un *explicite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance est définie en spécifiant un [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) et un `x:Key` de valeur et en définissant l’élément cible [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété le `x:Key` référence. Pour plus d’informations sur *explicite* styles, consultez [explicite Styles](~/xamarin-forms/user-interface/styles/explicit.md).
- Un *implicite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance est définie en spécifiant uniquement un [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/). Le `Style` instance est ensuite automatiquement appliquée à tous les éléments de ce type. Remarque que les sous-classes de la `TargetType` n’avez pas automatiquement le `Style` appliqué. Pour plus d’informations sur *implicite* styles, consultez [Styles implicites](~/xamarin-forms/user-interface/styles/implicit.md).

Lorsque vous créez un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), le [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) propriété est toujours requise. Le code suivant montre d’exemple une *explicite* style (Remarque la `x:Key`) créées en XAML :

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Pour appliquer un `Style`, l’objet cible doit être un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) qui correspond à la [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) valeur de propriété de la `Style`, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Styles de niveau inférieurs dans la hiérarchie sont prioritaires sur ceux définis plus haut. Par exemple, si un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) qui définit [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) à `Red` au niveau de l’application au niveau sera remplacé par un style de niveau page définit `Label.TextColor` à `Green`. De même, un style de niveau page sera remplacé par un style de niveau de contrôle. En outre, si `Label.TextColor` est défini directement sur une propriété de contrôle, il prévaut sur tous les styles.

Les articles de cette section illustrent et expliquent comment créer et appliquer *explicite* et *implicite* styles, comment créer des styles globaux, appliquer un style d’héritage, comment répondre aux modifications de style lors de l’exécution, et comment utiliser les styles intégrés inclus dans Xamarin.Forms.

> [!NOTE]
> **Qu’est StyleId ?**
>
> Avant de Xamarin.Forms 2.2, le [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) propriété a été utilisée pour identifier des éléments individuels dans une application d’identification dans le test de l’interface utilisateur et dans les moteurs de thème comme Pixate. Toutefois, Xamarin.Forms 2.2 a introduit le [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) propriété, qui a remplacé le [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) propriété. Pour plus d’informations, consultez [Xamarin.Forms d’automatiser les tests avec Xamarin.UITest et Test Cloud](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Récapitulatif

Xamarin.Forms applications contiennent souvent plusieurs contrôles qui ont une apparence identiques. Définir l’apparence de chaque contrôle individuel peut être répétitive et sujet aux erreurs. Au lieu de cela, les styles peuvent être créés que personnaliser l’apparence du contrôle par les propriétés de regroupement et les paramètres disponibles sur le type de contrôle.


## <a name="related-links"></a>Liens associés

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
