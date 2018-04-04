---
title: Bitmap des principes de base
description: Charger des bitmaps de différentes sources et les afficher.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: b86068c2ed5063c25f76e81fdf477550b1437984
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="bitmap-basics"></a>Bitmap des principes de base

_Charger des bitmaps de différentes sources et les afficher._

La prise en charge des images bitmap dans SkiaSharp est assez longue. Cet article traite uniquement les notions de base &mdash; comment charger des bitmaps et pour les afficher :

![](bitmaps-images/bitmapssample.png "L’affichage de deux images")

Une image bitmap SkiaSharp est un objet de type [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Il existe plusieurs façons de créer une image bitmap, mais cet article restreint pour le [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) (méthode), qui charge le bitmap à partir d’un [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) objet qui fait référence à un fichier bitmap. Il convient d’utiliser le [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) classe qui dérive de `SKStream` , car il possède un constructeur qui accepte un .NET [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) objet.

Le **base Bitmaps** page dans le **SkiaSharpFormsDemos** programme montre comment charger des images bitmap à partir de trois sources différentes :

- Depuis Internet
- À partir d’une ressource incorporée dans le fichier exécutable
- À partir de la bibliothèque de photo de l’utilisateur

Trois `SKBitmap` objets ces trois sources sont définis en tant que champs dans le [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) classe :

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Le chargement d’une image Bitmap à partir du Web

Pour charger une image bitmap basée sur une URL, vous pouvez utiliser la [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) de classe, comme indiqué dans le code suivant exécuté dans le `BasicBitmapsPage` constructeur. Cette URL ici pointe vers une zone sur le site web de Xamarin avec certaines images de l’exemple. Un package sur le site web permet l’ajout d’une spécification pour redimensionner l’image bitmap à une largeur spécifique :

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Lorsque l’image bitmap a été téléchargé avec succès, la méthode de rappel passé à la `BeginGetResponse` s’exécute la méthode. Le `EndGetResponse` appel doit se trouver dans un `try` bloquer en cas d’une erreur s’est produite. Le `Stream` objet obtenu à partir de `GetResponseStream` n’est pas suffisante sur certaines plates-formes, donc le contenu de l’image bitmap est copié dans un `MemoryStream` objet. À ce stade, le `SKManagedStream` objet peut être créé. Est désormais fait référence au fichier de bitmap, ce qui est probablement un fichier JPEG ou PNG. Le `SKBitmap.Decode` méthode décode le fichier bitmap et stocke les résultats dans un format SkiaSharp interne.

Passé à la méthode de rappel à `BeginGetResponse` s’exécute une fois que le constructeur a terminé l’exécution, ce qui signifie que la `SKCanvasView` doit être invalidée pour permettre la `PaintSurface` gestionnaire à mettre à jour l’affichage. Toutefois, le `BeginGetResponse` rappel s’exécute dans un thread secondaire de l’exécution, il est nécessaire d’utiliser `Device.BeginInvokeOnMainThread` pour exécuter le `InvalidateSurface` méthode dans le thread d’interface utilisateur.

## <a name="loading-a-bitmap-resource"></a>Le chargement d’une ressource Bitmap

En termes de code, l’approche la plus simple pour le chargement des images bitmap est notamment une ressource bitmap directement dans votre application. Le **SkiaSharpFormsDemos** programme inclut un dossier nommé **Media** contenant un fichier bitmap nommé **monkey.png**. Dans le **propriétés** boîte de dialogue pour ce fichier, vous devez donner un tel fichier un **Action de génération** de **ressource incorporée**!

Chaque ressource incorporée a un *ID de ressource* qui comprend le nom du projet, le dossier et le nom de fichier, tous les connectés par des points : **SkiaSharpFormsDemos.Media.monkey.png**. Vous pouvez accéder à cette ressource en spécifiant cette ressource ID en tant qu’argument à la [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) méthode de la [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) classe :

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Cela `Stream` objet peut être converti directement à un `SKManagedStream` objet.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Le chargement d’une image Bitmap de la bibliothèque de photos

Il est également possible pour l’utilisateur à charger une photo à partir de la bibliothèque d’images de l’appareil. Cette fonctionnalité n’est pas fournie par Xamarin.Forms lui-même. Le travail nécessite un service de dépendance, tel que celui décrit dans l’article [prélèvement une Photo à partir de la bibliothèque d’images](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Le **IPicturePicker.cs** de fichiers et les trois **PicturePickerImplementation.cs** fichiers à partir de cet article ont été copiés sur les différents projets de la **SkiaSharpFormsDemos**solution et les nouveaux noms d’espace de noms. En outre, le Android **MainActivity.cs** fichier a été modifié comme décrit dans l’article et le projet iOS a été donné l’autorisation d’accéder à la bibliothèque de photos avec deux lignes vers le bas de la **info.plist**  fichier.

Le `BasicBitmapsPage` constructeur ajoute un `TapGestureRecognizer` à le `SKCanvasView` pour être averti de coefficients. Réception d’un clic, le `Tapped` gestionnaire obtient l’accès au service de dépendance de sélecteur d’images et les appels `GetImageStreamAsync`. Si un `Stream` est retourné, puis le contenu est copié dans un `MemoryStream`, comme requis par certains des plateformes. Le reste du code est similaire à deux autres techniques :

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Notez que la `Tapped` appels du Gestionnaire du `InvalidateSurface` méthode de la `SKCanvasView` objet. Cette opération génère un nouvel appel à la `PaintSurface` gestionnaire.

## <a name="displaying-the-bitmaps"></a>Afficher les Bitmaps

Le `PaintSurface` gestionnaire a besoin pour afficher trois images bitmap. Le gestionnaire part du principe que le téléphone est en mode portrait et divise la zone de dessin verticalement en trois parties égales.

La première bitmap s’affiche avec le plus simple [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) (méthode). Il que vous suffit de spécifier sont les coordonnées X et Y, où le coin supérieur gauche de l’image bitmap doit être positionné :

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Bien qu’un `SKPaint` est défini, il a la valeur par défaut `null` et vous pouvez l’ignorer. Les pixels de l’image bitmap sont simplement transférées aux pixels de la surface d’affichage avec un mappage.

Un programme peut obtenir les dimensions en pixels d’une image bitmap avec la [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) et [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) propriétés. Ces propriétés permettent de calculer des coordonnées pour positionner l’image bitmap dans le centre de tiers supérieur de la zone de dessin :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Les deux images sont affichés avec une version de [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) avec un `SKRect` paramètre :

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Une troisième version de [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) a deux `SKRect` arguments pour spécifier un sous-ensemble rectangulaire de la bitmap à afficher, mais cette version n’est pas utilisé dans cet article.

Voici le code pour afficher la bitmap chargée à partir d’un bitmap de la ressource incorporée :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

La bitmap est étendue aux dimensions du rectangle, c’est pourquoi le singe est étiré horizontalement dans ces captures d’écran :

[![](bitmaps-images/basicbitmaps-small.png "Une capture d’écran triple de la page de base Bitmaps")](bitmaps-images/basicbitmaps-large.png#lightbox "une capture d’écran triple de la page de base Bitmaps")

La troisième image &mdash; que vous pouvez visualiser uniquement si vous exécutez le programme et chargez une photo à partir de votre propre bibliothèque d’images &mdash; s’affiche également dans un rectangle, mais du rectangle position et la taille sont ajustées pour conserver les proportions de l’image bitmap. Ce calcul est un peu plus complexe, car elle requiert le calcul une échelle en fonction de la taille de la bitmap et le rectangle de destination et centrage du rectangle dans la zone :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Si aucune bitmap n’a encore été chargé à partir de la bibliothèque d’images, puis le `else` bloc affiche du texte pour inviter l’utilisateur à cliquer sur l’écran.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Sélection d’une Photo dans la bibliothèque d’images](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
