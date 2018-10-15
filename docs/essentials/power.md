---
title: 'Xamarin.Essentials : État de l’économiseur d’énergie'
description: La classe Power permet à un programme obtenir l’état de l’économiseur d’énergie pour déterminer si l’appareil fonctionne dans un mode de faible puissance.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 760a305280269734034a817182a8c2a07894ca2b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353488"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials : État de l’économiseur d’énergie

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Power** fournit des informations sur l’état du périphérique économiseur d’énergie, ce qui indique si l’appareil est en cours d’exécution dans un mode de faible puissance. Applications doivent éviter le traitement en arrière-plan si l’état d’économiseur d’énergie de l’appareil est activé.

## <a name="background"></a>Présentation

Les appareils qui s’exécutent sur les batteries peuvent être définis dans un mode de faible consommation d’énergie-veille. Parfois, les appareils sont permutés dans ce mode automatiquement, par exemple, lorsque la batterie est inférieure à 20 % de leur capacité. Le système d’exploitation répond aux économiseur d’énergie en réduisant les activités qui ont tendance à épuiser la batterie. Les applications peuvent aider à en évitant le traitement en arrière-plan ou autres activités haute puissance lorsque le mode économiseur d’énergie est activé.

Pour les appareils Android, le **Power** classe retourne des informations significatives uniquement pour la version d’Android 5.0 (Lollipop) et versions ultérieures.

## <a name="using-the-power-class"></a>Utilisation de **Power**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Obtenir l’état actuel de l’énergie-veille de l’appareil à l’aide de la méthode statique `Power.EnergySaverStatus` propriété :

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Cette propriété retourne un membre de la `EnergySaverStatus` énumération, qui est soit `On`, `Off`, ou `Unknown`. Si la propriété retourne `On`, l’application doit éviter de traitement en arrière-plan ou autres activités susceptibles de consommer beaucoup de puissance.

L’application doit également installer un gestionnaire d’événements. La classe **Power** expose un événement est déclenché lorsque l’état de l’économiseur d’énergie change :

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

Si l’état de l’économiseur d’énergie devient `On`, l’application doit s’arrêter l’exécution de traitement en arrière-plan. Si l’état passe à `Unknown` ou `Off`, l’application puisse reprendre le traitement en arrière-plan.

## <a name="api"></a>API

- [Code de source d’alimentation](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Documentation de l’API](xref:Xamarin.Essentials.Power)
