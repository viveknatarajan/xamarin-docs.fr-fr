---
title: Création d’un ControlTemplate
description: Modèles de contrôle peuvent être définis au niveau application ou au niveau de la page. Cet article montre comment créer et utiliser des modèles de contrôle.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998185"
---
# <a name="creating-a-controltemplate"></a>Création d’un ControlTemplate

_Modèles de contrôle peuvent être définis au niveau application ou au niveau de la page. Cet article montre comment créer et utiliser des modèles de contrôle._

## <a name="creating-a-controltemplate-in-xaml"></a>Création d’un ControlTemplate dans XAML

Pour définir un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) au niveau de l’application, un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) doit être ajouté à la `App` classe. Par défaut, toutes les applications Xamarin.Forms créées à partir d’un modèle utilisent la **application** classe pour implémenter le [ `Application` ](xref:Xamarin.Forms.Application) sous-classe. Pour déclarer un `ControlTemplate` au niveau de l’application, dans l’application `ResourceDictionary` à l’aide de XAML, la valeur par défaut **application** classe doit être remplacée par un XAML **application** classe et code-behind associé, en tant que indiqué dans l’exemple de code suivant :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Chaque [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instance est créée en tant qu’objet réutilisable dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary).  Cela est obtenue en donnant chaque déclaration unique `x:Key` attribut, ce qui lui fournit une clé descriptive dans la `ResourceDictionary`.

L’exemple de code suivant montre associé `App` code-behind :

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

Ainsi que le paramètre le [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) propriété, le code-behind doit également appeler le `InitializeComponent` méthode pour charger et analyser le XAML associé.

Le code suivant montre l’exemple un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) appliquant le `TealTemplate` à la [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

Le `TealTemplate` est affectée à la [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) propriété à l’aide de la `StaticResource` extension de balisage. Le [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) propriété est définie sur une [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) qui définit le contenu doit être affiché sur le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Ce contenu sera affiché par le [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) contenus dans le `TealTemplate`. Cela provoque l’affichage indiqué dans les captures d’écran suivante :

![](creating-images/teal-theme.png "Modèle de contrôle en bleu-vert")

### <a name="re-theming-an-application-at-runtime"></a>Une Application lors de l’exécution de RE-thèmes

En cliquant sur le **modifier le thème** bouton exécute le `OnButtonClicked` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Cette méthode remplace actif [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instance avec l’alternative `ControlTemplate` instance, ce qui entraîne la capture d’écran suivante :

![](creating-images/aqua-theme.png "Modèle de contrôle cyan")

> [!NOTE]
> Sur un `ContentPage`, le `Content` propriété peut être affectée et le `ControlTemplate` propriété peut également être définie. Lorsque cela se produit, si le `ControlTemplate` contient un `ContentPresenter` de l’instance, le contenu affecté à la `Content` propriété sera proposée par le `ContentPresenter` au sein de la `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Définition d’un ControlTemplate avec un Style

Un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) peuvent également être appliquées via une [ `Style` ](xref:Xamarin.Forms.Style) à développer davantage de capacité de thème. Cela peut être obtenue en créant un *implicite* ou *explicite* style pour la vue cible dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)et en définissant le `ControlTemplate` propriété de la cible afficher dans le [ `Style` ](xref:Xamarin.Forms.Style) instance. Le code suivant montre l’exemple un *implicite* style qui a été ajouté à un niveau de l’application [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Étant donné que le [ `Style` ](xref:Xamarin.Forms.Style) instance est *implicite*, elle sera appliquée à tous les `ContentView` instances dans l’application. Par conséquent, il n’est plus nécessaire de définir le [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) propriété, comme illustré dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Création d’un ControlTemplate au niveau de la Page

Outre la création de [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instances au niveau de l’application, ils peuvent également être créés au niveau de la page, comme indiqué dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

Lorsque vous ajoutez un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) au niveau de la page, un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) est ajouté à la [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), puis le `ControlTemplate` instances sont incluses dans le `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Création d’un ControlTemplate en C&#35;

Pour définir un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) au niveau de l’application, un `class` doit être créée qui représente le `ControlTemplate`. La classe doit dériver de la [disposition](~/xamarin-forms/user-interface/layouts/index.md) utilisé pour le modèle, comme illustré dans l’exemple de code suivant :

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

Le `AquaTemplate` classe est identique à la `TealTemplate` classe, à ceci près que différentes couleurs sont utilisées pour le [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) et [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) propriétés.

Le code suivant montre l’exemple un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) appliquant le `TealTemplate` à la [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

Le [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instances sont créées en spécifiant le type des classes qui définissent les modèles de contrôle, dans le `ControlTemplate` constructeur.

Le [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) propriété est définie sur une [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) qui définit le contenu doit être affiché sur le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Ce contenu sera affiché par le [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) contenus dans le `TealTemplate`. Le même mécanisme décrit précédemment est utilisé pour modifier le thème à l’exécution pour le `AquaTheme`.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer et utiliser des modèles de contrôle. Modèles de contrôle peuvent être définis au niveau application ou au niveau de la page.


## <a name="related-links"></a>Liens associés

- [Styles](~/xamarin-forms/user-interface/styles/index.md)
- [Thème simple (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
