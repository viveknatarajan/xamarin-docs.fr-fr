---
title: Créer les lecteurs vidéo de plateforme
description: Cet article explique comment implémenter un convertisseur personnalisé de lecteur vidéo sur chaque plateforme, à l’aide de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 0090ec798e8d7b1dfb9bd8e25f09d71ec0353b45
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171909"
---
# <a name="creating-the-platform-video-players"></a>Créer les lecteurs vidéo de plateforme

Le [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) solution contient tout le code pour implémenter un lecteur vidéo pour Xamarin.Forms. Il inclut également une série de pages qui montre comment utiliser le lecteur vidéo dans une application. Tous les `VideoPlayer` ses convertisseurs de plateforme et de code se trouvent dans des dossiers de projet nommés `FormsVideoLibrary`et également utiliser l’espace de noms `FormsVideoLibrary`. Cela doit faciliter copiez les fichiers dans votre propre application et les classes de référence.

## <a name="the-video-player"></a>Le lecteur vidéo

Le [ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) classe fait partie de la **VideoPlayerDemos** bibliothèque .NET Standard qui est partagé entre les plateformes. Il dérive `View`:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Les membres de cette classe (et le `IVideoPlayerController` interface) sont décrites dans les articles qui suivent.

Chacune des plateformes contient une classe nommée `VideoPlayerRenderer` qui contient le code spécifique à la plateforme pour implémenter un lecteur vidéo. La tâche principale de ce convertisseur consiste à créer un lecteur vidéo pour cette plateforme.

### <a name="the-ios-player-view-controller"></a>Le contrôleur d’affichage de lecteur iOS

Plusieurs classes sont impliqués lors de l’implémentation d’un lecteur vidéo dans iOS. L’application crée d’abord un [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) , puis définit la [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) propriété à un objet de type [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Des classes supplémentaires sont nécessaires lorsque le joueur est affecté à une source vidéo.

Comme tous les convertisseurs, iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) contient un `ExportRenderer` attribut qui identifie le `VideoPlayer` vue avec le moteur de rendu :

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

Généralement un convertisseur qui définit un contrôle de la plateforme dérive le [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) (classe), où `View` est la Xamarin.Forms `View` dérivé (dans ce cas, `VideoPlayer`) et `NativeView` est un iOS `UIView` dérivés de la classe de convertisseur. Pour ce convertisseur, cet argument générique est simplement défini sur `UIView`, pour des raisons de vous le verrez bientôt.

Si un convertisseur est basé sur un `UIViewController` dérivé (que celui-ci est), puis la classe doit remplacer le `ViewController` propriété et retournez le contrôleur d’affichage, dans ce cas `AVPlayerViewController`. Autrement dit l’objectif de la `_playerViewController` champ :

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

La responsabilité principale de la `OnElementChanged` remplacement consiste à vérifier si le `Control` propriété est `null` et, dans ce cas, créez un contrôle de la plateforme et transmettez-le à la `SetNativeControl` (méthode). Dans ce cas, cet objet est uniquement disponible à partir de la `View` propriété de la `AVPlayerViewController`. Que `UIView` dérivé qui se trouve être une classe privée nommée `AVPlayerView`, mais il est privé, il ne peut pas être explicitement spécifié comme deuxième argument générique pour `ViewRenderer`.

Généralement le `Control` propriété de la classe de convertisseur fait référence par la suite à la `UIView` utilisé pour implémenter le convertisseur, mais dans ce cas le `Control` propriété n’est pas utilisée ailleurs.

### <a name="the-android-video-view"></a>L’affichage vidéo Android

Le convertisseur Android pour `VideoPlayer` repose sur Android [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) classe. Toutefois, si `VideoView` est utilisé pour lire une vidéo dans une application Xamarin.Forms, la vidéo remplit la zone alloué pour le `VideoPlayer` sans avoir à conserver les proportions correcte. Pour cette raison (comme vous le verrez bientôt), le `VideoView` est défini comme enfant d’un Android `RelativeLayout`. Un `using` directive définit `ARelativeLayout` à distinguer les Xamarin.Forms `RelativeLayout`, et c’est le deuxième argument générique le `ViewRenderer`:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

À compter de Xamarin.Forms 2.5, les convertisseurs Android doivent inclure un constructeur avec un `Context` argument.

Le `OnElementChanged` remplacement crée à la fois le `VideoView` et `RelativeLayout` et définit les paramètres de disposition pour le `VideoView` pour centrer dans la `RelativeLayout`.


```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

Un gestionnaire pour le `Prepared` événements sont attaché dans cette méthode et détaché dans le `Dispose` (méthode). Cet événement est déclenché quand le `VideoView` dispose d’informations suffisantes pour commencer à lire un fichier vidéo.

### <a name="the-uwp-media-element"></a>L’élément de média UWP

Dans la plateforme de Windows universelle (UWP), le lecteur vidéo courant est [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Cette documentation de `MediaElement` indique que le [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) doit utiliser à la place quand il est uniquement nécessaire prendre en charge les versions de build 1607 à compter de Windows 10.

Le `OnElementChanged` remplacement doit créer un `MediaElement`, définir quelques gestionnaires d’événements et passer le `MediaElement` objet `SetNativeControl`:

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

Les deux gestionnaires d’événements sont détachés dans le `Dispose` événement pour le convertisseur.

## <a name="showing-the-transport-controls"></a>Affiche les contrôles de transport

Tous les lecteurs de vidéo inclus dans les plateformes prennent en charge un ensemble de contrôles de transport qui incluent des boutons pour la lecture et pause et une barre pour indiquer la position actuelle dans la vidéo et pour déplacer vers une nouvelle position par défaut.

Le `VideoPlayer` classe définit une propriété nommée `AreTransportControlsEnabled` et définit la valeur par défaut sur `true`:


```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

Bien que cette propriété dispose à la fois `set` et `get` accesseurs, le convertisseur a gérer les cas uniquement lorsque la propriété est définie. Le `get` accesseur retourne simplement la valeur actuelle de la propriété.

Propriétés, telles que `AreTransportControlsEnabled` sont gérées dans les convertisseurs de plateforme de deux manières :

- La première fois est quand Xamarin.Forms crée un `VideoPlayer` élément. Cela est indiqué dans le `OnElementChanged` remplacer du convertisseur lorsque le `NewElement` propriété n’est pas `null`. À ce stade, le convertisseur peut définir est le lecteur vidéo propre plate-forme à partir de la valeur initiale de la propriété, tel que défini dans le `VideoPlayer`.

- Si la propriété dans `VideoPlayer` change ultérieurement, puis le `OnElementPropertyChanged` méthode dans le convertisseur est appelé. Ainsi, le convertisseur mettre à jour le lecteur vidéo plateforme basé sur le nouveau paramètre de propriété.

Les sections suivantes décrivent comment la `AreTransportControlsEnabled` propriété est gérée sur chaque plateforme.

### <a name="ios-playback-controls"></a>contrôles de lecture iOS

La propriété du iOS `AVPlayerViewController` qui régit l’affichage du transport est contrôles [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). Voici comment cette propriété est définie sur le site iOS `VideoViewRenderer`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Le `Element` propriété du convertisseur se rapporte à la `VideoPlayer` classe.

### <a name="the-android-media-controller"></a>Le contrôleur multimédia Android

Dans Android, affichant les contrôles de transport requiert la création d’un [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) objet et son association avec le `VideoView` objet. Les mécanismes sont illustrées dans le `SetAreTransportControlsEnabled` méthode :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>La propriété des contrôles de Transport UWP

La plateforme Windows universelle `MediaElement` définit une propriété nommée [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled), de sorte que la propriété est définie à partir de la `VideoPlayer` propriété du même nom :

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
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Une seule propriété supplémentaire est nécessaire pour commencer la lecture d’une vidéo : c’est le très important `Source` propriété qui fait référence à un fichier vidéo. Implémentation de la `Source` propriété est décrite dans l’article suivant, [lecture d’une vidéo Web](web-videos.md).


## <a name="related-links"></a>Liens associés

- [Vidéos de démonstration Player (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
