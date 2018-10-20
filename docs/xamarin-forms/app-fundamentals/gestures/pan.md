---
title: Ajout d’un module de reconnaissance de mouvement panoramique
description: Cet article explique comment utiliser un mouvement panoramique à horizontalement et verticalement panoramique une image, afin que tout le contenu d’image puissent être affichées quand il est affiché dans une fenêtre d’affichage plus petite que les dimensions de l’image.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 59e9f4c61bda86faa5a55d70ef91411adb14da6d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38996804"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Ajout d’un module de reconnaissance de mouvement panoramique

_Le mouvement panoramique est utilisé pour détecter le mouvement des doigts sur l’écran et en appliquant ces déplacements au contenu et est implémenté avec les `PanGestureRecognizer` classe. Un scénario courant pour le mouvement panoramique est horizontalement et verticalement panoramique une image, afin que tout le contenu d’image puissent être affichées quand il est affiché dans une fenêtre d’affichage plus petite que les dimensions de l’image. Cela est accompli en déplaçant l’image dans la fenêtre d’affichage et est illustrée dans cet article._

Pour rendre un élément d’interface utilisateur pouvant être déplacé avec le mouvement panoramique, créer un [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) d’une instance, de gérer le [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) événement, et ajoutez la reconnaissance de mouvement de nouveau à la [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) collection sur l’élément d’interface utilisateur. Le code suivant montre l’exemple un `PanGestureRecognizer` attaché à un [ `Image` ](xref:Xamarin.Forms.Image) élément :

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Cela est également possible dans XAML, comme indiqué dans l’exemple de code suivant :

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Le code pour le `OnPanUpdated` Gestionnaire d’événements est ensuite ajouté au fichier code-behind :

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Panoramique correct sur Android requiert la [le package NuGet Xamarin.Forms 2.1.0-pre1](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) au minimum.

## <a name="creating-a-pan-container"></a>Création d’un conteneur de panoramique

Cette section contient une classe généralisée d’assistance qui effectue un panoramique forme libre, qui est généralement adaptée à la navigation dans les images ou des mappages. Gérer le mouvement panoramique pour effectuer cette opération nécessite quelques formules mathématiques pour transformer l’interface utilisateur. Cette mathématique est utilisé pour effectuer un panoramique uniquement dans les limites de l’élément d’interface utilisateur encapsulé. L’exemple de code suivant illustre la classe `PanContainer` :

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Cette classe peut être encapsulée autour d’un élément d’interface utilisateur afin que le mouvement s’effectuer un panoramique de l’élément d’interface utilisateur encapsulé. L’exemple de code XAML suivant montre le `PanContainer` encapsulant une [ `Image` ](xref:Xamarin.Forms.Image) élément :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Le code suivant montre l’exemple comment la `PanContainer` encapsule un [ `Image` ](xref:Xamarin.Forms.Image) élément dans une page c# :

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

Dans les deux exemples, le [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) et [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) propriétés sont définies sur les valeurs de largeur et la hauteur de l’image qui est affichée.

Lorsque le [ `Image` ](xref:Xamarin.Forms.Image) élément reçoit un mouvement panoramique, l’image affichée sera être panoramique. Le défilement est réalisé par le `PanContainer.OnPanUpdated` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Cette méthode met à jour le contenu visible de l’élément d’interface utilisateur encapsulé, selon le mouvement panoramique de l’utilisateur. Pour cela, en utilisant les valeurs de la [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) et [ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) propriétés de la [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs) instance pour laquelle calculer la direction et distance du pan. Le `App.ScreenWidth` et `App.ScreenHeight` propriétés fournissent la hauteur et la largeur de la fenêtre d’affichage et sont définies à la largeur d’écran et les valeurs de hauteur d’écran de l’appareil par les projets spécifiques à la plateforme respectifs. L’élément de l’utilisateur encapsulé est panoramique puis en définissant son [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) et [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) propriétés pour les valeurs calculées.

Lors de l’affichage panoramique contenu dans un élément qui n’occupe pas de mode plein écran, la hauteur et la largeur de la fenêtre d’affichage peuvent être obtenus à partir de l’élément [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) et [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) propriétés.

> [!NOTE]
> Affichage des images haute résolution permettre augmenter considérablement l’encombrement de mémoire de l’application. Par conséquent, ils doivent uniquement être créés si nécessaire et doivent être libérées dès que l’application n’exige plus leur intervention. Pour plus d’informations, consultez [Optimiser les ressources d’images](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="related-links"></a>Liens associés

- [PanGesture (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
