---
title: Les Types de remplissage de chemin d’accès
description: Cet article examine les différents effets possibles avec les types de remplissage de chemin d’accès SkiaSharp et illustre ceci avec l’exemple de code.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 17043054c920a69570f38b227d05980494e29139
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615468"
---
# <a name="the-path-fill-types"></a>Les Types de remplissage de chemin d’accès

_Découvrir les différents effets possibles avec les types de remplissage de chemin d’accès SkiaSharp_

Deux profils dans un chemin d’accès peuvent se chevaucher, et les lignes qui composent un contour unique peuvent se chevaucher. Toute zone délimitée peut potentiellement être remplie, mais vous souhaiterez peut-être pas remplir toutes les zones fermées. Voici un exemple :

![](fill-types-images/filltypeexample.png "Pointes cinq étoiles partiellement filles")

Vous contrôlez un peu cela. L’algorithme de remplissage est régie par le [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) propriété du `SKPath`, que vous définissez à un membre de la [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) énumération :

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), la valeur par défaut
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Les algorithmes enroulement et pair-impair déterminent si n’importe quelle zone délimitée est remplie ou pas remplie avec une ligne hypothétique dessinée à partir de cette zone à l’infini. Cette ligne traverse un ou plusieurs lignes de limite qui composent le chemin d’accès. Avec le mode enroulement, si le nombre de lignes de limite dessiné d’équilibre d’une seule direction le nombre de lignes dessinées dans l’autre sens, puis la zone n’est pas remplie. Sinon, la zone est remplie. L’algorithme pair-impair remplit une zone si le nombre de lignes de la limite est impair.

Avec plusieurs chemins de routine, l’algorithme enroulement remplit souvent toutes les zones délimités d’un chemin d’accès. Généralement, l’algorithme pair-impair produit des résultats plus intéressants.

L’exemple classique est une étoile à cinq branches, comme illustré dans le **Five-Pointed en étoile** page. Le [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) fichier instancie deux `Picker` remplir de vues pour sélectionner le chemin d’accès et si le chemin d’accès est rayé ou rempli ou aux deux et dans quel ordre :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Le fichier code-behind utilise à la fois `Picker` valeurs pour dessiner une étoile à cinq branches :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

En règle générale, le type de remplissage de chemin d’accès doit affecter uniquement des remplissages et des traits pas, mais les deux `Inverse` modes affectent les remplissages et les traits. Pour les remplissages, les deux `Inverse` types remplir des zones afin que la zone en dehors de l’étoile est remplie. Pour les tracés, les deux `Inverse` types tout sauf le trait de couleur. À l’aide de ces types de remplissage inverse peut produire des effets impairs, comme le montre la capture d’écran iOS :

[![](fill-types-images/fivepointedstar-small.png "Capture d’écran triple de la page en étoile Five-Pointed")](fill-types-images/fivepointedstar-large.png#lightbox "Triple capture d’écran de la page en étoile Five-Pointed")

Les captures d’écran Android et UWP montrent les effets de pair-impair et enroulement typiques, mais l’ordre de trait et de remplissage affecte également les résultats.

L’algorithme enroulement est dépendante de la direction que les lignes sont dessinées. Généralement lorsque vous créez un chemin d’accès, vous pouvez contrôler cette direction en tant que vous spécifiez que les lignes sont dessinées à partir d’un point vers un autre. Toutefois, le `SKPath` classe définit également les méthodes, telles que `AddRect` et `AddCircle` qui dessiner les contours entières. Pour contrôler la façon dont ces objets sont dessinés, les méthodes incluent un paramètre de type [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), qui a deux membres :

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Les méthodes dans `SKPath` qui incluent un `SKPathDirection` paramètre lui donner une valeur par défaut de `Clockwise`.

Le **cercles se chevauchant** page crée un chemin d’accès avec quatre cercles se chevauchant avec un type de remplissage de chemin d’accès pair-impair :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Il s’agit d’une image intéressante créée avec un minimum de code :

[![](fill-types-images/overlappingcircles-small.png "Capture d’écran triple de la page de cercles qui se chevauchent")](fill-types-images/overlappingcircles-large.png#lightbox "Triple capture d’écran de la page de cercles qui se chevauchent")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
