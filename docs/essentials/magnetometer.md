---
title: 'Xamarin.Essentials : magnétomètre'
description: La classe magnétomètre dans Xamarin.Essentials vous permet de surveiller le capteur magnétomètre de l’appareil, ce qui indique l’orientation du périphérique par rapport à champ magnétique de la terre.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 52790f78c2d78347a35f111b3c4db63900c24ec7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947359"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials : magnétomètre

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **magnétomètre** classe vous permet de surveiller le capteur de magnétomètre du périphérique qui indique l’orientation du périphérique par rapport à champ magnétique de la terre.

## <a name="using-magnetometer"></a>À l’aide du magnétomètre

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité magnétomètre fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à la magnétomètre. Toutes les modifications sont renvoyées via la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

Toutes les données sont retournées dans microteslas.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Code source de magnétomètre](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Documentation du magnétomètre API](xref:Xamarin.Essentials.Magnetometer)
