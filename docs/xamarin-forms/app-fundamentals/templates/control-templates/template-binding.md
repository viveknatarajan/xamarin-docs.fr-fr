---
title: Liaison à partir d’un modèle de contrôle de Xamarin.Forms
description: Liaisons de modèles permettent de lient des contrôles dans un modèle de contrôle aux données aux propriétés publiques, l’activation de valeurs de propriétés des contrôles dans le modèle de contrôle pour être facilement changée. Cet article montre comment utiliser des liaisons de modèles pour effectuer la liaison de données à partir d’un modèle de contrôle.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 99d798ce2c74da0cf7fa0d497128db628a12ead5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241579"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Liaison à partir d’un modèle de contrôle de Xamarin.Forms

_Liaisons de modèles permettent de lient des contrôles dans un modèle de contrôle aux données aux propriétés publiques, l’activation de valeurs de propriétés des contrôles dans le modèle de contrôle pour être facilement changée. Cet article montre comment utiliser des liaisons de modèles pour effectuer la liaison de données à partir d’un modèle de contrôle._

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) est utilisée pour lier la propriété d’un contrôle dans un modèle de contrôle à une propriété pouvant être liée sur le parent de la *cible* vue qui est propriétaire du modèle de contrôle. Par exemple, au lieu de définir le texte affiché par [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances à l’intérieur de la [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), vous pouvez utiliser une liaison de modèle pour lier le [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriété pour les propriétés pouvant être liées qui définissent le texte à afficher.

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) est similaire à un [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/), sauf que la *source* d’un `TemplateBinding` est toujours automatiquement définie sur le parent de la *cible* vue qui est propriétaire du modèle de contrôle. Toutefois, notez que l’utilisation un `TemplateBinding` en dehors d’un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) n’est pas pris en charge.

## <a name="creating-a-templatebinding-in-xaml"></a>Création d’un TemplateBinding en XAML

En XAML, un [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) est créé à l’aide de la [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) extension de balisage, comme illustré dans l’exemple de code suivant :

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Plutôt que de définir la [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriétés en texte statique, les propriétés peuvent utiliser des liaisons de modèles pour lier à des propriétés pouvant être liées sur le parent de la *cible* vue qui est propriétaire du [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Toutefois, notez que les liaisons de modèles lient `Parent.HeaderText` et `Parent.FooterText`, plutôt que `HeaderText` et `FooterText`. Il s’agit comme dans cet exemple, les propriétés pouvant être liées sont définies sur le grand-parent de la *cible* afficher, plutôt que de la page parente, comme illustré dans l’exemple de code suivant :

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

Le *source* du modèle de liaison est automatiquement définie sur le parent de la *cible* vue qui est propriétaire du modèle de contrôle, qui est ici le [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) instance. Le modèle de liaison utilise le [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) propriété à retourner l’élément parent de la `ContentView` instance, qui est la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance. Par conséquent, à l’aide un [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) dans les [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) à lier à `Parent.HeaderText` et `Parent.FooterText` localise les propriétés pouvant être liées qui sont définies sur le `ContentPage`, en tant que illustrées dans l’exemple de code suivant :

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

Cela provoque l’affichage indiqué dans les captures d’écran suivants :

![](template-binding-images/teal-theme.png "Bleu-modèle de contrôle à l’aide de liaisons de modèles")

## <a name="creating-a-templatebinding-in-c35"></a>Création d’un TemplateBinding en C&#35;

En c#, un [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) est créé à l’aide de la `TemplateBinding` constructeur, comme illustré dans l’exemple de code suivant :

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

Plutôt que de définir la [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriétés en texte statique, les propriétés peuvent utiliser des liaisons de modèles pour lier à des propriétés pouvant être liées sur le parent de la *cible* vue qui est propriétaire du [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). La liaison de modèle est créée à l’aide de la [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) (méthode), en spécifiant un [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) instance comme second paramètre. Notez que les liaisons de modèles lier à `Parent.HeaderText` et `Parent.FooterText`, car les propriétés pouvant être liées sont définies sur le grand-parent de la *cible* afficher, plutôt que de la page parente, comme illustré dans l’exemple de code suivant :

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

Les propriétés pouvant être liées sont définies sur le `ContentPage`, comme indiqué précédemment.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Un BindableProperty de liaison à une propriété ViewModel

Comme indiqué plus haut, un [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) lie la propriété d’un contrôle dans un modèle de contrôle à une propriété pouvant être liée sur le parent de la *cible* vue qui est propriétaire du modèle de contrôle. À son tour, ces propriétés pouvant être liées peuvent être liées aux propriétés dans ViewModel.

L’exemple de code suivant définit deux propriétés sur un modèle de vues :

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

Le `HeaderText` et `FooterText` ViewModel propriétés peuvent être liées à, comme indiqué dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

Le `HeaderText` et `FooterText` propriétés pouvant être liées sont liées à la `HomePageViewModel.HeaderText` et `HomePageViewModel.FooterText` propriétés, en raison du paramètre de la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) à une instance de la `HomePageViewModel` classe. En général, cela entraîne des propriétés du contrôle dans le [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) liée à [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instances sur le [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), qui à son tour lier à Propriétés du modèle de vues.

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

Pour plus d’informations sur la liaison de données à ViewModel, consultez [à partir des liaisons de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Récapitulatif

Cet article est présenté à l’aide de liaisons de modèles pour effectuer la liaison de données à partir d’un modèle de contrôle. Liaisons de modèles permettent de lient des contrôles dans un modèle de contrôle aux données aux propriétés publiques, l’activation de valeurs de propriétés des contrôles dans le modèle de contrôle pour être facilement changée.



## <a name="related-links"></a>Liens associés

- [Principes fondamentaux de liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [À partir des liaisons de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Thème simple avec la liaison de modèle (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Thème simple avec la liaison de modèle et ViewModel (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
