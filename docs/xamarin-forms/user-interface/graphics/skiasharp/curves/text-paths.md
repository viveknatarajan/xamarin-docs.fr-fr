---
title: Chemins d’accès et le texte
description: Explorer l’intersection des chemins d’accès et le texte
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: c0b793a495278d91429045d7e396917d02c1412e
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="paths-and-text"></a>Chemins d’accès et le texte

_Explorer l’intersection des chemins d’accès et le texte_

Dans les systèmes de graphique moderne, les polices de texte sont des collections de contours de caractère, généralement définies par les courbes Bézier quadratiques. Par conséquent, de nombreux systèmes graphique moderne incluent une fonction pour convertir les caractères de texte dans un chemin d’accès des graphiques. 

Vous avez déjà vu que vous pouvez tracer des contours des caractères de texte ainsi que les remplir. Cela vous permet d’afficher ces contours de caractère avec une épaisseur de trait particulier et même un effet de chemin d’accès comme décrit dans la [ **les effets de chemin d’accès** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) l’article. Mais il est également possible de convertir une chaîne de caractères dans un `SKPath` objet. Cela signifie que les contours du texte peuvent être utilisés pour le découpage avec les techniques décrites dans le [ **consiste à faire coïncider avec des chemins d’accès et les régions** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) l’article. 

Outre l’utilisation d’un effet de chemin d’accès pour tracer un contour du caractère, vous pouvez également créer des effets qui reposent sur un chemins d’accès sont dérivés de chaînes de caractères de chemin, et vous pouvez même combiner les deux effets :

![](text-paths-images/pathsandtextsample.png "Effet de texte")

Dans le [article précédent](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) , vous avez vu comment les [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) méthode de `SKPaint` peut obtenir un plan d’un chemin d’accès de tracé. Vous pouvez également utiliser cette méthode avec les chemins d’accès dérivés de contours de caractère.

Enfin, cet article montre une autre intersection des chemins d’accès et du texte : le [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) méthode `SKCanvas` vous permet d’afficher une chaîne de texte afin que la ligne de base du texte suit un tracé courbé.

## <a name="text-to-path-conversion"></a>Texte à la Conversion du chemin d’accès

Le [ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) méthode `SKPaint` convertit une chaîne de caractères en une `SKPath` objet :

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

Le `x` et `y` arguments indiquent le point de départ de la ligne de base du côté gauche du texte. Ils jouent le même rôle ici comme dans le `DrawText` méthode `SKCanvas`. Dans le chemin d’accès, la ligne de base du côté gauche du texte aura les coordonnées (x, y). 

Le `GetTextPath` méthode est excessif si vous souhaitez simplement remplir ou entourer le chemin d’accès résultant. Vecteur normal `DrawText` méthode vous permet de faire. Le `GetTextPath` méthode est plus utile pour d’autres tâches qui impliquent des chemins d’accès.

Une de ces tâches est extrait. Le **l’extrait de texte** page crée un tracé de détourage basé sur les contours des caractères du mot « CODE ». Ce chemin d’accès est étendue à la taille de la page pour les images bitmap qui contient une image de la **extrait le texte** code source :

[![](text-paths-images/clippingtext-small.png "Capture d’écran de triple de la page de texte de découpage")](text-paths-images/clippingtext-large.png#lightbox "Triple capture d’écran de la page de texte de détourage")

Le [ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) constructeur de classe de charge de la bitmap qui est stockée en tant que ressource incorporée dans le **Media** dossier de la solution :

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

Le `PaintSurface` gestionnaire commence par créer un `SKPaint` objet adapté au texte. Le `Typeface` est définie, ainsi que les `TextSize`, bien que pour cette application particulière le `TextSize` propriété est purement arbirtrary. Notez également existe aucune `Style` paramètre.

Le `TextSize` et `Style` les paramètres de propriété ne sont pas nécessaires, car cela `SKPaint` objet est utilisé uniquement pour les `GetTextPath` appeler à l’aide de la chaîne de texte « CODE ». Le Gestionnaire de mesure puis résultant `SKPath` de l’objet et applique des trois transformations pour centrer et mettre à l’échelle à la taille de la page. Vous pouvez ensuite définir le chemin d’accès en tant que le chemin d’accès de l’extrait :

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, 
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

Une fois le tracé de détourage est défini, l’image bitmap peut être affiché, et il coïncident avec les contours des caractères. Notez l’utilisation de la [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) méthode `SKRect` qui calcule un rectangle pour remplir la page tout en conservant les proportions.

Le **effet texte** page convertit un caractère de signe pour un chemin d’accès pour créer un effet de chemin d’accès 1D. Un objet de peinture avec l’effet de ce chemin d’accès est ensuite utilisé pour tracer le contour d’une version supérieure de ce même caractère :

[![](text-paths-images/textpatheffect-small.png "Capture d’écran de triple de la page de l’effet texte")](text-paths-images/textpatheffect-large.png#lightbox "Triple capture d’écran de la page de l’effet de texte chemin d’accès")

Quantité de travail dans le [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) classe se produit dans les champs et le constructeur. Les deux `SKPaint` objets définis comme champs sont utilisés pour deux raisons différentes : la première (nommé `textPathPaint`) est utilisée pour convertir l’esperluette avec un `TextSize` de 50 à un chemin d’accès pour l’effet de 1D. La deuxième (`textPaint`) est utilisé pour afficher la version la plus grande de la « et commercial » avec l’effet de ce chemin d’accès. La raison pour laquelle le `Style` de ce deuxième peinture objet a la valeur `Stroke`, mais la `StrokeWidth` propriété n’est pas définie, car cette propriété n’est pas nécessaire lors de l’utilisation d’un effet 1D :

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint 
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds;
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character, 
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

Le constructeur utilise d’abord la `textPathPaint` objet pour mesurer l’esperluette avec un `TextSize` de 50. Les négatifs des coordonnées du rectangle center sont ensuite transmis à la `GetTextPath` méthode pour convertir le texte dans un chemin d’accès. Le chemin d’accès résultant est le (0, 0) point dans le centre du caractère, qui est idéal pour un effet 1D.

Vous pouvez penser que le `SKPathEffect` objet créé à la fin du constructeur peut être défini sur le `PathEffect` propriété du `textPaint` au lieu d’enregistrer en tant que champ. Mais ce n’est pas activé les fonctionne très bien, car il déforme les résultats de la `MeasureText` appeler dans le `PaintSurface` gestionnaire :

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds;
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Que `MeasureText` appel est utilisé pour le caractère dans la page du centre. Pour éviter les problèmes, le `PathEffect` est définie sur l’objet de peinture après le texte a été mesuré mais avant son affichage.

## <a name="outlines-of-character-outlines"></a>Décrit des contours des caractères

Normalement le [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) méthode `SKPaint` convertit un chemin d’accès à un autre en appliquant des propriétés de peinture, notamment la largeur et le chemin d’accès effet de contour. Lorsqu’il est utilisé sans effets du chemin d’accès, `GetFillPath` fait crée un chemin d’accès qui décrit un autre chemin. Cela est illustré dans le **appuyez sur contour le chemin d’accès** page dans le [ **les effets de chemin d’accès** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) l’article.

Vous pouvez également appeler `GetFillPath` sur le chemin d’accès retourné par `GetTextPath` , mais dans un premier temps vous peut-être pas entièrement que quoi cela ressemble.

Le **contours de structure du caractère** page montre la technique. Tout le code est dans le `PaintSurface` Gestionnaire de la [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) classe.

Le constructeur commence par créer un `SKPaint` objet nommé `textPaint` avec un `TextSize` propriété basée sur la taille de la page. Il est converti en un chemin d’accès à l’aide de la `GetTextPath` (méthode). Les arguments de coordonnées à `GetTextPath` efficacement centrer le chemin d’accès à l’écran :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds;
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

Le `PaintSurface` gestionnaire crée ensuite un nouveau chemin d’accès nommé `outlinePath`. Cela devient le chemin d’accès de destination dans l’appel à `GetFillPath`. Le `StrokeWidth` propriété 25 causes `outlinePath` pour décrire le contour d’un chemin d’accès de 25 pixels de large traçant les caractères de texte. Ce chemin d’accès est ensuite affichée en rouge avec une épaisseur de trait de 5 :

[![](text-paths-images/characteroutlineoutlines-small.png "Capture d’écran de triple de la page contours de structure du caractère")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Triple capture d’écran de la page contours de contour de caractère")

Examinez attentivement, et vous verrez des chevauchements et où la structure du chemin d’accès est un angle aigu. Il s’agit d’artefacts normales de ce processus.

## <a name="text-along-a-path"></a>Texte le long d’un chemin d’accès

Texte s’affiche normalement sur une ligne de base horizontale. Texte peut être pivotée pour exécuter verticalement ou en diagonale, mais la ligne de base est toujours une ligne droite.

Il existe des cas, toutefois, lorsque vous souhaitez que le texte à exécuter une courbe. Ceci est l’objectif de la [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) méthode `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Le texte spécifié dans le premier argument est fait pour exécuter le long du chemin d’accès spécifié comme deuxième argument. Vous pouvez commencer le texte à un offset à partir du début du chemin d’accès avec le `hOffset` argument. Normalement le chemin d’accès constitue la base du texte : hampes supérieures de texte sont sur le côté « un » du chemin d’accès et hampes de texte sont sur l’autre. Mais vous pouvez décaler la ligne de base du chemin d’accès avec le `vOffset` argument.

Cette méthode n’a aucune fonctionnalité de fournir des conseils sur la configuration de la `TextSize` propriété du `SKPaint` pour rendre le texte de taille parfaitement pour s’exécuter à partir du début du chemin d’accès à la fin. Vous pouvez parfois déterminer cette taille de texte sur votre propre. Parfois, vous devez utiliser les fonctions de mesure de chemin d’accès à décrire dans un prochain article.

Le **texte circulaire** programme encapsule le texte autour d’un cercle. Il est facile de déterminer la circonférence d’un cercle, il est facile de la taille du texte pour ajuster exactement. Le `PaintSurface` Gestionnaire de la [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) classe calcule un rayon d’un cercle en fonction de la taille de la page. Cercle devient `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

Le `TextSize` propriété du `textPaint` est alors ajusté afin que la largeur du texte correspond à la circonférence du cercle :

[![](text-paths-images/circulartext-small.png "Capture d’écran de triple de la page de texte circulaire")](text-paths-images/circulartext-large.png#lightbox "Triple capture d’écran de la page de texte circulaire")

Le texte proprement dit a été choisi pour être quelque peu circulaires ainsi : le mot « circle » représente le sujet de la phrase et l’objet d’une expression préposition. 

## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
