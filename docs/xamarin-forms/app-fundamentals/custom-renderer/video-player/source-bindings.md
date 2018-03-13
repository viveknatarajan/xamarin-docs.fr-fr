---
title: "Liaison de sources vidéo du lecteur"
ms.topic: article
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 209b301c44da9bbb52ad8bf7fe867811a9b7617f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="binding-video-sources-to-the-player"></a>Liaison de sources vidéo du lecteur

Lorsque le `Source` propriété de la `VideoPlayer` affichage est défini sur un nouveau fichier vidéo, la vidéo existante s’arrête et la nouvelle vidéo commence. Cela est illustré par le **sélectionner la vidéo Web** page de la [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) exemple. Cette page inclut un `ListView` avec les titres des trois vidéos référencés à partir de la **App.xaml** fichier :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Lorsqu’une vidéo est sélectionnée, le `ItemSelected` Gestionnaire d’événements dans le fichier code-behind est exécutée. Le gestionnaire supprime toutes les valeurs vides et des apostrophes le titre et l’utilise en tant que clé pour obtenir d’une des ressources définies dans le **App.xaml** fichier. Que `UriVideoSource` objet est ensuite affecté à la `Source` propriété de la `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

Lorsque le chargement de la page, aucun élément n’est sélectionné dans le `ListView`, donc vous devez sélectionner l’une pour la vidéo commencer la lecture :

[![Sélectionnez la vidéo Web](source-bindings-images/selectwebvideo-small.png "sélectionnez vidéo Web")](source-bindings-images/selectwebvideo-large.png#lightbox "sélectionnez vidéo sur le Web")

Le `Source` propriété du `VideoPlayer` est soutenu par une propriété pouvant être liée, ce qui signifie qu’il peut être la cible d’une liaison de données. Cela est illustré par le **lier à VideoPlayer** page. Le balisage dans le **BindToVideoPlayer.xaml** fichier est pris en charge par la classe suivante qui encapsule un titre d’une vidéo et correspondante `VideoSource` objet :

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

Le `ListView` dans les **BindToVideoPlayer.xaml** fichier contient un tableau de ces `VideoInfo` objets, chacun d’eux initialisée avec un titre de la vidéo et la `UriVideoSource` objet du dictionnaire de ressources dans  **App.XAML**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Le `Source` propriété de la `VideoPlayer` est lié à la `ListView`. Le `Path` de la liaison est spécifiée en tant que `SelectedItem.VideoSource`, qui est un tracé composite constituée de deux propriétés : `SelectedItem` est une propriété de `ListView`. L’élément sélectionné est de type `VideoInfo`, qui a un `VideoSource` propriété.

Comme avec la première **sélectionner la vidéo Web** page, aucun élément n’est sélectionné initialement à partir de la `ListView`, de sorte que vous devez sélectionner une des vidéos avant le début de la lecture.


## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
