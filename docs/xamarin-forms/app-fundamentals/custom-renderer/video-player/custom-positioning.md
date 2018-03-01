---
title: "Positionnement personnalisé de vidéo"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d4446d30491ee796ca93eadf2e107fc9d74748df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="custom-video-positioning"></a>Positionnement personnalisé de vidéo

Les contrôles de transport implémentées par chaque plate-forme incluent une barre de position. Cette barre ressemble à un curseur ou une barre de défilement et affiche l’emplacement actuel de la vidéo au sein de sa durée totale. En outre, l’utilisateur peut manipuler la barre de position pour faire avancer ou reculer vers une nouvelle position dans la vidéo.

Cet article explique comment vous pouvez implémenter votre propre barre emplacement personnalisé.

## <a name="the-duration-property"></a>La propriété Duration

Un élément d’information qui `VideoPlayer` doit prendre en charge une personnalisée barre de position est la durée de la vidéo. Le `VideoPlayer` définit en lecture seule `Duration` propriété de type `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Comme le `Status` propriété décrite dans le [article précédent](custom-transport.md), cette `Duration` propriété est en lecture seule. Elle est définie avec une privée `BindablePropertyKey` et ne peut être définie en référençant la `IVideoPlayerController` interface, qui inclut ce `Duration` propriété :

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

Notez également le Gestionnaire de modification de propriété qui appelle une méthode nommée `SetTimeToEnd` qui est décrite plus loin dans cet article.

La durée d’une vidéo est *pas* disponibles immédiatement après le `Source` propriété du `VideoPlayer` est définie. Le fichier vidéo doit être partiellement téléchargé avant que le lecteur vidéo sous-jacente peut déterminer sa durée.

Voici comment chacun des convertisseurs plateforme Obtient la durée de la vidéo :

### <a name="video-duration-in-ios"></a>Durée de la vidéo dans iOS

Dans iOS, la durée d’une vidéo est obtenue à partir de la `Duration` propriété du `AVPlayerItem`, mais pas immédiatement après le `AVPlayerItem` est créé. Il est possible de définir un observateur iOS pour le `Duration` propriété, mais la `VideoPlayerRenderer` Obtient la durée dans la `UpdateStatus` (méthode), qui est appelé 10 fois par seconde :

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

Le `ConvertTime` méthode convertit un `CMTime` de l’objet à un `TimeSpan` valeur.

### <a name="video-duration-in-android"></a>Durée de la vidéo dans Android

Le `Duration` propriété de la Android `VideoView` signale une durée valide en millisecondes lors de la `Prepared` l’événement de `VideoView` est déclenché. Le Android `VideoPlayerRenderer` classe utilise ce gestionnaire pour obtenir le `Duration` propriété :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>Durée de la vidéo dans la plateforme Windows universelle

Le `NaturalDuration` propriété du `MediaElement` est un `TimeSpan` valeur et est valide lorsque `MediaElement` se déclenche le `MediaOpened` événement :

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>La propriété Position

`VideoPlayer` a également besoin d’un `Position` propriété augmente entre zéro et `Duration` comme la lecture. `VideoPlayer` implémente cette propriété, comme le `Position` propriété UWP `MediaElement`, qui est une propriété pouvant être liée normale avec public `set` et `get` accesseurs :

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

Le `get` accesseur retourne la position actuelle de la vidéo, comme il est actif, mais la `set` accesseur est destiné à répondre à la manipulation de l’utilisateur de la barre de position en déplaçant la position vidéo avancer ou reculer.

Dans iOS et Android, la propriété qui obtient la position actuelle n’a qu’un `get` accesseur et un `Seek` méthode n’est disponible pour effectuer cette tâche deuxième. Si vous pensez à son sujet, distinct `Seek` méthode semble être une approche plus pratique qu’un seul `Position` propriété. Un seul `Position` propriété a un problème : lors de la lecture vidéo, le `Position` propriété doive être continuellement mis à jour pour refléter la nouvelle position. Mais vous ne souhaitez pas la plupart des modifications apportées à la `Position` propriété pour que le lecteur vidéo à déplacer vers une nouvelle position dans la vidéo. Si cela se produit, le lecteur vidéo répondrait par la recherche à la dernière valeur de la `Position` propriété et la vidéo semblent ne pas avancer.

Malgré les difficultés de mise en œuvre un `Position` propriété avec `set` et `get` accesseurs, cette approche a été choisie, car il est cohérent avec la plateforme Windows universelle `MediaElement`, et qu’il a un grand avantage avec la liaison de données : le `Position` propriété de la `VideoPlayer` peut être liée au curseur qui est utilisé pour afficher la position et effectuer une recherche vers un nouvel emplacement. Toutefois, plusieurs précautions sont nécessaires lors de l’implémentation de cette `Position` propriété afin d’éviter les boucles de commentaires.

### <a name="setting-and-getting-ios-position"></a>Définir et obtenir la position d’e/s

Dans iOS, le `CurrentTime` propriété de la `AVPlayerItem` objet indique la position actuelle de la lecture de la vidéo. IOS `VideoPlayerRenderer` définit le `Position` propriété dans le `UpdateStatus` gestionnaire en même temps qu’il définit le `Duration` propriété :

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Le convertisseur détecte lorsque le `Position` jeu de propriétés de `VideoPlayer` a été modifié dans le `OnElementPropertyChanged` remplacer et utilise cette nouvelle valeur pour appeler un `Seek` méthode sur le `AVPlayer` objet :

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

N’oubliez pas que chaque fois le `Position` propriété dans `VideoPlayer` est définie à partir de la `OnUpdateStatus` gestionnaire, la `Position` se déclenche la propriété un `PropertyChanged` événement, qui est détecté dans le `OnElementPropertyChanged` remplacer. Pour la plupart de ces modifications, la `OnElementPropertyChanged` méthode ne doit rien faire. Dans le cas contraire, avec chaque modification de la position de la vidéo, elle est déplacée à la même position qu'il atteint !

Pour éviter cette boucle d’évaluation, le `OnElementPropertyChanged` uniquement d’appels de méthode `Seek` lors de la différence entre la `Position` propriété et la position actuelle de la `AVPlayer` est supérieure à une seconde.

### <a name="setting-and-getting-android-position"></a>Définition et l’obtention de position Android

Comme dans le convertisseur iOS, le Android `VideoPlayerRenderer` définit une nouvelle valeur pour le `Position` propriété dans le `OnUpdateStatus` gestionnaire. Le `CurrentPosition` propriété du `VideoView` contient la nouvelle position dans les unités de millisecondes :

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Comme dans le convertisseur iOS, le convertisseur Android appelle également la `SeekTo` méthode de `VideoView` lors de la `Position` propriété a changé, mais uniquement lorsque la modification est différent de plus d’une seconde la `CurrentPosition` valeur de `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Définir et obtenir la position de la plateforme Windows universelle

UWP `VideoPlayerRenderer` gère la `Position` de la même façon qu’iOS et Android convertisseurs, mais étant donné que la `Position` propriété de la plateforme Windows universelle `MediaElement` est également un `TimeSpan` valeur, aucune conversion n’est nécessaire :

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>Calcul d’une propriété TimeToEnd

Lecteurs vidéo parfois affichent la durée restante dans la vidéo. Cette valeur commence à durée de la vidéo lorsque la vidéo commence et diminue à zéro lorsque la vidéo se termine.

`VideoPlayer` inclut en lecture seule `TimeToEnd` propriété entièrement au sein de la classe en fonction des modifications apportées à la `Duration` et `Position` propriétés :

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

Le `SetTimeToEnd` méthode est appelée à partir des gestionnaires de modification de propriété des deux `Duration` et `Position`.

## <a name="a-custom-slider-for-video"></a>Un curseur personnalisé pour la vidéo

Il est possible d’écrire un contrôle personnalisé pour une barre de position, ou utiliser le Xamarin.Forms `Slider` ou une classe qui dérive de `Slider`, telle que la suivante `PositionSlider` classe. La classe définit deux nouvelles propriétés nommées `Duration` et `Position` de type `TimeSpan` qui sont destinés à être liés aux données pour les deux propriétés portant le même nom dans `VideoPlayer`. Notez que la valeur par défaut en mode de la liaison la `Position` propriété est bidirectionnelle :

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Le Gestionnaire de modification de propriété pour le `Duration` jeux de propriétés la `Maximum` propriété de l’objet sous-jacent `Slider` à la `TotalSeconds` propriété de la `TimeSpan` valeur. De même, le gestionnaire modification de propriété pour `Position` définit le `Value` propriété de la `Slider`. De cette façon, sous-jacent `Slider` effectue le suivi de la position de la `PositionSlider`.

Le `PositionSlider` est mis à jour sous-jacent `Slider` dans une seule instance : lorsque l’utilisateur manipule le `Slider` pour indiquer que la vidéo doit être avancée ou inversée vers une nouvelle position. Cette exception est détectée dans le `PropertyChanged` gestionnaire dans le constructeur de la `PositionSlider`. Le Gestionnaire de recherche un changement dans le `Value` propriété, et si elle est différente de la `Position` propriété, le `Position` propriété est définie à partir de la `Value` propriété.

En théorie, interne `if` instruction peut être écrite comme suit :

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Toutefois, l’implémentation Android de `Slider` a uniquement 1 000 étapes distinctes, quel que soit le `Minimum` et `Maximum` paramètres. La longueur d’une vidéo est supérieure à 1 000 secondes, deux `Position` valeurs correspond à la même `Value` définition de la `Slider`et cela `if` instruction déclenche un faux positif pour la manipulation d’un utilisateur de le `Slider`. Il est plus sûr de contrôler que les positions nouvelles et existantes sont supérieurs à centièmes de la durée globale.

## <a name="using-the-positionslider"></a>À l’aide de la PositionSlider

Documentation de la plateforme Windows universelle [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) avertit sur la liaison à la `Position` propriété, car la propriété fréquemment mises à jour. La documentation recommande d’utiliser un minuteur pour interroger le `Position` propriété.

Qui est un bon, mais les trois `VideoPlayerRenderer` classes utilisent déjà indirectement un minuteur pour mettre à jour le `Position` propriété. Le `Position` propriété est modifiée dans un gestionnaire pour le `UpdateStatus` événement, qui est déclenché uniquement 10 fois par seconde.

Par conséquent, le `Position` propriété de la `VideoPlayer` peuvent être liés à la `Position` propriété de la `PositionSlider` sans les problèmes de performances, comme illustré dans le **personnalisé Position barre** page :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

Le premier bouton de sélection (···) masque la `ActivityIndicator`; il est identique à celle du précédent **Transport personnalisé** page. Notez que les deux `Label` éléments affichant le `Position` et `TimeToEnd` propriétés. Les points de suspension entre ces deux `Label` éléments masque les deux `Button` éléments affichés dans le **Transport personnalisé** page de la lecture, Pause et l’arrêter. La logique de code-behind est également le même que le **Transport personnalisé** page.

[![Positionnement personnalisé](custom-positioning-images/custompositioning-small.png "positionnement personnalisé")](custom-positioning-images/custompositioning-large.png "positionnement personnalisé")

Ceci conclut la description de le `VideoPlayer`.

## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
