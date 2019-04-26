---
title: Utilisation de ressources Android
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 1e9a71de7725c8382e133d85977407bcc859fc58
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013666"
---
# <a name="using-android-assets"></a>Utilisation de ressources Android

_Ressources_ offrent un moyen pour inclure des fichiers arbitraires comme texte, xml, polices, musique et vidéo dans votre application. Si vous essayez d’inclure ces fichiers en tant que « ressources », Android les traite à son système de ressources et vous ne serez pas en mesure d’obtenir les données brutes. Si vous souhaitez accéder aux données intactes, ressources constituent un moyen de le faire.

Ressources ajoutés à votre projet seront affichera comme un système de fichiers qui permettre lire à partir par votre application à l’aide [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
Dans cette démonstration simple, nous allons ajouter un élément multimédia du fichier texte à notre projet, lisez-le à l’aide `AssetManager`et les afficher dans un contrôle TextView.


## <a name="add-asset-to-project"></a>Ajoute un élément multimédia au projet

Cliquez le `Assets` dossier de votre projet. Ajouter un nouveau fichier texte dans ce dossier appelé `read_asset.txt`. Placer du texte qu’il contient, comme « Je suis tombé à partir d’un élément multimédia ! ».

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio doit avoir le **Action de génération** pour ce fichier pour **AndroidAsset**:

![Définition de l’action de génération sur AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Visual Studio pour Mac doit avoir le **Action de génération** pour ce fichier pour **AndroidAsset**:

[![Définition de l’action de génération sur AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

En sélectionnant le bon **BuildAction** garantit que le fichier est empaqueté dans l’APK au moment de la compilation.


## <a name="reading-assets"></a>Lecture des ressources

Actifs sont lus en utilisant un [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Une instance de la `AssetManager` est disponible en accédant à la [actifs](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) propriété sur un `Android.Content.Context`, telle qu’une activité.
Dans le code suivant, nous ouvrons notre **read_asset.txt** asset, lire le contenu et l’afficher à l’aide d’un contrôle TextView.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```


## <a name="running-the-application"></a>Exécution de l'application

Exécutez l’application et vous devez voir les éléments suivants :

![Capture d’écran de l’exemple](android-assets-images/screenshot.png)


## <a name="related-links"></a>Liens associés

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [Contexte](https://developer.xamarin.com/api/type/Android.Content.Context/)
