---
title: Étiquette de Xamarin.Forms
description: Cet article explique comment utiliser la classe d’étiquette de Xamarin.Forms pour afficher le texte unique et plusieurs lignes dans les applications.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995964"
---
# <a name="xamarinforms-label"></a>Étiquette de Xamarin.Forms

_Texte affiché dans Xamarin.Forms_

Le `Label` vue est utilisée pour afficher du texte, à la fois unique et plusieurs lignes. Étiquettes peuvent avoir des polices personnalisées (familles de tailles et options) et le texte en couleur. Cet article aborde les rubriques suivantes :

- **[Troncation et habillage](#Truncation_and_Wrapping)**  &ndash; troncation et options de gestion des situations où le texte ne peut pas tenir sur une seule ligne d’habillage.
- **[Police](#Font)**  &ndash; options de police.
- **[Couleur](#Color)**  &ndash; étiqueter des options de couleur de texte.
- **[Texte mis en forme](#Formatted_Text)**  &ndash; options d’affichage de texte avec plusieurs formats/styles inline.

## <a name="styling-label"></a>Étiquette de style

Les sections suivantes abordent les propriétés des paramètres de `Label` manuellement sur chaque instance. Notez que définit des propriétés peut être regroupée dans un style qui est appliqué uniformément à une ou plusieurs vues. Cela peut augmenter la lisibilité du code et apporter des modifications de conception plus facile à implémenter. Consultez [Styles](~/xamarin-forms/user-interface/text/styles.md) pour plus d’informations.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Troncation et habillage

Étiquettes peuvent être définies pour gérer le texte qui ne rentrent pas sur une seule ligne de plusieurs manières, exposées par le `LineBreakMode` propriété. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) est une énumération des options suivantes :

- **HeadTruncation** &ndash; tronque la tête du texte, indiquant la fin.
- **CharacterWrap** &ndash; encapsule le texte sur une nouvelle ligne à une limite de caractère.
- **MiddleTruncation** &ndash; affiche le début et la fin du texte, avec l’opération de remplacement intermédiaire par les points de suspension.
- **NoWrap** &ndash; n’encapsule pas de texte, afficher uniquement de texte peut tenir sur une seule ligne.
- **TailTruncation** &ndash; illustre le début du texte, la troncation de la fin.
- **WordWrap** &ndash; encapsule le texte à la limite de mot.

## <a name="font"></a>Police

Consultez [utilisation des polices](~/xamarin-forms/user-interface/text/fonts.md) pour plus d’informations.

## <a name="color"></a>Color

`Label`s peut être définie pour utiliser une couleur de texte personnalisé via la peut être liée `TextColor` propriété.

Une attention particulière est nécessaire pour garantir que les couleurs seront utilisables sur chaque plateforme. Étant donné que chaque plateforme a des valeurs par défaut différentes pour les couleurs de texte et d’arrière-plan, vous devrez veiller à choisir une valeur par défaut qui fonctionne sur chacun.

Utilisez le code suivant pour définir la couleur du texte d’une étiquette :

En code :

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

Dans XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Exemple d’étiquette TextColor")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Texte mis en forme

Étiquettes exposent un `FormattedText` propriété qui vous permet de présenter du texte avec plusieurs polices et couleurs dans la même vue.

Le `FormattedText` propriété est de type [ `FormattedString` ](xref:Xamarin.Forms.FormattedString). Chaînes mises en forme sont composées d’un ou plusieurs `Span`s, chacun d’eux avec les propriétés suivantes :

- **BackgroundColor** &ndash; peut être utilisée pour définir une couleur d’arrière-plan, par exemple pour obtenir un effet de surligneur.
- **Attributs de la police** &ndash; peut être défini sur gras, italique ou aucun.
- **FontFamily** &ndash; définit la police à utiliser.
- **FontSize** &ndash; définit la taille du texte.
- **ForegroundColor** &ndash; définit la couleur du texte.
- **Texte** &ndash; le texte qui sera présenté.

Le code c# suivant illustre une étiquette où le premier mot est en gras et le dernier mot est en rouge :

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

Dans XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Exemple d’étiquette FormattedText")


## <a name="related-links"></a>Liens associés

- [Création d’applications mobiles avec Xamarin.Forms, chapitre 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API de l’étiquette](xref:Xamarin.Forms.Label)
