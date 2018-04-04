---
title: Dessiner la géométrie avec CCDrawNode
description: CCDrawNode fournit des méthodes pour dessiner des objets de type primitif tels que des lignes, des cercles et des triangles.
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 2a4278f97a69ef9eb705c6fedf1a5c7ec98875b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Dessiner la géométrie avec CCDrawNode

_`CCDrawNode` Fournit des méthodes pour dessiner des objets de type primitif tels que des lignes, des cercles et des triangles._

La `CCDrawNode` classe dans CocosSharp fournit plusieurs méthodes pour dessiner des formes géométriques courantes. Il hérite de la `CCNode` de classe et est généralement ajoutée au `CCLayer` instances. Ce guide explique comment utiliser `CCDrawNode` instances pour effectuer le rendu personnalisé. Il fournit également une liste complète des fonctions de dessin disponibles avec des captures d’écran et des exemples de code.


## <a name="creating-a-ccdrawnode"></a>Création d’un CCDrawNode

La `CCDrawNode` classe peut être utilisée pour dessiner des objets géométriques telles que des cercles, des rectangles et des lignes. Par exemple, l’exemple de code suivant montre comment créer un `CCDrawNode` instance qui dessine un cercle dans un `CCLayer` classe d’implémentation :


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Ce code génère le cercle suivant lors de l’exécution :

![](ccdrawnode-images/image1.png "Ce code génère ce cercle lors de l’exécution")


## <a name="draw-method-details"></a>Dessiner les détails de méthode

Examinons quelques détails relatifs au dessin avec un `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Les méthodes sont des positions par rapport à la CCDrawNode de dessin

Toutes les méthodes de dessin requièrent une valeur de la position d’au moins un pour le dessin. Cette valeur de position est relative à la `CCDrawNode` instance. Cela signifie que la `CCDrawNode` lui-même a une position, et toutes les dessiner des appels effectués sur le `CCDrawNode` également prendre une ou plusieurs valeurs de position. Pour comprendre comment ces valeurs sont combinées, examinons quelques exemples.

Tout d’abord, nous allons examiner la `DrawCircle` exemple ci-dessus :


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

Dans ce cas, le `CCDrawNode` est positionné à (100,100), et le cercle dessiné à (0,0) relatif à la `CCDrawNode`, se traduisant par le cercle en cours centrés 100 pixels de haut et à droite de l’angle inférieur gauche de l’écran de jeu.

Le `CCDrawNode` peuvent également être positionnées à l’origine (bas à gauche de l’écran), sur le cercle pour les décalages de partie de confiance :


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

Le code situé au-dessus des résultats dans le centre du cercle à 50 unités (`drawNode.PositionX` + la `CCPoint.X`) à droite du côté gauche de l’écran, 60 (`drawNode.PositionY` + la `CCPoint.Y`) unités au-dessus du bas de l’écran.

Une fois qu’une méthode de dessin a été appelée, l’objet dessiné ne peut pas être modifié, sauf si le `CCDrawNode.Clear` est appelée, tout le repositionnement doit être effectuée sur le `CCDrawNode` lui-même.

Objets dessinés par `CCNodes` sont également affectés par la `CCNode` l’instance `Rotation` et `Scale` propriétés.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Les méthodes de dessin n’avez pas besoin d’être appelée à chaque frame

Les méthodes de dessin doivent être appelée qu’une seule fois pour créer un élément visuel persistant. Dans l’exemple ci-dessus, l’appel à `DrawCircle` dans le constructeur de la `GameLayer` – `DrawCircle` n'a pas besoin d’être appelée à chaque image pour redessiner le cercle lorsque l’écran est actualisé.

Cela diffère des méthodes de dessin dans MonoGame, qui sera rendu quelque chose à l’écran pour que le frame, et qui doit être appelé à chaque image.

Si les méthodes de dessin sont appelées chaque frame, les objets seront accumulent finalement à l’intérieur de l’appel `CCDrawNode` instance, ce qui entraîne une baisse des taux de trames comme plusieurs objets sont dessinés.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Chaque CCDrawNode prend en charge plusieurs appels de dessin

`CCDrawNode` instances peuvent être utilisées pour dessiner des formes multiples. Ainsi, les objets visuels complexes être entourés dans un seul objet. Par exemple, le code suivant peut servir à rendre plusieurs cercles avec une `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Ainsi, le graphique suivant :

![](ccdrawnode-images/image2.png "Il en résulte dans ce graphique")


## <a name="draw-call-examples"></a>Exemples d’appels de dessin

Les appels de dessin suivants sont disponibles dans `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` Crée une courbe via un nombre variable de points. 

Le `config` paramètre définit les points de transmettre la spline. L’exemple ci-dessous montre une courbe passant par quatre points.

Le `tension` paramètre contrôle pointu comment ou round les points sur la spline s’affichent. A `tension` la valeur 0 entraîne une spline en arc et un `tension` la valeur 1 entraîne une spline dessinée par des lignes droites et les bords de disque durs.

Bien que des lignes, splines CocosSharp dessine splines avec des lignes droites. Le `segments` paramètre contrôle le nombre de segments pour utiliser pour dessiner la spline. Un plus grand nombre entraîne une douceur courbe spline au détriment des performances. 

Exemple de code :


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Le paramètre de segments contrôle le nombre de segments pour utiliser pour dessiner la spline")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` Crée une courbe via un nombre variable de points, semblables à `DrawCardinalLine`. Cette méthode n’inclut pas un paramètre de tension.

Exemple de code :

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom crée une courbe via un nombre variable de points, similaires aux DrawCardinalLine")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` Crée un périmètre d’un cercle d’une donnée `radius`.

Exemple de code :

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle crée un périmètre d’un cercle d’un rayon donné")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` Dessine une courbe entre deux points, à l’aide de points de contrôle pour définir le chemin d’accès entre les deux points.

Exemple de code :

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier trace une courbe entre deux points")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` crée le contour d’un *ellipse*, qui est souvent appelé une ellipse (bien que les deux ne sont pas identiques géométrique). La forme de l’ellipse peut être définie par un `CCRect` instance.

Exemple de code :


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse crée le contour d’une ellipse, qui est souvent appelé une ellipse")


### <a name="drawline"></a>DrawLine

`DrawLine` se connecte à des points d’une ligne d’une largeur donnée. Cette méthode est similaire à `DrawSegment`, excepté qu’il crée des points de terminaison plats par opposition aux points de terminaison round.

Exemple de code :


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine se connecte à des points d’une ligne d’une largeur donnée")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` crée plusieurs lignes en vous connectant à chaque paire de points spécifiés par un `CCV3F_C4B` tableau. Le `CCV3F_C4B` struct contient des valeurs de position et la couleur. Le `verts` paramètre doit toujours contenir un nombre pair de points, telle que chaque ligne est définie par deux points.

Exemple de code :


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Le paramètre de sommet doit toujours contenir un nombre pair de points, telle que chaque ligne est définie par deux points")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` Crée un polygone rempli avec un contour de couleur et de largeur variable.

Exemple de code :


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon crée un polygone rempli avec un contour de couleur et de largeur variable")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` se connecte deux points d’une ligne. Il se comporte de la même `DrawCubicBezier` mais prend en charge uniquement un point de contrôle unique.

Exemple de code :


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier se connecte deux points d’une ligne")


### <a name="drawrect"></a>DrawRect

`DrawRect` Crée un rectangle rempli avec un contour de couleur et de largeur variable.

Exemple de code : 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect crée un rectangle rempli avec un contour de couleur et de largeur variable")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` se connecte deux points avec un trait de couleur et de largeur variable. Il est similaire à `DrawLine`, excepté qu’il crée des points de terminaison arrondis plutôt que des points de terminaison plats.

Exemple de code :


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment se connecte deux points avec un trait de couleur et de largeur variable")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` Crée un coin rempli d’une couleur donnée et le rayon.

Exemple de code :


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc crée un coin rempli d’une couleur donnée et le rayon")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` Crée un cercle rempli de rayon donné.

Exemple de code :


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle crée un cercle rempli d’un rayon donné")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` Crée une liste de triangles. Chaque facette est défini par trois `CCV3F_C4B` instances dans un tableau. Le nombre de sommets dans le tableau passé à la `verts` le paramètre doit être un multiple de trois. Notez que les seules informations contenues dans `CCV3F_C4B` est la position du sommet et leur couleur – la `DrawTriangleList` méthode ne prend pas en charge le dessin triangles avec des textures.

Exemple de code :


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList crée une liste de triangles")


## <a name="summary"></a>Récapitulatif

Ce guide explique comment créer un `CCDrawNode` et effectuer le rendu sur Primitives. Il fournit un exemple de chacun des appels de dessin.

## <a name="related-links"></a>Liens connexes

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Exemple complet](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
