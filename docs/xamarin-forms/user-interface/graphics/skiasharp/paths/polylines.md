---
title: "Polylignes et équations paramétriques"
description: "Permet d’afficher n’importe quelle ligne, que vous pouvez définir avec des équations paramétriques SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e40fd215d23e7da6f1356bba17fac84ce91007ae
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="polylines-and-parametric-equations"></a>Polylignes et équations paramétriques

_Permet d’afficher n’importe quelle ligne, que vous pouvez définir avec des équations paramétriques SkiaSharp_

Dans une partie ultérieure de ce guide, vous verrez les différentes méthodes qui `SKPath` définit pour afficher certains types de courbes. Cependant, il est parfois nécessaire de dessiner un type de courbe n’est pas directement pris en charge par `SKPath`. Dans ce cas, vous pouvez utiliser une polyligne (il s’agit d’une collection de lignes reliées) pour dessiner une courbe que vous pouvez définir mathématiquement. Si vous modifiez les lignes assez petites et nombreux suffisante, le résultat doit ressembler à une courbe. Cette spirale est réellement de 3 600 lignes peu :

![](polylines-images/spiralexample.png "Une spirale")

En général, il est préférable de définir une courbe en termes d’une paire d’équations paramétriques. Ce sont les équations de coordonnées X et Y qui dépendent d’une troisième variable, parfois appelée `t` pour le moment. Par exemple, les équations paramétriques suivantes définissent un cercle avec un rayon de 1 centrée sur le point (0, 0) pour *t* de 0 à 1 :

 x = cos(2πt) y = sin(2πt)

 Si vous souhaitez un rayon supérieures à 1, vous pouvez simplement multiplier les valeurs sinus et cosinus par ce rayon et si vous devez déplacer le centre vers un autre emplacement, ajoutez ces valeurs :

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Pour une ellipse avec parallèle axes à l’horizontale et verticale, les deux rayons sont impliquées :

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Vous pouvez ensuite placer le code SkiaSharp équivalent dans une boucle qui calcule les différents points et ajoute celles utilisées pour un chemin d’accès. Le code SkiaSharp suivant crée un `SKPath` objet pour une ellipse qui remplit la surface d’affichage. La boucle parcourt les 360 degrés directement. Le centre est moitié la largeur et la hauteur de la surface d’affichage, et sont donc les rayons des deux :

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

Cela entraîne une ellipse définie par des lignes peu 360. Lorsqu’il est restitué, il s’affiche en continu.

Bien entendu, vous n’avez pas besoin créer une ellipse à l’aide de la polyligne car `SKPath` inclut un `AddOval` méthode qui fait pour vous. Toutefois, vous pouvez dessiner un objet visuel qui n’est pas fourni par `SKPath`.

Le **Archimède spirale** page comporte du code qui semblable au code ellipse, mais avec une différence essentielle. Il effectue une boucle autour du cercle 360 degrés 10 fois, en continu en ajustant le rayon :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Le résultat est également appelé un *spirale arithmétique* , car le décalage entre chaque boucle est constant :

[![](polylines-images/archimedeanspiral-small.png "Capture d’écran de triple de la page Archimède spirale")](polylines-images/archimedeanspiral-large.png#lightbox "Triple capture d’écran de la page Archimède spirale")

Notez que la `SKPath` est créé dans un `using` bloc. Cela `SKPath` consomme plus de mémoire que le `SKPath` objets dans les programmes précédentes, ce qui suggère qu’un `using` bloc est la plus approprié supprimer les ressources non managées.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
