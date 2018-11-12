---
title: 'Xamarin.Essentials : informations sur l’affichage de l’appareil'
description: Ce document décrit la classe DeviceDisplay de Xamarin.Essentials, qui fournit les métriques d’écran de l’appareil sur lequel l’application s’exécute.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ebe97cf7fbb78bff17196110e835bd35ff76b826
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674884"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials : informations sur l’affichage de l’appareil

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **DeviceDisplay** fournit des informations sur les métriques d’écran de l’appareil sur lequel l’application s’exécute.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>Utilisation de DeviceDisplay

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Métriques d’écran

En plus des informations de base relatives à l’appareil, la classe **DeviceDisplay** contient des informations sur l’écran et l’orientation de l’appareil.

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

La classe **DeviceDisplay** expose également un événement auquel il est possible de s’abonner, et qui se déclenche chaque fois qu’une métrique d’écran change :

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

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune différence.

# <a name="iostabios"></a>[iOS](#tab/ios)

* L’accès à `ScreenMetrics` doit être effectué sur le thread d’IU, sinon une exception est levée. Vous pouvez utiliser la méthode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) pour exécuter ce code sur le thread d’IU.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune différence.

--------------


## <a name="api"></a>API

- [Code source de DeviceDisplay](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Documentation sur l’API de DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)
