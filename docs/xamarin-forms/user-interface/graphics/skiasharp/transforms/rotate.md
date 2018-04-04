---
title: La transformation de rotation
description: Explorer les effets et les animations possibles avec la transformation de rotation SkiaSharp
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 177437ef016a25849e7c34d0a26270ce14173b7d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="the-rotate-transform"></a>La transformation de rotation

_Explorer les effets et les animations possibles avec la transformation de rotation SkiaSharp_

Avec la transformation de rotation, les objets graphiques SkiaSharp interrompre libres de la contrainte d’alignement sur les axes horizontales et verticales :

![](rotate-images/rotateexample.png "Texte pivoté autour d’un centre")

Pour faire pivoter un objet de graphique autour du point (0, 0), SkiaSharp prend en charge à la fois un [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) (méthode) et un [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) méthode :

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Un cercle de 360 degrés est le même que 2π radians, il est facile d’effectuer une conversion entre les deux unités. Utilisez selon ce qui convient. Toutes les fonctions trigonométriques statiques [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) classe les unités de radians.

La rotation est dans le sens d’augmentation des angles. (Bien que la rotation sur le système de coordonnées cartésiennes aiguilles par convention, une rotation est cohérente avec les coordonnées Y augmentation continue vers le bas). Négatif angles et des angles supérieurs à 360 degrés sont autorisés.

Les formules de la transformation de rotation sont plus complexes que celles pour traduire et l’échelle. Pour un angle de α, les formules de transformation sont :

x' = x•cos(α) – y•sin(α)   

y` = x•sin(α) + y•cos(α)

Le **pivoter base** page montre le `RotateDegrees` (méthode). Le [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) fichier affiche du texte avec sa ligne de base centrée sur la page et la faire pivoter selon un `Slider` avec une plage de – 360 et 360. Voici la partie appropriée de la `PaintSurface` gestionnaire :

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Étant donné que la rotation est centrée autour de l’angle supérieur gauche de la zone de dessin, pour la plupart des angles défini dans ce programme, la rotation du texte désactivé l’écran :

[![](rotate-images/basicrotate-small.png "Capture d’écran de triple de la page pivoter base")](rotate-images/basicrotate-large.png#lightbox "Triple capture d’écran de la page de rotation de base")

Très souvent, vous souhaiterez faire pivoter un objet centrée autour d’un point pivot spécifiée à l’aide de ces versions de la [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) et [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) méthodes :

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

Le **centré faire pivoter** page est comparable à la **pivoter base** , sauf que la version développée de la `RotateDegrees` est utilisée pour définir le centre de rotation du même point utilisé pour positionner le texte :

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Désormais, le texte pivote autour du point utilisé pour positionner le texte, qui est le Centre horizontal de la ligne de base du texte :

[![](rotate-images/centeredrotate-small.png "Capture d’écran de triple de la page centrée pivoter")](rotate-images/centeredrotate-large.png#lightbox "Triple capture d’écran de la page centrée faire pivoter")

Comme avec la version centrée de la `Scale` (méthode), la version centrée de la `RotateDegrees` appel est un raccourci :

```csharp
RotateDegrees (degrees, px, py);
```

est équivalente à celle-ci :

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Vous allez découvrir que vous pouvez parfois combiner `Translate` appelle avec `Rotate` appels. Par exemple, voici la `RotateDegrees` et `DrawText` appelle le **centré pivoter** page ;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Le `RotateDegrees` appel équivaut à deux `Translate` appels et non-centré `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

Le `DrawText` appel pour afficher le texte à un emplacement particulier est équivalent à un `Translate` appeler pour cet emplacement suivi `DrawText` au point (0, 0) :

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Les deux consécutifs `Translate` appels annulent :

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Point de vue conceptuel, les deux sont appliquées dans l’ordre inverse de celui comment ils apparaissent dans le code. Le `DrawText` appel affiche le texte dans le coin supérieur gauche de la zone de dessin. Le `RotateDegrees` appel fait pivoter le texte par rapport à l’angle supérieur gauche. Le `Translate` appel déplace le texte au centre de la zone de dessin.

Il existe généralement plusieurs façons de combiner la rotation et la translation. Le **pivoter le texte** page crée l’affichage suivant :

[![](rotate-images/rotatedtext-small.png "Capture d’écran de triple de la page pivoter le texte")](rotate-images/rotatedtext-large.png#lightbox "Triple capture d’écran de la page de rotation de texte")

Voici le `PaintSurface` Gestionnaire de la [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) classe :

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

Le `xCenter` et `yCenter` valeurs indiquent au centre de la zone de dessin. Le `yText` valeur est un peu de décalage à partir de ce. Cela indique la coordonnée Y nécessaire pour positionner le texte afin qu’il soit réellement verticalement centré sur la page. Le `for` boucle définit ensuite une rotation centrée sur le centre de la zone de dessin. La rotation est par incréments de 30 degrés. Le texte est dessiné à l’aide de la `yText` valeur. Le nombre de valeurs vides avant le mot « pivoter » dans la `text` valeur a été déterminée de manière empirique pour rendre la connexion entre ces chaînes de 12 texte semble être un dodecagon.

Un pour simplifier ce code consiste à incrémenter l’angle de rotation de 30 degrés par chaque fois que la boucle après le `DrawText` appeler. Cela élimine la nécessité pour les appels à `Save` et `Restore`. Notez que la `degrees` variable n’est plus utilisée dans le corps de la `for` bloc :

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Il est également possible d’utiliser la forme simple de `RotateDegrees` par en le faisant précéder la boucle avec un appel à `Translate` pour déplacer tous les éléments au centre de la zone de dessin :

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Modifié `yText` calcul n’incorpore plus `yCenter`. Maintenant le `DrawText` appel Centre le texte verticalement en haut de la zone de dessin.

Les transformations sont appliquées sur le plan conceptuel au lieu de la manière dont ils apparaissent dans le code, il est souvent possible de transformations pour commencer plus globales, suivies de plusieurs transformations locales. Cela est souvent le moyen le plus simple pour combiner la rotation et la translation.

Par exemple, supposons que vous souhaitez dessiner un objet graphique qui pivote autour de son centre comme une planète rotation sur l’axe. Mais vous souhaitez également cet objet en révolution autour du centre de l’écran à l’instar d’un monde tournant autour du soleil.

Ce faire, vous pouvez positionner l’objet dans le coin supérieur gauche de la zone de dessin et l’utilisation d’une animation pour faire pivoter autour de cet angle. Ensuite, traduit l’objet horizontalement comme un rayon orbital. Maintenant appliquer une rotation animée deuxième, également autour de l’origine. Cela rend l’objet de dépendre de l’angle. Traduire désormais au centre de la zone de dessin.

Voici le `PaintSurface` gestionnaire qui contient ces transformer des appels dans l’ordre inverse :

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

Le `revolveDegrees` et `rotateDegrees` champs sont animées. Ce programme utilise une technique d’animation différentes selon le Xamarin.Forms `Animation` classe. (Cette classe est décrite dans [chapitre 22 de *créer des applications mobiles avec Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) le `OnAppearing` remplacement crée deux `Animation` objets avec les méthodes de rappel, puis appelle `Commit` sur ces derniers pour une durée de l’animation :

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

La première `Animation` réalise une animation de l’objet `revolveDegrees` de 0 à 360 degrés plus de 10 secondes. Réalise une animation de la deuxième `rotateDegrees` de 0 à 360 degrés le chaque 1 seconde et également invalide la surface pour générer un autre appel à la `PaintSurface` gestionnaire. Le `OnDisappearing` remplacement annule ces deux animations :

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

Le **horloge analogique laid** programme (appelée ainsi car une horloge analogique plus attrayante sera décrite dans un article plus loin) utilise rotation pour dessiner les marques minute et l’heure de l’horloge et faire tourner les aiguilles. Le programme Dessine l’horloge à l’aide d’un système de coordonnées arbitraire basé sur un cercle est centré au point (0, 0) avec un rayon de 100. Elle utilise la traduction et mise à l’échelle pour le développer ou le centre du cercle sur la page.

Le `Translate` et `Scale` appels s’appliquent globalement à l’horloge, ce sont les premières à être appelée après l’initialisation de la `SKPaint` objets :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Enfin, le `PaintSurface` gestionnaire obtient l’heure actuelle et calcule les degrés de rotation de l’heure, la minute et la deuxième mains. Chaque aiguille est dessiné dans la position de 12:00 afin que l’angle de rotation est relatif qui :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

L’horloge est certainement fonctionnelle même si les mains sont plutôt brutes :

[![](rotate-images/uglyanalogclock-small.png "Capture d’écran de triple de la page de texte de l’horloge analogique laid")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
