---
title: Chargement de vidéos de ressource d’application
description: Cet article explique comment charger des vidéos stockées en tant que ressources d’application dans une application de lecteur vidéo, à l’aide de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 17e9e7061e4329431a0f34abdbbb616a1aff1b43
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171324"
---
# <a name="loading-application-resource-videos"></a>Chargement de vidéos de ressource d’application

Les convertisseurs personnalisés pour le `VideoPlayer` vue sont capables de lire des fichiers vidéo qui ont été incorporés dans les projets de plateforme individuels en tant que ressources de l’application. Toutefois, la version actuelle de `VideoPlayer` ne peut pas accéder aux ressources incorporées dans une bibliothèque .NET Standard.

Pour charger ces ressources, créez une instance de `ResourceVideoSource` en définissant le `Path` propriété sur le nom de fichier (ou le dossier et nom de fichier) de la ressource. Vous pouvez également appeler la méthode statique `VideoSource.FromResource` méthode à référencer la ressource. Ensuite, définissez le `ResourceVideoSource` de l’objet à la `Source` propriété du `VideoPlayer`.

## <a name="storing-the-video-files"></a>Stocker les fichiers vidéo

Stockage d’un fichier vidéo dans le projet de plateforme est différente pour chaque plateforme.

### <a name="ios-video-resources"></a>ressources vidéo iOS

Dans un projet iOS, vous pouvez stocker une vidéo dans le **ressources** dossier ou un sous-dossier de la **ressources** dossier. Le fichier vidéo doit avoir un `Build Action` de `BundleResource`. Définir le `Path` propriété du `ResourceVideoSource` au nom de fichier, par exemple, **MyFile.mp4** pour un fichier dans le **ressources** dossier, ou **MyFolder/MyFile.mp4**, où **MyFolder** est un sous-dossier du **ressources**.

Dans le **VideoPlayerDemos** solution, le **VideoPlayerDemos.iOS** projet contient un sous-dossier de **ressources** nommé **vidéos** qui contient un fichier nommé **iOSApiVideo.mp4**. Il s’agit d’une courte vidéo qui vous montre comment utiliser le site web de Xamarin pour trouver la documentation pour l’iOS `AVPlayerViewController` classe.

### <a name="android-video-resources"></a>Ressources vidéo Android

Dans un projet Android, les vidéos doivent être stockées dans un sous-dossier de **ressources** nommé **brutes**. Le **brutes** dossier ne peut pas contenir de sous-dossiers. Attribuez au fichier vidéo une `Build Action` de `AndroidResource`. Définir le `Path` propriété du `ResourceVideoSource` au nom de fichier, par exemple, **MyFile.mp4**.

Le **VideoPlayerDemos.Android** projet contient un sous-dossier de **ressources** nommé **brutes**, qui contient un fichier nommé **AndroidApiVideo.mp4**.

### <a name="uwp-video-resources"></a>Ressources vidéo UWP

Dans un projet de plateforme Windows universelle, vous pouvez stocker des vidéos dans n’importe quel dossier dans le projet. Attribuez au fichier un `Build Action` de `Content`. Définir le `Path` propriété du `ResourceVideoSource` dans le dossier et le nom de fichier, par exemple, **MyFolder/MyVideo.mp4**.

Le **VideoPlayerDemos.UWP** projet contient un dossier nommé **vidéos** avec le fichier **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Le chargement des fichiers vidéo

Chacune des classes de convertisseur de plateforme contient le code dans son `SetSource` méthode de chargement des fichiers vidéo stockés en tant que ressources.

### <a name="ios-resource-loading"></a>chargement de ressources iOS

La version iOS de `VideoPlayerRenderer` utilise le `GetUrlForResource` méthode de `NSBundle` pour le chargement de la ressource. Le chemin d’accès complet doit être divisé en un nom de fichier, une extension et un répertoire. Le code utilise le `Path` classe dans le .NET `System.IO` espace de noms pour diviser le chemin d’accès de fichier en ces composants :

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
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Chargement des ressources Android

Android `VideoPlayerRenderer` utilise le nom de fichier et le nom package pour construire un `Uri` objet. Le nom du package est le nom de l’application, dans ce cas **VideoPlayerDemos.Android**, qui peut être obtenu à partir de la méthode statique `Context.PackageName` propriété. La résultante `Uri` objet est ensuite passé à la `SetVideoURI` méthode de `VideoView`:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>Chargement de ressources UWP

La plateforme Windows universelle `VideoPlayerRenderer` construit un `Uri` objet pour le chemin d’accès et lui affecte le `Source` propriété de `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>La lecture du fichier de ressources

Le **lire la ressource vidéo** page dans le **VideoPlayerDemos** solution utilise le `OnPlatform` classe pour spécifier le fichier vidéo pour chaque plateforme :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

Si la ressource iOS est stockée dans le **ressources** dossier, et si la ressource UWP est stockée dans le dossier racine du projet, vous pouvez utiliser le même nom de fichier pour chaque plateforme. Si tel est le cas, vous pouvez définir ce nom directement à la `Source` propriété du `VideoPlayer`.

Voici la page en cours d’exécution :

[![Lire la ressource vidéo](loading-resources-images/playvideoresource-small.png "lire la ressource vidéo")](loading-resources-images/playvideoresource-large.png#lightbox "lire les ressources vidéo")

Vous avez maintenant vu comment [charger des vidéos à partir d’un URI Web](web-videos.md) et comment lire les ressources incorporées. En outre, vous pouvez [charger des vidéos à partir de la bibliothèque vidéo de l’appareil](accessing-library.md).


## <a name="related-links"></a>Liens associés

- [Vidéos de démonstration Player (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
