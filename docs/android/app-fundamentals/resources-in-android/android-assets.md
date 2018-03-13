---
title: "À l’aide des ressources Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: 83e58625438a0b50d89ca8dac3e940c8742e5aec
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="using-android-assets"></a>À l’aide des ressources Android

_Ressources_ fournissent une façon d’inclure des fichiers arbitraires, texte, xml, polices, musique et vidéo dans votre application. Si vous essayez d’inclure ces fichiers en tant que « ressources », Android les traitera dans son système de ressource et vous ne serez pas en mesure d’obtenir les données brutes. Si vous souhaitez accéder aux données intactes, actifs constituent un moyen de le faire.

Actifs ajoutés à votre projet apparaît comme un système de fichiers qui permettre lire par votre application à l’aide de [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
Dans cette démonstration simple, nous allons ajouter une ressource de fichier texte à notre projet, lecture à l’aide de `AssetManager`et les afficher dans un contrôle TextView.


## <a name="add-asset-to-project"></a>Ajoute un élément multimédia au projet

Cliquez la `Assets` dossier de votre projet. Ajouter un nouveau fichier texte dans ce dossier appelé `read_asset.txt`. Placer du texte qu’il contient comme « Je provient d’un élément multimédia ! ».

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio doit avoir le **Action de génération** pour ce fichier **AndroidAsset**:

![Définition de l’action de génération pour AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Visual Studio pour Mac doit avoir le **Action de génération** pour ce fichier **AndroidAsset**:

[![Définition de l’action de génération pour AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

En sélectionnant le bon **Buiidaction** garantit que le fichier est empaqueté dans APK au moment de la compilation.


## <a name="reading-assets"></a>La lecture des éléments multimédias

Actifs sont lus en utilisant un [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Une instance de la `AssetManager` est disponible via l’accès le [actifs](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) propriété sur un `Android.Contet.Context`, comme une activité.
Dans le code suivant, nous allons ouvrir notre **read_asset.txt** asset, lire le contenu et l’afficher à l’aide d’un contrôle TextView.

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

![Capture d’écran](android-assets-images/screenshot.png)


## <a name="related-links"></a>Liens associés

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
