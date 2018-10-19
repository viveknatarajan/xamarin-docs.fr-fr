---
title: Ajout d’un module de reconnaissance de geste de pincement
description: Cet article explique comment utiliser le mouvement de pincement pour effectuer un zoom interactif d’une image à l’emplacement de pincement.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: f67cbb136c42a4bc476c1715ea6fd15255d71dc7
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38998697"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Ajout d’un module de reconnaissance de geste de pincement

_Le mouvement de pincement est utilisé pour effectuer le zoom interactif et est implémenté avec la classe PinchGestureRecognizer. Un scénario courant pour le mouvement de pincement consiste à effectuer un zoom interactif d’une image à l’emplacement de pincement. Cela s’effectue par le contenu de la fenêtre d’affichage de mise à l’échelle et est illustrée dans cet article._

Pour rendre un élément d’interface utilisateur zoomable avec le mouvement de pincement, créer un [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) d’une instance, de gérer le [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) événement, et ajoutez la reconnaissance de mouvement de nouveau à la [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) collection sur l’élément d’interface utilisateur. Le code suivant montre l’exemple un `PinchGestureRecognizer` attaché à un [ `Image` ](xref:Xamarin.Forms.Image) élément :

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Cela est également possible dans XAML, comme indiqué dans l’exemple de code suivant :

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Le code pour le `OnPinchUpdated` Gestionnaire d’événements est ensuite ajouté au fichier code-behind :

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Création d’un conteneur PinchToZoom

Gestion du mouvement de pincement pour effectuer une opération de zoom nécessite quelques formules mathématiques pour transformer l’interface utilisateur. Cette section contient une classe d’assistance généralisée pour effectuer les calculs, ce qui peuvent être utilisé pour le mode interactif Zoom sur n’importe quel élément d’interface utilisateur. L’exemple de code suivant illustre la classe `PinchToZoomContainer` :

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Cette classe peut être encapsulée autour d’un élément d’interface utilisateur afin que le geste de pincement applique un zoom avant l’élément d’interface utilisateur encapsulé. L’exemple de code XAML suivant montre le `PinchToZoomContainer` encapsulant une [ `Image` ](xref:Xamarin.Forms.Image) élément :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Le code suivant montre l’exemple comment la `PinchToZoomContainer` encapsule un [ `Image` ](xref:Xamarin.Forms.Image) élément dans une page c# :

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Lorsque le [ `Image` ](xref:Xamarin.Forms.Image) élément reçoit un mouvement de pincement, l’image affichée sera être agrandi ou réduit-in ou out. Le zoom est effectué par le `PinchZoomContainer.OnPinchUpdated` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Cette méthode met à jour le niveau de zoom de l’élément d’interface utilisateur encapsulé en fonction de mouvement de pincement de l’utilisateur. Pour cela, en utilisant les valeurs de la [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) et [ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) propriétés de la [ `PinchGestureUpdatedEventArgs` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) instance pour laquelle calculer le facteur d’échelle à appliquer à l’origine du geste de pincement. L’élément de l’utilisateur encapsulé est ensuite redimensionnée à l’origine du geste de pincement en définissant son [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY), et [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) propriétés pour les valeurs calculées.

## <a name="related-links"></a>Liens associés

- [PinchGesture (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
