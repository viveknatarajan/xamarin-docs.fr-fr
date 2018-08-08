---
title: Manipulations tactiles
description: Cet article explique comment utiliser des transformations de matrice pour implémenter en faisant glisser des fonctions tactiles, de pincement et de rotation et illustre ceci avec l’exemple de code.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: e2c1529980681ed1013c53343c2d077297352b95
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615390"
---
# <a name="touch-manipulations"></a>Manipulations tactiles

_Matrice d’utiliser les transformations à implémenter en faisant glisser des fonctions tactiles, de pincement et de rotation_

Dans les environnements de l’interaction tactile multipoint, telles que celles sur les appareils mobiles, les utilisateurs utilisent souvent leurs doigts pour manipuler des objets sur l’écran. Des mouvements courants tels que d’une opération de glissement d’un doigt et un pincement de deux doigts peuvent déplacer et mettre à l’échelle des objets ou même les faire pivoter. Ces mouvements sont généralement implémentées à l’aide de matrices de transformation, et cet article vous montre comment procéder.

![](touch-images/touchmanipulationsexample.png "Une image bitmap soumise à translation, de mise à l’échelle et rotation")

## <a name="manipulating-one-bitmap"></a>Manipulation de l’image Bitmap

Le **Manipulation tactile** page montre les manipulations tactiles sur une seule bitmap.
Cet exemple utilise l’effet de suivi de tactile présentée dans l’article [appel des événements à partir des effets](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Plusieurs autres fichiers prennent en charge pour le **Manipulation tactile** page. La première est la [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) énumération, qui indique les différents types de manipulation tactile implémentée par le code que vous verrez :

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` est une opération de glissement d’un doigt qui est implémentée avec la traduction. Toutes les options suivantes également inclure le panoramique, mais impliquent deux doigts : `IsotropicScale` est une opération de pincement qui résulte dans l’objet de mise à l’échelle de manière égale dans le sens horizontal et vertical. `AnisotropicScale` permet la mise à l’échelle inégaux.

Le `ScaleRotate` option est de mise à l’échelle des deux doigts et de rotation. Mise à l’échelle est Isotrope. Implémentation de rotation de deux doigts avec mise à l’échelle ANISOTROPIQUE est problématique, car les doigt mouvements sont essentiellement les mêmes.

Le `ScaleDualRotate` option ajoute la rotation d’un doigt. Quand un seul doigt fait glisser l’objet, l’objet glissé est tout d’abord pivoter autour de son centre afin que le centre de l’objet est alignée avec le vecteur de déplacement.

Le [ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) fichier inclut un `Picker` avec les membres de la `TouchManipulationMode` énumération :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>

        <Grid BackgroundColor="White"
              Grid.Row="1">

            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

Vers le bas est un `SKCanvasView` et un `TouchEffect` attaché à la cellule unique `Grid` qui l’englobe.

Le [ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) fichier code-behind a un `bitmap` champ, mais il n’est pas de type `SKBitmap`. Le type est `TouchManipulationBitmap` (une classe que vous le verrez bientôt) :

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Le constructeur instancie un `TouchManipulationBitmap` objet, en passant au constructeur un `SKBitmap` obtenu à partir d’une ressource incorporée. Le constructeur se termine en définissant le `Mode` propriété de la `TouchManager` propriété de la `TouchManipulationBitmap` objet à un membre de la `TouchManipulationMode` énumération.

Le `SelectedIndexChanged` gestionnaire pour le `Picker` cela définit également `Mode` propriété :

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

Le `TouchAction` Gestionnaire de la `TouchEffect` instancié dans les appels de fichier XAML, deux méthodes dans `TouchManipulationBitmap` nommé `HitTest` et `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Si le `HitTest` retourne de la méthode `true` &mdash; ce qui signifie qu’un doigt a touché l’écran dans la zone occupée par l’image bitmap &mdash; l’ID tactile est ajouté à la `TouchIds` collection. Cet ID représente la séquence d’événements tactiles pour ce doigt jusqu'à ce que le doigt se soulève à partir de l’écran. Si plusieurs doigts touch à l’image bitmap, puis le `touchIds` collection contient un ID tactile pour chaque doigt.

Le `TouchAction` gestionnaire appelle également la `ProcessTouchEvent` classe dans `TouchManipulationBitmap`. C’est là où certains (mais pas toutes) des fonctions tactiles véritable traitement se produit.

Le [ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) classe est une classe wrapper pour `SKBitmap` qui contient le code pour restituer l’image bitmap et traiter les événements tactiles. Il fonctionne conjointement avec la plus généralisée du code dans un `TouchManipulationManager` classe (qui vous le verrez bientôt).

Le `TouchManipulationBitmap` constructeur enregistre le `SKBitmap` et instancie les deux propriétés, la `TouchManager` propriété de type `TouchManipulationManager` et `Matrix` propriété de type `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

Cela `Matrix` propriété est la transformation accumulée résultant de toutes les activités tactile. Comme vous le verrez, chaque événement tactile est résolu en une matrice, qui est ensuite concaténée avec le `SKMatrix` valeur stockée par le `Matrix` propriété.

Le `TouchManipulationBitmap` objet qui se dessine lui-même son `Paint` (méthode). L’argument est un `SKCanvas` objet. Cela `SKCanvas` disposez peut-être déjà d’une transformation appliquée, donc la `Paint` méthode concatène le `Matrix` propriété associé à la bitmap à la transformation existante et restaure la zone de dessin lorsqu’il a terminé :

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

Le `HitTest` retourne de la méthode `true` si l’utilisateur touche l’écran à un point dans les limites de la bitmap. Comme l’utilisateur manipule la bitmap, l’image bitmap peut être pivoté, ou même (via une combinaison de rotation et de mise à l’échelle ANISOTROPIQUE) dans la forme d’un parallélogramme. Vous pouvez la peur que le `HitTest` méthode doit implémenter plutôt complexe géométrie analytique dans ce cas.

Toutefois, un raccourci est disponible :

Déterminer si un point se trouve dans les limites d’un rectangle transformé est identique à la façon de déterminer si un point transformé inverse se trouve dans les limites du rectangle de transformation. C’est un quantité calcul plus facile et il peut utiliser le pratique `Contains` méthode définie par `SKRect`:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

La deuxième méthode publique dans `TouchManipulationBitmap` est `ProcessTouchEvent`. Lorsque cette méthode est appelée, il a déjà été établi que l’événement tactile appartienne à cette image bitmap particulier. La méthode gère un dictionnaire de [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) objets, qui est simplement le point précédent et le nouveau point de chaque doigt :

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Voici le dictionnaire et le `ProcessTouchEvent` méthode proprement dite :

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

Dans le `Moved` et `Released` événements, les appels de méthode `Manipulate`. À cette occasion, le `touchDictionary` contient un ou plusieurs `TouchManipulationInfo` objets. Si le `touchDictionary` contient un élément, il est probable que le `PreviousPoint` et `NewPoint` valeurs sont inégales et représentent le mouvement d’un doigt. Si plusieurs doigts touchent l’image bitmap, le dictionnaire contient plusieurs éléments, mais qu’un seul de ces éléments a différents `PreviousPoint` et `NewPoint` valeurs. Les autres ont égal `PreviousPoint` et `NewPoint` valeurs.

Ceci est important : le `Manipulate` méthode peut supposer qu’il traite le mouvement du doigt qu’une seule. Au moment de cet appel, aucune des autres les doigts sont mobile, et si elles sont vraiment déplacez (comme c’est probablement), ces mouvements sont traités dans les appels suivants à `Manipulate`.

Le `Manipulate` méthode copie tout d’abord le dictionnaire dans un tableau pour des raisons pratiques. Il ignore tout élément autre que les deux premières entrées. Si plus de deux doigts tente de manipuler l’image bitmap, les autres sont ignorés. `Manipulate` est le dernier membre de `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Si un doigt manipule l’image bitmap, `Manipulate` appelle le `OneFingerManipulate` méthode de la `TouchManipulationManager` objet. Pour deux doigts, il appelle `TwoFingerManipulate`. Les arguments de ces méthodes sont les mêmes : la `prevPoint` et `newPoint` arguments représentent le doigt se déplace. Mais le `pivotPoint` argument est différent pour les deux appels :

Pour la manipulation d’un doigt, la `pivotPoint` correspond au centre de l’image bitmap. Cela consiste à autoriser pour la rotation d’un doigt. Pour la manipulation des deux doigts, l’événement indique le déplacement d’un seul doigt, afin que le `pivotPoint` est le doigt ne bouge pas.

Dans les deux cas, `TouchManipulationManager` retourne un `SKMatrix` valeur, la méthode concatène avec actuel `Matrix` propriété qui `TouchManipulationPage` utilise pour restituer l’image bitmap.

`TouchManipulationManager` est généralisé et n’utilise aucun autre fichier à l’exception `TouchManipulationMode`. Vous pouvez être en mesure d’utiliser cette classe sans modification dans vos propres applications. Il définit une propriété unique de type `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Toutefois, vous souhaiterez probablement éviter le `AnisotropicScale` option. Il est très facile avec cette option pour manipuler le bitmap afin qu’un des facteurs d’échelle devient égal à zéro. Ainsi, l’image bitmap disparaissent de la vue, jamais à retourner. Si vous avez véritablement besoin ANISOTROPIQUE de mise à l’échelle, vous souhaitez améliorer la logique pour éviter des résultats indésirables.

`TouchManipulationManager` utilise des vecteurs, mais dans la mesure où il existe aucune `SKVector` structure dans SkiaSharp, `SKPoint` est utilisé à la place. `SKPoint` prend en charge que l’opérateur de soustraction et le résultat peuvent être traité comme un vecteur. La logique spécifique uniquement vecteur qui doit être ajoutés est un `Magnitude` calcul :

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Chaque fois que la rotation a été sélectionnée, les méthodes de manipulation d’un doigt et deux doigts gérer tout d’abord la rotation. Si la rotation est détectée, le composant de rotation est alors effectivement supprimé. Ce qui reste est interprété comme panoramique et de mise à l’échelle.

Voici le `OneFingerManipulate` (méthode). Si un doigt rotation n’a pas été activée, la logique est simple &mdash; il utilise simplement le point précédent et le point pour construire un vecteur nommé `delta` qui correspond précisément à la traduction. Avec une rotation d’un doigt est activée, la méthode utilise angles à partir du point de pivot (au centre de l’image bitmap) pour le point précédent et le point pour construire une matrice de rotation :

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

Dans le `TwoFingerManipulate` (méthode), le point pivot est la position du doigt qui ne bouge pas dans cet événement tactile donnée. La rotation est très similaire à la rotation d’un doigt, et ensuite le vecteur nommé `oldVector` (basé sur le point précédent) est ajustée pour la rotation. Le mouvement restant est interprété en tant que mise à l’échelle :

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Vous remarquerez qu’il n’existe aucune conversion explicite dans cette méthode. Toutefois, à la fois le `MakeRotation` et `MakeScale` méthodes sont basées sur le point pivot, et qui inclut la conversion implicite. Si vous utilisez deux doigts sur l’image bitmap et les faisant glisser dans la même direction, `TouchManipulation` obtiendra une série d’événements tactiles alternant entre deux doigts. Comme chaque doigt par rapport à l’autres, résultats mise à l’échelle ou la rotation, mais elle est invalidée par le déplacement de l’autre doigt, et le résultat est la traduction.

La seule partie restante de la **Manipulation tactile** page est la `PaintSurface` gestionnaire dans le `TouchManipulationPage` fichier code-behind. Ce code appelle la `Paint` méthode de la `TouchManipulationBitmap`, auquel s’applique la matrice représentant l’activité cumulée tactile :

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

Le `PaintSurface` gestionnaire se termine en affichant un `MatrixDisplay` objet en affichant la matrice cumulée tactile :

[![](touch-images/touchmanipulation-small.png "Capture d’écran triple de la page de la Manipulation tactile")](touch-images/touchmanipulation-large.png#lightbox "Triple capture d’écran de la page de la Manipulation tactile")

## <a name="manipulating-multiple-bitmaps"></a>Manipulation des Bitmaps plusieurs

Un des avantages de l’isolation du code de traitement de touch dans les classes telles que `TouchManipulationBitmap` et `TouchManipulationManager` est la possibilité de réutiliser ces classes dans un programme qui permet à l’utilisateur de manipuler plusieurs images bitmap.

Le **Bitmap à nuages de points vue** page montre comment procéder. Au lieu de la définition d’un champ de type `TouchManipulationBitmap`, le [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) classe définit un `List` des objets bitmap :

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Le constructeur se charge dans toutes les images disponibles en tant que ressources incorporées et les ajoute à la `bitmapCollection`. Notez que le `Matrix` propriété est initialisée sur chaque `TouchManipulationBitmap` de l’objet, donc les angles supérieur gauche de chaque bitmap sont décalés de 100 pixels.

Le `BitmapScatterView` page doit également gérer les événements tactiles pour les bitmaps de plusieurs. Au lieu de définir un `List` de touch ID des actuellement manipulées `TouchManipulationBitmap` objets, ce programme nécessite un dictionnaire :

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Notez comment la `Pressed` logique effectue une itération sur les `bitmapCollection` dans l’ordre inverse. Les bitmaps souvent chevauchent. Les bitmaps plus loin dans la collection concordent visuellement les bitmaps plus haut dans la collection. S’il existe plusieurs images bitmap sous le doigt appuie sur l’écran, celui qui est au premier plan doit être celui qui est manipulé par ce doigt.

Notez également que le `Pressed` logique déplace ce bitmap à la fin de la collection afin qu’il se déplace visuellement en haut de l’accumulation d’autres images bitmap.

Dans le `Moved` et `Released` événements, le `TouchAction` appels du gestionnaire le `ProcessingTouchEvent` méthode dans `TouchManipulationBitmap` comme programme précédent.

Enfin, le `PaintSurface` appels du gestionnaire le `Paint` (méthode) de chaque `TouchManipulationBitmap` objet :

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Le code effectue une itération sur la collection et affiche l’accumulation de bitmaps à partir du début de la collection à la fin :

[![](touch-images/bitmapscatterview-small.png "Capture d’écran triple de la page de vue de nuage de points de Bitmap")](touch-images/bitmapscatterview-large.png#lightbox "Triple capture d’écran de la page de vue de nuage de points de Bitmap")

## <a name="single-finger-scaling"></a>Mise à l’échelle d’un seul doigt

Une opération de mise à l’échelle nécessite généralement un mouvement de pincement à l’aide de deux doigts. Toutefois, il est possible d’implémenter la mise à l’échelle avec un seul doigt en demandant le doigt à déplacer les angles d’une image bitmap.

Cela est illustré dans le **unique à l’échelle de coin doigt** page. Étant donné que cet exemple utilise un type quelque peu différent de mise à l’échelle que qui implémentées dans le `TouchManipulationManager` (classe), il n’utilise pas cette classe ou la `TouchManipulationBitmap` classe. Au lieu de cela, toute la logique de tactile est dans le fichier code-behind. Il s’agit d’une logique un peu plus simple que d’habitude, car il effectue le suivi à la fois qu’un seul doigt et ignore simplement les doigts secondaire qui peuvent être toucher l’écran.

Le [ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) page instancie le `SKCanvasView` classe et crée un `TouchEffect` objet de suivi des événements tactiles :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Le [ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) fichier charge une ressource bitmap à partir de la **Media** directory et affiche à l’aide un `SKMatrix` objet défini en tant qu’un champ :

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

Cela `SKMatrix` objet est modifié par la logique de touch indiquée ci-dessous.

Le reste du fichier code-behind est le `TouchEffect` Gestionnaire d’événements. Il commence par convertir l’emplacement actuel du doigt pour un `SKPoint` valeur. Pour le `Pressed` type d’action, le gestionnaire vérifie qu’aucun autre doigt ne touche l’écran, et qu’il se trouve dans les limites de la bitmap.

La partie essentielle du code est un `if` instruction impliquant deux appels à la `Math.Pow` (méthode). Cette mathématiques vérifie si le doigt se trouve en dehors d’une ellipse qui remplit la bitmap. Dans ce cas, puis qui est une opération de mise à l’échelle. Il se trouve près d’un des angles de l’image bitmap et un point pivot est déterminé qui est l’angle opposé. Si le doigt se trouve au sein de cette ellipse, il est une opération de panoramique régulière :

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

Le `Moved` type d’action calcule une matrice correspondant à l’activité tactile à partir du moment que le doigt utilisateur appuie sur l’écran jusqu'à cette heure. Il concatène cette matrice avec la matrice en vigueur au moment où que le doigt enfoncé tout d’abord l’image bitmap. L’opération de mise à l’échelle est toujours relatif au coin opposé à celui qui le doigt couvertes.

Pour les bitmaps de petite taille ou allongées, une ellipse intérieure peut occupent la majeure partie de l’image bitmap et laissez les zones très petits dans les angles à l’échelle de l’image bitmap. Vous préférerez peut-être une approche un peu différente, auquel cas vous pouvez remplacer cet ensemble `if` bloc définit `isScaling` à `true` avec ce code :

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Ce code divise efficacement la zone de l’image bitmap en forme de losange intérieurs et quatre triangles dans les angles. Ainsi, beaucoup plus grande zones dans les angles pour récupérer et mettre à l’échelle de l’image bitmap.

## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Appel d’événements à partir d’effets](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
