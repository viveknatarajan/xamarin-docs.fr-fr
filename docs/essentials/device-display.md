---
title: 'Xamarin.Essentials : Informations sur l'écran de l'appareil'
description: Ce document décrit la classe DeviceDisplay dans Xamarin.Essentials, qui fournit des métriques de l’écran de l’appareil sur lequel l’application est en cours d’exécution.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cb42da4c8c2d0e381a5b00f7e60da6f427d19c66
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353826"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials : Informations sur l'écran de l'appareil

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **DeviceDisplay** fournit des informations à propos des métriques d’écran de l’appareil, l’application est en cours d’exécution.

## <a name="using-devicedisplay"></a>Utilisation de **DeviceDisplay**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Métriques de l’écran

En plus des informations de base de l’appareil le **DeviceDisplay** classe contient des informations sur l’écran et l’orientation de l’appareil.

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

La classe **DeviceDisplay** expose également un événement est déclenché chaque fois qu’un écran de modifications des métriques qui peut être abonné à :

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
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
