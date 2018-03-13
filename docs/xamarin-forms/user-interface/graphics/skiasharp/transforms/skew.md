---
title: "La transformation d’inclinaison"
description: "Découvrez comment la transformation d’inclinaison peut créer des objets graphiques inclinées dans SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: a18b60d486a911e4a76298fd20a70f16ac392881
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="the-skew-transform"></a>La transformation d’inclinaison

_Découvrez comment la transformation d’inclinaison peut créer des objets graphiques inclinées dans SkiaSharp_

Dans SkiaSharp, la transformation d’inclinaison, fait pivoter les objets graphiques, tels que l’ombre dans cette image :

![](skew-images/skewexample.png "Un exemple d’inclinaison à partir du programme d’inclinaison de texte de clichés instantanés")

L’inclinaison transforme des rectangles en Parallélogrammes, mais une ellipse rétrécie est toujours une ellipse.

Bien que Xamarin.Forms définit les propriétés de mise à l’échelle, de translation et de rotations, il n’existe aucune propriété correspondante, dans Xamarin.Forms pour l’inclinaison.

Le [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) méthode `SKCanvas` accepte deux arguments pour le décalage horizontal et vertical incliner :

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Une seconde [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) méthode combine ces arguments dans une seule `SKPoint` valeur :

```csharp
public void Skew (SKPoint skew)
```

Toutefois, il est peu probable que vous allez utiliser une de ces deux méthodes de manière isolée.

Le **d’inclinaison de faire des essais** page permet de vous faire des essais avec inclinaison les valeurs comprises entre -10 et 10. Une chaîne de texte est placée dans le coin supérieur gauche de la page, avec des valeurs de décalage obtenus à partir de deux `Slider` éléments. Voici le `PaintSurface` gestionnaire dans le [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) classe :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Les valeurs de la `xSkew` argument Décalage vers le bas du texte pour les valeurs positives à droite ou gauche pour les valeurs négatives. Les valeurs de `ySkew` décalage vers la droite du texte vers le bas pour les valeurs positives ou pour les valeurs négatives :

[![](skew-images/skewexperiment-small.png "Capture d’écran de triple de la page d’inclinaison de l’expérience")](skew-images/skewexperiment-large.png#lightbox "Triple capture d’écran de la page d’inclinaison de l’expérience")

Si `xSkew` est la valeur négative de `ySkew`, le résultat est la rotation, mais également mis à l’échelle un peu comme les fenêtres d’affichage indique.

Les formules de transformation sont les suivantes :

x' = x + xSkew · y

y' = ySkew · x + y

Par exemple, pour un nombre positif `xSkew` valeur transformé `x'` valeur augmente à mesure que `y` augmente. C’est pourquoi l’inclinaison.

Si un pixels de triangle 200 et 100 pixels de haut est positionné avec son coin supérieur gauche au niveau du point (0, 0) et est rendue avec un `xSkew` valeur 1.5, les résultats du parallélogramme suivants :

![](skew-images/skeweffect.png "L’effet de la transformation d’inclinaison d’un rectangle")

Les coordonnées du bord inférieur ont `y` valeurs de 100, donc il est décalés 150 pixels vers la droite.

Pour les valeurs non nulles de `xSkew` ou `ySkew`, seul le point (0, 0) reste le même. Ce point peut être considéré comme le centre d’inclinaison. Si vous avez besoin du centre d’inclinaison pour être autre chose (qui est généralement le cas), il est sans `Skew` méthode qui qui fournit. Vous devez explicitement associer `Translate` appelle avec la `Skew` appeler. Pour centrer l’inclinaison à `px` et `py`, effectuer les appels suivants :

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Les formules de transformations composites sont :

x' = x + xSkew · (y – pier)

y' = ySkew · (x – px) + y

Si `ySkew` est égal à zéro et que vous spécifiez uniquement une valeur non nulle de `xSkew`, puis `px` valeur n’est pas utilisée. La valeur est sans importance et même pour `ySkew` et `py`.

Vous vous sentirez plus à l’aise spécifiant le décalage à un angle d’inclinaison, telles que l’angle α dans ce diagramme :

![](skew-images/skewangleeffect.png "L’effet de la transformation d’inclinaison sur un rectangle avec un angle d’inclinaison indiqué")

Le rapport de l’équipe de 150 pixels à la verticale de 100 pixels est la tangente de cet angle, dans cet exemple et de 56,3 degrés.

Le fichier XAML de la **expérience de Angle d’inclinaison** page est similaire à la **Angle d’inclinaison** page, sauf que le `Slider` éléments comprise entre -90 et 90 degrés. Le [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Centre le texte dans la page de fichier code-behind et utilise `Translate` pour définir un centre de l’inclinaison au centre de la page. Short `SkewDegrees` méthode en bas du code convertit des angles pour incliner des valeurs :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Un angle d’approche de 90 degrés positifs ou négatifs, la tangente est proche de l’infini, mais les angles jusqu’environ 80 degrés sont utilisables :

[![](skew-images/skewangleexperiment-small.png "Capture d’écran de triple de la page de l’expérience de Angle d’inclinaison")](skew-images/skewangleexperiment-large.png#lightbox "Triple capture d’écran de la page de l’expérience de Angle d’inclinaison")

Une inclinaison horizontale négative petite peut simuler texte en italique ou oblique de, comme le **texte Oblique** montre de page. Le [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) classe montre comment procéder :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Le `TextAlign` propriété du `SKPaint` a la valeur `Center`. Sans les transformations, le `DrawText` appeler avec les coordonnées de (0, 0) positionne le texte avec le Centre horizontal de la ligne de base dans l’angle supérieur gauche. Le `SkewDegrees` inclinaisons le texte horizontalement par rapport à la ligne de base de 20 degrés. Le `Translate` appel déplace le Centre horizontal de la ligne de base du texte au centre de la zone de dessin :

[![](skew-images/obliquetext-small.png "Capture d’écran de triple de la page de texte Oblique")](skew-images/obliquetext-large.png#lightbox "Triple capture d’écran de la page de texte Oblique")

Le **incliner le texte ombré** page montre comment utiliser une combinaison d’une échelle d’inclinaison verticale et de 45 degrés pour ajouter une ombre de texte qui fait pivoter le texte en s’éloignant de. Voici la partie pertinente du `PaintSurface` gestionnaire :

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

L’ombre est affiché en premier, puis le texte :

[![](skew-images/skewshadowtext1-small.png "Capture d’écran de triple de la page de texte de clichés instantanés incliner")](skew-images/skewshadowtext1-large.png#lightbox "Triple capture d’écran de la page d’inclinaison de texte de clichés instantanés")

Coordonnée verticale est passé à la `DrawText` méthode indique la position du texte par rapport à la ligne de base. Qui est la même coordonnée verticale utilisée pour le centre d’inclinaison. Cette technique ne fonctionnera pas si la chaîne de texte contient les jambages inférieurs. Par exemple, remplacez le mot « étrange » pour « Shadow » et ici du résultat :

[![](skew-images/skewshadowtext2-small.png "Capture d’écran de triple de la page de texte de clichés instantanés incliner avec un autre mot descendants")](skew-images/skewshadowtext2-large.png#lightbox "Triple capture d’écran de la page de texte de clichés instantanés incliner avec un autre mot descendants")

Les clichés instantanés et le texte sont alignés au niveau de la ligne de base, mais uniquement l’effet semble incorrect. Pour corriger cela, vous devez obtenir les limites de texte :

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

Le `Translate` appels doivent être ajustés par la hauteur de la hampe inférieure :

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Maintenant, l’ombre s’étend au bas de ces descendants :

[![](skew-images/skewshadowtext3-small.png "Capture d’écran de triple de la page de texte de clichés instantanés incliner ajustements pour hampes")](skew-images/skewshadowtext3-large.png#lightbox "Triple capture d’écran de la page de texte de clichés instantanés incliner ajustements pour hampes")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
