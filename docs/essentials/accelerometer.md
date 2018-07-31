---
title: 'Xamarin.Essentials : accéléromètre'
description: La classe Accelerometer dans Xamarin.Essentials vous permet de surveiller le capteur d’accéléromètre du périphérique, ce qui indique l’accélération de l’appareil dans un espace tridimensionnel.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b5a24e214eb129b4d53b94586632791c8827447b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353839"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials : accéléromètre

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **accéléromètre** classe vous permet de surveiller le capteur d’accéléromètre du périphérique qui indique l’accélération de l’appareil dans un espace tridimensionnel.

## <a name="using-accelerometer"></a>En utilisant l’accéléromètre

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de l’accéléromètre fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à l’accélération. Toutes les modifications sont renvoyées via la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

Lectures de l’accéléromètre sont retournés dans G. Un G est une unité de par voie sèche forcer égale à celle exercée par champ gravitationnelle la terre (9,81 m/s ^ 2).

Le système de coordonnées est défini par rapport à l’écran du téléphone dans son orientation par défaut. Les axes ne sont pas échangés lors de l’orientation du périphérique écran change.

L’axe X est horizontal et pointe vers la droite, l’axe des Y est verticale vers le haut et l’axe Z pointe vers l’extérieur de la face avant de l’écran. Dans ce système, les coordonnées derrière l’écran ont des valeurs Z négatives.

Exemples :

* Lorsque l’appareil se trouve à plat sur une table et est transmise sur le côté gauche vers la droite, la valeur d’accélération x est un nombre positive.

* Lorsque l’appareil se trouve à plat sur une table, la valeur de l’accélération est + 1.00 G ou (+ 9,81 m/s ^ 2), qui correspondent à l’accélération de l’appareil (0 m/s ^ 2) moins la force de gravité (-9,81 m/s ^ 2) et normalisée comme dans G.

* Lorsque l’appareil repose à plat sur une table et est poussé vers le ciel avec une accélération d’un m/s ^ 2, la valeur d’accélération est égale à A + 9.81 qui correspondent à l’accélération de l’appareil (+ m/s ^ 2) moins la force de gravité (-9,81 m/s ^ 2) et normalisée dans G.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Code de l’accéléromètre](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Documentation de l’accéléromètre API](xref:Xamarin.Essentials.Accelerometer)
