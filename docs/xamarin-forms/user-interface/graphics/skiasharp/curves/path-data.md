---
title: Données de chemin d’accès SVG
description: Définir les chemins d’accès à l’aide de chaînes de texte dans le format Scalable Vector Graphics
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: 0fcef7ce40a575e015232e1ed5996e6b799dd085
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="svg-path-data"></a>Données de chemin d’accès SVG

_Définir les chemins d’accès à l’aide de chaînes de texte dans le format Scalable Vector Graphics_

La `SKPath` classe prend en charge la définition des objets de chemin d’accès complet à partir de chaînes de texte dans un format défini par la spécification de graphiques SVG (Scalable Vector). Plus loin dans cet article, vous allez voir comment vous pouvez représenter un chemin d’accès complet de ce type dans une chaîne de texte :

![](path-data-images/pathdatasample.png "Un chemin d’accès de l’exemple défini avec les données de chemin d’accès SVG")

SVG est un langage de programmation pour les pages web des graphiques basés sur XML. Étant donné que SVG doit autoriser les chemins d’accès à être défini dans le balisage, plutôt qu’une série d’appels de fonction, le SVG standard inclut une méthode très concise de la spécification d’un chemin d’accès graphique entière comme une chaîne de texte.

Dans SkiaSharp, ce format est dénommé « Chemin d’accès SVG-données ». Le format est également pris en charge dans les environnements programmation basée sur XAML de Windows, y compris Windows Presentation Foundation et la plateforme Windows universelle, où il est appelé le [Path Markup Syntax](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) ou [déplacer Dessinez la syntaxe des commandes](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Il peut également servir à un format d’échange pour les images de graphiques vectoriels, en particulier dans des fichiers texte tel que XML.

SkiaSharp définit deux méthodes avec les mots `SvgPathData` dans leur nom :

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statiques [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) méthode convertit une chaîne en un `SKPath` objet, tandis que [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) convertit un `SKPath` objet en une chaîne.

Voici une chaîne SVG pour une étoile à cinq branches centrée sur le point (0, 0) avec un rayon de 100 :

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Les lettres sont des commandes qui génèrent un `SKPath` objet. `M` Indique un `MoveTo` appeler, `L` est `LineTo`, et `Z` est `Close` pour fermer un contour. Chaque paire de numéros fournit une coordonnée X et Y d’un point. Notez que la `L` commande est suivie par plusieurs points séparés par des virgules. Dans une série de coordonnées et les points, des virgules et espaces blancs sont traités de manière identique. Certains programmeurs préfèrent placez des virgules entre les coordonnées X et Y plutôt qu’entre les points, mais des virgules ou des espaces sont requis uniquement pour éviter toute ambiguïté. Cela est parfaitement conforme :

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

La syntaxe des données de chemin d’accès SVG est documentée de façon formelle dans [8.3 de Section de la spécification SVG](http://www.w3.org/TR/SVG11/paths.html#PathData). Voici un résumé :

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Commence un nouveau profil dans le chemin d’accès en définissant la position actuelle. Les données de chemin d’accès doivent commencer par une `M` commande.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Cette commande ajoute une ligne droite (ou lignes) pour le chemin d’accès et définit la nouvelle position actuelle jusqu'à la fin de la dernière ligne. Vous pouvez suivre le `L` avec plusieurs paires de *x* et *y* coordonnées.

## <a name="horizontal-lineto"></a>**LineTo horizontale**

```csharp
H x ...
```

Cette commande ajoute une ligne horizontale pour le chemin d’accès et définit la nouvelle position actuelle jusqu'à la fin de la ligne. Vous pouvez suivre le `H` commande avec plusieurs *x* coordonnées, mais il ne rend pas beaucoup de sens.

## <a name="vertical-line"></a>**Ligne verticale**

```csharp
V y ...
```

Cette commande ajoute une ligne verticale pour le chemin d’accès et définit la nouvelle position actuelle jusqu'à la fin de la ligne.

## <a name="close"></a>**Fermer**

```csharp
Z
```

Le `C` commande ferme le contour en ajoutant une ligne droite à partir de la position actuelle vers le début du contour.

## <a name="arcto"></a>**ArcTo**

La commande pour ajouter un arc elliptique pour le profil est de loin la commande plus complexe dans la spécification de données de chemin d’accès entier SVG. Il s’agit de la seule commande dans lequel nombres peuvent représenter un élément autre que les valeurs de coordonnées :

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

Le *rx* et *rapide* paramètres sont les rayons horizontales et verticales de l’ellipse. Le *angle de rotation* est dans le sens horaire en degrés.

Définir le *volumineux qu’un indicateur d’arc de cercle* 1 de l’arc volumineux ou 0 pour le petit arc.

Définir le *balayage-indicateur* 1 pour vers la droite et la valeur 0 pour aiguilles.

L’arc est dessiné au point (*x*, *y*), qui devient la nouvelle position actuelle.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Cette commande ajoute une courbe de Bézier cubique à partir de la position actuelle pour (*x3*, *y3*), qui devient la nouvelle position actuelle. Les points (*x1*, *y1*) et (*x2*, *y2*) sont des points de contrôle.

Plusieurs courbes Bézier peuvent être spécifiés par un seul `C` commande. Le nombre de points doit être un multiple de 3.

Il existe également une commande de courbe de Bézier « bon » :

```csharp
S x2 y2 x3 y3 ...
```

Cette commande doit suivre une commande de courbe de Bézier régulière (mais qui ne soit pas strictement requis). La commande de courbe de Bézier smooth calcule le premier point de contrôle afin qu’il soit une réflexion du deuxième point de contrôle de la courbe de Bézier précédent autour de leur point commun. Ces trois points sont par conséquent colinéaires, et la connexion entre les deux courbes Bézier est arrondie.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Pour les courbes Bézier quadratiques, le nombre de points doit être un multiple de 2. Le point de contrôle (*x1*, *y1*) et le point de terminaison (et la nouvelle position en cours) (*x2*, *y2*)

Il existe également une commande de la courbe quadratique lisse :

```csharp
T x2 y2 ...
```

Le point de contrôle est calculé en fonction de point de contrôle de la courbe quadratique précédente.

Toutes ces commandes sont également disponibles dans les versions « relatives », où les points de coordonnées sont relatives à la position actuelle. Ces commandes relatifs commencent par des lettres minuscules, par exemple `c` plutôt que `C` pour la version relative de la commande de courbe de Bézier cubique.

Il s’agit de l’étendue de la définition de données de chemin d’accès SVG. Il n’existe aucune fonctionnalité pour répéter les groupes de commandes ou pour exécuter n’importe quel type de calcul. Commandes de `ConicTo` ou d’autres types de spécifications de l’arc ne sont pas disponibles.

La méthode statique [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) méthode attend une chaîne de commandes SVG valide. Si une erreur de syntaxe est détectée, la méthode retourne `null`. C’est l’indication des erreurs uniquement.

Le [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) méthode est utile pour obtenir des données de chemin d’accès SVG existant `SKPath` objet à transférer vers un autre programme ou à stocker dans un format de fichier texte tel que XML. (Le `ToSvgPathData` méthode n’est pas illustrée dans l’exemple de code dans cet article.) Faire *pas* attendent `ToSvgPathData` pour retourner une chaîne correspondant exactement aux appels de méthode qui a créé le chemin d’accès. En particulier, vous allez découvrir que les arcs sont convertis en plusieurs `QuadTo` commandes, et comment ils apparaissent dans les données de chemin d’accès retournées par `ToSvgPathData`.

Le **chemin d’accès données Hello** page sorts le mot « HELLO » à l’aide des données de chemin d’accès SVG. À la fois le `SKPath` et `SKPaint` objets sont définis en tant que champs dans le [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) classe :

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Le chemin d’accès de définition de la chaîne de texte commence dans le coin supérieur gauche au niveau du point (0, 0). Chaque lettre est 50 unités de large et hauteur de 100 unités, et des lettres sont séparés par un autre 25 unités, ce qui signifie que le chemin d’accès complet est de 350 unités large.

« H » de « Hello » est composé de trois profils d’une ligne, alors que « E » est deux courbes de Bézier cubiques connectées. Notez que la `C` commande est suivie de six points, et deux des points de contrôle ont des coordonnées Y de -10 et 110, ce qui les met en dehors de la plage des coordonnées Y des autres lettres. Le « L » est de deux lignes connectées, tandis que la ' o ' est une ellipse qui est rendue avec un `A` commande.

Notez que le `M` commande qui commence au contour dernière définit la position au point (350, 50), qui est le centre vertical de la gauche de la ' o '. Comme indiqué par le premier suivant numéros le `A` de commande, l’ellipse a un rayon horizontal du 25 et un rayon vertical de 50. Le point de terminaison est indiqué par la dernière paire de nombres dans la `A` commande, qui représente le point (300, 49,9). Qui est délibérément légèrement différent du point de départ. Si le point de terminaison est défini comme égal au point de départ, l’arc n’est pas rendu. Pour dessiner une ellipse complète, vous devez définir le point de terminaison fermer pour (mais non égal à) le point de départ, ou vous devez utiliser deux ou plusieurs `A` des commandes, chaque partie de l’ellipse complète.

Vous pouvez souhaiter ajouter l’instruction suivante au constructeur de la page, puis définissez un point d’arrêt pour examiner la chaîne résultante :

```csharp
string str = helloPath.ToSvgPathData();
```

Vous allez découvrir que l’arc a été remplacée par une longue série de `Q` commandes pour une approximation fragmentaire de l’arc en utilisant des courbes Bézier quadratiques.

Le `PaintSurface` gestionnaire obtient les limites étroits du chemin d’accès, ce qui n’inclut pas les points de contrôle pour le « E » et « o ' courbes. Les trois transformations déplacement le centre du chemin d’accès au point (0, 0), le chemin d’accès à la taille de la zone de dessin (mais compte tenu également l’épaisseur du trait) de mise à l’échelle, puis déplacement le centre du chemin d’accès au centre de la zone de dessin :

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Le chemin d’accès remplit la zone de dessin, qui se présente plus raisonnable lorsqu’ils sont affichés en mode paysage :

[![](path-data-images/pathdatahello-small.png "Capture d’écran de triple de la page de chemin d’accès données Hello")](path-data-images/pathdatahello-large.png#lightbox "Triple capture d’écran de la page Hello chemin d’accès données")

Le **chemin d’accès données Cat** page est similaire. Les objets de chemin d’accès et peinture sont définis en tant que champs dans le [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) classe :

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Le début d’un Chat est un cercle, et ici qu’il est rendu avec deux `A` commandes, dont chacun Dessine un demi-cercle. Les deux `A` commandes de la tête définissent le rayon horizontal et vertical de 100. La première arc commence à (240, 100) et se termine à (240, 300), qui devient le point de départ de l’arc seconde qui se termine au (240, 100).

Les deux yeux sont également rendus avec deux `A` commandes et à l’instar de tête de sa, le deuxième `A` commande se termine sur le même point comme point de départ du premier `A` commande. Toutefois, ces paires de `A` commandes ne définissent pas une ellipse. Le de chaque arc est 40 unités et le rayon est également 40 unités, ce qui signifie que ces courbes ne sont pas semicircles complètes.

Le `PaintSurface` gestionnaire effectue les transformations similaires que l’exemple précédent, mais définit un seul `Scale` facteur pour conserver les proportions et de fournir une peu marge pour les valeurs de la cat toucher les côtés de l’écran :

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Voici le programme en cours d’exécution sur les trois plateformes :

[![](path-data-images/pathdatacat-small.png "Capture d’écran de triple de la page de chemin d’accès données Cat")](path-data-images/pathdatacat-large.png#lightbox "Triple capture d’écran de la page de chemin d’accès données Cat")

Normalement, quand un `SKPath` objet est défini comme un champ, les contours du chemin d’accès doivent être définis dans le constructeur ou une autre méthode. Toutefois, lorsque vous utilisez des données de chemin d’accès SVG, vous avez vu que le chemin d’accès peut être spécifiée entièrement dans la définition du champ.

La plus antérieure **l’horloge analogique laid** présenté dans le [ **la transformation de rotation** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) article affiche les aiguilles de l’horloge en tant que lignes simples. Le **assez l’horloge analogique** programme ci-dessous remplace ces lignes avec `SKPath` objets définis en tant que champs dans le [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) classe avec `SKPaint` objets :

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

Les aiguilles des heures et minutes ont maintenant encadré de zones, par conséquent, pour rendre les mains distincts entre eux, ils sont dessinés avec un contour noir et gris à l’aide du `handStrokePaint` et `handFillPaint` objets.

Dans la plus antérieure **l’horloge analogique laid** exemple, les petits cercles qui a marqué les heures et minutes dessinées dans une boucle. Dans ce **assez l’horloge analogique** exemple, une approche différente est utilisée : les marques d’heure sont des lignes en pointillés dessinées avec la `minuteMarkPaint` et `hourMarkPaint` objets :

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

Le [ **points et des tirets** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) guide décrit comment vous pouvez utiliser la `SKPathEffect.CreateDash` méthode pour créer une ligne en pointillés. Le premier argument est un `float` tableau qui a généralement de deux éléments : le premier élément est la longueur des tirets, et le deuxième élément est l’écart entre les tirets. Lorsque le `StrokeCap` est définie sur `SKStrokeCap.Round`, puis les arrondis de tiret allonger efficacement la longueur de tiret à l’épaisseur du trait sur les deux côtés de tiret. Par conséquent, la le premier élément du tableau 0 crée une ligne en pointillés.

La distance entre ces points est régie par le deuxième élément du tableau. Comme vous le verrez, ces deux `SKPaint` objets sont utilisés pour tracer des cercles avec un rayon de 90 unités. La circonférence de ce cercle est par conséquent 180π, ce qui signifie que les marques de 60 minutes doivent apparaître chaque unités 3pi, qui est la deuxième valeur de la `float` de tableau dans `minuteMarkPaint`. Les marques de douze heures doivent apparaître toutes les unités 15π, qui est la valeur de la seconde `float` tableau.

Le `PrettyAnalogClockPage` classe définit un minuteur pour invalider l’aire de toutes les 16 millisecondes et le `PaintSurface` gestionnaire est appelé à ce rythme. Les définitions antérieures de la `SKPath` et `SKPaint` objets permettent de très propre code de dessin :

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Quelque chose de spécial est effectuée avec l’aiguille, toutefois. Étant donné que l’horloge est mise à jour toutes les 16 millisecondes, le `Millisecond` propriété de la `DateTime` valeur peut potentiellement être utilisée pour animer un balayage ensuite manuellement au lieu d’un objet qui se déplace dans sauts discrets à partir de la seconde à la seconde. Mais ce code n’autorise pas le déplacement à être lisse. En revanche, il utilise le Xamarin.Forms [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) et [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) accélération des fonctions pour un autre type de déplacement de l’animation. Ces fonctions d’accélération provoquent l’aiguille déplacer de manière plus rapide &mdash; extraire un peu avant son déplacement, puis légèrement investissements sa destination, un effet qui malheureusement ne peut pas être reproduit dans ces captures d’écran statiques :

[![](path-data-images/prettyanalogclock-small.png "Capture d’écran de triple de la page de l’horloge analogique assez")](path-data-images/prettyanalogclock-large.png#lightbox "Triple capture d’écran de la page de l’horloge analogique assez")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
