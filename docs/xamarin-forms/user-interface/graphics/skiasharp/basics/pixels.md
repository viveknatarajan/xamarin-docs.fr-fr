---
title: Unités indépendantes du périphérique et pixels
description: Explorez les différences entre les coordonnées SkiaSharp et Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 95f782fd4670782217d8ce4bc055341747a71170
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="pixels-and-device-independent-units"></a>Unités indépendantes du périphérique et pixels

_Explorez les différences entre les coordonnées SkiaSharp et Xamarin.Forms_

Cet article présente les différences dans le système de coordonnées utilisé dans SkiaSharp et Xamarin.Forms. Vous pouvez obtenir des informations pour effectuer une conversion entre les deux systèmes de coordonnées et également dessiner des graphiques qui remplissent une zone particulière :

![](pixels-images/screenfillexample.png "Une ellipse qui remplit l’écran")

Si vous avez été programmation dans Xamarin.Forms pendant un certain temps, vous pouvez avoir une idée pour les tailles et les coordonnées de Xamarin.Forms. Les cercles dans les deux articles précédentes peuvent sembler un peu petites pour vous.

Ces cercles *sont* faible par rapport de tailles de Xamarin.Forms. Par défaut, SkiaSharp Dessine en unités de pixels alors que Xamarin.Forms base coordonnées et tailles sur une unité indépendante du périphérique établie par la plateforme sous-jacente. (Vous trouverez plus d’informations sur le système de coordonnées Xamarin.Forms dans [chapitre 5. Vous traitez des tailles](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) du livre *créer des applications mobiles avec Xamarin.Forms*.)

La page dans le [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme intitulée **taille de la Surface** utilise la sortie de texte SkiaSharp pour afficher la taille de la surface d’affichage à partir de trois sources différentes :

- Le Xamarin.Forms normal [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) et [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) propriétés de la `SKCanvasView` objet.
- Le [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) propriété de la `SKCanvasView` objet.
- Le [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) propriété de la `SKImageInfo` valeur, qui est cohérent avec la `Width` et `Height` propriétés utilisées dans les deux pages précédentes.

Le [ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) classe illustre comment afficher ces valeurs. Le constructeur enregistre le `SKCanvasView` objet comme un champ et sont accessibles dans le `PaintSurface` Gestionnaire d’événements :

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

`SKCanvas` inclut six différentes `DrawText` méthodes, mais cela [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) méthode est la plus simple :

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Si vous spécifiez la chaîne de texte, les coordonnées X et Y où le texte doit commencer, ainsi qu’un `SKPaint` objet. La coordonnée X spécifie où est positionné à gauche du texte, méfiez-vous : coordonnée Y spécifie la position de la *base* du texte. Si vous avez déjà écrit manuellement sur le papier, la ligne de base est la ligne sur le caractères de mot et en dessous les jambages inférieurs (telles que celles sur les lettres g, p, q et y) aboutissent.

Le `SKPaint` objet vous permet de spécifier la couleur du texte, la famille de polices et la taille du texte. Par défaut, le [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) propriété a la valeur 12, ce qui entraîne un texte très faible sur les appareils tels que les téléphones haute résolution. Dans une autre valeur que les applications de la plus simple, vous aurez également besoin des informations sur la taille du texte que vous affichez. Le `SKPaint` classe définit un [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) propriété et plusieurs [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) méthodes, mais pour des besoins de moins fantaisiste, le [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) propriété fournit une valeur recommandée pour l’espacement des lignes de texte consécutives.

Les éléments suivants `PaintSurface` gestionnaire crée un `SKPaint` de l’objet pour un `TextSize` de 40 pixels, ce qui correspond à la hauteur souhaitée verticale du texte à partir du haut de hampes supérieures au bas de jambages inférieurs. Le `FontSpacing` valeur le `SKPaint` retourne d’objet est un peu plus grande que celle, environ 47 pixels.

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

La méthode commence à la première ligne de texte avec une coordonnée X de 20 (pour une peu marge à gauche) et une coordonnée Y de `fontSpacing`, qui est un peu plus que ce qui est nécessaire pour afficher la totalité de la première ligne de texte en haut de la surface d’affichage. Après chaque appel à `DrawText`, la coordonnée Y est augmentée par incréments d’une ou deux de `fontSpacing`.

Voici le programme en cours d’exécution sur les trois plateformes :

[![](pixels-images/surfacesize-small.png "Capture d’écran de triple de la page de la taille de la Surface")](pixels-images/surfacesize-large.png#lightbox "Triple capture d’écran de la page de la taille de la Surface")

Comme vous pouvez le voir, la `CanvasSize` propriété de la `SKCanvasView` et le `Size` propriété de la `SKImageInfo` valeur sont cohérentes dans les dimensions en pixels de création de rapports. Le `Height` et `Width` propriétés de la `SKCanvasView` sont des propriétés de Xamarin.Forms et la taille de la vue dans les unités indépendantes du périphérique définies par la plateforme de rapports.

Le simulateur iOS 7 sur la gauche a 2 pixels indépendants du périphérique unitaire et les 5 Nexus Android dans le centre a 3 pixels par unité. C’est pourquoi le cercle simple présenté précédemment a différentes tailles sur différentes plateformes.

Si vous préférez travailler entièrement en unités indépendantes du périphérique, vous pouvez le faire en définissant le `IgnorePixelScaling` propriété de la `SKCanvasView` à `true`. Toutefois, vous n’aimez pas les résultats. SkiaSharp effectue le rendu du graphique sur une surface de périphérique plus petite, avec une taille de pixel égale à la taille de la vue en unités indépendantes du périphérique. (Par exemple, SkiaSharp utilisera une surface d’affichage de 360 x 512 pixels sur les 5 Nexus.) Il puis passe cette image dans la taille, ce qui en bitmap notable crénelage.

Pour conserver la même résolution de l’image, une meilleure solution est d’écrire vos propres fonctions simples pour effectuer une conversion entre les deux systèmes de coordonnées.

Outre la `DrawCircle` (méthode), `SKCanvas` définit également deux `DrawOval` méthodes dessiner une ellipse. Une ellipse est définie par deux rayons au lieu d’un rayon unique. Ils sont appelés les *radius principaux* et *petit rayon*. Le `DrawOval` méthode dessine une ellipse avec les deux rayons parallèles aux axes X et Y. Cette restriction peut être évitée avec des transformations ou de l’utilisation d’un chemin d’accès de graphiques (à être traitées ultérieurement), mais [cela `DrawOval` méthode](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) désigne l’argument deux rayons `rx` et `ry` pour indiquer qu’ils sont parallèles à les axes X et Y :

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Il est possible de dessiner une ellipse qui remplit la surface d’affichage ? Le **Ellipse remplir** page montre comment. Le `PaintSurface` Gestionnaire d’événements dans le [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) classe soustrait la moitié de la largeur du trait à partir de la `xRadius` et `yRadius` valeurs en fonction de l’ellipse entière et son montrer au sein de la surface d’affichage :

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

Ici, il s’exécute sur les plateformes de trois :

[![](pixels-images/ellipsefill-small.png "Capture d’écran de triple de la page de la taille de la Surface")](pixels-images/ellipsefill-large.png#lightbox "Triple capture d’écran de la page de la taille de la Surface")

Le [autres `DrawOval` méthode](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) a un [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) argument, qui est un rectangle défini d’après les coordonnées X et Y de son coin supérieur gauche et le coin inférieur droit. L’ellipse remplit ce rectangle, ce qui suggère qu’il est possible d’utiliser dans le **Ellipse remplir** page comme suit :

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Toutefois, qui tronque toutes les arêtes du contour de l’ellipse sur les quatre côtés. Vous devez ajuster toutes les le `SKRect` les arguments de constructeur basés sur la `strokeWidth` pour résoudre ce problème à droite :

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
