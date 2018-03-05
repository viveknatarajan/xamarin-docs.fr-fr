---
title: Manipulations tactile
description: "Matrice de l’utilisation des transformations pour implémenter en faisant glisser des fonctions tactiles, pincement et rotation"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: b418e0179c95a424c88d5f5063a09f984bb13ec0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="touch-manipulations"></a>Manipulations tactile

_Matrice de l’utilisation des transformations pour implémenter en faisant glisser des fonctions tactiles, pincement et rotation_

Dans les environnements tactiles telles que celles sur les appareils mobiles, les utilisateurs utilisent souvent leurs doigts pour manipuler des objets sur l’écran. Les mouvements courantes telles que faites glisser un doigt et un pincement de deux à un doigt peuvent déplacer et mettre à l’échelle des objets ou même faire pivoter les. Les mouvements sont généralement implémentées à l’aide de la transformation de matrices, et cet article vous explique comment procéder.

![](touch-images/touchmanipulationsexample.png "Une image bitmap soumise à la rotation, de mise à l’échelle et de translation")

## <a name="manipulating-one-bitmap"></a>Manipulation de l’image Bitmap

Le **Touch une Manipulation** page montre tactile de manipulations sur une seule bitmap.
Cet exemple utilise l’effet de suivi des fonctions tactiles présentée dans l’article [appeler des événements à partir des effets](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Plusieurs autres fichiers fournissent la prise en charge pour le **Touch une Manipulation** page. La première est la [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) énumération qui indique les différents types de manipulation tactile implémentée par le code que vous allez voir :

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

`PanOnly` est un glisser un doigt qui est implémenté avec la traduction. Toutes les options suivantes également inclure le panoramique, mais implique deux doigts : `IsotropicScale` est une opération de pincement qui résulte de l’objet de mise à l’échelle de manière égale dans le sens horizontal et vertical. `AnisotropicScale` permet la mise à l’échelle inégaux.

Le `ScaleRotate` option est destinée à un doigt deux échelle et de rotation. Mise à l’échelle est Isotrope. Implémentation de rotation à un doigt deux avec la mise à l’échelle ANISOTROPIQUE est problématique, car les mouvements de doigt sont essentiellement les mêmes.

Le `ScaleDualRotate` option ajoute la rotation d’un doigt. Quand un seul doigt fait glisser l’objet, l’objet pivote tout d’abord autour de son centre afin que le centre de l’objet est alignée avec le vecteur de déplacement.

Le [ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) fichier inclut un `Picker` avec les membres de le `TouchManipulationMode` énumération :

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

Vers le bas est une `SKCanvasView` et un `TouchEffect` attaché à la cellule unique `Grid` qui l’englobe.

Le [ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) fichier code-behind a un `bitmap` champ, mais il n’est pas de type `SKBitmap`. Le type est `TouchManipulationBitmap` (une classe que vous le verrez) :

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Le constructeur instancie un `TouchManipulationBitmap` objet, en passant au constructeur un `SKBitmap` obtenue à partir d’une ressource incorporée. Le constructeur se termine en définissant le `Mode` propriété de la `TouchManager` propriété de la `TouchManipulationBitmap` objet à un membre de la `TouchManipulationMode` énumération.

Le `SelectedIndexChanged` gestionnaire pour le `Picker` définit également cette `Mode` propriété :

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

Le `TouchAction` Gestionnaire de la `TouchEffect` instancié dans les appels de fichier XAML deux méthodes de `TouchManipulationBitmap` nommé `HitTest` et `ProcessTouchEvent`:

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

Si le `HitTest` méthode retourne `true` & #x 2014 ; signification qu’un doigt a touché l’écran dans la zone occupée par l’image bitmap & #x 2014 ; puis l’ID tactile est ajouté à la `TouchIds` collection. Cet ID représente la séquence des événements tactiles pour ce doigt jusqu'à ce que le doigt lève à partir de l’écran. Si plusieurs doigts touchent l’image bitmap, puis le `touchIds` collection contient un ID tactile pour chaque doigt.

Le `TouchAction` gestionnaire appelle également la `ProcessTouchEvent` classe dans `TouchManipulationBitmap`. Cette propriété est si certains (mais pas tous) des fonctions tactiles réel le traitement se produit.

Le [ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) classe est une classe wrapper pour `SKBitmap` contenant du code pour restituer l’image bitmap et de traiter les événements tactiles. Il fonctionne conjointement avec plusieurs généralisé de code dans un `TouchManipulationManager` classe (vous verrez bientôt).

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

Cela `Matrix` propriété est la transformation cumulée résultant de toutes les activités tactile. Comme vous le verrez, chaque événement tactile est résolu dans une matrice, qui est ensuite concaténée avec le `SKMatrix` valeur stocké par le `Matrix` propriété.

Le `TouchManipulationBitmap` objet dessine lui-même son `Paint` (méthode). L’argument est un `SKCanvas` objet. Cela `SKCanvas` disposez peut-être déjà d’une transformation appliquée, donc la `Paint` méthode concatène le `Matrix` propriété associé à l’image bitmap à la transformation existante et restaure la zone de dessin lorsqu’il a terminé :

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

Le `HitTest` retourne de la méthode `true` si l’utilisateur touche l’écran à un point dans les limites de la bitmap. Comme l’utilisateur manipule la bitmap, l’image bitmap peut-être être pivotée, ou même (via une combinaison de rotation et de mise à l’échelle ANISOTROPIQUE) dans la forme d’un parallélogramme. Vous pouvez craindre que le `HitTest` (méthode) doit implémenter les plutôt complexe géométrie analytique dans ce cas.

Toutefois, un raccourci est disponible :

Déterminer si un point se situe dans les limites d’un rectangle transformé est identique à la détermination de si un point transformé inverse se situe dans les limites du rectangle transformé. Qui est plus facile de calcul et il peut utiliser la pratique `Contains` méthode définie par `SKRect`:

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

La deuxième méthode publique de `TouchManipulationBitmap` est `ProcessTouchEvent`. Lorsque cette méthode est appelée, il a déjà été établi que l’événement tactile appartienne à cette image bitmap particulier. La méthode gère un dictionnaire de [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) objets, qui est simplement le point précédent et le nouveau point de chaque doigt :

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Voici le dictionnaire et `ProcessTouchEvent` méthode lui-même :

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

Dans le `Moved` et `Released` événements, les appels de méthode `Manipulate`. À ce stade le `touchDictionary` contient un ou plusieurs `TouchManipulationInfo` objets. Si le `touchDictionary` contient un élément, il est probable que le `PreviousPoint` et `NewPoint` valeurs ne sont pas égales et représentent le déplacement d’un doigt. Si plusieurs doigts touchent la bitmap, le dictionnaire contient plusieurs éléments, mais uniquement un de ces éléments dispose de différents `PreviousPoint` et `NewPoint` valeurs. Les autres ont égal `PreviousPoint` et `NewPoint` valeurs.

Ceci est important : le `Manipulate` méthode peut supposer qu’il est en train de traiter le déplacement d’un seul doigt. Au moment de cet appel, aucune des autres doigts sont mobile, et si elles sont réellement déplacez (comme c’est probablement), les mouvements sont traitées dans les appels ultérieurs à `Manipulate`.

Le `Manipulate` méthode copie tout d’abord le dictionnaire dans un tableau pour des raisons pratiques. Il ignore tout ce que les deux premières entrées. Si plus de deux doigts tente de manipuler l’image bitmap, les autres sont ignorés. `Manipulate` est le dernier membre de `TouchManipulationBitmap`:

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

Si un doigt est la manipulation de l’image bitmap, `Manipulate` appelle la `OneFingerManipulate` méthode de la `TouchManipulationManager` objet. Pour deux doigts, il appelle `TwoFingerManipulate`. Les arguments de ces méthodes sont les mêmes : le `prevPoint` et `newPoint` arguments représentent le doigt qui se déplacent. Mais le `pivotPoint` argument est différent pour les deux appels :

Pour la manipulation d’un doigt, le `pivotPoint` est le centre de l’image bitmap. Il s’agit pour permettre un doigt rotation. Pour une manipulation de deux à un doigt, l’événement indique le déplacement d’un seul doigt, afin que le `pivotPoint` est le doigt ne bouge pas.

Dans les deux cas, `TouchManipulationManager` retourne un `SKMatrix` valeur, la méthode concatène avec actuel `Matrix` propriété qui `TouchManipulationPage` utilise pour restituer l’image bitmap.

`TouchManipulationManager` généralisé et n’utilise aucun autre fichier à l’exception de `TouchManipulationMode`. Vous pouvez être en mesure d’utiliser cette classe sans modification dans vos propres applications. Il définit une propriété unique de type `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Toutefois, vous souhaiterez probablement éviter le `AnisotropicScale` option. Il est très facile avec cette option pour manipuler la bitmap afin qu’un des facteurs d’échelle devient égal à zéro. Ainsi, l’image bitmap disparaissent de la vue, jamais à retourner. Si vous avez vraiment besoin la mise à l’échelle ANISOTROPIQUE, que vous souhaitez améliorer la logique pour éviter des résultats indésirables.

`TouchManipulationManager` utilise des vecteurs, mais étant donné qu’aucun `SKVector` structure dans SkiaSharp, `SKPoint` est utilisé à la place. `SKPoint` prend en charge que l’opérateur de soustraction et le résultat peuvent être traitée comme un vecteur. La logique uniquement vecteur spécifiques requises pour être ajouté est un `Magnitude` calcul :

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

Chaque fois que la rotation a été sélectionnée, les deux méthodes de manipulation d’un doigt et deux doigt le gérer tout d’abord la rotation. Si la rotation est détectée, le composant de rotation est alors effectivement supprimé. Ce qui reste est interprété comme le panoramique et de mise à l’échelle.

Voici le `OneFingerManipulate` (méthode). Si un doigt rotation n’a pas été activée, puis la logique est simple & #x 2014 ; elle utilise simplement le point précédent et le point pour construire un vecteur nommé `delta` qui correspond exactement à la traduction. Rotation d’un doigt activé, la méthode utilise angles à partir du point pivot (le centre de l’image bitmap) pour le point précédent et le point pour construire une matrice de rotation :

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

Dans le `TwoFingerManipulate` , le point pivot est la position du doigt qui ne bouge pas dans ce cas particulier tactile. La rotation est très similaire à la rotation d’un doigt, et ensuite le vecteur nommé `oldVector` (basé sur le point précédent) est ajustée pour la rotation. Le déplacement des autres sont interprété comme la mise à l’échelle :

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

Vous remarquerez qu’il n’existe aucune conversion explicite dans cette méthode. Toutefois, à la fois le `MakeRotation` et `MakeScale` méthodes sont basées sur le point de pivot, et qui inclut une conversion implicite. Si vous utilisez deux doigts sur l’image bitmap et faisant glisser dans la même direction, `TouchManipulation` obtenez une série d’événements tactiles en alternant entre deux doigts. Comme chaque doigt par rapport aux autres, résultats mise à l’échelle ou la rotation, mais elle est invalidée par le déplacement d’autres doigt, et le résultat est la traduction.

La seule partie restante de la **Touch une Manipulation** page est la `PaintSurface` gestionnaire dans le `TouchManipulationPage` fichier code-behind. Cette action appelle le `Paint` méthode de la `TouchManipulationBitmap`, qui s’applique à la matrice représentant l’activité cumulée tactile :

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

Le `PaintSurface` gestionnaire se termine en affichant un `MatrixDisplay` objet en affichant la matrice tactile cumulée :

[![](touch-images/touchmanipulation-small.png "Capture d’écran de triple de la page de Manipulation de Touch")](touch-images/touchmanipulation-large.png "Triple capture d’écran de la page de Manipulation de tactile")

## <a name="manipulating-multiple-bitmaps"></a>Manipuler plusieurs Bitmaps

Un des avantages de l’isolation du code de traitement de l’interaction tactile dans les classes telles que `TouchManipulationBitmap` et `TouchManipulationManager` est la possibilité de réutiliser ces classes dans un programme qui permet à l’utilisateur manipuler plusieurs images bitmap.

Le **vue à nuages de points de Bitmap** page montre comment procéder. Au lieu de la définition d’un champ de type `TouchManipulationBitmap`, le [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) classe définit un `List` des objets de bitmap :

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
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
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

Le constructeur de charge dans toutes les images disponibles en tant que ressources incorporées et les ajoute à la `bitmapCollection`. Notez que la `Matrix` propriété est initialisée sur chaque `TouchManipulationBitmap` de l’objet, afin des angles supérieur gauche de chaque bitmap sont décalés de 100 pixels.

Le `BitmapScatterView` page doit également gérer les événements tactiles pour plusieurs images bitmap. Au lieu de définir un `List` de touch ID des actuellement manipulées `TouchManipulationBitmap` des objets, ce programme requiert un dictionnaire :

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

Notez comment la `Pressed` logique parcourt le `bitmapCollection` dans l’ordre inverse. Les bitmaps souvent chevauchent. Les bitmaps plus loin dans la collection concordent visuellement les bitmaps plus haut dans la collection. S’il existe plusieurs bitmaps sous le doigt appuie sur l’écran, celle au premier plan doit être celui qui est manipulé par ce doigt.

Notez également que la `Pressed` logique déplace cette bitmap à la fin de la collection afin qu’elle se déplace visuellement en haut de la pile d’autres images bitmap.

Dans le `Moved` et `Released` événements, le `TouchAction` appels du Gestionnaire du `ProcessingTouchEvent` méthode dans `TouchManipulationBitmap` comme programme précédent.

Enfin, le `PaintSurface` appels du Gestionnaire du `Paint` méthode de chaque `TouchManipulationBitmap` objet :

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

Le code parcourt la collection et affiche l’accumulation des images bitmap depuis le début de la collection à la fin :

[![](touch-images/bitmapscatterview-small.png "Capture d’écran de triple de la page de vue à nuages de points de Bitmap")](touch-images/bitmapscatterview-large.png "Triple capture d’écran de la page de vue de Bitmap à nuages de points")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Appel des événements à partir des effets](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
