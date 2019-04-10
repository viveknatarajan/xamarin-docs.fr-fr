---
title: 'Xamarin.Essentials: Partager'
description: La classe Share de Xamarin.Essentials permet à une application de partager des données comme du texte et des liens web avec d’autres applications sur l’appareil.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: 1a9a7b008773255d9d7743a4fcb21f02feb3e116
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58869375"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials: Partager

La classe **Share** permet à une application de partager des données comme du texte et des liens web avec d’autres applications sur l’appareil.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Utilisation de Share

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Partager appelle la méthode `RequestAsync` avec une charge utile de demande de données qui comprend les informations à partager avec d’autres applications. Le texte et l’URI peuvent être mélangés, et chaque plateforme gère le filtrage en fonction du contenu.

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

L’interface utilisateur de partage avec une application externe s’affiche quand la requête est effectuée :

![Partager](images/share.png)

## <a name="platform-differences"></a>Différences entre les plateformes

# [<a name="android"></a>Android](#tab/android)

* `Subject`  : propriété indiquant l’objet souhaité du message.

# [<a name="ios"></a>iOS](#tab/ios)

* `Subject` non utilisée
* `Title` non utilisée

# [<a name="uwp"></a>UWP](#tab/uwp)

* `Title` correspond par défaut au nom de l’application, si aucune valeur n’est définie.
* `Subject` non utilisée

-----

## <a name="files"></a>Fichiers

![Fonctionnalité d’évaluation](~/media/shared/preview.png)

Le partage de fichiers est disponible en tant que préversion expérimentale dans Xamarin.Essentials version 1.1.0. Cette fonctionnalité permet à une application de partager des fichiers avec d’autres applications sur l’appareil. Pour activer cette fonctionnalité, définissez la propriété suivante dans le code de démarrage de votre application :

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.ShareFileRequest);
```

Une fois la fonctionnalité activée, n’importe quel fichier peut être partagé. Xamarin.Essentials détectera automatiquement le type de fichier (MIME) et demandera un partage. Chaque plateforme peut prendre uniquement en charge certaines extensions de fichiers spécifiques.

Voici un exemple d’écriture de texte sur le disque et de partage avec d’autres applications :

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## <a name="api"></a>API

- [Code source de Share](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Documentation de l’API Share](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Vidéo associée

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
