---
title: 'Xamarin.Essentials : État d’économie d’énergie'
description: La classe Power permet à un programme d’obtenir l’état d’économie d’énergie pour déterminer si l’appareil fonctionne en mode faible consommation d’énergie.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: jamesmontemagno
ms.author: jamont
ms.date: 06/27/2018
ms.openlocfilehash: 5a89dba16a93b007c5d7312221d8d33e00c7404a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110002"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials : État d’économie d’énergie

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **Power** fournit des informations sur l’état d’économie d’énergie de l’appareil, qui indique si ce dernier fonctionne en mode faible consommation d’énergie. Les applications doivent de préférence éviter le traitement en arrière-plan lorsque l’économie d’énergie de l’appareil est activée.

## <a name="background"></a>Présentation

Les appareils qui fonctionnent sur batterie peuvent être mis en mode faible consommation d’énergie. Parfois, ce basculement se fait automatiquement, par exemple, lorsque la batterie est à moins de 20 % de sa capacité. En mode économie d’énergie, le système d’exploitation réduit les activités qui ont tendance à épuiser la batterie. Les applications peuvent aller dans ce sens en évitant le traitement en arrière-plan ou autres activités énergivores lorsque le mode économie d’énergie est activé.

Pour les appareils Android, la classe **Power** ne retourne des informations significatives que pour Android version 5.0 (Lollipop) et les versions ultérieures.

## <a name="using-the-power-class"></a>Utiliser la classe Power

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Utilisez la propriété statique `Power.EnergySaverStatus` pour connaître l’état d’économie d’énergie actuel de l’appareil :

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Cette propriété retourne un membre de l’énumération `EnergySaverStatus` : `On`, `Off` ou `Unknown`. Si elle retourne `On`, l’application doit de préférence éviter le traitement en arrière-plan ou autres activités susceptibles de consommer beaucoup d’énergie.

L’application doit également installer un gestionnaire d’événements. La classe **Power** expose un événement qui est déclenché lorsque l’état d’économie d’énergie change :

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Si l’état d’économie d’énergie devient `On`, l’application doit arrêter le traitement en arrière-plan. S’il passe à `Unknown` ou `Off`, elle peut reprendre le traitement en arrière-plan.

## <a name="api"></a>API

- [Code source de Power](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Documentation de l’API Power](xref:Xamarin.Essentials.Power)
