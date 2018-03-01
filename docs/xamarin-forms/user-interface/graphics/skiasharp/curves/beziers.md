---
title: "Trois Types de courbes Bézier"
description: "Découvrir comment utiliser SkiaSharp pour restituer des courbes de Bézier cubiques, quadratiques et coniques"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 8fdebd00b0b33c184337eacd0836dd81a19f161b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="three-types-of-bzier-curves"></a>Trois Types de courbes Bézier

_Découvrir comment utiliser SkiaSharp pour restituer des courbes de Bézier cubiques, quadratiques et coniques_

La courbe de Bézier est nommée d’après la Pierre Bézier (1910 : 1999), un ingénieur Français de la société automobile Renault, ce qui a utilisé la courbe pour la conception assisté par ordinateur de corps de la voiture.

Courbes Bézier sont connus pour être particulièrement bien adapté à la conception interactive : ils sont bien au comportement médiocre & #x 2014 ; en d’autres termes, il n’existe pas de singularités qui provoquent la courbe infinie ou lourde & #x 2014 ; et ils sont généralement esthétiquement. Contours de caractère des polices basées sur l’ordinateur sont généralement définies avec les courbes Bézier :

![](beziers-images/beziersample.png "Une courbe de Bézier exemple")

L’article de Wikipedia sur [courbe de Bézier](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) contient des informations utiles. Le terme *courbe de Bézier* fait référence à une famille de courbes similaire. SkiaSharp prend en charge trois types de courbes Bézier, appelées la *cubique*, le *quadratique*et le *conique*. Le conique est également connue sous le *quadratic rationnelle*.

## <a name="the-cubic-bzier-curve"></a>La courbe de Bézier cubique

La courbe cubique est le type de courbe de Bézier considérer la plupart des développeurs lors de l’objet des courbes Bézier s’affiche.

Vous pouvez ajouter une courbe de Bézier cubique à un `SKPath` à l’aide de l’objet le [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) méthode avec trois `SKPoint` paramètres, ou le [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) surcharge avec distinct `x` et `y` paramètres :

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

La courbe commence au point actuel du contour. La courbe de Bézier cubique complète est définie par quatre points :

- point de départ : actuel point dans le profil, ou (0, 0) si `MoveTo` n’a pas été appelée
- premier point de contrôle : `point1` dans le `CubicTo` appeler
- deuxième point de contrôle : `point2` dans le `CubicTo` appeler
- point de terminaison : `point3` dans le `CubicTo` appeler

La courbe résultante commence au point de départ et se termine au point de terminaison. La courbe généralement ne passe pas par deux points de contrôle ; au lieu de cela, ils fonctionnent bien aimants similaire pour extraire la courbe vers les.

La meilleure façon de faire une idée de la courbe de Bézier cubique est par expérimentation. Ceci est l’objectif de la **courbe de Bézier** page, qui dérive de `InteractivePage`. Le [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) fichier instancie le `SKCanvasView` et un `TouchEffect`. Le [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) fichier code-behind crée quatre `TouchPoint` objets dans son constructeur. Le `PaintSurface` Gestionnaire d’événements crée un `SKPath` pour restituer une courbe de Bézier basée sur les quatre `TouchPoint` objets et dessine également les lignes en pointillés de tangente à partir des points de contrôle pour les points de terminaison :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Ici, il s’exécute sur toutes les plateformes de trois :

[![](beziers-images/beziercurve-small.png "Capture d’écran de triple de la page de la courbe de Bézier")](beziers-images/beziercurve-large.png "Triple capture d’écran de la page de la courbe de Bézier")

Mathématiquement, la courbe est une représentation polynomiale cubique. La courbe entre en intersection avec une ligne droite à trois points au maximum. Au point de départ, la courbe est toujours tangent et dans la même direction que, une ligne droite à partir du début pointez le premier point de contrôle. Au point de terminaison, la courbe est toujours tangent et dans la même direction que, une ligne droite à partir du deuxième contrôle pointez le point de terminaison.

La courbe de Bézier cubique est toujours limitée par un quadrilatère convexe reliant les quatre points. Cela s’appelle un *forme convexe*. Si les points de contrôle se trouvent sur la ligne droite entre le début et le point de terminaison, la courbe de Bézier est rendu sous la forme d’une ligne droite. Mais la courbe peut également se croiser lui-même, comme la capture d’écran de l’appareil Windows Mobile.

Un profil de chemin d’accès peut contenir plusieurs courbes de Bézier cubiques connectées, mais la connexion entre deux courbes Bézier cubiques sera lisse uniquement si les trois points suivants sont colinéaires (autrement dit, se trouve sur une ligne droite) :

- le deuxième point de contrôle de la courbe en premier
- le point de terminaison de la première courbe, qui est également le point de départ de la deuxième courbe
- le premier point de contrôle de la deuxième courbe

Dans l’article suivant sur [ **données de chemin d’accès SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) vous allez découvrir une fonctionnalité pour faciliter la définition de smooth courbes Bézier connectées.

Il est parfois utile de connaître les équations paramétriques sous-jacent qui restituent une courbe de Bézier cubique. Pour *t* comprises entre 0 et 1, les équations paramétriques sont les suivantes :

x(t) = (1 – t) ³x₀ + 3P (1 – t) ²x₁ + 3t² (1 – t) x₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3P (1 – t) ²y₁ + 3t² (1 – t) y₂ + t³y₃

L’exposant le plus élevé de 3 confirme qu’il s’agit des POLYNÔMES cubiques. Il est facile de vérifier que quand `t` est égal à 0, le point est (x₀ y₀), qui est le point de départ et à quel moment `t` est égal à 1, le point est (x₃ y₃), qui est le point de terminaison. Près le point de départ (pour les valeurs basses de `t`), le premier point de contrôle (x₁, y₁) a un fort effet et près le point de terminaison (les valeurs élevées de ' t ») le deuxième point de contrôle (x₂, y₂) a une répercussion.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Rapprochement de courbe de Bézier Arcs de cercle

Il est parfois commode d’utiliser une courbe de Bézier pour restituer un arc de cercle. Une courbe de Bézier cubique peut se rapprocher un arc de cercle très bien jusqu'à un quart cercle, quatre courbes Bézier connectées peuvent définir un cercle complet. Ce rapprochement est décrite dans les deux articles publiés il y a plus de 25 ans :

> Tor Dokken, et autres, « Bonne Approximation des cercles par des courbes Bézier courbure continue, » *ordinateur assistée par géométrique conception 7* (1990), 33-41.

> Michael Goldapp, « Rapprochement d’Arcs de cercle en POLYNÔMES cubiques, » *conception géométrique 8 assistée par ordinateur* (1991), 227-238.

Le diagramme suivant montre quatre points étiquetés `pto`, `pt1`, `pt2`, et `pt3` définition d’une courbe de Bézier (indiquée en rouge) qui se rapproche un arc de cercle :

![](beziers-images/bezierarc45.png "Approximation d’un arc circulaire avec une courbe de Bézier")

Les lignes du début et les points de terminaison pour les points de contrôle sont tangente au cercle et à la courbe de Bézier, et ils ont une longueur de *L*. Le premier article cité ci-dessus indique que la courbe de Bézier meilleures rapproche un arc lorsque cette longueur *L* est calculé comme suit :

L = 4 × tan(α / 4) / 3

L’illustration montre un angle de 45 degrés, donc L est égal à 0,265. Dans le code, cette valeur est multipliée par le rayon du cercle souhaité.

Le **Bézier en Arc de cercle** page vous permet de faire des essais avec la définition d’une courbe de Bézier pour rapprocher un arc des angles compris jusqu'à 180 degrés. Le [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) fichier instancie le `SKCanvasView` et un `Slider` pour la sélection de l’angle. Le `PaintSurface` Gestionnaire d’événements dans le [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) fichier code-behind utilise une transformation pour définir le point (0, 0) au centre de la zone de dessin. Dessine un cercle centré sur ce point de comparaison, elle calcule ensuite les deux points de contrôle de la courbe de Bézier :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
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

```

Le début et points de terminaison (`point0` et `point3`) sont calculées selon les équations paramétriques normales du cercle. Étant donné que le cercle est centré sur (0, 0), ces points peuvent également être traitées comme des vecteurs radiales à partir du centre du cercle à la circonférence. Les points de contrôle se trouvent sur les lignes de la tangente à un cercle, afin qu’ils soient perpendiculairement à ces vecteurs radiales. Un vecteur à un angle droit est simplement le vecteur d’origine avec les coordonnées X et Y échangées et qu’un d’eux apportées négatif.

Voici le programme en cours d’exécution sur les trois plateformes avec trois différents angles :

[![](beziers-images/beziercirculararc-small.png "Capture d’écran de triple de la page de Bézier en Arc de cercle")](beziers-images/beziercirculararc-large.png "Triple capture d’écran de la page de Bézier en Arc de cercle")

Examinez attentivement l’écran de Windows Mobile, et vous verrez que la courbe de Bézier diffère notamment un demi-cercle lors de l’angle est de 180 degrés, mais l’écran d’iOS indique qu’il semble s’ajuster un cercle trimestre tout à fait correctement lors de l’angle est de 90 degrés.

Il est facile de calculer les coordonnées de deux points de contrôle lorsque le quart de cercle est orienté comme suit :

![](beziers-images/bezierarc90.png "Approximation d’un quart de cercle avec une courbe de Bézier")

Si le rayon du cercle est de 100, puis *L* est 55, et qui est un numéro facile à mémoriser.

Le **mise au carré le cercle** page réalise une animation figure entre un cercle et un carré. Le cercle est estimée par quatre courbes Bézier dont les coordonnées sont affichées dans la première colonne de cette définition de tableau dans le [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) classe :

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

La deuxième colonne contient les coordonnées de quatre courbes Bézier qui définissent un carré dont la zone est environ le même que la zone du cercle. (Dessin d’un carré avec la *exacte* au problème géométrique insoluble classique de la zone comme un cercle donné est [mise au carré le cercle](https://en.wikipedia.org/wiki/Squaring_the_circle).) Pour le rendu d’un carré avec des courbes Bézier, deux points de contrôle pour chaque courbe sont les mêmes, et elles sont colinéaires aux points de terminaison, de début et la courbe de Bézier est restituée sous la forme d’une ligne droite.

La troisième colonne du tableau est pour les valeurs interpolées d’une animation. La page définit un minuteur pour 16 millisecondes et le `PaintSurface` gestionnaire est appelé avec ce taux de :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Les points sont interpolés selon une valeur sinusoïdal OSCILLANTE de `t`. Les points interpolées sont ensuite utilisés pour construire une série de quatre courbes Bézier connectées. Voici l’animation en cours d’exécution sur les trois plateformes indiquant la progression à partir d’un cercle, un carré :

[![](beziers-images/squaringthecircle-small.png "Capture d’écran de triple de la Squaring la page cercle")](beziers-images/squaringthecircle-large.png "Triple capture d’écran de la Squaring la page de cercle")

Ce type d’une animation est impossible sans courbes par algorithme suffisamment flexibles pour être restitué sous la forme des arcs de cercle et des lignes droites.

Le **Bézier infini** page tire également parti de la capacité d’une courbe de Bézier à rapprocher un arc. Voici le `PaintSurface` gestionnaire à partir de la [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) classe :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
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

Il peut être un bon exercice pour tracer ces coordonnées sur du papier graphique pour voir comment elles sont liées. Le signe de l’infini est centré autour du point (0, 0), et les deux boucles ont des centres de (–150, 0) et (150, 0) et de rayons de 100. Dans la série de `CubicTo` des commandes, vous pouvez voir les coordonnées de points de contrôle sur les valeurs de –95 et –205 X (ces valeurs sont –150 signes plus et moins 55), 205 et 95 (150 plus et moins 55), ainsi que les 250 et –250 des côtés gauche et droite. La seule exception est lorsque le signe de l’infini croise elle-même dans le centre. Dans ce cas, les points de contrôle ont des coordonnées avec une combinaison des 50 -50 à Aplanissez la courbe près du centre.

Voici le signe de l’infini sur toutes les plateformes de trois :

[![](beziers-images/bezierinfinity-small.png "Capture d’écran de triple de la page de Bézier infini")](beziers-images/bezierinfinity-large.png "Triple capture d’écran de la page de l’infini de Bézier")

Il s’agit d’un peu plus lisse en direction du Centre le signe de l’infini restitué par le **d’arc de cercle infini** page à partir de la [ **trois comment dessiner un Arc** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) l’article.

## <a name="the-quadratic-bzier-curve"></a>La courbe de Bézier

La courbe de Bézier quadratique a uniquement un point de contrôle et de la courbe est définie par les points de seulement trois : le point de départ, le point de contrôle et le point de terminaison. Les équations paramétriques sont très semblables à la courbe de Bézier cubique, sauf que l’exposant la plus élevée est 2, par conséquent, la courbe est une représentation polynomiale quadratique :

x(t) = (1 – t) ²x₀ + 2 t (1 – t) x₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2 t (1 – t) y₁ + t²y₂

Pour ajouter une courbe de Bézier à un chemin d’accès, utilisez le [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) méthode ou la [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) surcharge avec distinct `x` et `y` coordonnées :

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Les méthodes d’ajoutent d’une courbe à partir de la position actuelle pour `point2` avec `point1` en tant que le point de contrôle.

Vous pouvez expérimenter des courbes Bézier quadratiques avec la **courbe quadratique** page, ce qui est très similaire à la **courbe de Bézier** page sauf qu’il possède uniquement trois points tactiles. Voici le `PaintSurface` gestionnaire dans le [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) fichier code-behind :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Et ici, il s’exécute sur toutes les plateformes de trois :

[![](beziers-images/quadraticcurve-small.png "Capture d’écran de triple de la page de la courbe quadratique")](beziers-images/quadraticcurve-large.png "Triple capture d’écran de la page de la courbe quadratique")

Les traits pointillés sont la tangente de la courbe en le point de départ et le point de terminaison et respecter au niveau du point de contrôle.

La courbe de Bézier quadratique est valable si vous avez besoin d’une courbe d’une forme en générale, mais que vous préférez la commodité de simplement un point de contrôle au lieu de deux. La courbe de Bézier quadratique rend plus efficacement que n’importe quel autre courbe, c’est pourquoi il est utilisé en interne dans Skia pour restituer des arcs elliptiques.

Toutefois, la forme d’une courbe de Bézier quadratique n’est pas elliptique, c’est pourquoi plusieurs points de Bézier quadratiques sont requis pour se rapprocher un arc elliptique. La courbe de Bézier quadratique est à la place un segment d’un parabole.

## <a name="the-conic-bzier-curve"></a>La courbe de Bézier conique

La courbe de Bézier conique & #x 2014 ; également connu sous la courbe de Bézier quadratique rationnelle & #x 2014 ; est un ajout relativement récent de la famille des courbes Bézier. Comme la courbe de Bézier quadratique, la courbe de Bézier quadratique rationnelle implique un point de départ, un point de terminaison et un point de contrôle. Mais la courbe de Bézier quadratique rationnelle nécessite également un *poids* valeur. Il est appelé un *rationnelle* quadratique, car les formules paramétriques impliquent des rapports.

Les équations paramétriques pour X et Y sont des rapports qui partagent le même dénominateur. Voici l’équation pour le dénominateur de *t* allant de 0 à 1 et une valeur de poids de *w*:

Ät = (1 – t) ² + 2wt(1 – t) + t²

En théorie, un quadratic rationnelle peut impliquer des trois valeurs de poids distincts, un pour chacune des trois conditions, mais ceux-ci peuvent être simplifiées pour qu’une seule valeur de poids sur le terme intermédiaire.

Les équations paramétriques des coordonnées X et Y sont semblables aux équations paramétriques pour la courbe de Bézier quadratique, sauf que le terme intermédiaire inclut également la valeur de pondération et l’expression est divisée par le dénominateur :

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) x₁ + t²x₂)) ÷ Ät

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) y₁ + t²y₂)) ÷ Ät

Des courbes Bézier quadratiques rationnelles sont également appelées *conics* , car ils peuvent représenter exactement les segments de toute la section de conique & #x 2014 ; hyperboles, paraboles ou, ellipses et des cercles.

Pour ajouter une courbe de Bézier rationnelle à un chemin d’accès, utilisez le [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) méthode ou la [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) surcharge avec distinct `x` et `y` coordonnées :

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Notez la dernière `weight` paramètre.

Le **courbe conique** page vous permet de faire des essais avec ces courbes. La classe `ConicCurvePage` dérive de la classe `InteractivePage`. Le [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) fichier instancie un `Slider` pour sélectionner une valeur de pondération entre – 2 et 2. Le [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) fichier code-behind crée trois `TouchPoint` objets et le `PaintSurface` Gestionnaire restitue simplement la courbe résultante avec les lignes tangentes au contrôle points suivants :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Ici, il s’exécute sur toutes les plateformes de trois :

[![](beziers-images/coniccurve-small.png "Capture d’écran de triple de la page de la courbe conique")](beziers-images/coniccurve-large.png "Triple capture d’écran de la page de la courbe conique")

Comme vous pouvez le voir, le point de contrôle semble pour extraire la courbe vers elle plus lorsque le poids est supérieur. Lorsque le poids est égal à zéro, la courbe devient une ligne droite entre le point de départ pour le point de terminaison.

En théorie, négatif sont autorisés et provoquer la courbe à plier *absent* à partir du point de contrôle. Toutefois, des poids de -1 ou en dessous cause dénominateur dans les équations paramétriques négatif pour certaines valeurs particulières de *t*. Probablement pour cette raison, négatif est ignorés dans le `ConicTo` méthodes. Le **courbe conique** programme vous permet de définir les pondérations négatives, mais comme vous pouvez le voir en expérimentant, négatif ont le même effet qu’un poids égal à zéro et entraîner une ligne droite doit être restitué.

Il est très facile de dériver le point de contrôle et le poids à utiliser le `ConicTo` méthode pour dessiner un arc de cercle jusqu'à (mais non compris) un demi-cercle. Dans le diagramme suivant, tangentes de début et de points de terminaison répond au niveau du point de contrôle.

![](beziers-images/conicarc.png "Un rendu de l’arc conique d’un arc de cercle")

Vous pouvez utiliser trigonométriques pour déterminer la distance du point de contrôle à partir du centre du cercle : il est le rayon du cercle divisé par le cosinus de la moitié de l’angle α. Pour dessiner un arc circulaire entre le début et les points de terminaison, définissez le poids de ce même cosinus de l’angle de moitié. Notez que si l’angle est de 180 degrés, puis les lignes tangentes ne rencontrez jamais et que le poids est égal à zéro. Mais pour les angles inférieur à 180 degrés, math fonctionne correctement.

Le **d’Arc de cercle conique** page illustre cela. Le [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) fichier instancie un `Slider` pour la sélection de l’angle. Le `PaintSurface` gestionnaire dans le [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) fichier code-behind calcule le point de contrôle et le poids :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Comme vous pouvez le voir, il n’existe aucune différence visuelle entre les `ConicTo` chemin d’accès indiqué en rouge et le cercle sous-jacent affichés pour référence :

[![](beziers-images/coniccirculararc-small.png "Capture d’écran de triple de la page d’Arc de cercle conique")](beziers-images/coniccirculararc-large.png "Triple capture d’écran de la page conique d’Arc de cercle")

Mais la valeur de l’angle de 180 degrés et le basculement mathématiques.

Il est dans ce cas regrettable qui `ConicTo` ne prend pas en charge les pondérations négatives, car en théorie (basée sur les équations paramétriques), le cercle peut être effectué avec un autre appel à `ConicTo` avec les mêmes points, mais une valeur négative du poids. Cela permet la création d’un cercle complet avec deux `ConicTo` courbes selon un angle entre (mais sans inclure) zéro degrés et 180 degrés.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
