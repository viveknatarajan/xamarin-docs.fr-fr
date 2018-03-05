---
title: "Introduction à SkiaSharp"
description: "Cela fournit une brève introduction aux concepts derrière SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 50f99c48d0f53bd6a2dfaf42284137ce1e424b30
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="an-introduction-to-skiasharp"></a>Introduction à SkiaSharp

_Cela fournit une brève introduction aux concepts derrière SkiaSharp_

SkiaSharp fournit un graphique 2D riche et puissant API qui peut être utilisé pour effectuer le rendu dans des mémoires tampons 2D.  Vous pouvez utiliser ces pour implémenter les éléments d’interface utilisateur personnalisée et les graphiques 2D qui peuvent être incorporées dans votre application.  SkiaSharp est une liaison de .NET pour les [Skia](https://skia.org) bibliothèque et hérite les fonctionnalités et la puissance de cette bibliothèque.

La bibliothèque est actuellement disponible en un inter-plateformes [NuGet Package](https://www.nuget.org/packages/SkiaSharp), vous pouvez à votre projet en ajoutant la référence de NuGet.

Pour dessiner, votre code crée un `SkCanvas` qui décrit la surface où les opérations de dessin aura lieu.

## <a name="obtaining-an-skcanvas"></a>Obtention d’un SKCanvas

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>Dessin sur SKCanvas

Le `SKCanvas` utilise un modèle de dessin même esprit autre dessin modèles que vous connaissez peut-être, il utilise des couleurs avec un canal de transparence facultatif et que vous pouvez dessiner des lignes, courbes, texte et images.

Voici quelques-uns de ses nombreuses fonctionnalités différentes qui peuvent être effectuées avec SkiaSharp.  Dans les exemples ci-dessous de la variable `canvas` est de type SKCanvas.

### <a name="drawing-xamagon"></a>Dessin Xamagon

Cet exemple dessine le logo de Xamarin le Xamagon :

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>Dessiner du texte

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>Dessiner des Bitmaps

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>Dessiner avec des filtres de l’Image

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

# <a name="more-information"></a>Complément d'information

Vous trouverez plus d’informations sur l’utilisation de SkiaSharp sur la [documentation en ligne de l’API](https://developer.xamarin.com/api/namespace/SkiaSharp/)


## <a name="related-links"></a>Liens associés

- [SkiaSharp iOS classeur](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
