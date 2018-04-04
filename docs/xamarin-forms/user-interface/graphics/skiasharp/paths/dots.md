---
title: Points et des tirets
description: Maîtrisez les complexités de dessiner des lignes en pointillés et dans SkiaSharp
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 274c8e9a79fa3fadff14f1174d86aad04d902b05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="dots-and-dashes"></a>Points et des tirets

_Maîtrisez les complexités de dessiner des lignes en pointillés et dans SkiaSharp_

SkiaSharp vous permet de dessiner des lignes qui ne sont pas correctes, mais au lieu de cela sont composés de points et de tirets :

![](dots-images/dottedlinesample.png "Ligne en pointillés")

Cela avec un *effet*, qui est une instance de la [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) classe que vous définissez pour le [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) propriété de `SKPaint`. Vous pouvez créer un chemin d’accès (ou les combiner chemin effets) à l’aide de la méthode statique `Create` méthodes définies par `SKPathEffect`.

Pour dessiner des lignes en pointillés ou en pointillé, vous utilisez la [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) méthode statique. Il y a deux arguments : il s’agit d’un tableau d’abord `float` valeurs qui indiquent la longueur de la taille en points et des tirets et la longueur des espaces entre eux. Ce tableau doit avoir un nombre pair d’éléments, et il doit y avoir au moins deux éléments. (Il peut être zéro élément dans le tableau, mais qui se traduit par un trait plein.) S’il existe deux éléments, la première est la longueur d’un point ou un tiret, et la seconde la longueur de l’espace avant le point ou un tiret suivant. Si plus de deux éléments sont, ils sont dans cet ordre : tiret-longueur, longueur de l’intervalle, longueur de tiret, longueur de l’intervalle et ainsi de suite.

En règle générale, que vous souhaitez rendre les longueurs de tiret et espace un multiple de la largeur du trait. Si la largeur du trait est 10 pixels, par exemple, puis le tableau {10, 10} Dessine une ligne en pointillés où les points et les espaces sont de même longueur que l’épaisseur du trait.

Toutefois, le `StrokeCap` définition de la `SKPaint` objet affecte également ces points et tirets. Comme vous le verrez, qui a un impact sur les éléments de ce tableau.

Séparée par des points et les lignes en pointillés sont illustrées dans le **points et des tirets** page. Le [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) fichier instancie deux `Picker` vues, une pour vous permettant de sélectionner une extrémité de trait et la seconde pour sélectionner un tableau de tiret :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
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

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
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

Les trois premiers éléments dans le `dashArrayPicker` supposent que la largeur du trait est 10 pixels. Le {10, 10} tableau est pour une ligne en pointillés, {30, 10} est pour une ligne en pointillé et {10, 10, 30, 10} est pour une ligne de point-tiret. (Les trois autres aborderons sous peu.)

Le [ `DotsAndDashesPage` fichier code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) contient le `PaintSurface` Gestionnaire d’événements et deux des routines d’assistance pour l’accès à la `Picker` vues :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

Dans les captures d’écran ci-dessous, l’écran iOS à l’extrême gauche affiche une ligne en pointillés :

[![](dots-images/dotsanddashes-small.png "Capture d’écran de triple de la page points et de tirets")](dots-images/dotsanddashes-large.png#lightbox "Triple capture d’écran de la page points et des tirets")

Toutefois, l’écran Android est également prévu pour afficher une ligne en pointillés à l’aide du tableau {10, 10}, mais au lieu de cela, la ligne est pleine. Que s'est-il passé ? Le problème est que l’écran Android a également un paramètre de majuscules de trait de `Square`. Cela permet d’étendre tous les tirets par moitié la largeur du trait, d'où leur combler les vides.

Pour contourner ce problème lorsque vous utilisez une extrémité de trait de `Square` ou `Round`, vous devez les longueurs de tiret dans le tableau par la longueur du trait (parfois entraînant une longueur de tiret de 0) et d’augmenter les longueurs d’écart par la longueur du trait. C’est la façon dont les trois derniers tiret-tableaux dans les `Picker` dans le fichier XAML ont été calculées :

- {10, 10} est {0, 20} pour une ligne en pointillés
- {30, 10} est {20, 20} pour une ligne en pointillés
- {10, 10, 30, 10} est {0, 20, 20, 20} pour une ligne en pointillés et

L’écran affiche Windows pointillés et tirets de la ligne pour un trait imposer de `Round`. Le `Round` extrémité de trait donne souvent la meilleure apparence des points et des tirets dans les lignes de type « définitif ».

Jusqu'à présent, aucune mention n’a été effectuée du deuxième paramètre à la `SKPathEffect.CreateDash` (méthode). Ce paramètre est nommé `phase` et il fait référence à un offset dans le modèle de point-tiret au début de la ligne. Par exemple, si le tableau de tiret est {10, 10} et le `phase` est 10, la ligne commence par un espace au lieu d’un point.

Une application intéressante de la `phase` du paramètre est une animation. Le **animés de spirale** page est similaire à la **Archimède spirale** page, à ceci près que le [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) réalise une animation de la classe le `phase` paramètre. La page montre également une autre approche de l’animation. L’exemple précédent de la [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) utilisé le `Task.Delay` méthode pour contrôler l’animation. Cet exemple utilise à la place la Xamarin.Forms `Device.Timer` méthode :


```csharp
const double cycleTime = 250;       // in milliseconds

SKCanvasView canvasView;
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float dashPhase;

public AnimatedSpiralPage()
{
    Title = "Animated Spiral";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    stopwatch.Start();

    Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
    {
        double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
        dashPhase = (float)(10 * t);
        canvasView.InvalidateSurface();

        if (!pageIsActive)
        {
            stopwatch.Stop();
        }

        return pageIsActive;
    });
}
```

Bien entendu, vous devrez qui exécutent ce programme pour voir l’animation :

[![](dots-images/animatedspiral-small.png "Triple capture d’écran de la page animé de spirale")](dots-images/animatedspiral-large.png#lightbox "Triple capture d’écran de la page spirale d’animation")

Vous savez maintenant pour dessiner des lignes et de définir des courbes à l’aide des équations paramétriques. Une section pour être publiées ultérieurement décrit les différents types de courbes qui `SKPath` prend en charge.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
