---
title: Le chargement XAML lors de l’exécution dans Xamarin.Forms
description: XAML peut être chargé et analysé lors de l’exécution avec les méthodes d’extension LoadFromXaml.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: ce8ba32a1a6a1f69033615558c7ebf15d41e70fe
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53814097"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Le chargement XAML lors de l’exécution dans Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)

Le [ `Xamarin.Forms.Xaml` ](xref:Xamarin.Forms.Xaml) espace de noms inclut deux [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) méthodes d’extension qui peuvent être utilisées pour charger et analyser le XAML lors de l’exécution.

## <a name="background"></a>Présentation

Quand une classe XAML de Xamarin.Forms est construite, le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) méthode est appelée indirectement. Cela se produit car le fichier code-behind pour un XAML de la classe appelle le `InitializeComponent` méthode à partir de son constructeur :

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Lorsque Visual Studio crée un projet contenant un fichier XAML, il analyse le fichier XAML pour générer un C# fichier de code (par exemple, **MainPage.xaml.g.cs**) qui contient la définition de la `InitializeComponent` méthode :

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

Le `InitializeComponent` les appels de méthode le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) méthode pour extraire le fichier XAML (ou son fichier binaire compilé) à partir de la bibliothèque .NET Standard. Après l’extraction, il initialise tous les objets définis dans le fichier XAML, les connecter ensemble dans des relations parent-enfant, attache les gestionnaires d’événements définis dans le code aux événements dans le fichier XAML et définit l’arborescence résultante des objets en tant que le contenu de la page.

## <a name="loading-xaml-at-runtime"></a>Le chargement XAML lors de l’exécution

Le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) sont des méthodes `public`et par conséquent, peut être appelée à partir d’applications Xamarin.Forms pour charger et analyser le XAML lors de l’exécution. Cela permet des scénarios tels que d’une application de téléchargement de XAML à partir d’un service web, la création de la vue requise à partir du XAML et de les afficher dans l’application.

> [!WARNING]
> Le chargement de XAML lors de l’exécution a un impact significatif sur les performances et doit généralement être évité.

L’exemple de code suivant montre une utilisation simple :

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

Dans cet exemple, un [ `Button` ](xref:Xamarin.Forms.Button) instance est créée, avec ses [ `Text` ](xref:Xamarin.Forms.Button.Text) valeur de propriété qui est définie à partir du XAML définis dans le `string`. Le `Button` est ensuite ajoutée à un [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) qui a été défini dans le XAML de la page.

> [!NOTE]
> Le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) méthodes d’extension autorisent un argument de type générique à être spécifié. Toutefois, il est rarement nécessaire de spécifier l’argument de type, car elle sera déduit du type de l’instance de son fonctionnement sur.

Le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) méthode peut être utilisée pour la majoration n’importe quel XAML, avec l’exemple de suivant gonfler un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) et en accédant à celui-ci :

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>L’accès aux éléments

Le chargement XAML lors de l’exécution avec le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) (méthode) n’autorise pas l’accès fortement typé pour les éléments XAML que vous avez spécifié des noms d’objets runtime (à l’aide de `x:Name`). Toutefois, ces éléments XAML peuvent être récupérées à l’aide de la [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) (méthode) et ensuite accessibles en fonction des besoins :

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

Dans cet exemple, le XAML pour un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) est agrandie. Ce XAML inclut un [ `Label` ](xref:Xamarin.Forms.Label) nommé `monkeyName`, qui est récupéré à l’aide de la [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) (méthode), avant qu’il ait [ `Text` ](xref:Xamarin.Forms.Label.Text) propriété est définie.

## <a name="related-links"></a>Liens connexes

- [LoadRuntimeXAML (exemple)](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)
