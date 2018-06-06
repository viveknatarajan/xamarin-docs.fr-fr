---
title: 'Xamarin.Essentials : Transfert de données'
description: La classe DataTransfer dans Xamarin.Essentials permet à une application de partager des données telles que des liens web et de texte pour d’autres applications sur l’appareil.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 69d429b1cdbbbd6dbb53e3cefa89695666494ba7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782383"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials : Transfert de données

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **DataTransfer** classe permet à une application de partager des données telles que des liens web et de texte pour d’autres applications sur l’appareil.

## <a name="using-data-transfer"></a>À l’aide de transfert de données

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité transfert de données fonctionne en appelant le `RequestAsync` méthode avec une charge de demande de données qui inclut des informations à partager avec d’autres applications. Texte et les Uri peuvent être mélangés et chaque plate-forme gère un filtrage basé sur le contenu.

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

Interface utilisateur à partager dans une application externe qui s’affiche lorsque la demande est effectuée :

![Transfert de données](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Différences de plateformes

| Plateforme | Différence |
| --- | --- |
| Android | Propriété de l’objet est utilisée pour l’objet souhaité d’un message. |
| iOS | Objet ne pas utilisé. |
| iOS | Titre non utilisé. |
| UWP | Titre sera par défaut pour le nom de l’Application s’il n’est pas définie. |
| UWP | Objet ne pas utilisé. |

## <a name="api"></a>API

- [Code source de transfert de données](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Documentation de l’API de transfert de données](xref:Xamarin.Essentials.DataTransfer)
