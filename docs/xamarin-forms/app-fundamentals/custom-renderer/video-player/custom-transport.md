---
title: "Contrôles de transport vidéo personnalisée"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d37c2c9dbb12668680b5813e9e083d2ebb184c66
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="custom-video-transport-controls"></a>Contrôles de transport vidéo personnalisée

Les contrôles de transport d’un lecteur vidéo sont les boutons qui effectuent les fonctions **lire**, **Pause**, et **arrêter**. Ces boutons sont généralement identifiés par les icônes familiers plutôt que du texte et le **lire** et **Pause** fonctions sont généralement combinées en un seul bouton.

Par défaut, la `VideoPlayer` affiche les contrôles pris en charge par chaque plate-forme de transport. Lorsque vous définissez la `AreTransportControlsEnabled` propriété `false`, ces contrôles sont supprimés. Vous pouvez ensuite contrôler le `VideoPlayer` par programmation ou fournir vos propres contrôles de transport.

## <a name="the-play-pause-and-stop-methods"></a>Les méthodes de lecture, Pause et arrêt

Le `VideoPlayer` classe définit trois méthodes nommées `Play`, `Pause`, et `Stop` qui sont implémentées par le déclenchement d’événements :

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Gestionnaires d’événements pour ces événements sont définis par le `VideoPlayerRenderer` classe dans chaque plate-forme, comme indiqué ci-dessous :

### <a name="ios-transport-implementations"></a>les implémentations de transport iOS

La version iOS de `VideoPlayerRenderer` utilise le `OnElementChanged` méthode pour définir des gestionnaires pour ces trois événements lorsque le `NewElement` propriété n’est pas `null` et détache les gestionnaires d’événements lorsque `OldElement` n’est pas `null`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Les gestionnaires d’événements sont implémentées en appelant des méthodes sur le `AVPlayer` objet. Il est sans `Stop` méthode pour `AVPlayer`, donc il est simulé en suspendant la vidéo et de placer la position de début.

### <a name="android-transport-implementations"></a>Implémentations de transport Android

L’implémentation Android est similaire à l’implémentation d’iOS. Les gestionnaires pour les trois fonctions sont définies lorsque `NewElement` n’est pas `null` et détachée lorsque `OldElement` n’est pas `null`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Les trois fonctions appellent des méthodes définies par `VideoView`.

### <a name="uwp-transport-implementations"></a>Implémentations de transport UWP

L’implémentation UWP des fonctions de trois transport est très similaire aux implémentations Android et iOS :

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>L’état de lecteur vidéo

Implémentation de la **lire**, **Pause**, et **arrêter** fonctions n’est pas suffisante pour prendre en charge les contrôles de transport. Fréquence à laquelle le **lire** et **Pause** commandes sont implémentées avec le même bouton qui change d’apparence pour indiquer si la vidéo est actuellement en cours de lecture ou en pause. En outre, le bouton même ne doit pas être activé que si la vidéo n’a pas encore chargé.

Ces conditions requises impliquent que le lecteur vidéo doit rendre disponible un état indiquant s’il est actif ou en pause, ou s’il n’est pas encore prêt pour lire une vidéo. (Les trois plateformes prennent également en charge les propriétés qui indiquent si la vidéo peut être suspendue ou peut être déplacée vers un nouvel emplacement, mais ces propriétés sont applicables pour la diffusion en continu de vidéo plutôt que des fichiers vidéo, afin qu’ils ne sont pas pris en charge dans les `VideoPlayer` décrites ici.)

Le **VideoPlayerDemos** projet comprend un `VideoStatus` énumération avec trois membres :

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

Le `VideoPlayer` classe définit une propriété pouvant être liée uniquement réel nommée `Status` de type `VideoStatus`. Cette propriété est définie comme étant en lecture seule, car il doit être défini uniquement dans le convertisseur de plateforme :

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

En règle générale, une propriété en lecture seule aurait privé `set` accesseur sur le `Status` pour lui permettre d’être définies à partir de la classe de propriété. Pour un `View` dérivé pris en charge par les convertisseurs, toutefois, la propriété doit être définie à partir d’à l’extérieur de la classe, mais uniquement par le convertisseur de plateforme.

Pour cette raison, une autre propriété est définie avec le nom `IVideoPlayerController.Status`. Il s’agit d’une implémentation d’interface explicite qui est rendue possible par le `IVideoPlayerController` interface fournie par le `VideoPlayer` la classe implémente :

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Ceci est similaire à la façon dont le [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) de contrôles utilise la [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/) interface à implémenter la `CanGoBack` et `CanGoForward` propriétés. (Consultez le code source de [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) et ses convertisseurs pour plus d’informations.)

Il est ainsi possible pour une classe externe à `VideoPlayer` pour définir le `Status` propriété en référençant le `IVideoPlayerController` interface. (Vous verrez le code dans quelques instants.) La propriété peut être définie à partir d’autres classes, mais il est peu susceptible d’être définie par inadvertance. Plus important encore, le `Status` propriété ne peut pas être définie via une liaison de données.

Pour faciliter les convertisseurs de conserver cette `Status` propriété mise à jour, le `VideoPlayer` classe définit un `UpdateStatus` événement est déclenché à chaque dixième de seconde :

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>Le paramètre de l’état iOS

IOS `VideoPlayerRenderer` définit un gestionnaire pour le `UpdateStatus` événement (et détache ce gestionnaire lorsque sous-jacent `VideoPlayer` élément est absent) et utilise le gestionnaire pour définir le `Status` propriété :

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
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

Deux propriétés de `AVPlayer` doit être accessible : le [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) propriété de type `AVPlayerStatus` et [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) propriété de type `AVPlayerTimeControlStatus`. Notez que la `Element` propriété (qui est la `VideoPlayer`) doivent être converties en `IVideoPlayerController` pour définir le `Status` propriété.

### <a name="the-android-status-setting"></a>Le paramètre état Android

Le [ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) propriété de la Android `VideoView` est une valeur booléenne qui indique uniquement si la vidéo est en cours de lecture ou en pause. Pour déterminer si le `VideoView` ne peut ni lire ni interrompre la vidéo encore la `Prepared` l’événement de `VideoView` doit être gérée. Ces deux gestionnaires sont définis dans le `OnElementChanged` (méthode) et détachées pendant la `Dispose` remplacer :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

Le `UpdateStatus` Gestionnaire utilise la `isPrepared` champ (définie dans le `Prepared` gestionnaire) et le `IsPlaying` propriété à définir le `Status` propriété :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>Le paramètre de l’état UWP

UWP `VideoPlayerRenderer` utilise le `UpdateStatus` événements, mais il n’en avez pas besoin de paramètre le `Status` propriété. Le `MediaElement` définit un [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) les événements qui permet le convertisseur à être notifié lorsque le [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) propriété a changé. La propriété est détachée dans le `Dispose` remplacer :

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

Le `CurrentState` propriété est de type [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)et mappe facilement `VideoStatus`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Lecture, Pause, des boutons et arrêter

À l’aide de caractères Unicode pour symbolique **lire**, **Pause**, et **arrêter** images peut être problématique. Le [diverses techniques](https://unicode-table.com/en/blocks/miscellaneous-technical/) section de la norme Unicode définit trois caractères de symbole apparemment appropriées à cet effet. Ces équivalents sont :

- 0x23F5 (moyenne pointant vers la droite triangle noir) ou & #x23F5 ; pour **lire**
- 0x23F8 (barre verticale double) ou & #x23F8 ; pour **Pause**
- 0x23F9 (carré noir) ou & #x23F9 ; pour **arrêter**

Peu importe comment ces symboles apparaissent dans votre navigateur (et différents navigateurs les gérer de différentes façons), ils ne figurent pas toujours sur les plateformes prises en charge par Xamarin.Forms. Sur les appareils iOS et UWP, le **Pause** et **arrêter** caractères ont une apparence de graphique, avec un arrière-plan 3D bleu et un premier plan blanc. Cela n’est pas le cas sur Android, où le symbole est simplement bleu. Toutefois, le code 0x23F5 **lire** n’a pas que même apparence sur la plateforme Windows universelle et il n’est pas encore pris en charge sur iOS et Android.

Pour cette raison, le point de code 0x23F5 ne peut pas être utilisé pour **lire**. Un substitut correct est :

- 0x25B6 (triangle noir pointant vers la droite) ou & #x25B6 ; pour **lire**

Cela est pris en charge par les trois plateformes, sauf qu’il s’agit d’un triangle noir simple qui ne ressemble pas à l’apparence 3D de **Pause** et **arrêter**. Il se peut suivre le point de code 0x25B6 avec un code variante :

- 0x25B6 suivie 0xFE0F (variant 16) ou & #x25B6 ; & #xFE0F ; pour **lire**

Voici ce qui est utilisé dans le balisage indiqué ci-dessous. Sur iOS, il donne le **lire** de symboles de la même apparence 3D en tant que le **Pause** et **arrêter** boutons, mais la variante ne fonctionne pas sur Android et la plateforme Windows universelle.

Le **Transport personnalisé** page définit le **AreTransportControlsEnabled** propriété **false** et inclut un `ActivityIndicator` affichées lors du chargement de la vidéo et deux boutons. `DataTrigger` objets sont utilisés pour activer et désactiver la `ActivityIndicator` et des boutons et pour changer le premier bouton entre **lire** et **Pause**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Les déclencheurs de données sont décrites en détail dans l’article [déclencheurs Data](~/xamarin-forms/app-fundamentals/triggers.md#data).

Le fichier code-behind a les gestionnaires pour le bouton `Clicked` événements :

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Étant donné que `AutoPlay` a la valeur `false` dans les **CustomTransport.xaml** fichier, vous devrez appuyer sur le **lire** bouton lorsque celui-ci est activé pour commencer la vidéo. Les boutons sont définies afin que les caractères Unicode abordées ci-dessus sont accompagnés de leurs équivalents de texte. Les boutons ont une apparence cohérente sur chaque plateforme lors de la lecture :

[![Lecture du Transport personnalisé](custom-transport-images/customtransportplaying-small.png "Transport personnalisé lecture")](custom-transport-images/customtransportplaying-large.png "lecture du Transport personnalisé")

Mais sur Android et la plateforme Windows universelle, le **lire** bouton ressemble très différente lorsque la vidéo est suspendue :

[![Transport personnalisé en pause](custom-transport-images/customtransportpaused-small.png "Transport personnalisé en pause")](custom-transport-images/customtransportpaused-large.png "Transport personnalisé en pause")

Dans une application de production, vous souhaiterez probablement utiliser vos propres images bitmap pour les boutons pour atteindre l’uniformité de visual.


## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
