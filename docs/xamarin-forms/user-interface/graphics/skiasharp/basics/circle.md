---
title: Dessin d’un cercle Simple
description: Apprenez les principes fondamentaux de dessin SkiaSharp, y compris le canevas et peinture
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: a718d051c2bf04e75c87b0e4a18aced4824802ba
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-a-simple-circle"></a>Dessin d’un cercle Simple

_Apprenez les principes fondamentaux de dessin SkiaSharp, y compris le canevas et peinture_

Cet article présente les concepts de dessin des graphiques dans Xamarin.Forms à l’aide de SkiaSharp, y compris la création une `SKCanvasView` objet pour héberger les graphiques, la gestion de la `PaintSurface` et événements à l’aide un `SKPaint` pour spécifier la couleur et autres dessins attributs.

Le [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme contient des exemples de code pour cette série d’articles de SkiaSharp. La première page est autorisée à utiliser **Simple cercle** et appelle la classe de page [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Ce code montre comment tracer un cercle dans le centre de la page avec un rayon de 100 pixels. Le contour du cercle est rouge et l’intérieur du cercle est bleu.

![](circle-images/circleexample.png "Un cercle bleu indiqué en rouge")

Le [ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) dérive de la classe de la page `ContentPage` et contient deux `using` directives pour les espaces de noms SkiaSharp :

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Le constructeur suivant de la classe crée un [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) d’objet, attache un gestionnaire pour le [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) événement et définit le `SKCanvasView` objet en tant que le contenu de la page :

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

Le `SKCanvasView` occupe toute la zone de contenu de la page. Vous pouvez également combiner une `SKCanvasView` avec autres Xamarin.Forms `View` dérivés, comme vous le verrez dans d’autres exemples.

Le `PaintSurface` Gestionnaire d’événements est où vous le faites tout le dessin. Cette méthode est appelée en général plusieurs fois pendant l’exécution de votre programme, afin qu’il doit mettre à jour toutes les informations nécessaires pour recréer les graphismes s’afficher :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

Le [ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) objet qui accompagnent l’événement possède deux propriétés :

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) de type [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) de type [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

Le `SKImageInfo` structure contient des informations sur la surface de dessin, plus important encore, il est largeur et la hauteur en pixels. Le `SKSurface` objet représente la surface de dessin. Dans ce programme, la surface de dessin est un moniteur vidéo, mais dans d’autres programmes un `SKSurface` objet peut également représenter une bitmap qui vous permet de dessiner sur SkiaSharp.

La propriété la plus importante de `SKSurface` est [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) de type [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Cette classe est un contexte que vous utilisez pour effectuer le dessin proprement dit de dessin des graphiques. Le `SKCanvas` objet encapsule un état graphique qui inclut les transformations graphiques et découpage.

Voici un démarrage par défaut d’un `PaintSurface` Gestionnaire d’événements :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

Le [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) méthode efface la zone de dessin avec une couleur transparente. Une surcharge vous permet de spécifier une couleur d’arrière-plan pour la zone de dessin.

Le but ici est pour tracer un cercle rouge rempli en bleu. Cette image de graphique particulier contenant deux couleurs différentes, la tâche doit être effectuée en deux étapes. La première étape consiste à dessiner le contour du cercle. Pour spécifier la couleur et autres caractéristiques de la ligne, vous devez créer et initialiser une [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) objet :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

Le [ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) propriété indique que vous souhaitez *trait* une ligne (dans ce cas, le contour du cercle) au lieu de *remplissage* l’intérieur. Les trois membres de le [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) énumération sont les suivantes :

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

La valeur par défaut est `Fill`. La troisième option permet de tracer la ligne et de remplir l’intérieur de la même couleur.

Définir le [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) propriété à une valeur de type [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Pour obtenir un `SKColor` valeur est en convertissant un Xamarin.Forms `Color` valeur un `SKColor` valeur à l’aide de la méthode d’extension [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). Le [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) classe dans le `SkiaSharp.Views.Forms` espace de noms inclut d’autres méthodes qui convertissent entre les valeurs de Xamarin.Forms et SkiaSharp.

Le [ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) propriété indique l’épaisseur de la ligne. Ici, il est défini à 25 pixels.

Vous utilisez qui `SKPaint` objet sur lequel dessiner le cercle :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Les coordonnées sont spécifiées par rapport au coin supérieur gauche de la surface d’affichage. Les coordonnées X augmentation vers la droite et l’augmentation de coordonnées Y accédant vers le bas. Dans la discussion à propos des graphiques, souvent la notation mathématique (x, y) est utilisée pour désigner un point. Le point (0, 0) est l’angle supérieur gauche de la surface d’affichage et est souvent appelé la *origine*.

Les deux premiers arguments de `DrawCircle` indiquer les coordonnées X et Y du centre du cercle. Celles-ci sont affectées de moitié la largeur et la hauteur de la surface d’affichage pour placer le centre du cercle dans le centre de la surface d’affichage. Le troisième argument spécifie le rayon du cercle et le dernier argument est le `SKPaint` objet.

Pour remplir l’intérieur du cercle, vous pouvez modifier deux propriétés de la `SKPaint` objet et appelez `DrawCircle` à nouveau. Ce code illustre également une autre façon d’obtenir un `SKColor` valeur parmi les nombreux champs de la [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) structure :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Cette fois-ci, le `DrawCircle` appel remplit le cercle à l’aide des nouvelles propriétés de la `SKPaint` objet.

Voici le programme en cours d’exécution sur iOS, Android et la plateforme Windows universelle :

[![](circle-images/simplecircle-small.png "Capture d’écran de triple de la page Simple cercle")](circle-images/simplecircle-large.png#lightbox "Triple capture d’écran de la page Simple cercle")

Lorsque vous exécutez le programme vous-même, vous pouvez activer le téléphone ou le simulateur sur le côté pour voir comment le graphique est redessiné. Chaque fois que le graphique doit être redessiné, le `PaintSurface` Gestionnaire d’événements est appelé à nouveau.

Un `SKPaint` objet est qu’une collection de propriétés de dessin des graphiques. Ces objets sont très légères. Vous pouvez réutiliser `SKPaint` objets que ce programme, ou vous pouvez créer plusieurs `SKPaint` objets pour différentes combinaisons des propriétés de dessin. Vous pouvez créer et initialiser ces objets en dehors de la `PaintSurface` Gestionnaire d’événements et vous pouvez les enregistrer en tant que champs dans votre classe de page.

Bien que la largeur du contour du cercle est spécifiée comme 25 pixels &mdash; ou un quart de rayon du cercle &mdash; il semble plus étroit, et il existe une bonne raison de ce : moitié de la largeur de la ligne est masquée par le cercle bleu. Les arguments de la `DrawCircle` méthode définissent les coordonnées géométriques abstraites d’un cercle. L’intérieur de bleu est dimensionné à cette dimension pour le pixel le plus proche, mais le contour de 25 pixels de large chevauche le cercle géométrique &mdash; moitié à l’intérieur et l’autre moitié à l’extérieur.

L’exemple suivant dans le [intégrant Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) article montre visuellement.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
