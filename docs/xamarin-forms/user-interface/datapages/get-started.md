---
title: "Prise en main de pages de données"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 79ce4a1393b2affade3422f73e1c3d4680d199df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="getting-started-with-datapages"></a>Prise en main de pages de données

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!IMPORTANT]
> Pages de données requiert un [Xamarin.Forms thème](~/xamarin-forms/user-interface/themes/index.md) référence à restituer.


Pour commencer la création d’une page de lecteur de données simple à l’aide de l’aperçu de pages de données, suivez les étapes ci-dessous. Ce utilise démonstration un style codé en dur (« événements ») dans l’aperçu de builds qui fonctionne uniquement avec le format JSON spécifique dans le code.

[ ![](get-started-images/demo-sml.png "Exemple de pages de données d’Application")](get-started-images/demo.png "exemple des pages de données d’Application")

## <a name="1-add-nuget-packages"></a>1. Ajouter des Packages NuGet

Ajoutez ces packages Nuget pour vos projets d’application et de Xamarin.Forms PCL :

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Une implémentation de thème Nuget (par exemple). Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Ajouter une référence de thème

Dans le **App.xaml** , ajoutez une personnalisée `xmlns:mytheme` pour le thème et assurez-vous que le thème est fusionné dans le dictionnaire de ressources de l’application :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

**IMPORTANT :** vous devez également suivre les étapes permettant de [charger des assemblys du thème (ci-dessous)](#loadtheme) en ajoutant du code réutilisable pour les e/s `AppDelegate` et Android `MainActivity`. Cela améliorera en version préliminaire de futures.


## <a name="3-add-a-xaml-page"></a>3. Ajouter une Page XAML

Ajouter une nouvelle page XAML à l’application de Xamarin.Forms, et *modifier la classe de base* de `ContentPage` à `Xamarin.Forms.Pages.ListDataPage`. Cela doit être fait dans c# et le code XAML :

**Fichier c#**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**Fichier XAML**

Outre la modification de l’élément racine pour `<p:ListDataPage>` l’espace de noms personnalisé `xmlns:p` doivent également être ajoutés :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Sous-classe de l’application**

Modifier la `App` constructeur de classe afin que la `MainPage` est définie sur une `NavigationPage` contenant la nouvelle `SessionDataPage`. Une page de navigation *doit* être utilisé.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Ajouter la source de données

Supprimer le `Content` élément et remplacez-le par un `p:ListDataPage.DataSource` pour remplir la page avec des données. Fichier de données est chargé dans l’exemple ci-dessous Json à distance à partir d’une URL.

**Remarque :** l’aperçu *requiert* un `StyleClass` attribut pour fournir des indications de rendu pour la source de données. Le `StyleClass="Events"` fait référence à une disposition qui est prédéfinie dans la version préliminaire et contient des styles de *codé en dur* pour correspondre à la source de données JSON utilisée.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**Données JSON**

Un exemple des données JSON à partir de la [source demo](http://demo3143189.mockable.io/sessions) est indiqué ci-dessous :

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Exécutez !

Les étapes ci-dessus doivent entraîner une page de données de travail :

[ ![](get-started-images/demo-sml.png "Exemple de pages de données d’Application")](get-started-images/demo.png "exemple des pages de données d’Application")

Cela fonctionne parce que le style avant génération **« Événements »** existe dans le package Nuget de thème clair et a les styles définis qui correspondent à la source de données (par exemple). "title", "image", "presenter").

Les « événements » `StyleClass` est conçu pour afficher le `ListDataPage` contrôle personnalisé `CardView` contrôle qui est défini dans Xamarin.Forms.Pages. Le `CardView` contrôle possède trois propriétés : `ImageSource`, `Text`, et `Detail`. Le thème est codé en dur pour lier trois champs la source de données (à partir du fichier JSON) à ces propriétés pour l’affichage.

## <a name="5-customize"></a>5. Personnaliser

Le style hérité peut être remplacé par la spécification d’un modèle et à l’aide de liaisons de sources de données. Le code XAML suivant déclare un modèle personnalisé pour chaque ligne à l’aide de la nouvelle `ListItemControl` et `{p:DataSourceBinding}` syntaxe qui est inclus dans le **Xamarin.Forms.Pages** Nuget :

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

En fournissant un `DataTemplate` ce code substitue la `StyleClass` et utilise à la place de la disposition par défaut pour un `ListItemControl`.

[ ![](get-started-images/custom-sml.png "Exemple de pages de données d’Application")](get-started-images/custom.png "exemple des pages de données d’Application")

Les développeurs qui préfèrent c# pour XAML peuvent créer des données de source de liaisons trop (n’oubliez pas d’inclure un `using Xamarin.Forms.Pages;` instruction) :

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Il est un peu plus de travail pour créer des thèmes à partir de zéro (consultez la [guide de thèmes](~/xamarin-forms/user-interface/themes/index.md)) mais la version préliminaire de futures versions Cela rendra plus faciles à effectuer.


## <a name="troubleshooting"></a>Résolution des problèmes

<a name="loadtheme"/>

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Impossible de charger fichier ou l’assembly 'Xamarin.Forms.Theme.Light' ou une de ses dépendances

Dans la version préliminaire, thèmes n’est peut-être pas en mesure de charger lors de l’exécution. Ajoutez le code indiqué ci-dessous dans les projets pour corriger cette erreur.

**iOS**

Dans le **AppDelegate.cs** ajoutez les lignes suivantes après `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

Dans le **MainActivity.cs** ajoutez les lignes suivantes après `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>Liens associés

- [Exemple de DataPagesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
