---
title: Introduction aux Styles de Xamarin.Forms
description: Les styles permettent l’apparence des éléments visuels à personnaliser. Les styles sont définis pour un type spécifique et contiennent des valeurs pour les propriétés disponibles sur ce type.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 9e5fb0196b67e23a034856ee0dd7b9c7d4846b31
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971113"
---
# <a name="introduction-to-xamarinforms-styles"></a>Introduction aux Styles de Xamarin.Forms

_Les styles permettent l’apparence des éléments visuels à personnaliser. Les styles sont définis pour un type spécifique et contiennent des valeurs pour les propriétés disponibles sur ce type._

Les applications Xamarin.Forms contiennent souvent plusieurs contrôles qui ont une apparence identique. Par exemple, une application peut avoir plusieurs [ `Label` ](xref:Xamarin.Forms.Label) instances qui ont les mêmes options de police et les options de disposition, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
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

L’exemple de code suivant montre la page équivalente créée en C# :

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
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

Chaque [ `Label` ](xref:Xamarin.Forms.Label) instance a les valeurs de propriété identiques pour contrôler l’apparence du texte affiché par le `Label`. Cela donne l’affichage illustré dans les captures d’écran suivantes :

[![](introduction-images/no-styles.png "Étiquette d’apparence sans Styles")](introduction-images/no-styles-large.png#lightbox "apparence sans Styles de l’étiquette")

Définition de l’apparence de chaque contrôle individuel peut être répétitive et sujet aux erreurs. Au lieu de cela, vous pouvez créé un style qui définit l’apparence, puis appliquée pour les contrôles requis.

## <a name="create-a-style"></a>Créer un style

Le [ `Style` ](xref:Xamarin.Forms.Style) classe regroupe une collection de valeurs de propriété dans un objet qui peut ensuite être appliqué à plusieurs instances de l’élément visuel. Cela permet de réduire le balisage répétitif et permet une apparence des applications à modifier plus facilement.

Bien que les styles ont été conçus principalement pour les applications basées sur XAML, ils peuvent également être créés en c# :

- [`Style`](xref:Xamarin.Forms.Style) instances créées en XAML sont généralement définies dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) qui est affectée à la [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) collection d’un contrôle, page, ou à la [ `Resources` ](xref:Xamarin.Forms.Application.Resources) collecte de l’application.
- [`Style`](xref:Xamarin.Forms.Style) les instances créées en c# sont généralement définies dans la classe de la page, ou dans une classe qui est accessible dans le monde entier.

Le fait de choisir où définir [`Style`](xref:Xamarin.Forms.Style) impacte l’emplacement où il peut être utilisé :

- [`Style`](xref:Xamarin.Forms.Style) instances définies au niveau du contrôle peuvent uniquement être appliquées au contrôle et à ses enfants.
- [`Style`](xref:Xamarin.Forms.Style) instances définies au niveau de la page peuvent uniquement être appliquées à la page et à ses enfants.
- [`Style`](xref:Xamarin.Forms.Style) les instances définies au niveau de l’application peuvent être appliquées dans toute l’application.

Chaque [ `Style` ](xref:Xamarin.Forms.Style) instance contient une collection d’un ou plusieurs [ `Setter` ](xref:Xamarin.Forms.Setter) objets, avec chacun `Setter` ayant un [ `Property` ](xref:Xamarin.Forms.Setter.Property) et un [`Value`](xref:Xamarin.Forms.Setter.Value). Le `Property` est le nom de la propriété peut être liée de l’élément auquel le style est appliqué, et le `Value` est la valeur qui est appliquée à la propriété.

Chaque [ `Style` ](xref:Xamarin.Forms.Style) instance peut être *explicite*, ou *implicite*:

- Un *explicite* [ `Style` ](xref:Xamarin.Forms.Style) instance est définie en spécifiant un [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) et un `x:Key` valeur et en configurant le paramètre del’élémentcible[ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) propriété le `x:Key` référence. Pour plus d’informations sur *explicite* styles, consultez [Styles explicites](~/xamarin-forms/user-interface/styles/explicit.md).
- Un *implicite* [ `Style` ](xref:Xamarin.Forms.Style) instance est définie en spécifiant uniquement un [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType). Le `Style` instance est automatiquement appliquée à tous les éléments de ce type. Remarque que les sous-classes de la `TargetType` n’ont pas automatiquement le `Style` appliqué. Pour plus d’informations sur *implicite* styles, consultez [Styles implicites](~/xamarin-forms/user-interface/styles/implicit.md).

Lorsque vous créez un [ `Style` ](xref:Xamarin.Forms.Style), le [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) propriété est toujours requise. Le code suivant montre l’exemple un *explicite* style (Notez le `x:Key`) créé dans XAML :

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Pour appliquer un `Style`, l’objet cible doit être un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) qui correspond à la [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) valeur de propriété de la `Style`, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Styles de niveau inférieurs dans la hiérarchie d’affichage sont prioritaires sur ceux définis plus haut. Par exemple, si un [ `Style` ](xref:Xamarin.Forms.Style) qui définit [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) à `Red` au niveau de l’application niveau sera remplacé par un style de niveau page définit `Label.TextColor` à `Green`. De même, un style de niveau page sera remplacé par un style de niveau de contrôle. En outre, si `Label.TextColor` est définie directement sur une propriété de contrôle, il est prioritaire sur tous les styles.

Les articles de cette section montrent et expliquent comment créer et appliquer *explicite* et *implicite* définit le style, la création de styles globaux, de style de l’héritage, comment répondre aux modifications de style lors de l’exécution, et comment utiliser les styles intégrés inclus dans Xamarin.Forms.

> [!NOTE]
> **Qu’est StyleId ?**
>
> Avant de Xamarin.Forms 2.2, le [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) propriété a été utilisée pour identifier des éléments individuels dans une application pour l’identification dans l’interface utilisateur de test et dans des moteurs de thème comme Pixate. Toutefois, 2.2 de Xamarin.Forms a introduit le [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) propriété, qui a remplacé le [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) propriété.

## <a name="related-links"></a>Liens connexes

- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
