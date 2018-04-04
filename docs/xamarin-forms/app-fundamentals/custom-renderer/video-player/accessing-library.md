---
title: L’accès à la vidéothèque du périphérique
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: eb3d66630613225c9b2becaa20f73a82f409ce7e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="accessing-the-devices-video-library"></a>L’accès à la vidéothèque du périphérique

Les appareils mobiles les plus récents et les ordinateurs de bureau ont la possibilité d’enregistrements vidéos à l’aide photo de l’appareil. Les vidéos créé par un utilisateur sont ensuite stockées sous forme de fichiers sur l’appareil. Ces fichiers peuvent être récupérées à partir de la bibliothèque d’images et lu par le `VideoPlayer` classe comme toute autre vidéo.

## <a name="the-photo-picker-dependency-service"></a>Le service de dépendance de sélecteur de photo

Chacune des trois plateformes inclut une fonctionnalité qui permet à l’utilisateur de sélectionner une photo ou vidéo à partir de la bibliothèque d’images de l’appareil. La première étape de la lecture d’une vidéo à partir de la bibliothèque d’images de l’appareil est création d’un service de dépendance qui appelle le sélecteur d’image sur chaque plateforme. Le service de dépendance décrit ci-dessous est très similaire à celui défini dans le [ **prélèvement une Photo à partir de la bibliothèque d’images** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) de l’article, à ceci près que le sélecteur de vidéo retourne un nom de fichier, plutôt qu’un `Stream`objet.

Projet de bibliothèque PCL définit une interface nommée `IVideoPicker` pour le service de dépendance :

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Chacune des trois plateformes contient une classe nommée `VideoPicker` qui implémente cette interface.

### <a name="the-ios-video-picker"></a>Le sélecteur de vidéo iOS

IOS `VideoPicker` utilise iOS [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) pour accéder à la bibliothèque d’images, en spécifiant qu’il doit être limité aux vidéos (appelés « films ») dans le fichier iOS `MediaType` propriété. Notez que `VideoPicker` implémente de manière explicite la `IVideoPicker` interface. Notez également le `Dependency` attribut qui identifie cette classe comme un service de dépendance. Voici deux spécifications qui permettent de Xamarin.Forms pour trouver le service de dépendance dans le projet de plateforme :

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Le sélecteur de vidéo Android

L’implémentation Android de `IVideoPicker` nécessite une méthode de rappel qui fait partie de l’activité de l’application. La raison pour laquelle le `MainActivity` classe définit deux propriétés, un champ et une méthode de rappel :

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

Le `OnCreate` méthode dans `MainActivity` stocke sa propre instance de la méthode statique `Current` propriété. Cela permet l’implémentation de `IVideoPicker` pour obtenir le `MainActivity` instance pour démarrer le **sélectionner la vidéo** sélecteur :

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Les ajouts à la `MainActivity` objet sont le seul code dans le [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) solution où le code d’application normal doit être modifiée pour prendre en charge la `FormsVideoLibrary` classes.

### <a name="the-uwp-video-picker"></a>Le sélecteur de vidéo UWP

L’implémentation de la plateforme Windows universelle de la `IVideoPicker` interface utilise UWP [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Commence la recherche de fichiers avec la bibliothèque d’images et il restreint les types de fichiers MP4 et WMV (Windows Media Video) :

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Appeler le service de dépendance

Le **lire la vidéo bibliothèque** page de la [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) programme montre comment utiliser le service de dépendance sélecteur vidéo. Le fichier XAML contient un `VideoPlayer` instance et un `Button` intitulé **afficher une vidéo de la bibliothèque**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Le fichier code-behind contient le `Clicked` gestionnaire pour le `Button`. L’appel de service de dépendance requiert un appel à `DependencyService.Get` pour obtenir de l’implémentation d’un `IVideoPicker` interface dans le projet de plateforme. Le `GetVideoFileAsync` méthode est ensuite appelée sur cette instance :

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

Le `Clicked` Gestionnaire utilise ensuite ce nom de fichier pour créer un `FileVideoSource` objet et lui affecter la `Source` propriété de la `VideoPlayer`.

Chacun de la `VideoPlayerRenderer` contient des classes de code dans son `SetSource` méthode pour les objets de type `FileVideoSource`. Ceux-ci sont illustrés ci-dessous :

### <a name="handling-ios-files"></a>Gestion des fichiers d’e/s

La version iOS de `VideoPlayerRenderer` processus `FileVideoSource` objets à l’aide de la méthode statique `Asset.FromUrl` méthode avec le nom de fichier. Cela créer un `AVAsset` objet représentant le fichier dans la bibliothèque d’images de l’appareil :

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Gestion des fichiers Android

Lors du traitement des objets de type `FileVideoSource`, l’implémentation Android de `VideoPlayerRenderer` utilise le `SetVideoPath` méthode `VideoView` pour spécifier le fichier dans la bibliothèque d’images de l’appareil :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>Gestion des fichiers de la plateforme Windows universelle

Lors de la gestion des objets de type `FileVideoSource`, l’implémentation de la plateforme Windows universelle de la `SetSource` méthode doit créer un `StorageFile` de l’objet, ouvrez ce fichier pour la lecture et passer l’objet de flux de données à la `SetSource` méthode de la `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

Pour les trois plateformes, la vidéo démarre presque immédiatement après la vidéo source est définie, car le fichier est sur le périphérique et n’a pas besoin d’être téléchargées.



## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Sélection d’une Photo dans la bibliothèque d’images](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
