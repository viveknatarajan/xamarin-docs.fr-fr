---
title: 'Xamarin.Essentials : État de l’économiseur d’énergie'
description: La classe Power vous permet d'obternir et de surveiller les informations sur l'économiseur d’énergie de l’appareil. En rêgle générale, les applications évitent les traitement en arrière-plan lorsque l'économiseur d’énergie de l’appareil est activé.
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
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials : État de l’économiseur d’énergie

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Power** fournit des informations sur l’état de économiseur d’énergie de l'appareil, ce qui indique si ce dernier fonctionne en mode économique. Les applications doivent éviter le traitement en arrière-plan si l’’économiseur d’énergie de l’appareil est activé.

## <a name="background"></a>Présentation

Les appareils contenant des batteries peuvent être mis dans un mode de faible consommation d’énergie. Parfois, les appareils se mettent dans ce mode automatiquement, par exemple, lorsque la batterie est inférieure à 20 % de leur capacité. Le système d’exploitation répond aux économiseur d’énergie en réduisant les activités qui ont tendance à épuiser la batterie. Les applications peuvent contribuer à cet effort en évitant les traitements en arrière-plan ou autres activités gourmandes lorsque le mode économiseur d’énergie est activé.

Pour les appareils Android, la classe **Power** fonctionne uniquement pour la version d’Android 5.0 (Lollipop) et les versions ultérieures.

## <a name="using-the-power-class"></a>Utilisation de **Power**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Obtenir l’état actuel de l'économiseur d’énergie de l’appareil à l’aide de la méthode statique `Power.EnergySaverStatus` :

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Cette propriété retourne un membre de l'énumération `EnergySaverStatus`, qui est soit `On`, `Off`, ou `Unknown`. Si la propriété retourne `On`, l’application doit éviter les traitements en arrière-plan ou autres activités gourmandes. Si l’état passe à `Unknown` ou `Off`, l’application peut reprendre les traitements en arrière-plan.

L’application doit également installer un gestionnaire d’événements. La classe **Power** expose un événement qui est déclenché lorsque l’état de l’économiseur d’énergie change :

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

## <a name="api"></a>API

- [Code de source d’alimentation](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Documentation de l’API](xref:Xamarin.Essentials.Power)
