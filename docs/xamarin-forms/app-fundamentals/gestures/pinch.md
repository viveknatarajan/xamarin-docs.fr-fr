---
title: "Ajout d’un module de reconnaissance de mouvement de pincement"
description: "Le mouvement de pincement est utilisé pour effectuer le zoom interactif et est implémenté avec la classe PinchGestureRecognizer. Un scénario courant pour le mouvement de pincement consiste à effectuer un zoom interactif d’une image à l’emplacement de pincement. Cela s’effectue par la mise à l’échelle le contenu de la fenêtre d’affichage et est illustré dans cet article."
ms.topic: article
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 38e46af1d928a3d4e5dc33e2a46fe04cd169ed60
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Ajout d’un module de reconnaissance de mouvement de pincement

_Le mouvement de pincement est utilisé pour effectuer le zoom interactif et est implémenté avec la classe PinchGestureRecognizer. Un scénario courant pour le mouvement de pincement consiste à effectuer un zoom interactif d’une image à l’emplacement de pincement. Cela s’effectue par la mise à l’échelle le contenu de la fenêtre d’affichage et est illustré dans cet article._

## <a name="overview"></a>Vue d'ensemble

Pour rendre un élément d’interface utilisateur zoomable avec le mouvement de pincement, créer un [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) d’une instance, de gérer la [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) événement, et ajouter la nouvelle reconnaissance de mouvement à le [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) collection sur l’élément d’interface utilisateur. Le code suivant montre d’exemple un `PinchGestureRecognizer` attaché à un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément :

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Cela peut être obtenu en XAML, comme indiqué dans l’exemple de code suivant :

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

Le mouvement de pincement pour effectuer une opération de zoom de la gestion des nécessite quelques calculs pour transformer l’interface utilisateur. Cette section contient une classe d’assistance généralisée pour effectuer les calculs, ce qui peuvent être utilisé de manière interactive zoom n’importe quel élément d’interface utilisateur pour. L’exemple de code suivant illustre la classe `PinchToZoomContainer` :

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

Cette classe peut être autour d’un élément d’interface utilisateur afin que le mouvement de pincement effectuera un zoom avant l’élément d’interface utilisateur encapsulé. L’exemple de code XAML suivant montre la `PinchToZoomContainer` encapsulant un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément :

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

Le code suivant montre d’exemple comment la `PinchToZoomContainer` encapsule un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément dans une page c# :

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

Lorsque le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément reçoit un mouvement de pincement, l’image affichée sera être agrandi-in ou out. Le zoom est effectué par le `PinchZoomContainer.OnPinchUpdated` (méthode), qui est affichée dans l’exemple de code suivant :

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

Cette méthode met à jour le niveau de zoom de l’élément d’interface utilisateur encapsulé en fonction de mouvement de pincement de l’utilisateur. Cela est possible en utilisant les valeurs de la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) et [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) propriétés de la [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) instance pour calculer le facteur d’échelle à appliquer à l’origine du mouvement de pincement. L’élément utilisateur encapsulé est agrandi puis à l’origine du mouvement de pincement en définissant son [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), et [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriétés pour les valeurs calculées.

## <a name="summary"></a>Récapitulatif

Le mouvement de pincement est utilisé pour effectuer le zoom interactif et est implémenté avec les [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) classe.


## <a name="related-links"></a>Liens associés

- [PinchGesture (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
