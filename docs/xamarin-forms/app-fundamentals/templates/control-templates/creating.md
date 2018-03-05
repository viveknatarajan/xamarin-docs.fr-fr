---
title: "Création d’un ControlTemplate"
description: "Modèles de contrôle peuvent être définies au niveau application ou au niveau de la page. Cet article montre comment créer et utiliser les modèles de contrôle."
ms.topic: article
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: e09ac502298296277d9264bcd18f1ce1cbbf0c55
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-controltemplate"></a>Création d’un ControlTemplate

_Modèles de contrôle peuvent être définies au niveau application ou au niveau de la page. Cet article montre comment créer et utiliser les modèles de contrôle._

## <a name="creating-a-controltemplate-in-xaml"></a>Création d’un modèle de contrôle en XAML

Pour définir un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) au niveau de l’application, un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) doit être ajouté à la `App` classe. Par défaut, toutes les applications Xamarin.Forms créées à partir d’un modèle utilisent le **application** classe pour implémenter le [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) sous-classe. Pour déclarer un `ControlTemplate` au niveau de l’application, dans l’application `ResourceDictionary` à l’aide de XAML, la valeur par défaut **application** classe doit être remplacé par un code XAML **application** classe et code-behind associé, en tant que indiqué dans l’exemple de code suivant :

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

Chaque [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance est créée en tant qu’objet réutilisable dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).  Cela est possible en donnant chaque déclaration unique `x:Key` attribut, qui lui fournit une clé Description dans la `ResourceDictionary`.

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

Ainsi que le paramètre de la [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) propriété, le code-behind doit également appeler le `InitializeComponent` méthode pour charger et analyser le XAML associé.

Ce qui suit montre l’exemple de code un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) appliquant la `TealTemplate` à la [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

Le `TealTemplate` est affectée à la [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) propriété à l’aide de la `StaticResource` extension de balisage. Le [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) est définie sur une [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) qui définit le contenu à afficher sur le [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Ce contenu est affiché par le [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) contenus dans le `TealTemplate`. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

![](creating-images/teal-theme.png "Modèle de contrôle bleu-vert")

### <a name="re-theming-an-application-at-runtime"></a>Une Application lors de l’exécution de RE-thèmes

En cliquant sur le **modifier le thème** bouton exécute le `OnButtonClicked` (méthode), qui est affichée dans l’exemple de code suivant :

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Cette méthode remplace actif [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance avec l’alternative `ControlTemplate` instance, ce qui entraîne la capture d’écran suivante :

![](creating-images/aqua-theme.png "Modèle de contrôle cyan")

> [!NOTE]
> **Remarque**: sur un `ContentPage`, le `Content` peut être assignée à la propriété et la `ControlTemplate` propriété peut également être définie. Lorsque cela se produit, si le `ControlTemplate` contient un `ContentPresenter` de l’instance, le contenu affecté à la `Content` propriété sera présentée par le `ContentPresenter` dans le `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Définition d’un modèle de contrôle avec un Style

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) peuvent également être appliquées via une [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) à développer davantage de capacité de thème. Cela peut être obtenue en créant un *implicite* ou *explicite* style de la vue cible dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)et en définissant le `ControlTemplate` propriété de la cible afficher dans le [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance. Le code suivant exemple illustre un *implicite* style qui a été ajouté à un niveau de l’application [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Étant donné que la [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance est *implicite*, elle sera appliquée à tous les `ContentView` instances dans l’application. Par conséquent, il n’est plus nécessaire de définir la [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) propriété, comme illustré dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Création d’un modèle de contrôle au niveau de la Page

Outre la création [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instances au niveau de l’application, ils peuvent également être créées au niveau de la page, comme indiqué dans l’exemple de code suivant :

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

Lors de l’ajout un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) au niveau de la page, un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) est ajouté à la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), puis le `ControlTemplate` instances sont inclus dans le `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Création d’un modèle de contrôle dans le C &#35;

Pour définir un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) au niveau de l’application, un `class` doit être créée qui représente le `ControlTemplate`. La classe doit dériver de la [disposition](~/xamarin-forms/user-interface/layouts/index.md) utilisé pour le modèle, comme indiqué dans l’exemple de code suivant :

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

Le `AquaTemplate` classe est identique à la `TealTemplate` classe, à ceci près que les différentes couleurs sont utilisées pour le [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) et [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) propriétés.

Ce qui suit montre l’exemple de code un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) appliquant la `TealTemplate` à la [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

Le [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instances sont créées en spécifiant le type des classes qui définissent les modèles de contrôle, dans le `ControlTemplate` constructeur.

Le [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) est définie sur une [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) qui définit le contenu à afficher sur le [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Ce contenu est affiché par le [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) contenus dans le `TealTemplate`. Le même mécanisme décrit précédemment est utilisé pour modifier le thème à l’exécution pour le `AquaTheme`.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer et utiliser les modèles de contrôle. Modèles de contrôle peuvent être définies au niveau application ou au niveau de la page.


## <a name="related-links"></a>Liens associés

- [Styles](~/xamarin-forms/user-interface/styles/index.md)
- [Thème simple (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
