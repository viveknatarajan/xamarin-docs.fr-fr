---
title: Trois méthodes pour dessiner un Arc
description: Découvrez comment utiliser SkiaSharp pour définir des arcs de trois façons différentes
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 86623886e2429cbf2f076fbe4583301edf684262
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="three-ways-to-draw-an-arc"></a>Trois méthodes pour dessiner un Arc

_Découvrez comment utiliser SkiaSharp pour définir des arcs de trois façons différentes_

Un arc est une courbe sur la circonférence d’une ellipse, telles que les parties arrondies de ce signe infini :

![](arcs-images/arcsample.png "Connexion de l’infini")

En dépit de la simplicité de cette définition, il n’existe aucun moyen de définir une fonction de dessin de l’arc qui satisfait à tous les besoins, et par conséquent, aucun consensus entre les systèmes de graphiques de la meilleure méthode pour dessiner un arc. Pour cette raison, la `SKPath` classe ne se limite pas à simplement une approche.

`SKPath` définit un `AddArc` (méthode), les cinq différents `ArcTo` méthodes et deux relative `RArcTo` méthodes. Ces méthodes se répartissent en trois catégories, en spécifiant un arc représentant trois approches très différentes. Votre choix dépend des informations disponibles pour la définition de l’arc et la manière dont cet arc s’intègre avec les autres graphiques que vous dessinez.

## <a name="the-angle-arc"></a>L’Arc Angle

L’approche d’arc de cercle angle le dessin des arcs nécessite que vous spécifiez un rectangle qui délimite une ellipse. L’arc sur la circonférence de cette ellipse est indiqué par les angles à partir du centre de l’ellipse qui effectue le début de l’arc et sa longueur. Deux méthodes différentes dessiner des courbes de l’angle. Voici le [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) (méthode) et le [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) méthode :

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Ces méthodes sont identiques à la Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) et [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) méthodes. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) méthode est similaire, mais est limité aux arcs sur la circonférence d’un cercle plutôt que généralisé à une ellipse.

Les deux méthodes commencent par un `SKRect` valeur qui définit l’emplacement et la taille d’une ellipse :

![](arcs-images/anglearcoval.png "L’ellipse qui commence un arc angle")

L’arc est une partie de la circonférence de cette ellipse.

Le `startAngle` argument est un angle dans le sens horaire en degrés par rapport à une ligne horizontale dessinée à partir du centre de l’ellipse à droite. Le `sweepAngle` est relative à l’argument le `startAngle`. Voici `startAngle` et `sweepAngle` les valeurs de degrés de 60 et 100, respectivement :

![](arcs-images/anglearcangles.png "Les angles qui définissent un arc angle")

L’arc commence à l’angle de début. Sa longueur est régie par l’angle de balayage :

![](arcs-images/anglearchighlight.png "L’arc angle en surbrillance")

La courbe ajoutée pour le chemin d’accès avec le `AddArc` ou `ArcTo` consiste simplement à cette partie de la circonférence de l’ellipse, illustré ici en rouge :

![](arcs-images/anglearc.png "L’arc angle par lui-même")

Le `startAngle` ou `sweepAngle` arguments peuvent être négatifs : l’arc est dans le sens horaire pour les valeurs positives de `sweepAngle` et pour les valeurs négatives dans le sens inverse des aiguilles.

Toutefois, `AddArc` est *pas* définir un contour fermé. Si vous appelez `LineTo` après `AddArc`, une ligne est tracée à partir de la fin de l’arc au point dans le `LineTo` (méthode) et le même a la valeur true de `ArcTo`.

`AddArc` démarre un nouveau profil et est fonctionnellement équivalent à un appel à automatiquement `ArcTo` avec l’argument final de `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Que le dernier argument est appelée `forceMoveTo`, et il entraîne efficacement une `MoveTo` appeler au début de l’arc. Qui commence un nouveau profil. Autrement dit pas le cas avec un dernier argument de `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Cette version de `ArcTo` Dessine une ligne à partir de la position actuelle vers le début de l’arc. Cela signifie que l’arc peut être quelque part au milieu d’un profil de plus grand.

Le **Angle Arc** page vous permet d’utiliser deux curseurs pour spécifier le début et l’angle de balayage. Le fichier XAML instancie deux `Slider` éléments et une `SKCanvasView`. Le `PaintCanvas` gestionnaire dans le [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) fichier Dessine l’ellipse et l’arc en utilisant deux `SKPaint` objets définis en tant que champs :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Comme vous pouvez le voir, l’angle de début et de l’angle de balayage peuvent prendre les valeurs négatives :

[![](arcs-images/anglearc-small.png "Capture d’écran de triple de la page de l’Angle d’arc de cercle")](arcs-images/anglearc-large.png#lightbox "Triple capture d’écran de la page de l’Angle d’arc de cercle")

Cette approche à la génération d’un arc est obtenue la plus simple, et il est facile de dériver les équations paramétriques qui décrivent l’arc. Connaître la taille et l’emplacement de l’ellipse, ainsi que les angles de début et de balayage, le début et points de terminaison de l’arc peut être calculées à l’aide de la trigonométrie simple :

x = oval. MidX + (oval. Largeur / 2) * cos(angle)

y = oval. MidY + (oval. Hauteur / 2) * sin(angle)

Le `angle` valeur est soit `startAngle` ou `startAngle + sweepAngle`.

L’utilisation de deux angles pour définir un arc est idéal pour les cas où vous connaissez la longueur angulaire de l’arc souhaité à dessiner, par exemple, pour créer un graphique en secteurs. Le **éclaté un graphique à secteurs** page illustre cela. Le [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) classe utilise une classe interne pour définir des couleurs et des données fabriquées :

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

Le `PaintSurface` Gestionnaire parcourt tout d’abord les éléments à calculer un `totalValues` nombre. À partir de cela, il peut déterminer la taille de chaque élément comme la fraction du total et de les convertir un angle :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Un nouveau `SKPath` objet est créé pour chaque secteur. Le chemin d’accès se compose d’une ligne à partir du centre, alors un `ArcTo` pour dessiner l’arc et une autre ligne de retour pour les résultats du centre du `Close` appeler. Ce programme affiche « éclaté » secteurs en les déplaçant toutes les à partir du centre de 50 pixels. Cette tâche nécessite un vecteur dans le sens du milieu de l’angle de balayage pour chaque secteur :

[![](arcs-images/explodedpiechart-small.png "Capture d’écran de triple de la page éclaté un graphique à secteurs")](arcs-images/explodedpiechart-large.png#lightbox "Triple capture d’écran de la page éclaté un graphique à secteurs")

Pour voir à quoi il ressemble sans le « éclatement », simplement en commentaire la `Translate` appeler :

[![](arcs-images/explodedpiechartunexploded-small.png "Capture d’écran de triple de la page éclaté un graphique à secteurs sans l’explosion")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Triple capture d’écran de la page éclaté un graphique à secteurs sans l’explosion")

## <a name="the-tangent-arc"></a>L’Arc tangente

Le second type de l’arc pris en charge par `SKPath` est la *arc tangente*, appelée ainsi car l’arc correspond à la circonférence d’un cercle tangente à deux lignes connectées.

Un arc tangent est ajouté à un chemin d’accès avec un appel à la [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) méthode avec deux `SKPoint` paramètres, ou le [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) surcharge avec distinct `Single` paramètres pour le points suivants :

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Cela `ArcTo` méthode est similaire à la PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (fonction) (page 532 dans le document PDF) et le fichier iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) (méthode).

Le `ArcTo` méthode implique trois points :

- Point d’en cours de contour, ou le point (0, 0) si `MoveTo` n’a pas été appelée
- Le premier argument de point à la `ArcTo` méthode, appelée la *point d’angle*
- Le deuxième argument point `ArcTo`, appelée la *point de destination*:

![](arcs-images/tangentarcthreepoints.png "Trois points qui commencent un arc tangent")

Ces trois points définissent deux lignes connectées :

![](arcs-images/tangentarcconnectinglines.png "Lignes reliant les trois points d’un arc tangent")

Si les trois points sont colinéaires &mdash; , autrement dit, si elles se trouvent sur la même ligne droite &mdash; aucun arc ne sera dessiné.

Le `ArcTo` inclut également un `radius` paramètre. Définit le rayon d’un cercle :

![](arcs-images/tangentarccircle.png "Le cercle d’un arc tangent")

L’arc tangente n’est pas généralisée pour d’une ellipse.

Si les deux lignes rencontrent en un angle, cercle peut être inséré entre ces lignes afin qu’il soit tangente dans les deux lignes :

![](arcs-images/tangentarctangentcircle.png "Le cercle en arc tangente entre les deux lignes")

La courbe est ajoutée pour le profil ne touchez pas un des points définis dans le `ArcTo` (méthode). Il se compose d’une ligne droite à partir du point actif pour le premier point de tangente et un arc qui se termine par le deuxième point tangent :

![](arcs-images/tangentarchighlight.png "L’arc tangente en surbrillance entre les deux lignes")

Voici la dernière ligne droite et arc est ajouté pour le profil de :

![](arcs-images/tangentarc.png "L’arc tangente en surbrillance entre les deux lignes")

Le profil peut être poursuivi à partir du deuxième point tangent.

Le **tangente Arc** page vous permet de faire des essais avec l’arc tangente. Voici la première de plusieurs pages qui dérivent de [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/InteractivePage.cs), qui définit certaines pratiques `SKPaint` des objets et effectue `TouchPoint` traitement :

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

La classe `TangentArcPage` dérive de la classe `InteractivePage`. Le constructeur dans le [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) fichier est responsable de l’instanciation et initialisation le `touchPoints` tableau et paramètre `baseCanvasView` (dans `InteractivePage`) à la `SKCanvasView` objet instancié dans le [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) fichier :

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Le `PaintSurface` Gestionnaire utilise la `ArcTo` méthode pour dessiner l’arc en se basant sur les points tactiles et un `Slider`, mais également par algorithme calcule le cercle qui dépend de l’angle :

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }

    // Vector methods
    SKPoint Normalize(SKPoint v)
    {
        float magnitude = Magnitude(v);
        return new SKPoint(v.X / magnitude, v.Y / magnitude);
    }

    float Magnitude(SKPoint v)
    {
        return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
    }
}
```

Voici le **tangente Arc** page en cours d’exécution sur les trois plateformes :

[![](arcs-images/tangentarc-small.png "Capture d’écran de triple de la page de la tangente d’arc de cercle")](arcs-images/tangentarc-large.png#lightbox "Triple capture d’écran de la page de la tangente d’arc de cercle")

Sur l’appareil Windows Mobile, les trois points sont colinéaires presque, et l’arc est très faible.

L’arc tangente est idéal pour la création des angles arrondis, par exemple un rectangle arrondi. Étant donné que `SKPath` inclut déjà une `AddRoundedRect` (méthode), la **arrondi de sur un heptagone** page montre comment utiliser `ArcTo` pour arrondir les angles d’un polygone sept recto verso. (Le code est généralisé pour tout polygone régulier).

Le `PaintSurface` Gestionnaire de la [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) classe contient un `for` boucle pour calculer les coordonnées des sommets de le sur un heptagone et l’autre pour calculer les points centraux des sept côtés de ces sept sommets. Ces points centraux est ensuite utilisées pour construire le chemin d’accès :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Voici le programme en cours d’exécution sur les plateformes de trois :

[![](arcs-images/roundedheptagon-small.png "Capture d’écran de triple de la page de l’arrondi de sur un heptagone")](arcs-images/roundedheptagon-large.png#lightbox "Triple capture d’écran de la page de l’arrondi de sur un heptagone")

## <a name="the-elliptical-arc"></a>L’Arc elliptique

L’arc elliptique est ajouté à un chemin d’accès avec un appel à la [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) méthode qui a deux `SKPoint` paramètres, ou le [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) surcharge avec distinct X et Y coordonnées :

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

L’arc elliptique est cohérent avec la [arc elliptique](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) inclus dans les graphiques SVG (Scalable Vector) et la plateforme Windows universelle [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) classe.

Ces `ArcTo` méthodes dessiner un arc entre deux points, qui est le point actuel du contour, et le dernier paramètre de la `ArcTo` (méthode) (le `xy` paramètre ou particulières `x` et `y` paramètres) :

![](arcs-images/ellipticalarcpoints.png "Les deux points qui a défini un arc elliptique")

Le premier paramètre de point pour le `ArcTo` (méthode) (`r`, ou `rx` et `ry`) n’est pas un point du tout, mais au lieu de cela spécifie le rayon horizontal et vertical d’une ellipse ;

![](arcs-images/ellipticalarcellipse.png "L’ellipse définie par un arc elliptique")

Le `xAxisRotate` paramètre est le nombre de degrés dans le sens horaire pour faire pivoter cette ellipse :

![](arcs-images/ellipticalarctiltedellipse.png "L’ellipse inclinée défini par un arc elliptique")

Si cette ellipse inclinée est ensuite placée afin qu’il touche les deux points, les points sont connectés par deux façons différentes :

![](arcs-images/ellipticalarcellipse1.png "Le premier ensemble d’arcs elliptiques")

Ces deux arcs puissent être distinguées de deux manières : l’arc supérieur est supérieure à l’arc en bas, et comme l’arc est dessiné de gauche à droite, l’arc supérieur est dessiné dans le sens horaire dans pendant l’arc bas est dessiné dans un sens inverse des aiguilles.

Il est également possible en fonction de l’ellipse entre les deux points d’une autre façon :

![](arcs-images/ellipticalarcellipse2.png "Le deuxième ensemble d’arcs elliptiques")

Il existe désormais un arc plus petit en premier est dessiné dans le sens horaire et un plus grand arc qui est dessiné en bas des aiguilles.

Ces deux points peuvent par conséquent être connectés à un arc défini par les points de suspension bascule dans un total de quatre manières :

![](arcs-images/ellipticalarccolors.png "Toutes les quatre arcs elliptiques")

Ces quatre arcs sont distinguent par les quatre combinaisons de le [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) et [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) des arguments de type énumération à la `ArcTo` méthode :

- rouge : SKPathArcSize.Large et SKPathDirection.Clockwise
- vert : SKPathArcSize.Small et SKPathDirection.Clockwise
- bleu : SKPathArcSize.Small et SKPathDirection.CounterClockwise
- magenta : SKPathArcSize.Large et SKPathDirection.CounterClockwise

Si les points de suspension inclinée n’est pas assez grande pour tenir entre les deux points, puis il est uniforme à l’échelle jusqu'à ce que sa taille est suffisante. Seuls deux arcs de cercle uniques connectent les deux points dans ce cas. Ceux-ci peuvent être distingués avec la `SKPathDirection` paramètre.

Bien que cette approche de la définition d’un arc semble complexe sur la première rencontre, il est la seule approche qui permet de définir un arc avec une ellipse avec une rotation, et il est souvent plus simple lorsque vous avez besoin intégrer des arcs de cercle avec d’autres parties du contour.

Le **Arc elliptique** page permet de définir de manière interactive les deux points et la taille et la rotation de l’ellipse. Le `EllipticalArcPage` dérive de la classe `InteractivePage`et le `PaintSurface` gestionnaire dans le [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) fichier code-behind dessine les quatre façons :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Ici, il s’exécute sur les plateformes de trois :

[![](arcs-images/ellipticalarc-small.png "Capture d’écran de triple de la page de l’Arc elliptique")](arcs-images/ellipticalarc-large.png#lightbox "Triple capture d’écran de la page de l’Arc elliptique")

Le **d’arc de cercle infini** page utilise l’arc elliptique pour dessiner un signe de l’infini. Le signe de l’infini est basé sur deux cercles à rayon de 100 unités séparées par des unités de 100 :

![](arcs-images/infinitycircles.png "Deux cercles")

Deux lignes traversant eux sont tangent à deux cercles :

![](arcs-images/infinitycircleslines.png "Deux cercles avec les tangentes")

Le signe de l’infini est une combinaison des éléments de ces cercles et les deux lignes. Pour utiliser l’arc elliptique pour dessiner le signe de l’infini, les coordonnées où les deux lignes sont tangente milieux doivent être déterminées.

Construire un rectangle à droite dans un cercles de :

![](arcs-images/infinitytriangle.png "Deux cercles avec tangentes et cercle incorporé")

Le rayon du cercle est de 100 unités et l’hypoténuse du triangle est 150 unités, par conséquent, l’angle α est l’arc sinus (sinus inverse) de 100 divisé par 150 ou 41,8 degrés. La longueur de l’autre côté du triangle est 150 fois le cosinus de degrés 41,8 ou 112, ce qui peut également être calculée par le théorème de Pythagore.

Les coordonnées du point tangent peuvent ensuite être calculées à l’aide de ces informations :

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Les quatre points de tangentes sont tout ce qui est nécessaire d’établir une connexion de l’infini centrée sur le point (0, 0) à rayon du cercle de 100 :

![](arcs-images/infinitycoordinates.png "Deux cercles, avec les coordonnées et les lignes de tangente")

Le `PaintSurface` gestionnaire dans le [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) classe positionne le signe de l’infini afin que le (0, 0) point se trouve dans le centre de la page et ajuste le chemin d’accès à la taille d’écran :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
                              info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Le code utilise le `Bounds` propriété du `SKPath` pour déterminer les dimensions du sinus de l’infini à l’échelle à la taille de la zone de dessin :

[![](arcs-images/arcinfinity-small.png "Capture d’écran de triple de la page d’arc de cercle infini")](arcs-images/arcinfinity-large.png#lightbox "Triple capture d’écran de la page de l’infini d’Arc")

Le résultat paraît peu, ce qui suggère que le `Bounds` propriété du `SKPath` signale une taille supérieure au chemin d’accès.

En interne, Skia effectue une approximation de l’arc à l’aide de plusieurs courbes Bézier quadratiques. Ces courbes (comme vous le verrez dans la section suivante) contiennent des points de contrôle qui régissent la façon dont la courbe est dessinée, mais ne font pas partie de la courbe de rendu. Le `Bounds` propriété inclut ces points de contrôle.

Pour obtenir un ajustement plus strict, utilisez le `TightBounds` propriété, ce qui exclut les points de contrôle. Voici le programme en cours d’exécution en mode paysage et en utilisant le `TightBounds` propriété pour obtenir les limites de chemin d’accès :

[![](arcs-images/arcinfinitytightbounds-small.png "Capture d’écran de triple de la page d’arc de cercle infini avec des limites étroits")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Triple capture d’écran de la page d’arc de cercle infini avec des limites étroits")

Bien que les connexions entre les lignes droites et d’arcs de cercle sont mathématiquement smooth, la modification de l’arc linéaire peut sembler un peu brusque. Un signe de l’infini meilleure est présenté dans la page suivante.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
