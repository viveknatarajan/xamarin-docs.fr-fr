---
title: Lignes et les extrémités de trait
description: Cet article explique comment utiliser SkiaSharp pour dessiner des lignes avec des embouts de contour différentes dans les applications de Xamarin.Forms et cela est illustré par l’exemple de code.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: f63d14ae0dff5f94580c86d1c5bfdfc66bbee7f9
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244092"
---
# <a name="lines-and-stroke-caps"></a>Lignes et les extrémités de trait

_Découvrez comment utiliser SkiaSharp pour dessiner des lignes avec des embouts de contour différentes_

Dans SkiaSharp, le rendu d’une seule ligne est très différente de rendu d’une série de lignes droites connectées. Même lors du dessin de lignes uniques, toutefois, il est souvent nécessaire de donner les lignes d’un trait particulier et la ligne de plus, le plus important devient l’apparence de la fin des lignes, appelé le *extrémité de trait*:

![](lines-images/strokecapsexample.png "Les options de majuscules trois trait")

Pour tracer des lignes uniques, `SKCanvas` définit un simple [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) méthode dont les arguments indiquent le début et fin des coordonnées de la ligne avec une `SKPaint` objet :

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Par défaut, le `StrokeWidth` propriété nouvellement instancié `SKPaint` objet est 0, ce qui a le même effet que la valeur 1 dans le rendu d’une ligne d’un pixel d’épaisseur. Cela semble très fine sur les appareils haute résolution tels que les téléphones, donc vous voudrez probablement définir le `StrokeWidth` une valeur plus élevée. Mais une fois que vous commencez à tracer des lignes d’une épaisseur dimensionnable, qui déclenche un autre problème : comment doivent le démarre et les extrémités de ces lignes épaisses restituées ?

L’apparence du démarrage et fin des lignes est appelée un *extrémité de la ligne* ou Skia, un *extrémité de trait*. Le mot « cap » dans ce contexte fait référence à un type de hat &mdash; quelque chose qui se trouve à la fin de la ligne. Vous définissez la [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) propriété de la `SKPaint` objet à un des membres suivants de la [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) énumération :

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (la valeur par défaut)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Ces méthodes sont illustrées mieux avec un exemple de programme. La deuxième partie de la page d’accueil de la [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme commence par une page intitulée **extrémités de trait** selon la [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) classe. Cette page définit un `PaintSurface` Gestionnaire d’événements qui effectue une itération sur les trois membres de le `SKStrokeCap` énumération, afficher le nom de membre de l’énumération et le dessin d’une ligne à l’aide de cette extrémité de trait :

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
        TextAlign = SKTextAlign.Center
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

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Pour chaque membre de la `SKStrokeCap` énumération, le gestionnaire dessine deux lignes, une avec une épaisseur de trait de 50 pixels et une autre ligne positionné en haut, avec une épaisseur de trait de 2 pixels. Cette deuxième ligne est destinée à illustrer début géométrique et fin de la ligne indépendante de l’épaisseur du trait et une extrémité de trait :

[![](lines-images/strokecaps-small.png "Capture d’écran de triple de la page des extrémités de trait")](lines-images/strokecaps-large.png#lightbox "Triple capture d’écran de la page des extrémités de trait")

Comme vous pouvez le voir, la `Square` et `Round` extrémités de trait développer de manière efficace la longueur de la ligne à la moitié l’épaisseur du trait au début de la ligne et de nouveau à la fin. Cette extension est importante lorsqu’il est nécessaire de déterminer les dimensions d’un objet rendu graphique.

La `SKCanvas` classe inclut également une autre méthode pour le dessin de plusieurs lignes qui est un peu étrange :

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

Le `points` paramètre est un tableau de `SKPoint` valeurs et `mode` est un membre de la [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) énumération, qui a trois membres :

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) Pour afficher des points
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) chaque paire de points de connexion
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) Pour vous connecter à tous les points consécutifs

Le **plusieurs lignes** page illustre cette méthode. Le [ `MultipleLinesPage` fichier XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) instancie deux `Picker` vues qui vous permettent de sélectionner un membre de la `SKPointMode` énumération et un membre de la `SKStrokeCap` énumération :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Le `SelectedIndexChanged` gestionnaire pour les deux `Picker` vues invalide simplement le `SKCanvasView` objet :

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Ce gestionnaire doit vérifier l’existence de la `SKCanvasView` , car le Gestionnaire d’événements est le premier objet appelé lorsque le `SelectedIndex` propriété de la `Picker` est définie sur 0 dans le fichier XAML, et qui se produit avant le `SKCanvasView` a été instancié.

Le `PaintSurface` gestionnaire accède à une méthode générique pour obtenir les deux éléments sélectionnés à partir de la `Picker` de vues et de les convertir en valeurs d’énumération :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

La capture d’écran affiche diverses `Picker` sélections sur les plateformes de trois :

[![](lines-images/multiplelines-small.png "Capture d’écran de triple de la page de plusieurs lignes")](lines-images/multiplelines-large.png#lightbox "Triple capture d’écran de la page de plusieurs lignes")

IPhone à gauche montre comment la `SKPointMode.Points` entraîne de membre de l’énumération `DrawPoints` pour chacun des points de restituer le `SKPoint` tableau sous la forme d’un carré si embout de ligne est `Butt` ou `Square`. Cercles sont rendus si embout de ligne est `Round`.

Lorsque vous utilisez à la place `SKPointMode.Lines`, comme indiqué dans l’écran Android dans le centre de la `DrawPoints` méthode dessine une ligne entre chaque paire de `SKPoint` valeurs, à l’aide de l’extrémité de la ligne spécifiée, dans ce cas `Round`.

La capture d’écran UWP indique le résultat de la `SKPointMode.Polygon` valeur. Une ligne est dessinée entre les points successifs dans le tableau, mais si vous examinez très attentivement, vous verrez que ces lignes ne sont pas connectés. Chacune de ces lignes distincts commence et se termine par l’extrémité de fin de ligne spécifié. Si vous sélectionnez le `Round` Cap, les lignes peuvent sembler être connecté, mais ils sont connectés vraiment pas.

Si les lignes sont connectés ou non connectés sont un aspect essentiel de l’utilisation de chemins d’accès des graphiques.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
