---
title: Étiquette de Xamarin.Forms
description: Cet article explique comment utiliser la classe d’étiquette de Xamarin.Forms pour afficher le texte unique et plusieurs lignes dans les applications.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203034"
---
# <a name="xamarinforms-label"></a>Étiquette de Xamarin.Forms

_Texte affiché dans Xamarin.Forms_

Le [ `Label` ](xref:Xamarin.Forms.Label) vue est utilisée pour afficher du texte, à la fois unique et plusieurs lignes. Étiquettes peuvent avoir des polices personnalisées (familles de tailles et options) et le texte en couleur.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Troncation et habillage

Étiquettes peuvent être définies pour gérer le texte qui ne rentrent pas sur une seule ligne de plusieurs manières, exposées par le `LineBreakMode` propriété. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) est une énumération avec les valeurs suivantes :

- **HeadTruncation** &ndash; tronque la tête du texte, indiquant la fin.
- **CharacterWrap** &ndash; encapsule le texte sur une nouvelle ligne à une limite de caractère.
- **MiddleTruncation** &ndash; affiche le début et la fin du texte, avec l’opération de remplacement intermédiaire par les points de suspension.
- **NoWrap** &ndash; n’encapsule pas de texte, afficher uniquement de texte peut tenir sur une seule ligne.
- **TailTruncation** &ndash; illustre le début du texte, la troncation de la fin.
- **WordWrap** &ndash; encapsule le texte à la limite de mot.

## <a name="fonts"></a>Polices

Pour plus d’informations sur la spécification des polices dans une `Label`, consultez [polices](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="colors"></a>Couleurs

`Label`s peut être définie pour utiliser une couleur de texte personnalisé via la peut être liée [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) propriété.

Une attention particulière est nécessaire pour garantir que les couleurs seront utilisables sur chaque plateforme. Étant donné que chaque plateforme a des valeurs par défaut différentes pour les couleurs de texte et d’arrière-plan, vous devrez veiller à choisir une valeur par défaut qui fonctionne sur chacun.

Utilisez le XAML suivant pour définir la couleur du texte d’une étiquette :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

Le code c# équivalent est :

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Les captures d’écran suivantes affichent le résultat du paramètre de la `TextColor` propriété :

![](label-images/textcolor.png "Exemple d’étiquette TextColor")

Pour plus d’informations sur les couleurs, consultez [couleurs](~/xamarin-forms/user-interface/colors.md).

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Texte mis en forme

Étiquettes exposent un [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) propriété qui permet à la présentation du texte avec plusieurs polices et couleurs dans la même vue.

Le `FormattedText` propriété est de type [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), qui comprend un ou plusieurs [ `Span` ](xref:Xamarin.Forms.Span) instances, définis par le biais du [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) propriété . Chaque `Span` possède les propriétés suivantes :

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – la couleur d’arrière-plan span.
- [`Font`](xref:Xamarin.Forms.Span.Font) – la police du texte dans l’étendue.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – les attributs de police du texte dans l’étendue.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – la famille de polices à laquelle appartient la police du texte dans l’étendue.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – la taille de la police du texte dans l’étendue.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – la couleur du texte dans l’étendue. Cette propriété est obsolète et a été remplacée par le `TextColor` propriété.
- [`Style`](xref:Xamarin.Forms.Span.Style) – le style à appliquer à l’étendue.
- [`Text`](xref:Xamarin.Forms.Span.Text) – le texte de l’étendue.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – la couleur du texte dans l’étendue.

L’exemple XAML suivant montre un `FormattedText` propriété se compose de trois `Span` instances :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Le code c# équivalent est :

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> Le [ `Text` ](xref:Xamarin.Forms.Span.Text) propriété d’un `Span` peut être définie via la liaison de données. Pour plus d’informations, consultez [une liaison de données](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Les captures d’écran suivantes affichent le résultat du paramètre de la `FormattedString` propriété à trois `Span` instances :

![](label-images/formattedtext.png "Exemple d’étiquette FormattedText")

## <a name="styling-a-label"></a>Appliquer un style d’une étiquette

Les sections précédentes couverts paramètre [ `Label` ](xref:Xamarin.Forms.Label) propriétés sur chaque instance. Toutefois, les ensembles de propriétés peuvent être regroupées dans un style qui est appliqué uniformément à une ou plusieurs vues. Cela peut augmenter la lisibilité du code et apporter des modifications de conception plus facile à implémenter. Pour plus d’informations, consultez [Styles](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Liens associés

- [Création d’applications mobiles avec Xamarin.Forms, chapitre 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API de l’étiquette](xref:Xamarin.Forms.Label)
