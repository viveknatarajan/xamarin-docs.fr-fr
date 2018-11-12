---
title: 'Xamarin.Essentials : verrouillage d’écran'
description: Ce document décrit la classe ScreenLock de Xamarin.Essentials, qui permet d’empêcher l’écran de passer en veille quand l’application est en cours d’exécution.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3bf8c949650cf9f039a5a516366a90e717dc944b
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675313"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials : verrouillage d’écran

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **ScreenLock** permet d’empêcher l’écran de passer en veille quand l’application est en cours d’exécution.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-screenlock"></a>Utilisation de ScreenLock

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de verrouillage d’écran appelle les méthodes `RequestActive` et `RequestRelease` pour empêcher l’écran de s’éteindre.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Code source de la fonctionnalité de verrouillage d’écran](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Documentation sur l’API de verrouillage d’écran](xref:Xamarin.Essentials.ScreenLock)
