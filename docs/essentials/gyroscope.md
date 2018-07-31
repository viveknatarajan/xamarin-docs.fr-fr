---
title: 'Xamarin.Essentials : Gyroscope'
description: La classe Gyroscope dans Xamarin.Essentials vous permet de surveiller le capteur gyroscope de l’appareil, qui mesure la rotation autour des trois axes du principal de l’appareil.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f1e1199ae32158889ec569eb5f7e9742f37d45d4
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353624"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials : Gyroscope

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **Gyroscope** classe vous permet de surveiller le capteur de gyroscope du périphérique qui est la rotation autour des trois axes du principal de l’appareil.

## <a name="using-gyroscope"></a>À l’aide de Gyroscope

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de Gyroscope fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à la gyroscope. Toutes les modifications sont renvoyées via la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
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

## <a name="api"></a>API

- [Code source de gyroscope](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Documentation de gyroscope API](xref:Xamarin.Essentials.Gyroscope)
