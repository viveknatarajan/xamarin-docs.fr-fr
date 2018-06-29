---
title: 'Xamarin.Essentials : Informations d’affichage périphérique'
description: Ce document décrit la classe DeviceDisplay dans Xamarin.Essentials, qui fournit des mesures de l’écran de l’appareil sur lequel l’application est en cours d’exécution.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3060d56e14fb0d3801a96ec0fe6e24c9efda4dac
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080311"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials : Informations d’affichage périphérique

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

Le **DeviceDisplay** classe expose également un événement qui peut s’abonner à qui est déclenché chaque fois que les modifications de métrique d’écran :

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [Code source de DeviceDisplay](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Documentation de l’API de DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)
