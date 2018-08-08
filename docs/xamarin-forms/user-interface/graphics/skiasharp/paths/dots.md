---
title: Points et tirets dans SkiaSharp
description: Cet article explique en détail comment maîtriser les complexités de dessin des lignes en pointillés et dans SkiaSharp et illustre ceci avec l’exemple de code.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7c336e6b5224f61ff84eb39652788b23f52b806e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615416"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Points et tirets dans SkiaSharp

_Maîtrisez les complexités de dessiner des lignes en pointillés et dans SkiaSharp_

SkiaSharp vous permet de dessiner des lignes qui ne sont pas correctes, mais au lieu de cela sont composés de points et de tirets :

![](dots-images/dottedlinesample.png "Ligne en pointillés")

Cela avec une *effet*, qui est une instance de la [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) classe que vous définissez pour le [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) propriété de `SKPaint`. Vous pouvez créer un chemin d’accès (ou les combiner chemin effets) à l’aide de la méthode statique `Create` méthodes définies par `SKPathEffect`.

Pour dessiner des lignes en pointillés ou en pointillés, utilisez le [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) méthode statique. Il existe deux arguments : il s’agit tout d’abord un tableau de `float` valeurs qui indiquent les longueurs des points et des tirets et la longueur des espaces entre eux. Ce tableau doit avoir un nombre pair d’éléments, et il doit y avoir au moins deux éléments. (Il peut être zéro élément dans le tableau mais qui se traduit par un trait plein.) S’il existe deux éléments, la première est la longueur d’un point ou un tiret et le second est la longueur de l’espacement avant le prochain point ou tiret. S’il existe plus de deux éléments, ils se trouvent dans cet ordre : tiret de longueur, longueur de l’intervalle, longueur du tiret, longueur de l’intervalle et ainsi de suite.

En règle générale, vous souhaitez rendre les longueurs de dash et gap un multiple de la largeur du trait. Si la largeur du trait est 10 pixels, par exemple, puis le tableau {10, 10} Dessine une ligne en pointillés où les points et les espaces sont la même longueur que l’épaisseur du trait.

Toutefois, le `StrokeCap` définition de la `SKPaint` objet affecte également ces points et tirets. Comme vous le verrez bientôt, qui a un impact sur les éléments de ce tableau.

Séparée par des points et de lignes en pointillés sont décrites sur le **points et tirets** page. Le [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) fichier instancie deux `Picker` vues, une pour ce qui vous permet de sélectionner une extrémité de trait et le second pour sélectionner un tableau de tiret :

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

Les trois premiers éléments dans le `dashArrayPicker` supposent que la largeur du trait est de 10 pixels. Le {10, 10} tableau est pour une ligne en pointillés, {30, 10} est pour une ligne en pointillés et {10, 10, 30, 10} est pour une ligne de point-tiret. (Les trois autres seront abordées sous peu.)

Le [ `DotsAndDashesPage` fichier code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) contient le `PaintSurface` Gestionnaire d’événements et deux routines d’assistance pour accéder à la `Picker` vues :

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

Dans les captures d’écran suivante, l’écran iOS à l’extrême gauche montre une ligne en pointillés :

[![](dots-images/dotsanddashes-small.png "Capture d’écran triple de la page de points et de tirets")](dots-images/dotsanddashes-large.png#lightbox "Triple capture d’écran de la page de points et de tirets")

Toutefois, l’écran Android est également supposé pour faire une ligne en pointillés à l’aide du tableau {10, 10}, mais au lieu de cela, la ligne est pleine. Que s'est-il passé ? Le problème est que l’écran Android a également un paramètre d’embouts de trait de `Square`. Cela permet d’étendre tous les tirets par le trait demi-chasse, provoquant la remplir les écarts.

Pour contourner ce problème lors de l’utilisation d’une extrémité de trait de `Square` ou `Round`, vous devez les longueurs de tiret dans le tableau par la longueur du trait (parfois, ce qui entraîne une longueur de tiret de 0) et d’augmenter les longueurs d’écart par la longueur du trait. Voici comment les trois derniers tirets tableaux dans les `Picker` dans le fichier XAML ont été calculés :

- {10, 10} est {0, 20} pour une ligne en pointillés
- {30, 10} devient {20, 20} pour une ligne en pointillés
- {10, 10, 30, 10} est {0, 20, 20, 20} pour une ligne en pointillés et

Limite de l’écran affiche UWP qui séparée par des points et tirets de ligne pour un trait de `Round`. Le `Round` extrémité de trait permet souvent la meilleure apparence des points et des tirets dans les lignes épaisses.

Jusqu'à présent, aucune mention n’a été effectuée du deuxième paramètre à la `SKPathEffect.CreateDash` (méthode). Ce paramètre est nommé `phase` et il fait référence à un offset dans le modèle de point-tiret au début de la ligne. Par exemple, si le tableau dash est {10, 10} et le `phase` est 10, la ligne commence par un écart plutôt que d’un point.

Une application intéressante de la `phase` paramètre figure dans une animation. Le **spirale animée** page est similaire à la **Archimède spirale** page, à ceci près que le [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) classe anime le `phase` paramètre. La page montre également une autre approche pour l’animation. L’exemple précédent de la [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) utilisé le `Task.Delay` méthode pour contrôler l’animation. Cet exemple utilise à la place le Xamarin.Forms `Device.Timer` méthode :


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

Bien sûr, vous devrez exécuter le programme pour voir l’animation :

[![](dots-images/animatedspiral-small.png "Triple capture d’écran de la page de spirale animée")](dots-images/animatedspiral-large.png#lightbox "Triple capture d’écran de la page de spirale animée")

Vous avez désormais appris à dessiner des lignes et de définir des courbes à l’aide des équations paramétriques. Une section pour être publiées ultérieurement couvre les différents types de courbes qui `SKPath` prend en charge.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
