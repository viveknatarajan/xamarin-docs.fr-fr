---
title: Verrouillage d’écran de Xamarin.Essentials
description: La classe ScreenLock peut demander à conserver l’écran de tomber en mode veille lorsque l’application est en cours d’exécution.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 66702e9f8f5e6ad07f8f7c96f739edf1b2e66617
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
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
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease;
    }
}
```

## <a name="api"></a>API

- [Écran de code source du verrou](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [Documentation de l’API de verrou écran](xref:Xamarin.Essentials.ScreenLock)
