---
title: Chargement de vidéos de ressources d’application
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 89c424ee80a4ebf6363a836e752b72ee9bc5cd5a
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="loading-application-resource-videos"></a>Chargement de vidéos de ressources d’application

Les convertisseurs personnalisés pour le `VideoPlayer` compatibles avec le mode de lecture des fichiers vidéo qui ont été incorporés dans les projets de plateforme individuels en tant que ressources de l’application. Toutefois, la version actuelle de `VideoPlayer` ne peut pas accéder à des ressources incorporées dans une bibliothèque .NET Standard.

Pour charger ces ressources, créez une instance de `ResourceVideoSource` en définissant le `Path` propriété le nom de fichier (ou le dossier et le nom) de la ressource. Vous pouvez également appeler la méthode statique `VideoSource.FromResource` méthode fassent référence à la ressource. Ensuite, définissez la `ResourceVideoSource` de l’objet à la `Source` propriété du `VideoPlayer`. 

## <a name="storing-the-video-files"></a>Stocker les fichiers vidéos

Stockage d’un fichier vidéo dans le projet de plateforme est différent pour les trois plateformes :

### <a name="ios-video-resources"></a>ressources vidéo iOS

Dans un projet iOS, vous pouvez stocker une vidéo dans le **ressources** dossier ou un sous-dossier de la **ressources** dossier. Le fichier vidéo doit avoir un `Build Action` de `BundleResource`. Définir le `Path` propriété du `ResourceVideoSource` au nom de fichier, par exemple, **MyFile.mp4** d’un fichier dans le **ressources** dossier, ou **MyFolder/MyFile.mp4**, où **MonDossier** est un sous-dossier du **ressources**.

Dans le **VideoPlayerDemos** solution, le **VideoPlayerDemos.iOS** projet contient un sous-dossier de **ressources** nommé **vidéos** qui contient un fichier nommé **iOSApiVideo.mp4**. Il s’agit d’une courte vidéo qui montre comment utiliser le site web de Xamarin pour trouver la documentation pour les e/s `AVPlayerViewController` classe.

### <a name="android-video-resources"></a>Ressources vidéo Android

Dans un projet Android, des vidéos doivent être stockées dans un sous-dossier du **ressources** nommé **brutes**. Le **brutes** dossier ne peut pas contenir de sous-dossiers. Donnez au fichier vidéo un `Build Action` de `AndroidResource`. Définir le `Path` propriété du `ResourceVideoSource` au nom de fichier, par exemple, **MyFile.mp4**. 

Le **VideoPlayerDemos.Android** projet contient un sous-dossier de **ressources** nommé **brutes**, qui contient un fichier nommé **AndroidApiVideo.mp4**. 

### <a name="uwp-video-resources"></a>Ressources vidéo UWP

Dans un projet de plateforme Windows universelle, vous pouvez stocker des vidéos dans n’importe quel dossier dans le projet. Donnez au fichier un `Build Action` de `Content`. Définir le `Path` propriété du `ResourceVideoSource` dans le dossier et le nom de fichier, par exemple, **MyFolder/MyVideo.mp4**. 

Le **VideoPlayerDemos.UWP** projet contient un dossier nommé **vidéos** avec le fichier **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Le chargement des fichiers vidéos

Chacune des classes de convertisseur de plateforme contient le code dans son `SetSource` méthode de chargement des fichiers vidéos stockés en tant que ressources.

### <a name="ios-resource-loading"></a>chargement de ressources d’e/s

La version iOS de `VideoPlayerRenderer` utilise le `GetUrlForResource` méthode `NSBundle` pour le chargement de la ressource. Le chemin d’accès complet doit être divisé en un nom de fichier, une extension et un répertoire. Le code utilise le `Path` classe dans le .NET `System.IO` espace de noms pour diviser le chemin d’accès à ces composants :

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

Le Android `VideoPlayerRenderer` utilise le nom de fichier et nom de package pour construire un `Uri` objet. Le nom du package est le nom de l’application, dans ce cas **VideoPlayerDemos.Android**, qui peut être obtenu à partir de la méthode statique `Context.PackageName` propriété. Résultant `Uri` objet est ensuite transmis à la `SetVideoURI` méthode `VideoView`:

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

### <a name="uwp-resource-loading"></a>Chargement des ressources UWP

UWP `VideoPlayerRenderer` construit un `Uri` objet pour le chemin d’accès et lui affecte le `Source` propriété du `MediaElement`:

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

Si la ressource d’e/s est stockée dans le **ressources** dossier, et si la ressource de la plateforme Windows universelle est stockée dans le dossier racine du projet, vous pouvez utiliser le même nom de fichier pour les trois plateformes. Si tel est le cas, vous pouvez définir ce nom directement à la `Source` propriété du `VideoPlayer`. 

Voici cette page en cours d’exécution sur les plateformes de trois :

[![Lire les ressources vidéo](loading-resources-images/playvideoresource-small.png "lire la ressource vidéo")](loading-resources-images/playvideoresource-large.png#lightbox "lire la ressource vidéo")

Vous savez maintenant comment [charger des vidéos à partir d’un URI Web](web-videos.md) et comment lire les ressources incorporées. En outre, vous pouvez [charger des vidéos à partir de la vidéothèque du périphérique](accessing-library.md).


## <a name="related-links"></a>Liens associés

- [Démonstrations de lecteur vidéo (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
