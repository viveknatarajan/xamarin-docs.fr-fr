---
title: "Principes fondamentaux de chemin d’accès"
description: "Explorer l’objet SkiaSharp SKPath pour combiner des lignes reliées et courbes"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: f02c5cfd75fd9d9cd97d28ca276b32808f7a45ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="path-basics"></a>Principes fondamentaux de chemin d’accès

_Explorer l’objet SkiaSharp SKPath pour combiner des lignes reliées et courbes_

Parmi les plus importantes décrit les fonctionnalités du chemin d’accès du graphique est la capacité à définir lorsque plusieurs lignes doivent être connectées et lorsqu’ils doivent être connectés pas. La différence peut être très visible, comme les parties supérieures de ces deux triangles montrent :

![](paths-images/connectedlinesexample.png "Deux triangles montrant la différence entre les lignes connectés et déconnectés")

Un chemin d’accès du graphique est encapsulée par la [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) objet. Un chemin d’accès est une collection d’un ou plusieurs *profils*. Chaque profil est une collection de *connecté* lignes droites et courbes. Profils ne sont pas connectés entre eux, mais ils peuvent chevaucher visuellement. Parfois, un contour unique permettre se chevaucher.

Un profil commence généralement par un appel à la méthode suivante de `SKPath`:

- `MoveTo` Pour commencer un nouveau profil

L’argument de cette méthode est un point unique, que vous pouvez exprimer comme un `SKPoint` valeur ou distinct X et Y coordonnées. Le `MoveTo` appel établit un point au début de contour et initial *point actuel*. Vous pouvez appeler les méthodes suivantes pour continuer le contour avec une ligne ou d’une courbe à partir du point actif vers un point spécifié dans la méthode, qui devient le nouveau point actuel :

- `LineTo` Pour ajouter une ligne droite pour le chemin d’accès
- `ArcTo` Pour ajouter un arc, qui est une ligne sur la circonférence d’un cercle ou d’une ellipse
- `CubicTo` Pour ajouter une courbe de Bézier cubique
- `QuadTo` Pour ajouter une courbe de Bézier quadratique
- `ConicTo` Pour ajouter une spline Bézier quadratique rationnelle, qui peut restituer avec précision les sections conique (ellipses, paraboles ou et hyperboles)

Aucun de ces cinq méthodes contient toutes les informations nécessaires pour décrire la ligne ou la courbe. Chacune de ces cinq méthodes fonctionne conjointement avec le point actuel établi par l’appel de méthode qui la précèdent immédiatement. Par exemple, le `LineTo` méthode ajoute une ligne droite pour le profil basé sur le point actuel, par conséquent, le paramètre `LineTo` n'est qu’un seul point.

Le `SKPath` classe définit également des méthodes qui ont les mêmes noms que ces six méthodes, mais avec un `R` au début :

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

Le `R` signifie *relatif*. Ils ont la même syntaxe que les méthodes correspondantes sans le `R` mais sont relatives au point actuel. Ceux-ci sont utiles pour le dessin des parties similaires d’un chemin d’accès dans une méthode que vous appelez plusieurs fois.

Un profil de charge se termine par un autre appel à `MoveTo` ou `RMoveTo`, qui commence un nouveau profil, ou un appel à `Close`, qui ferme le contour. Le `Close` méthode automatiquement ajoute une ligne droite à partir du point actuel et le premier point du contour et marque le chemin d’accès comme étant fermé, ce qui signifie qu’elle est restituée sans les extrémités de trait.

La différence entre les contours ouvertes et fermées est illustrée dans le **deux profils de Triangle** page, qui utilise un `SKPath` objet avec les deux profils pour afficher deux triangles. Le premier contour est ouvert et le second est fermé. Voici le [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) classe :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Le premier contour se compose d’un appel à [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) à l’aide des coordonnées X et Y plutôt qu’une `SKPoint` valeur suivie de trois appels à [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) pour dessiner les trois parties de la triangle. Le deuxième contour a uniquement deux appels à `LineTo` mais il termine le contour avec un appel à [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), qui ferme le contour. La différence est importante :

[![](paths-images/twotrianglecontours-small.png "Capture d’écran de triple de la page de deux profils de Triangle")](paths-images/twotrianglecontours-large.png "Triple capture d’écran de la page de deux profils de Triangle")

Comme vous pouvez le voir, le premier contour est évidemment une série de trois lignes connectées, mais la fin ne connecte pas au début. Les deux lignes se chevauchent en haut. Le contour du deuxième est évidemment fermé et a été effectué avec un moins `LineTo` appelle, car le `Close` méthode ajoute automatiquement une ligne finale pour fermer le contour.

`SKCanvas` définit une uniquement [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) (méthode), qui est appelée deux fois dans cette démonstration pour remplir et tracer le chemin d’accès. Tous les profils sont remplies, y compris ceux qui ne sont pas fermées. Pour des raisons de remplissage des chemins d’accès non fermées, une ligne droite est censée pour exister entre les points d’entrée et des profils. Si vous supprimez le dernier `LineTo` à partir de la première contour, ou supprimez la `Close` appel à partir du deuxième contour, chaque profil aura uniquement deux côtés mais n’est rempli comme s’il s’agissait d’un triangle.

`SKPath` définit de nombreuses autres méthodes et propriétés. Les méthodes suivantes ajoutent les contours entières pour le chemin d’accès, qui peut être fermé ou pas fermée en fonction de la méthode :

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Pour ajouter une courbe sur la circonférence d’une ellipse
- `AddPath` Pour ajouter un autre chemin pour le chemin d’accès actuel
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) Pour ajouter un autre chemin dans l’ordre inverse

Gardez à l’esprit qu’un `SKPath` objet définit uniquement une géométrie & #x 2014 ; une série de points et de connexions. Uniquement lorsqu’un `SKPath` est associée à un `SKPaint` objet est le chemin d’accès rendu avec une couleur spécifique, la largeur du trait et ainsi de suite. En outre, n’oubliez pas que le `SKPaint` objet passé à la `DrawPath` méthode définit les caractéristiques de l’intégralité du chemin. Si vous souhaitez dessiner un élément nécessitant plusieurs couleurs, vous devez utiliser un chemin d’accès distinct pour chaque couleur.

Tout comme l’apparence de début et de fin d’une ligne est défini à une extrémité de trait, l’apparence de la connexion entre deux lignes est définie par un *jointure de trait*. Vous spécifiez cela en définissant le [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) propriété du `SKPaint` à un membre de la [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) énumération :

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) pour une jointure après
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) pour une jointure arrondie
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) pour une jointure coupées désactivé

Le **trait jointures** page affiche ces trois tracer les jointures avec un code similaire à la **extrémités de trait** page. Il s’agit de la `PaintSurface` Gestionnaire d’événements dans le [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) classe :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Voici le programme en cours d’exécution sur les plateformes de trois :

[![](paths-images/strokejoins-small.png "Triple capture d’écran de la page trait joint")](paths-images/strokejoins-large.png "Triple capture d’écran de la page joint de trait")

La jointure d’angle se compose d’un point aigu où les lignes de se connectent. Lorsque deux lignes se joindre à un petit angle, la jointure de pointe permettre considérablement s’allonger. Pour empêcher qu’excessivement longs les jointures, la longueur de la jointure de pointe est limitée par la valeur de la [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) propriété du `SKPaint`. Une jointure de pointe qui dépasse cette longueur est coupée pour devenir un biseau.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
