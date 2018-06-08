---
title: À l’aide de UrhoSharp dans Xamarin.Forms
description: UrhoSharp peut être utilisé pour ajouter des graphiques 3D à une application de visualisation avancée
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: fbe07b81c8818378c3f6c12e09ae74bca2d89543
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847483"
---
# <a name="using-urhosharp-in-xamarinforms"></a>À l’aide de UrhoSharp dans Xamarin.Forms

## <a name="what-is-urhosharp"></a>Qu’est UrhoSharp ?

[UrhoSharp](~/graphics-games/urhosharp/index.md) est un puissant moteur 3D pour les développeurs Xamarin et .NET. Le [introduction](~/graphics-games/urhosharp/introduction.md) explique en détail la bibliothèque UrhoSharp, et [ces notes](~/graphics-games/urhosharp/using.md) expliquent comment programmer les séquences et les actions.

UrhoSharp peut être utilisé pour restituer des graphiques dans les applications de Xamarin.Forms.
Cela [exemple](https://github.com/xamarin/urho-samples/tree/master/FormsSample) montre comment UrhoSharp peut être utilisé pour construire un graphique 3D interactif :

![](urhosharp-images/ios-animation.gif "Graphique d’interactif 3D UrhoSharp sur iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp un graphique interactif 3D sur Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Ajouter les packages Nuget de UrhoSharp

Avant d’utiliser UrhoSharp, les développeurs doivent ajouter le package Nuget de UrhoSharp à leur solution. Ce guide suppose un projet de Xamarin.Forms avec un iOS, Android et .NET Standard projet de bibliothèque. Tout le code sera écrit dans le projet de bibliothèque .NET Standard. mais UrhoSharp Nuget doivent être ajouté pour les projets iOS et Android trop.

Le package Nuget de UrhoSharp.Forms contient tous les objets nécessaires pour créer des objets de UrhoSharp. Le package nuget UrhoSharp.Forms inclut la `UrhoSurface` classe, qui est utilisé pour héberger le UrhoSharp dans Xamarin.Forms.
Pour commencer, cliquez sur le **Packages** dossier dans le projet de bibliothèque .NET Standard et sélectionnez **ajouter des Packages en cours...** . Entrez le terme de recherche **UrhoSharp.Forms**, sélectionnez **UrhoSharp pour Xamarin.Forms**, puis cliquez sur **ajouter un Package**.

[![](urhosharp-images/add-package-sml.png "Packages de boîte de dialogue Ajouter")](urhosharp-images/add-package.png#lightbox "Packages la boîte de dialogue Ajouter")

Le package NuGet de UrhoSharp.Forms vont être ajouté au projet :

![](urhosharp-images/packages.png "Dossier packages")

Répétez les étapes ci-dessus pour les projets spécifiques à la plateforme (par exemple, iOS et Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Procédure pas à pas : Ajout de UrhoSharp à une application de Xamarin.Forms

Les étapes suivantes décrivent le code dans l’exemple de Xamarin.Forms UrhoSharp :

1. [Créer une Page de formulaires Xamarin](#1)
2. [Ajouter le UrhoSurface](#2)
3. [Générer une Application Urho](#3)
4. [Ajoutez la classe de graphiques à l’UrhoSurface](#4)
5. [Interaction avec UrhoSharp](#5)

Notez que l’exemple utilise des fonctionnalités de c# 6 et ne peut pas compiler sur les versions antérieures de Visual Studio.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Créer une Page de formulaires Xamarin

Le code suivant montre une page Xamarin.Forms `UrhoPage` avant tout code lié Urho a été ajouté :

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. Ajouter le UrhoSurface

UrhoSharp peut être hébergé dans un `ContentPage` comme d’autres contrôles de Xamarin.Forms.
L’extrait de code ci-dessous montre un `UrhoSurface` ajouté à la page Xamarin.Forms :

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Créer une Application Urho

Reportez-vous à la `Charts` classe pour l’implémentation des graphiques 3D Urho utilisés dans cet exemple. La structure du code de base est indiqué ci-dessous - Notez que la classe implémente `Urho.Application` qui est différent de la `Xamarin.Forms.Application` classe qui est implémenté dans **App.cs**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

Le [UrhoSharp documentation](~/graphics-games/urhosharp/index.md) contient plus d’informations sur la création d’actions et les scènes 3D.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Ajoutez la classe de graphiques à l’UrhoSurface

Utilisez le `UrhoSurface.Show<T>` une méthode générique pour ajouter l’application Urho à la page Xamarin.Forms. L’extrait de code ci-dessous montre le code supplémentaire requis pour créer la `Charts` classe :

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Remarque : le `Show<T>` méthode est asynchrone et doit être appelé avec le `await` (mot clé).

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. Interaction avec UrhoSharp

L’exemple permet de barres du graphique sélectionné et modifiés. Le `Charts` classe expose le `Bars` et `SelectedBar` pour permettre cette interaction.

Chaque « barre » a un gestionnaire d’événements de sélection ajouté après le `Charts` classe a été restitué en effectuant une itération sur exposés `Bars` collection :

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Le Gestionnaire d’événements utilise la valeur de la Xamarin.Forms `Slider` contrôle pour ajuster la hauteur de la barre de donnée :

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Enfin, rattachez les deux `Slider` contrôle afin que lorsque leur valeur change la zone de dessin UrhoSharp soit affectée. Le premier curseur fait pivoter l’image de graphique 3D, et le deuxième curseur ajuste la hauteur de la barre sélectionnée.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Les animations à le [supérieur de la page](#) afficher l’exécution de l’exemple.

## <a name="summary"></a>Récapitulatif

Cette page affiche comment UrhoSharp peut être utilisé pour ajouter la visualisation des données 3D de Xamarin.Forms. Lecture la [UrhoSharp documentation](~/graphics-games/urhosharp/index.md) pour plus d’informations sur la façon de construire des scènes Urho qui peuvent être inclus dans les applications de Xamarin.Forms à l’aide de la méthode illustrée ci-dessus.


## <a name="related-links"></a>Liens associés

- [Exemple de graphiques (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
