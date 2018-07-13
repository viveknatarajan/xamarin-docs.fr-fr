---
title: Liaison à partir d’un ControlTemplate de Xamarin.Forms
description: Liaisons de modèles permettent de lient des contrôles dans un modèle de contrôle aux données aux propriétés publiques, permettant des valeurs de propriété sur les contrôles dans le modèle de contrôle pour être facilement modifié. Cet article montre comment utiliser des liaisons de modèles pour effectuer la liaison de données à partir d’un modèle de contrôle.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 86de2ad6009365b3debbe1a2310651002b023219
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995562"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Liaison à partir d’un ControlTemplate de Xamarin.Forms

_Liaisons de modèles permettent de lient des contrôles dans un modèle de contrôle aux données aux propriétés publiques, permettant des valeurs de propriété sur les contrôles dans le modèle de contrôle pour être facilement modifié. Cet article montre comment utiliser des liaisons de modèles pour effectuer la liaison de données à partir d’un modèle de contrôle._

Un [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) est utilisé pour lier la propriété d’un contrôle dans un modèle de contrôle à une propriété peut être liée sur le parent de la *cible* vue propriétaire du modèle de contrôle. Par exemple, au lieu de définir le texte affiché par [ `Label` ](xref:Xamarin.Forms.Label) instances à l’intérieur de la [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate), vous pouvez utiliser une liaison de modèle pour lier le [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) propriété aux propriétés pouvant être liées qui définissent le texte à afficher.

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) est similaire à une [ `Binding` ](xref:Xamarin.Forms.Binding), sauf que le *source* d’un `TemplateBinding` est toujours automatiquement définie sur le parent de la *cible* vue propriétaire du modèle de contrôle. Toutefois, notez que l’utilisation d’un `TemplateBinding` en dehors d’un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) n’est pas pris en charge.

## <a name="creating-a-templatebinding-in-xaml"></a>Création d’un objet TemplateBinding dans XAML

Dans XAML, un [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) est créé à l’aide de la [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) extension de balisage, comme illustré dans l’exemple de code suivant :

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

Au lieu de définir la [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) propriétés en texte statique, les propriétés peuvent utiliser des liaisons de modèles à lier à des propriétés pouvant être liées sur le parent de la *cible* vue qui possède le [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Toutefois, notez que les liaisons de modèles lient `Parent.HeaderText` et `Parent.FooterText`, plutôt que `HeaderText` et `FooterText`. Il s’agit, car dans cet exemple, les propriétés pouvant être liées sont définies sur le grand-parent de la *cible* afficher, au lieu de la page parente, comme illustré dans l’exemple de code suivant :

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

Le *source* du modèle de liaison est automatiquement définie sur le parent de la *cible* vue propriétaire du modèle de contrôle, qui est ici la [ `ContentView` ](xref:Xamarin.Forms.ContentView) instance. Le modèle de liaison utilise le [ `Parent` ](xref:Xamarin.Forms.Element.Parent) propriété pour retourner l’élément parent de la `ContentView` instance, ce qui est le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance. Par conséquent, à l’aide un [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) dans le [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) à lier à `Parent.HeaderText` et `Parent.FooterText` localise les propriétés pouvant être liées qui sont définies sur le `ContentPage`, en tant que illustrées dans l’exemple de code suivant :

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

Cela provoque l’affichage indiqué dans les captures d’écran suivante :

![](template-binding-images/teal-theme.png "Modèle de contrôle en bleu-vert à l’aide de liaisons de modèles")

## <a name="creating-a-templatebinding-in-c35"></a>Création d’un objet TemplateBinding en C&#35;

En c#, un [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) est créé à l’aide de la `TemplateBinding` constructeur, comme illustré dans l’exemple de code suivant :

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

Au lieu de définir la [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) propriétés en texte statique, les propriétés peuvent utiliser des liaisons de modèles à lier à des propriétés pouvant être liées sur le parent de la *cible* vue qui possède le [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). La liaison de modèle est créée à l’aide de la [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) (méthode), en spécifiant un [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) instance comme second paramètre. Notez que les liaisons de modèles de lier à `Parent.HeaderText` et `Parent.FooterText`, car les propriétés pouvant être liées sont définies sur le grand-parent de la *cible* afficher, au lieu de la page parente, comme illustré dans l’exemple de code suivant :

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

Comme indiqué précédemment, un [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) lie la propriété d’un contrôle dans un modèle de contrôle à une propriété peut être liée sur le parent de la *cible* vue propriétaire du modèle de contrôle. À son tour, ces propriétés pouvant être liées peuvent être liées aux propriétés dans ViewModels.

L’exemple de code suivant définit deux propriétés sur un ViewModel :

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

Le `HeaderText` et `FooterText` propriétés ViewModel peuvent être liées à, comme indiqué dans l’exemple de code XAML suivant :

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

Le `HeaderText` et `FooterText` propriétés pouvant être liées sont liées à la `HomePageViewModel.HeaderText` et `HomePageViewModel.FooterText` propriétés, en raison du paramètre le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) à une instance de la `HomePageViewModel` classe. En général, il en résulte des propriétés de contrôle dans le [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) lié aux [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instances sur le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), qui à son tour de lier à Propriétés du ViewModel.

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

Pour plus d’informations sur la liaison de données pour les ViewModels, consultez [à partir des liaisons de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Récapitulatif

Cet article, présenté à l’aide de liaisons de modèles pour effectuer la liaison de données à partir d’un modèle de contrôle. Liaisons de modèles permettent de lient des contrôles dans un modèle de contrôle aux données aux propriétés publiques, permettant des valeurs de propriété sur les contrôles dans le modèle de contrôle pour être facilement modifié.



## <a name="related-links"></a>Liens associés

- [Principes fondamentaux de liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Liaisons de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Thème simple avec la liaison de modèle (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Thème simple avec la liaison de modèle et le ViewModel (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
