---
title: Rotations 3D
description: Transformations non affines permet de faire pivoter des objets 2D dans un espace 3D.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 2f5562475db17b7451fe7cb2ee8bbf4ccb782a87
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="3d-rotations"></a>Rotations 3D

_Transformations non affines permet de faire pivoter des objets 2D dans un espace 3D._

Une application courante de transformations non affines simule la rotation d’un objet 2D dans un espace 3D :

![](3d-rotation-images/3drotationsexample.png "Une chaîne de texte pivoté dans un espace 3D")

Cette tâche consiste à utiliser des rotations en trois dimensions, puis dérivation et non affines `SKMatrix` transformation qui effectue ces rotations 3D.

Il est difficile à développer ce `SKMatrix` transformation fonctionne uniquement dans les deux dimensions. Le travail devient plus facile lorsque cette matrice 3 x 3 est dérivé d’une matrice 4 x 4 utilisée dans les graphiques 3D. SkiaSharp inclut la [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) classe pour cet objectif, mais certains en arrière-plan dans les graphiques 3D est nécessaire pour comprendre les rotations 3D et la matrice de transformation de 4 x 4.

Un système de coordonnées en trois dimensions ajoute un troisième axe appelé Z. point de vue conceptuel, l’axe des Z perpendiculairement à l’écran. Points de coordonnées dans l’espace 3D sont indiqués par trois nombres : (x, y, z). Dans la 3D système de coordonnées utilisé dans cet article, augmenter les valeurs de X sont à droite et des valeurs croissantes de Y s’arrêtent, comme dans les deux dimensions. Les valeurs Z positifs croissantes sortent de l’écran. L’origine sont l’angle supérieur gauche, comme dans les graphiques 2D. Vous pouvez considérer l’écran comme un plan XY avec l’axe des Z perpendiculairement à ce plan.

Il s’agit d’un système de coordonnées de gauche. Si vous pointez l’index de la main gauche dans la direction de positif X coordonnées (à droite), et les coordonnées de votre doigt intermédiaire dans le sens d’augmentation Y (vers le bas), puis votre pouce pointe dans le sens d’augmentation des coordonnées Z — s’étend à partir de l’écran.

Dans les graphiques 3D, les transformations sont basées sur une matrice 4 x 4. Voici la matrice d’identité 4 x 4 :

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

Dans l’utilisation d’une matrice 4 x 4, il est pratique identifier les cellules avec leurs numéros de ligne et de colonne :

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Toutefois, le SkiaSharp `Matrix44` classe est un peu différent. La seule façon de définir ou obtenir des valeurs de cellules de `SKMatrix44` est à l’aide de la [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) indexeur. Les index de ligne et de colonne sont base zéro plutôt que basé sur un, et les lignes et colonnes sont échangés. La cellule M14 dans le diagramme ci-dessus est accessible à l’aide de l’indexeur `[3, 0]` dans un `SKMatrix44` objet.

Dans un système de graphiques 3D, un point 3D (x, y, z) est converti en une matrice de-1 à 4 pour multiplier par la matrice de transformation de 4 x 4 :

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Analogue à 2D les transformations qui se produisent en trois dimensions, les transformations 3D sont supposées se déroule en quatre dimensions. La quatrième dimension est appelée W, et l’espace 3D est censé pour exister dans l’espace 4D de W coordonnées sont égales à 1. Les formules de transformation sont les suivantes :

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' = M14·x + M24·y + M34·z + M44

Il est évident à partir de la transformation de formules qui les cellules `M11`, `M22`, `M33` sont des facteurs d’échelle dans les directions X, Y et Z, et `M41`, `M42`, et `M43` sont des facteurs de traduction pour les X, Y et Z directions.

Pour convertir ces coordonnées dans l’espace 3D où W est égal à 1, x', y', et z 'coordonnées sont tous divisées par w' :

x » = x' / w'

y » = y' / w'

z » = z' / w'

w » = w' / w' = 1

Cette division par w' propose la perspective dans l’espace 3D. Si w » est égal à 1, alors aucune perspective se produit.

Rotations dans l’espace 3D peuvent être complexes, mais que les rotations la plus simple ceux autour des axes X, Y et Z. Une rotation d’angle α autour de l’axe X est cette matrice :

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Valeurs de X restent les mêmes lorsqu’il est soumis à cette transformation. Rotation autour de l’axe des Y laisse les valeurs Y inchangé :

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Rotation autour de l’axe Z est le même que dans les graphiques 2D :

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

La direction de rotation est impliquée par l’ergonomie du système de coordonnées. Il s’agit d’un système de gauche, si vous pointez le curseur de la main gauche pour augmenter les valeurs pour un axe spécifique — à droite de rotation autour de l’axe X, vers le bas de la rotation autour de l’axe Y et à vous de rotation autour de l’axe Z, puis la courbe de v les autres doigts indique la direction de rotation pour les valeurs positives.

`SKMatrix44` a généralisé statique [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) et [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) méthodes qui vous permettent de spécifier l’axe autour duquel la rotation se produit :

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Pour la rotation autour de l’axe X, définir les trois premiers arguments à 1, 0, 0. Pour la rotation autour de l’axe Y, les définir sur 0, 1, 0 et de rotation autour de l’axe Z, les définir sur 0, 0, 1.

La quatrième colonne de 4-par-4 est pour la perspective. Le `SKMatrix44` n’a aucune méthode pour créer des transformations de point de vue, mais vous pouvez créer un vous-même à l’aide du code suivant :

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

La raison de nom de l’argument `depth` sera bientôt évident. Ce code crée la matrice :

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Les formules de la transformation entraînent le calcul suivant de w':

w' =-z / profondeur + 1

Ceci afin de réduire les coordonnées X et Y lorsque les valeurs Z sont inférieures à zéro (conceptuellement derrière le plan XY) et pour augmenter les coordonnées X et Y pour les valeurs positives de Z. Lorsque la coordonnée Z est égale à `depth`, puis w » est égal à zéro, et les coordonnées deviennent infinies. Les systèmes graphiques en trois dimensions sont construites autour d’une métaphore de l’appareil photo et le `depth` valeur ici représente la distance de l’appareil photo de l’origine du système de coordonnées. Si un objet graphique a un Z coordonner c'est-à-dire `depth` unités à l’origine, il affecte l’objectif de l’appareil photo sur le plan conceptuel et devient volumineux à l’infini.

N’oubliez pas que vous allez probablement utiliser cela `perspectiveMatrix` valeur en association avec des matrices de rotation. Si un objet de graphique en rotation contient les coordonnées X ou Y supérieur `depth`, puis la rotation de cet objet dans un espace 3D est susceptible d’impliquer les coordonnées Z supérieur `depth`. Cela doit être évité ! Lors de la création `perspectiveMatrix` vous souhaitez définir `depth` avec une valeur suffisamment élevée pour toutes les coordonnées de l’objet graphics, quel que soit la façon dont elle est pivotée. Cela garantit qu’il n’a jamais la division par zéro.

Combinaison de perspective et rotation 3D nécessite la multiplication des matrices de 4 x 4 entre elles. Pour cela, `SKMatrix44` définit des méthodes de concaténation. Si `A` et `B` sont `SKMatrix44` des objets, puis le code suivant définit un égal à A × b :

```csharp
A.PostConcat(B);
```

Lorsqu’une matrice de transformation de 4 x 4 est utilisée dans un système de graphiques 2D, il est appliqué aux objets 2D. Ces objets sont fixes et sont censés pour avoir les coordonnées Z égale à zéro. La multiplication de la transformation est un peu plus simple que la transformation présentée précédemment :

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Cette valeur 0 pour les résultats de plan dans les formules de transformation qui n’impliquent pas de toutes les cellules de la troisième ligne de la matrice :

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w' = M14·x + M24·y + M44

En outre, le z' coordonnée est également sans intérêt ici. Lorsqu’un objet 3D est affiché dans un système de graphiques 2D, il est réduit à un objet à deux dimensions en ignorant les valeurs des coordonnées Z. Les formules de transformation sont simplement ces deux :

x » = x' / w'

y » = y' / w'

Cela signifie que la troisième ligne *et* troisième colonne de la matrice 4 x 4 peut être ignoré.

Mais si qui est le cas, pourquoi est la matrice 4 x 4 même nécessaire en premier lieu ?

Bien que la troisième ligne et la troisième colonne de 4-par-4 sont sans effet pour les transformations à deux dimensions, la troisième ligne et colonne *faire* jouent un rôle avant que quand différents `SKMatrix44` valeurs sont multipliés. Par exemple, supposons que vous multipliez rotation autour de l’axe des Y avec la transformation du point de vue :

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

Dans le produit, la cellule `M14` contient maintenant une valeur de la perspective. Si vous souhaitez appliquer cette matrice aux objets 2D, la troisième ligne et colonne sont éliminés pour la convertir en une matrice 3 x 3 :

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Maintenant, il peut être utilisé pour transformer un point 2D :

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Les formules de transformation sont :

x' = cos (α) ·x

y' = y

z' = (sin (α) / profondeur) ·x + 1

À présent diviser tout en z » :

x" = cos(α)·x / ((sin(α)/depth)·x + 1)

y » = y / ((sin (α) / profondeur) ·x + 1)

Lorsque les objets 2D pivotent avec un angle positif autour de l’axe Y, puis est positive valeurs X sont bas à l’arrière-plan lors de la valeur négative les valeurs X sont fournis au premier plan. Les valeurs X semblent se rapprocher de l’axe des Y (qui est régie par la valeur du cosinus) en tant que coordonnées plus éloigné de l’axe des Y devient plus petits ou qu’elles s’éloigne de la visionneuse ou proche de la visionneuse.

Lorsque vous utilisez `SKMatrix44`, effectuer une rotation 3D les les opérations de point de vue en multipliant différents `SKMatrix44` valeurs. Ensuite, vous pouvez extraire une matrice 3 x 3 à deux dimensions de 4 x 4 à l’aide de la matrice le [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) propriété de la `SKMatrix44` classe. Cette propriété retourne un familier `SKMatrix` valeur.

Le **Rotation 3D** page vous permet de vous faire des essais avec rotation 3D. Le [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) fichier instancie quatre curseurs pour définir la rotation autour des axes X, Y et Z et pour définir une valeur de profondeur :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Notez que la `depthSlider` est initialisée avec un `Minimum` valeur de 250. Cela implique que l’objet 2D en rotation ici a des coordonnées X et Y, limitées à un cercle défini par un rayon de 250 pixels autour de l’origine. Rotation de cet objet dans l’espace 3D aura toujours pour résultat dans les valeurs de coordonnées inférieure à 250.

Le [ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) fichier code-behind se charge dans une image bitmap de 300 pixels carrés :

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Si la transformation 3D est centrée sur cette image bitmap, puis coordonnées X et Y compris entre –150 et 150, tandis que les angles étant 212 pixels à partir du centre, tout est dans le rayon de 250 pixels.

Le `PaintSurface` gestionnaire crée `SKMatrix44` objets basé sur les curseurs et multiplie les à l’aide de `PostConcat`. Le `SKMatrix` valeur extraite de la dernière `SKMatrix44` objet est entouré par translation pour centrer la rotation dans le centre de l’écran :

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

Lorsque vous utilisez le curseur quatrième, vous remarquerez que les paramètres de profondeur différente ne déplacent pas l’objet en dehors de la visionneuse, mais à la place de modifier l’étendue de l’effet de perspective :

[![](3d-rotation-images/rotation3d-small.png "Capture d’écran de triple de la page de Rotation 3D")](3d-rotation-images/rotation3d-large.png#lightbox "Triple capture d’écran de la page de Rotation 3D")

Le **animés de Rotation 3D** utilise également `SKMatrix44` pour animer une chaîne de texte dans un espace 3D. Le `textPaint` objet défini tel qu’un champ est utilisé dans le constructeur pour déterminer les limites du texte :

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

Le `OnAppearing` remplacement définit trois Xamarin.Forms `Animation` objets à animer la `xRotationDegrees`, `yRotationDegrees`, et `zRotationDegrees` champs à différentes vitesses. Notez que les périodes de ces animations sont définies pour les nombres premiers, 5 secondes, 7 et 11 secondes, pour la combinaison seulement répète chaque 385 secondes ou plus de 10 minutes :

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Comme dans le programme précédent, le `PaintCanvas` gestionnaire crée `SKMatrix44` de valeurs de rotation et de la perspective et les multiplie entre eux :

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Cette rotation 3D est entourée de plusieurs transformations 2D pour atteindre le centre de rotation du centre de l’écran et à l’échelle de la taille de la chaîne de texte afin qu’il soit la largeur de l’écran :

[![](3d-rotation-images/animatedrotation3d-small.png "Capture d’écran de triple de la page 3D animés de Rotation")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Triple capture d’écran de la page 3D animés de Rotation")


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
