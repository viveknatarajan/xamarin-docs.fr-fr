---
title: 'Xamarin.Essentials : transfert de données'
description: La classe DataTransfer de Xamarin.Essentials permet à une application de partager des données telles que du texte et des liens web avec d’autres applications de l’appareil.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 179d4327aa768e7aa2c81dbbffd694d078327400
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674832"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials : transfert de données

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **DataTransfer** permet à une application de partager des données telles que du texte et des liens web avec d’autres applications de l’appareil.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-data-transfer"></a>Utilisation du transfert de données

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de transfert de données appelle la méthode `RequestAsync` avec une charge utile de requête de données qui comprend les informations à partager avec d’autres applications. Le texte et l’URI peuvent être mélangés, et chaque plateforme gère le filtrage en fonction du contenu.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

L’interface utilisateur de partage avec une application externe s’affiche quand la requête est effectuée :

![Transfert de données](data-transfer-images/data-transfer.png)

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

- [Code source de la fonctionnalité de transfert de données](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Documentation sur l’API de transfert de données](xref:Xamarin.Essentials.DataTransfer)
