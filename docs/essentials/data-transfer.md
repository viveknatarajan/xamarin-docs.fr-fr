---
title: 'Xamarin.Essentials : Transfert de données'
description: La classe DataTransfer permet à votre application de partager des données, telles que des URL et du texte, avec d’autres applications (préalablement installées sur l'appareil).
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 31e27556a6681b144084d2177cf3fde8fe8e5459
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353517"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials : Transfert de données

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **DataTransfer** permet à votre application de partager des données, telles que des URL et du texte, avec d’autres applications (préalablement installées sur l'appareil).

## <a name="using-data-transfer"></a>Utilisation de **DataTransfer**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité transfert de données fonctionne en appelant le `RequestAsync` méthode avec une charge de requête de données qui inclut des informations à partager avec d’autres applications. Texte et les Uri peuvent être mélangés, et chaque plateforme gérera un filtrage basé sur le contenu.

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

Interface utilisateur de partage qui s'affichera alors ressemblera à ceci :

![Transfert de données](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

* La propriété `Subject` sera utilisée pour l’objet d’un message.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` ne sera pas utilisé.
* `Title` ne sera pas utilisé.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Si `Title` n'est pas défini, il prendra pour valeur par défaut le nom de l'application.
* `Subject` ne sera pas utilisé.

-----

## <a name="api"></a>API

- [Code de source de transfert de données](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Documentation de l’API de transfert de données](xref:Xamarin.Essentials.DataTransfer)
