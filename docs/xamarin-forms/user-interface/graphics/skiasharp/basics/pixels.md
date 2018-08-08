---
title: Pixels et des unités indépendantes du périphérique
description: Cet article explore les différences entre les coordonnées de SkiaSharp et Xamarin.Forms et illustre ceci avec l’exemple de code.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: b75eb1185e58de4e0524acd634a49e69142a324b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615299"
---
# <a name="pixels-and-device-independent-units"></a>Pixels et des unités indépendantes du périphérique

_Découvrez les différences entre les coordonnées de SkiaSharp et Xamarin.Forms_

Cet article explore les différences dans le système de coordonnées utilisé dans SkiaSharp et Xamarin.Forms. Vous pouvez obtenir des informations pour effectuer une conversion entre les deux systèmes de coordonnées et également dessiner des graphiques qui remplissent une zone particulière :

![](pixels-images/screenfillexample.png "Un ovale qui remplit l’écran")

Si vous avez programmé dans Xamarin.Forms pendant un certain temps, vous pouvez avoir une idée pour Xamarin.Forms coordonnées et tailles. Les cercles dessinées dans les deux précédents articles peuvent sembler un peu petites pour vous.

Ces cercles *sont* faible par rapport de tailles de Xamarin.Forms. Par défaut, SkiaSharp Dessine en unités de pixels tandis que les coordonnées et tailles des bases Xamarin.Forms sur une unité indépendante du périphérique établie par la plateforme sous-jacente. (Vous trouverez plus d’informations sur le système de coordonnées de Xamarin.Forms dans [chapitre 5. Gestion des tailles](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) du livre *création d’applications mobiles avec Xamarin.Forms*.)

La page dans le [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme intitulée **taille de la Surface** utilise la sortie de texte SkiaSharp pour afficher la taille de la surface d’affichage à partir de trois sources différentes :

- Xamarin.Forms normal [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) et [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) propriétés de la `SKCanvasView` objet.
- Le [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) propriété de la `SKCanvasView` objet.
- Le [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) propriété de la `SKImageInfo` valeur, qui est cohérente avec la `Width` et `Height` propriétés utilisées dans les deux pages précédentes.

Le [ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) classe montre comment afficher ces valeurs. Le constructeur enregistre le `SKCanvasView` objet sous la forme d’un champ et sont accessibles dans le `PaintSurface` Gestionnaire d’événements :

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` inclut six différents `DrawText` méthodes, mais cela [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) méthode est la plus simple :

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Si vous spécifiez la chaîne de texte, les coordonnées X et Y où le texte doit commencer et un `SKPaint` objet. La coordonnée X spécifie où le côté gauche du texte est positionné, méfiez-vous : coordonnée Y spécifie la position de la *baseline* du texte. Si vous avez déjà écrit à la main sur papier, la ligne de base est la ligne sur le site de caractères, et en dessous descendent lesquelles jambages (telles que celles sur les lettres g, p, q et y).

Le `SKPaint` objet vous permet de spécifier la couleur du texte, la famille de polices et la taille du texte. Par défaut, le [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) propriété a la valeur 12, ce qui se traduit en très petit texte sur les appareils tels que les téléphones haute résolution. Dans des déploiements autres que les applications la plus simple, vous devez également des informations sur la taille du texte que vous affichez. Le `SKPaint` classe définit un [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) propriété et plusieurs [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) méthodes, mais moins fantaisistes aux besoins de, la [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) propriété attribue une valeur recommandée pour l’espacement des lignes successives de texte.

Ce qui suit `PaintSurface` gestionnaire crée un `SKPaint` de l’objet pour un `TextSize` de 40 pixels, qui est la hauteur souhaitée verticale du texte à partir du haut des jambages supérieurs vers le bas des jambages. Le `FontSpacing` valeur qui le `SKPaint` retourne d’objet est un peu plus grande que celle, environ 47 pixels.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

La méthode commence à la première ligne de texte avec une coordonnée X de 20 (pour une petite marge à gauche) et une coordonnée Y de `fontSpacing`, qui est un peu plus que ce qui est nécessaire pour afficher la hauteur totale de la première ligne de texte en haut de la surface d’affichage. Après chaque appel à `DrawText`, la coordonnée Y est augmentée par incréments d’une ou deux de `fontSpacing`.

Voici le programme en cours d’exécution sur les trois plateformes :

[![](pixels-images/surfacesize-small.png "Capture d’écran triple de la page de la taille de la Surface")](pixels-images/surfacesize-large.png#lightbox "Triple capture d’écran de la page de la taille de la Surface")

Comme vous pouvez le voir, le `CanvasSize` propriété de la `SKCanvasView` et le `Size` propriété de la `SKImageInfo` valeur sont cohérents dans les dimensions en pixels de création de rapports. Le `Height` et `Width` propriétés de la `SKCanvasView` sont des propriétés de Xamarin.Forms et indiquent la taille de la vue dans les unités indépendantes du périphérique définies par la plateforme.

Le simulateur iOS 7 sur la gauche a 2 pixels par unité indépendante du périphérique, et le Android Nexus 5 dans le centre a 3 pixels par unité. C’est pourquoi le cercle simple illustré précédemment présente des tailles différentes sur différentes plateformes.

Si vous préférez travailler entièrement en unités indépendantes du périphérique, vous pouvez le faire en définissant le `IgnorePixelScaling` propriété de la `SKCanvasView` à `true`. Toutefois, vous n’aimez pas les résultats. SkiaSharp restitue les graphiques sur une surface de périphérique plus petits, avec une taille de pixel égale à la taille de la vue en unités indépendantes du périphérique. (Par exemple, SkiaSharp utiliserait une surface d’affichage de 360 x 512 pixels sur le Nexus 5.) Il puis monte cette image en taille, ce qui entraîne une bitmap notable crénelage.

Pour conserver la même résolution d’image, une meilleure solution consiste à écrire vos propres fonctions simples à effectuer une conversion entre les deux systèmes de coordonnées.

Outre le `DrawCircle` (méthode), `SKCanvas` définit également deux `DrawOval` méthodes dessiner une ellipse. Une ellipse est définie par deux rayons plutôt que dans un rayon unique. Ceux-ci sont connus comme les *radius principale* et *petit rayon*. Le `DrawOval` méthode dessine une ellipse avec les deux rayons parallèles aux axes X et Y. Cette restriction peut être surmontée avec des transformations ou de l’utilisation d’un chemin d’accès de graphiques (à être traitée ultérieurement), mais [cela `DrawOval` méthode](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) nomme l’argument deux rayons `rx` et `ry` pour indiquer qu’ils sont parallèles à les axes X et Y :

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Est-il possible de dessiner une ellipse qui remplit la surface d’affichage ? Le **Ellipse remplir** page montre comment. Le `PaintSurface` Gestionnaire d’événements dans le [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) classe soustrait la moitié de la largeur du trait à partir de la `xRadius` et `yRadius` valeurs en fonction de l’ensemble ellipse et son décrit dans la surface d’affichage :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Ici, il s’exécute sur les trois plateformes :

[![](pixels-images/ellipsefill-small.png "Capture d’écran triple de la page de la taille de la Surface")](pixels-images/ellipsefill-large.png#lightbox "Triple capture d’écran de la page de la taille de la Surface")

Le [autres `DrawOval` méthode](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) a un [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) argument, qui est un rectangle défini en termes des coordonnées X et Y de son coin supérieur gauche et le coin inférieur droit. L’ovale remplit ce rectangle, ce qui suggère qu’il est parfois possible d’utiliser dans le **Ellipse remplir** page comme celle-ci :

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Toutefois, qui tronque tous les bords du contour de l’ellipse sur les quatre côtés. Vous devez ajuster toutes le `SKRect` les arguments de constructeur basés sur le `strokeWidth` pour y parvenir à droite :

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
