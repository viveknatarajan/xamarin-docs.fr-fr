---
title: Informations d’affichage Xamarin.Essentials périphérique
description: La classe DeviceDisplay fournit plus d’informations sur les métriques d’écran de l’appareil de l’application est en cours d’exécution.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 05701ff2bc9fceac8a0a490989e52d0327079d46
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-display-information"></a>Informations d’affichage Xamarin.Essentials périphérique

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **DeviceDisplay** classe fournit des informations sur les métriques d’écran de l’appareil l’application est en cours d’exécution.

## <a name="using-devicedisplay"></a>À l’aide de DeviceDisplay

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Métriques de l’écran

En plus d’informations sur l’appareil de base la **DeviceDisplay** classe contient des informations sur l’écran et l’orientation du périphérique.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

Le **DeviceDisplay** classe également expose et événements qui peuvent s’abonner qui déclenche un événement chaque fois que les modifications de métrique d’écran :

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [Code source de DeviceDisplay](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Documentation de l’API de DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)
