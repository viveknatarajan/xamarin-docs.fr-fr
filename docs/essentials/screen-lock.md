---
title: Verrouillage d’écran de Xamarin.Essentials
description: La classe ScreenLock peut demander à conserver l’écran de tomber en mode veille lorsque l’application est en cours d’exécution.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0bdf75825d9c6dc594749fe7aa1e133207cfa0fa
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-screen-lock"></a>Verrouillage d’écran de Xamarin.Essentials

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **ScreenLock** classe peut demander à conserver l’écran de tomber en mode veille lorsque l’application est en cours d’exécution.

## <a name="using-screenlock"></a>À l’aide de ScreenLock

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de verrouillage d’écran fonctionne en appelant le `RequestActive` et `RequestRelease` méthodes pour demander de l’écran à partir de la mise hors tension.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Écran de code source du verrou](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Documentation de l’API de verrou écran](xref:Xamarin.Essentials.ScreenLock)
