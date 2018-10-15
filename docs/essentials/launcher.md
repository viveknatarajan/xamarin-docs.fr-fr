---
title: Lanceur de Xamarin.Essentials
description: La classe Lanceur dans Xamarin.Essentials permet à une application ouvrir un URI par le système.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 252bb873c1494265aafb2285057490ca29ce7419
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573631"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials : Lanceur

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Lanceur** permet à une application ouvrir un URI par le système. Cela est souvent utilisé lors de la profondeur de liaison dans les schémas d’URI personnalisés d’une autre application. Si vous souhaitez pour ouvrir le navigateur sur un site Web, reportez-vous à la **[navigateur](open-browser.md)** API.

## <a name="using-launcher"></a>À l’aide du Lanceur

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour utiliser l’appel de la fonctionnalité de lanceur le `OpenAsync` (méthode) et passez un `string` ou `Uri` pour ouvrir. Si vous le souhaitez, le `CanOpenAsync` méthode peut être utilisée pour vérifier si le schéma d’URI peut être géré par une application sur l’appareil.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>API

- [Code source de lanceur](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Documentation de l’API de lanceur](xref:Xamarin.Essentials.Launcher)
