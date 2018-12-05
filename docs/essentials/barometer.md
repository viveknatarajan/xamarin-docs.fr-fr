---
title: 'Xamarin.Essentials : Baromètre'
description: La classe Baromètre dans Xamarin.Essentials vous permet d’analyser le capteur du baromètre de l’appareil, qui mesure la pression.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 08/16/2018
ms.openlocfilehash: 9172d816fe9a15993ba8f015310d0e79874c2d84
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675027"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials : Baromètre

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **Baromètre** vous permet d’analyser le capteur du baromètre de l’appareil, qui mesure la pression.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-barometer"></a>Utilisation de Baromètre

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Baromètre fonctionne en appelant les méthodes `Start` et `Stop` qui permettent d’écouter les changements de lecture de la pression du baromètre en kilopascal. Toutes les variations peuvent être écoutées via l’événement `ReadingChanged`. Voici un exemple d’utilisation :

```csharp

public class BarometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public BarometerTest()
    {
        // Register for reading changes.
        Barometer.ReadingChanged += Barometer_ReadingChanged;
    }

    void Barometer_ReadingChanged(object sender, BarometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Pressure
        Console.WriteLine($"Reading: Pressure: {data.Pressure} kilopascals");
    }

    public void ToggleBarometer()
    {
        try
        {
            if (Barometer.IsMonitoring)
              Barometer.Stop();
            else
              Barometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre de la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucun détail d’implémentation spécifique à la plateforme.

# <a name="iostabios"></a>[iOS](#tab/ios)

Cette API utilise [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) pour surveiller les changements de pression, qui est une fonctionnalité matérielle ajoutée à l’iPhone 6 et aux appareils les plus récents. Un `FeatureNotSupportedException` sera levé sur les appareils qui ne prennent pas en charge l’altimètre.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucun détail d’implémentation spécifique à la plateforme.

-----

## <a name="api"></a>API

- [Code source de Baromètre](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Documentation de l’API Baromètre](xref:Xamarin.Essentials.Barometer)
