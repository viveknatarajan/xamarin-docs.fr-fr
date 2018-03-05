---
title: Peinture de doigt
description: Utilisez vos doigts pour peindre sur le canevas.
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: c9894cac0064b91049b0142971896d7dd67dec5f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="finger-painting"></a>Peinture de doigt

_Utilisez vos doigts pour peindre sur le canevas._

Un `SKPath` objet peut être mis à jour et affiché en permanence. Cette fonctionnalité permet à un chemin d’accès à utiliser pour le dessin interactif, comme dans un programme finger-painting.

![](finger-paint-images/fingerpaintsample.png "Un exercice de peinture au doigt")

La prise en charge tactile dans Xamarin.Forms n’autorise pas le suivi des doigts sur l’écran, donc un effet de suivi des fonctions tactiles Xamarin.Forms a été développé pour prendre en charge les fonctions tactiles supplémentaires. Cela est décrit dans l’article [ **appeler des événements à partir des effets**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). L’exemple de programme [ **tactile suivi effet démonstrations** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) inclut deux pages qui utilisent SkiaSharp, y compris un programme finger-painting.

Le [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) solution inclut cet événement de suivi des fonctions tactiles. Le projet de bibliothèque de classes portables inclut le `TouchEffect` (classe), le `TouchActionType` énumération, le `TouchActionEventHandler` déléguer et la `TouchActionEventArgs` classe. Les projets de plateforme incluent un `TouchEffect` de classe pour cette plateforme ; le projet iOS contient également un `TouchRecognizer` classe.

Le **doigt peinture** page **SkiaSharpFormsDemos** est une implémentation simplifiée de peinture au doigt. Il ne pas permettre de sélectionner la couleur ou largeur de trait, il n’a aucun moyen pour effacer la zone de dessin et bien sûr, vous ne pouvez enregistrer votre illustration.

Le [ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) fichier met le `SKCanvasView` dans une seule cellule `Grid` et attache la `TouchEffect` à celle `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Attacher le `TouchEffect` directement à le `SKCanvasView` ne fonctionne pas sous toutes les plateformes.

Le [ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) fichier code-behind définit deux collections pour stocker le `SKPath` objets, ainsi que d’un `SKPaint` objet pour le rendu de ces chemins d’accès :

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Comme le suggère de nom, le `inProgressPaths` dictionnaire stocke les chemins d’accès qui sont en cours de saisie par un ou plusieurs des doigts. La clé du dictionnaire est l’ID de contact qui accompagne les événements tactiles. Le `completedPaths` champ est une collection de chemins d’accès qui ont été terminées quand un doigt le tracé levées à partir de l’écran.

Le `TouchAction` Gestionnaire gère ces deux collections. Lorsqu’un doigt touche en premier l’écran, un nouveau `SKPath` est ajouté à `inProgressPaths`. Comme ce doigt, des points supplémentaires sont ajoutés pour le chemin d’accès. Lorsque le doigt est lancé, le chemin d’accès est transféré vers le `completedPaths` collection. Vous pouvez peindre avec plusieurs doigts simultanément. Après chaque modification apportée à un des chemins d’accès ou des regroupements, le `SKCanvasView` est non valide :

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Les points qui accompagne les événements de suivi de contact sont des coordonnées de Xamarin.Forms ; Il doivent être convertis en coordonnées SkiaSharp, qui sont les pixels. C’est le rôle de la `ConvertToPixel` (méthode).

Le `PaintSurface` Gestionnaire simplement restitue ensuite les deux collections de chemins d’accès. Les chemins d’accès précédemment terminés s’affichent sous les chemins d’accès en cours d’exécution :

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Votre doigt de peintures sont limitées uniquement par vos collaborateurs :

[![](finger-paint-images/fingerpaint-small.png "Capture d’écran de triple de la page de dessin du doigt")](finger-paint-images/fingerpaint-large.png "Triple capture d’écran de la page de dessin du doigt")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Suivi des fonctions tactiles effet démonstrations (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Appel des événements à partir des effets](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
