---
title: "Ajout d’un module de reconnaissance de mouvement panoramique"
description: "Le mouvement panoramique est utilisé pour détecter les faisant glisser et est implémenté avec la classe PanGestureRecognizer. Un scénario courant pour le mouvement panoramique est horizontalement et verticalement, faites glisser une image, afin que tout le contenu de l’image peuvent être affichés lorsqu’il est affiché dans une fenêtre d’affichage plus petite que les dimensions de l’image. Cela s’effectue en déplaçant l’image dans la fenêtre d’affichage et est illustré dans cet article."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 4da42a92c83dcc1ec0b0ba2528de672e3fcf3be3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>Ajout d’un module de reconnaissance de mouvement panoramique

_Le mouvement panoramique est utilisé pour détecter les faisant glisser et est implémenté avec la classe PanGestureRecognizer. Un scénario courant pour le mouvement panoramique est horizontalement et verticalement, faites glisser une image, afin que tout le contenu de l’image peuvent être affichés lorsqu’il est affiché dans une fenêtre d’affichage plus petite que les dimensions de l’image. Cela s’effectue en déplaçant l’image dans la fenêtre d’affichage et est illustré dans cet article._

## <a name="overview"></a>Vue d'ensemble

Pour rendre un élément d’interface utilisateur peut être déplacé avec le mouvement panoramique, créer un [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) d’une instance, de gérer la [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) événement, et ajouter la nouvelle reconnaissance de mouvement à le [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) collection sur l’élément d’interface utilisateur. Le code suivant montre d’exemple un `PanGestureRecognizer` attaché à un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément :

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Cela peut être obtenu en XAML, comme indiqué dans l’exemple de code suivant :

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
> Panoramique correct sur Android requiert la [package NuGet de 2.1.0-pre1 Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) au minimum.

## <a name="creating-a-pan-container"></a>Création d’un conteneur de défilement

Cette section contient une classe généralisée d’assistance qui effectue le panoramique de forme libre, qui convient généralement pour la navigation dans les images ou des cartes. Le mouvement panoramique pour effectuer une opération glisser de la gestion des nécessite quelques calculs pour transformer l’interface utilisateur. Cette mathématique est utilisé pour faire glisser uniquement dans les limites de l’élément d’interface utilisateur encapsulé. L’exemple de code suivant illustre la classe `PanContainer` :

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

Cette classe peut être autour d’un élément d’interface utilisateur afin que le mouvement panoramique faites glisser l’élément d’interface utilisateur encapsulé. L’exemple de code XAML suivant montre la `PanContainer` encapsulant un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément :

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

Le code suivant montre d’exemple comment la `PanContainer` encapsule un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément dans une page c# :

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

Dans les deux exemples, le [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) et [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) propriétés sont définies sur les valeurs de largeur et la hauteur de l’image qui est affichée.

Lorsque le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément reçoit un mouvement panoramique, l’image affichée est déplacé. L’opération glisser est effectuée par le `PanContainer.OnPanUpdated` (méthode), qui est affichée dans l’exemple de code suivant :

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

Cette méthode met à jour le contenu visible de l’élément d’interface utilisateur encapsulé, selon le mouvement panoramique de l’utilisateur. Cela est possible en utilisant les valeurs de la [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) et [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) propriétés de la [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) instance pour calculer la direction et distance du défilement. Le `App.ScreenWidth` et `App.ScreenHeight` propriétés fournissent la hauteur et la largeur de la fenêtre d’affichage et sont définies pour la largeur d’écran et les valeurs de hauteur d’écran de l’appareil par les projets spécifiques à une plateforme respectifs. L’élément utilisateur encapsulé est ensuite déplacé en définissant son [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) et [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) propriétés pour les valeurs calculées.

Lorsque le panoramique contenu dans un élément qui n’occupe pas de mode plein écran, la hauteur et la largeur de la fenêtre d’affichage peuvent être obtenus à partir de l’élément [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) et [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) propriétés.

> [!NOTE]
> Afficher des images haute résolution peut augmenter considérablement l’encombrement de mémoire d’une application. Par conséquent, elles doivent uniquement être créées si nécessaire et doivent être libérées dès que l’application n’exige plus leur. Pour plus d’informations, consultez [Optimiser les ressources d’images](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Récapitulatif

Le mouvement panoramique est utilisé pour la détection en faisant glisser et est implémenté avec les [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) classe.



## <a name="related-links"></a>Liens associés

- [PanGesture (sample)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
