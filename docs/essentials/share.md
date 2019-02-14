---
title: 'Xamarin.Essentials: Partager'
description: La classe Share de Xamarin.Essentials permet à une application de partager des données comme du texte et des liens web avec d’autres applications sur l’appareil.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 02/12/2019
ms.custom: video
ms.openlocfilehash: 7e61041fa33557c4e1db3613b75b575e9d456231
ms.sourcegitcommit: c6ff24b524d025d7e87b7b9c25f04c740dd93497
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56240420"
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

![Partager](share-images/share.png)

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

* La propriété `Subject` permet d’indiquer l’objet souhaité du message.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` ne sera pas utilisé.
* `Title` ne sera pas utilisé.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `Title` correspond par défaut au nom de l’application, si aucune valeur n’est définie.
* `Subject` ne sera pas utilisé.

-----

## <a name="api"></a>API

- [Code source de Share](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Documentation de l’API Share](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Vidéo associée

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
