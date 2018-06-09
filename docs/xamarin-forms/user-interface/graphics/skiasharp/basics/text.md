---
title: Intégration de texte et des graphiques
description: Cet article explique comment déterminer la taille de la chaîne de texte rendue pour intégrer des applications de Xamarin.Forms de texte avec des graphiques SkiaSharp et cela est illustré par l’exemple de code.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: eb3724c3520153a83e37932f8be0735337e27a4f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243656"
---
# <a name="integrating-text-and-graphics"></a>Intégration de texte et des graphiques

_Voir la rubrique déterminer la taille de la chaîne de texte rendue pour intégrer le texte avec des graphiques SkiaSharp_

Cet article explique comment mesurer le texte, éventuellement mettre à l’échelle le texte à une taille spécifique et d’intégrer le texte avec les autres graphiques :

![](text-images/textandgraphicsexample.png "Texte entouré par des rectangles")

Le SkiaSharp `Canvas` classe inclut également des méthodes pour dessiner un rectangle ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) et un rectangle à angles arrondis ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Ces méthodes requièrent le rectangle doit être défini comme un `SKRect` valeur.

Le **accompagnées de texte** page centres d’une chaîne de texte courte de la page et les encadrements avec un frame composé d’une paire de rectangles arrondis. Le [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) classe montre comment procéder.

Dans SkiaSharp vous utilisez la `SKPaint` classe pour définir les attributs de texte et de la police, mais vous pouvez également l’utiliser pour obtenir la taille de rendu de texte. Début de la commande suivante `PaintSurface` Gestionnaire d’événements appelle deux différents `MeasureText` méthodes. La première [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) appel a un simple `string` argument et retourne la largeur en pixels du texte basée sur les attributs de police actuelle. Le programme, puis calcule un nouveau `TextSize` propriété de la `SKPaint` objet basé sur cette largeur restituée, actuel `TextSize` propriété et la largeur de la zone d’affichage. Il est destiné à définir `TextSize` afin que la chaîne de texte pour être rendue à 90 % de la largeur de l’écran :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

La seconde [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) appel a un `SKRect` argument, afin qu’il obtient la largeur et la hauteur du texte rendu. Le `Height` propriété de ce `SKRect` valeur dépend de la présence de lettres majuscules, jambages supérieurs et hampes inférieures dans la chaîne de texte. Autre `Height` valeurs sont indiquées pour les chaînes de texte « mom », « cat » et « dog », par exemple.

Le `Left` et `Top` propriétés de la `SKRect` structure indiquent les coordonnées de l’angle supérieur gauche du texte rendu si le texte est affiché par un `DrawText` appeler avec les positions X et Y de 0. Par exemple, lorsque ce programme est en cours d’exécution sur un simulateur iPhone 7, `TextSize` reçoit la valeur 90.6254 à la suite le calcul suivant le premier appel à `MeasureText`. Le `SKRect` valeur obtenue à partir de la deuxième appel à `MeasureText` a des valeurs de propriété suivantes :

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88 ;

N’oubliez pas que les coordonnées X et Y vous passez à la `DrawText` méthode spécifier le côté gauche du texte à la ligne de base. Le `Top` valeur indique que le texte s’étend 68 pixels au-dessus de cette ligne de base (soustraction 68 informatique 88) 20 pixels sous la ligne de base. Le `Left` la valeur 6 indique que le texte commence à 6 pixels à droite de la valeur de X dans le `DrawText` appeler. Ainsi, pour l’espacement entre les caractère normaux. Si vous souhaitez afficher le texte correctement dans le coin supérieur gauche de l’affichage, passer les valeurs négatives de ces `Left` et `Top` valeurs comme les coordonnées X et Y de `DrawText`, dans cet exemple, &ndash;6 et 68.

Le `SKRect` structure définit plusieurs propriétés pratiques et méthodes, certains d'entre eux sont utilisés dans le reste de la `PaintSurface` gestionnaire. Le `MidX` et `MidY` valeurs indiquent les coordonnées du centre du rectangle. (Dans l’exemple iPhone 7, ces valeurs sont 338.4107 et &ndash;24.) Le code suivant utilise ces valeurs pour le calcul le plus simple de coordonnées pour centrer le texte sur l’affichage :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

Le `PaintSurface` gestionnaire se termine par deux appels à `DrawRoundRect`, qui nécessitent des arguments de `SKRect`. Cela `SKRect` valeur est certainement similaire à la `SKRect` valeur obtenue à partir de la `MeasureText` (méthode), mais il ne peut pas être le même. Tout d’abord, il doit être un peu plus grand afin que le rectangle arrondi ne dessinez sur les bords du texte, et Deuxièmement, il doit être déplacées dans l’espace afin que le `Left` et `Top` valeurs correspondent à l’angle supérieur gauche où le rectangle doit être positionné. Ces deux tâches sont effectuées en `Offset` et `Inflate` méthodes définies par `SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

Après cela, le reste de la méthode est simple. Il crée un autre `SKPaint` objet pour les bordures et les appels `DrawRoundRect` à deux reprises. Le deuxième appel utilise un rectangle agrandie par un autre 10 pixels. Le premier appel spécifie un rayon d’angle de 20 pixels. la seconde a un rayon d’angle 30 pixels, afin qu’ils semblent être parallèle :

 [![](text-images/framedtext-small.png "Capture d’écran de triple de la page de texte d’encadré")](text-images/framedtext-large.png#lightbox "Triple capture d’écran de la page de texte d’encadré")

Vous pouvez activer votre téléphone ou le simulateur sur le côté pour afficher le texte et l’augmentation de taille de trame.

Si vous devez uniquement centrer du texte sur l’écran, vous pouvez le faire environ sans mesure le texte en définissant le `TextAlign` propriété du `SKPaint` à `SKTextAlign.Center`. La coordonnée X que vous spécifiez dans le `DrawText` (méthode), puis indique où se trouve le Centre horizontal du texte. Si vous passez le milieu de l’écran pour les `DrawText` (méthode), centré horizontalement le texte et *presque* centré verticalement, car la ligne de base sera centré verticalement.

Texte proprement dit peut être traitée beaucoup comme une option graphique. Une option simple est d’afficher le plan de caractères du texte plutôt que de l’affichage plein normal :

[![](text-images/outlinedtext-small.png "Le triple de capture d’écran de la page de texte de décrites")](text-images/outlinedtext-large.png#lightbox "Triple capture d’écran de la page décrite de texte")

Cela est fait simplement en modifiant la normale `Style` propriété de la `SKPaint` objet à partir de sa valeur par défaut `SKPaintStyle.Fill` à `SKPaintStyle.Stroke` et en spécifiant une largeur de trait. Le `PaintSurface` Gestionnaire de la **décrites de texte** page montre comment procéder :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 Dans les articles de plusieurs cours, vous disposez maintenant vu comment utiliser SkiaSharp pour dessiner du texte, cercles, ellipses et rectangles arrondis. L’étape suivante consiste [SkiaSharp lignes et des chemins d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) dans lequel vous allez apprendre comment dessiner des lignes connectées dans un chemin d’accès des graphiques.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
