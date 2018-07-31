---
title: Lanceur de Xamarin.Essentials
description: La classe Lanceur dans Xamarin.Essentials permet à une application ouvrir un URI par le système.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 3ab820ece127558c1a5ca8b13c05fc4d6f20646e
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353932"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials : Lanceur

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **Lanceur** classe permet à une application ouvrir un URI par le système. Cela est souvent utilisé lors de la profondeur de liaison dans les schémas d’URI personnalisés d’une autre application. Si vous souhaitez pour ouvrir le navigateur sur un site Web, reportez-vous à la **[navigateur](open-browser.md)** API.

## <a name="using-launcher"></a>À l’aide du Lanceur

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour utiliser l’appel de la fonctionnalité de lanceur le `OpenAsync` (méthode) et passez un `string` ou `Uri` pour ouvrir. Si vous le souhaitez, le `CanOpenAsync` méthode peut être utilisée pour vérifier si le schéma d’URI peut être géré par une application sur l’appareil.

```csharp
public class LauncherTest
{
    public async Task OpenRideShare()
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
