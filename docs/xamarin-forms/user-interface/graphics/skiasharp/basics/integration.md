---
title: Intégration avec Xamarin.Forms
description: Créer des graphiques SkiaSharp répondent aux fonctions tactiles et des éléments de Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 3ebe153ead2bb62b19ad6b25bf0093e20bf15c04
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="integrating-with-xamarinforms"></a>Intégration avec Xamarin.Forms

_Créer des graphiques SkiaSharp répondent aux fonctions tactiles et des éléments de Xamarin.Forms_

SkiaSharp graphics peut s’intégrer avec le reste de Xamarin.Forms de plusieurs façons. Vous pouvez combiner un canevas SkiaSharp et Xamarin.Forms des éléments sur la même page et même positionner des éléments de Xamarin.Forms par-dessus un canevas SkiaSharp :

![](integration-images/integrationexample.png "Sélection d’une couleur avec les curseurs")

Une autre approche pour la création des graphiques interactifs SkiaSharp dans Xamarin.Forms est via tactile.
La deuxième page de la [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme est autorisée à utiliser **appuyez sur le remplissage de bascule**. Il dessine un cercle simple de deux façons &mdash; sans remplissage et avec un remplissage &mdash; activé ou désactivé par un clic. Le [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) classe montre comment vous pouvez modifier les graphiques SkiaSharp en réponse à l’entrée d’utilisateur.

De cette page, le `SKCanvasView` classe est instanciée dans le [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) fichier, qui définit également un Xamarin.Forms [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) sur la vue :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Notez le `skia` déclaration d’espace de noms XML.

Le `Tapped` gestionnaire pour le `TapGestureRecognizer` objet bascule simplement la valeur d’un champ booléen et appelle le [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) méthode `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

L’appel à `InvalidateSurface` efficacement génère un appel à la `PaintSurface` gestionnaire, qui utilise le `showFill` champ à remplir ou pas de remplissage du cercle :

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
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

Le `StrokeWidth` propriété a été définie à 50 pour accentuer la différence. Vous pouvez également voir la largeur de la ligne entière à l’intérieur de dessin tout d’abord, puis le plan. Par défaut, les graphiques chiffres dessiné plus loin dans le `PaintSurface` Gestionnaire d’événements masquent celles précédemment dans le gestionnaire.

Le **couleur Explorer** page montre comment vous pouvez également intégrer SkiaSharp graphiques avec d’autres éléments Xamarin.Forms et montre également la différence entre les deux autres méthodes permettant de définir des couleurs dans SkiaSharp. La méthode statique [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) méthode crée un `SKColor` valeur basée sur le modèle de Teinte-Saturation-Luminosité :

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

La méthode statique [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) méthode crée un `SKColor` valeur basée sur le modèle de valeur de Teinte-Saturation similaire :

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

Dans les deux cas, le `h` plages d’arguments de 0 à 360. Le `s`, `l`, et `v` arguments comprise entre 0 et 100. Le `a` (alpha ou opacité) plages d’arguments comprise entre 0 et 255.

Le [ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) fichier crée deux `SKCanvasView` des objets dans une `StackLayout` côte à côte avec `Slider` et `Label` les vues qui permettent à l’utilisateur à sélectionner TSL et Valeurs de couleur TSL :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Les deux `SKCanvasView` sont des éléments dans une seule cellule `Grid` avec un `Label` située en haut pour afficher la valeur résultante de la couleur RVB.

Le [ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) fichier code-behind est relativement simple. Le partage `ValueChanged` gestionnaire pour les trois `Slider` éléments invalide simplement les deux `SKCanvasView` éléments. Le `PaintSurface` gestionnaires d’effacer la zone de dessin avec la couleur indiquée par le `Slider` éléments et également définir le `Label` assis sur le `SKCanvasView` éléments :

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

Dans les modèles de couleurs TSL et TSL, la valeur de teinte comprise entre 0 et 360 et indique la teinte dominante de la couleur. Voici les couleurs traditionnelles de l’arc-en-ciel : rouge, orange, jaune, vert, bleu, indigo, violet et arrière dans un cercle rouge.

Dans le modèle TSL, une valeur 0 pour la luminosité est toujours noir, et une valeur de 100 est toujours blanc. Lorsque la valeur de Saturation est 0, les valeurs de luminosité comprise entre 0 et 100 sont en nuances de gris. Augmentez la Saturation ajoute plus de couleurs. Couleurs pures (qui sont des valeurs RVB d’un composant égal à 255, un autre égal à 0 et le troisième comprise entre 0 et 255) se produisent lorsque la Saturation est comprise entre 100 et la luminosité est 50.

Dans le modèle TSL, couleurs pures entraîner lors de la Saturation et la valeur sont 100. Lorsque la valeur est 0, indépendamment des autres paramètres, la couleur est noire. Nuances de gris se produisent lorsque la Saturation 0 et la valeur est comprise entre 0 et 100.

Cependant, la meilleure façon de faire une idée des deux modèles est à faire des essais avec eux vous-même :

[![](integration-images/colorexplore-large.png "Capture d’écran de triple de la page couleur Explorer")](integration-images/colorexplore-small.png#lightbox "Triple capture d’écran de la page couleur Explorer")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
