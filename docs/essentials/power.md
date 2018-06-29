---
title: 'Xamarin.Essentials : État de veille de l’énergie'
description: La classe de l’alimentation permet à un programme obtenir l’état de veille de l’énergie pour déterminer si le périphérique fonctionne en mode de faible puissance.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080475"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials : État de veille de l’énergie

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **Power** classe fournit des informations sur l’état du périphérique économiseur d’énergie, ce qui indique si l’appareil est en cours d’exécution dans un mode de faible puissance. Applications doivent éviter le traitement en arrière-plan si l’état du périphérique-économiseur se trouve sur.

## <a name="background"></a>Présentation

Les appareils qui s’exécutent sur les batteries peuvent être placés en mode faible consommation d’énergie-veille. Parfois les appareils sont transférés dans ce mode automatiquement, par exemple, lorsque la batterie devient inférieur à 20 % de la capacité. Le système d’exploitation répond à économiseur d’énergie en réduisant les activités susceptibles d’épuiser la batterie. Les applications peuvent aider à en évitant le traitement en arrière-plan ou autres activités forte lorsque économiseur d’énergie est activé.

Pour les appareils Android, le **Power** classe retourne des informations significatives uniquement pour la version Android 5.0 (Lollipop) et versions ultérieures.

## <a name="using-the-power-class"></a>À l’aide de la classe de l’alimentation

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Obtenir l’état actuel de la veille de l’énergie de l’appareil à l’aide de la méthode statique `Power.EnergySaverStatus` propriété :

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Cette propriété retourne un membre de la `EnergySaverStatus` énumération, qui est soit `On`, `Off`, ou `Unknown`. Si la propriété retourne `On`, l’application doit éviter le traitement en arrière-plan ou autres activités qui peuvent consommer beaucoup de puissance.

L’application doit également installer un gestionnaire d’événements. Le **Power** classe expose un événement est déclenché lorsque l’état de veille de l’énergie change :

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Si l’état de veille de l’énergie devient `On`, l’application doit s’arrêter l’exécution du traitement en arrière-plan. Si l’état passe à `Unknown` ou `Off`, l’application peut reprendre le traitement en arrière-plan.

## <a name="api"></a>API

- [Code de source d’alimentation](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Documentation de l’API](xref:Xamarin.Essentials.Power)
