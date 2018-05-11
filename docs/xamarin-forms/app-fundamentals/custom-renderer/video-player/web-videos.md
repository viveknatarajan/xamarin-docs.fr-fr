---
title: Lecture d’une vidéo du Web
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 322ae03fc813d180a6678f63b04488a17705523d
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="playing-a-web-video"></a>Lecture d’une vidéo du Web

Le `VideoPlayer` classe définit un `Source` propriété utilisée pour spécifier la source du fichier vidéo, ainsi que d’un `AutoPlay` propriété. `AutoPlay` paramètre par défaut de `true`, ce qui signifie que la vidéo doit être lu automatiquement après `Source` a été définie :

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }
        
        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

Le `Source` propriété est de type `VideoSource`, qui est inspirée le Xamarin.Forms [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) abstraire classe et ses dérivés de trois, [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/), [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/), et [ `StreamImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/). Aucune option de flux de données n’est disponible pour le `VideoPlayer` Toutefois, étant donné qu’iOS et Android ne prennent pas en charge une vidéo à partir d’un flux de données.

## <a name="video-sources"></a>Sources vidéo

Le résumé `VideoSource` classe se compose uniquement de trois méthodes statiques que les trois classes qui dérivent d’instancier `VideoSource`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

La `UriVideoSource` classe est utilisée pour spécifier un fichier vidéo téléchargeable avec un URI. Il définit une propriété unique de type `string`:

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

La gestion des objets de type `UriVideoSource` est décrite ci-dessous.

Le `ResourceVideoSource` classe est utilisée pour accéder à des fichiers vidéos sont stockés en tant que ressources incorporées dans l’application de plateforme, également spécifié avec un `string` propriété :

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

La gestion des objets de type `ResourceVideoSource` est décrite dans l’article [le chargement de vidéos de ressource Application](loading-resources.md). La `VideoPlayer` classe dispose d’aucune fonctionnalité pour charger un fichier vidéo est stocké en tant que ressource dans la bibliothèque .NET Standard.

La `FileVideoSource` classe est utilisée pour accéder à des fichiers vidéo à partir de la vidéothèque du périphérique. La propriété unique est également de type `string`:

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

La gestion des objets de type `FileVideoSource` est décrite dans l’article [l’accès à la vidéothèque du périphérique](accessing-library.md).

Le `VideoSource` classe inclut un `TypeConverter` attribut qui fait référence à `VideoSourceConverter`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

Ce convertisseur de type est appelé lorsque le `Source` est définie sur une chaîne en XAML. Voici la `VideoSourceConverter` classe :

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ? 
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

Le `ConvertFromInvariantString` méthode essaie de convertir la chaîne en un `Uri` objet. Si cette tentative réussit, et le schéma n’est pas `file:`, la méthode retourne un `UriVideoSource`. Sinon, elle retourne un `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Définition de la source vidéo

Logique impliquant des sources vidéo est implémentée dans les convertisseurs individuelle de la plateforme. Les sections suivantes montrent comment lire des vidéos dans les convertisseurs de plate-forme lors de la `Source` est définie sur une `UriVideoSource` objet.

### <a name="the-ios-video-source"></a>La source vidéo iOS

Deux sections de la `VideoPlayerRenderer` sont impliqués dans la définition de la source vidéo du lecteur vidéo. Lorsque Xamarin.Forms crée d’abord un objet de type `VideoPlayer`, le `OnElementChanged` méthode est appelée avec le `NewElement` propriété de l’objet d’arguments qui la valeur `VideoPlayer`. Le `OnElementChanged` les appels de méthode `SetSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Ultérieurement dans, lorsque le `Source` propriété est modifiée, le `OnElementPropertyChanged` méthode est appelée avec un `PropertyName` propriété de « Source », et `SetSource` est appelée à nouveau.

Pour lire un fichier vidéo dans iOS, un objet de type [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) est tout d’abord créé pour encapsuler le fichier vidéo, et qui est utilisé pour créer un [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), qui est ensuite remis à la `AVPlayer`objet. Voici comment la `SetSource` méthode gère le `Source` propriété lorsqu’il est de type `UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

Le `AutoPlay` propriété n’a aucun analogie dans les classes vidéo iOS, donc la propriété est examinée à la fin de la `SetSource` méthode à appeler le `Play` méthode sur le `AVPlayer` objet.

Dans certains cas, les vidéos suite lecture après la page avec le `VideoPlayer` accédé à la page d’accueil. Pour arrêter la vidéo, le `ReplaceCurrentItemWithPlayerItem` est également défini dans le `Dispose` remplacer :

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>La source vidéo Android

Le Android `VideoPlayerRenderer` doit définir la source vidéo du lecteur lors de la `VideoPlayer` est tout d’abord créé et versions ultérieures lorsque la `Source` modifications apportées aux propriétés :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Le `SetSource` méthode gère des objets de type `UriVideoSource` en appelant `SetVideoUri` sur la `VideoView` avec un Android `Uri` objet créé à partir de la chaîne URI. Le `Uri` classe est pleinement qualifié ici différencier .NET `Uri` classe :

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

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Le Android `VideoView` ne correspondante `AutoPlay` propriété, donc la `Start` méthode est appelée si une nouvelle vidéo a été définie.

Il existe une différence entre le comportement du iOS et Android convertisseurs si le `Source` propriété du `VideoPlayer` a la valeur `null`, ou si le `Uri` propriété de `UriVideoSource` a la valeur `null` ou une chaîne vide. Si le lecteur vidéo iOS est en cours de lecture vidéo et `Source` a la valeur `null` (ou la chaîne est `null` ou vide), `ReplaceCurrentItemWithPlayerItem` est appelée avec `null` valeur. La vidéo actuelle est remplacée et s’arrête.

Android ne prend pas en charge une fonctionnalité similaire. Si le `Source` est définie sur `null`, le `SetSource` ignore simplement par méthode, et la vidéo en cours continue d’être lu.

### <a name="the-uwp-video-source"></a>La source vidéo UWP

La plateforme Windows universelle `MediaElement` définit un `AutoPlay` propriété, qui est gérée dans le convertisseur comme toute autre propriété :

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

Le `SetSource` propriété gère un `UriVideoSource` objet en définissant le `Source` propriété de `MediaElement` pour .NET `Uri` valeur, ou à `null` si le `Source` propriété de `VideoPlayer` a la valeur `null`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>Définition d’une source de l’URL

Avec l’implémentation de ces propriétés dans les convertisseurs de trois, il est possible de lire une vidéo à partir d’une source de l’URL. Le **lire une vidéo de Web** page dans le [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) programme est défini par le fichier XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

Le `VideoSourceConverter` classe convertit la chaîne en un `UriVideoSource`. Lorsque vous accédez à la **lire la vidéo Web** page, la vidéo commence le chargement et démarre quand une quantité suffisante de données a été téléchargée et mis en mémoire tampon. La vidéo est 10 minutes environ, longueur :

[![Lire une vidéo de Web](web-videos-images/playwebvideo-small.png "lire une vidéo de Web")](web-videos-images/playwebvideo-large.png#lightbox "lire la vidéo sur le Web")

Sur chacune des trois plateformes, les contrôles de transport en fondu s’ils ne sont pas utilisés, mais peuvent être restaurés pour afficher en appuyant sur la vidéo.

Vous pouvez empêcher la vidéo automatiquement en définissant le `AutoPlay` propriété `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Vous devrez appuyer sur le **lire** bouton pour démarrer la vidéo.

De même, vous pouvez supprimer l’affichage des contrôles de transport en définissant le `AreTransportControlsEnabled` propriété `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Si vous définissez les deux propriétés `false`, la vidéo ne sont pas commencer la lecture, puis il n’y a aucun moyen de le démarrer ! Vous devez appeler `Play` à partir du fichier code-behind, ou pour créer vos propres contrôles de transport, comme décrit dans l’article [implémentation de contrôles personnalisés de vidéo Transport](custom-transport.md). 

Le **App.xaml** fichier inclut des ressources pour les deux autres vidéos :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />
            
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Référence un de ces autres films, vous pouvez remplacer l’URL explicite dans le **PlayWebVideo.xaml** de fichiers avec un `StaticResource` extension de balisage, auquel cas `VideoSourceConverter` n’est pas requis pour créer le `UriVideoSource` objet :

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Vous pouvez également définir le `Source` propriété à partir d’un fichier vidéo dans un `ListView`, comme décrit dans l’article suivant, [de liaison de Sources vidéo du lecteur](source-bindings.md).





## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
