---
title: Xamarin.Essentials magnétomètre
description: La classe magnétomètre vous permet de surveiller les capteurs magnétomètre du périphérique qui indique l’orientation du périphérique relatif au champ magnétique de la terre.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e43834756e6a582bd0fd30a5655da8d087a971b7
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials magnétomètre

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **magnétomètre** classe vous permet de surveiller les capteurs magnétomètre du périphérique qui indique l’orientation du périphérique relatif au champ magnétique de la terre.

## <a name="using-magnetometer"></a>À l’aide de magnétomètre

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité magnétomètre fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à la magnétomètre. Toutes les modifications sont envoyées par le biais de la `ReadingChanged` événement. Voici un exemple d’utilisation :

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

Toutes les données est retourné dans microteslas.

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur.](xref:Xamarin.Essentials.SensorSpeed)

- **Plus rapide** : obtenir les données de capteur aussi rapidement que possible (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Jeu** – taux approprié pour les jeux (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Normal** – taux par défaut approprié pour les modifications d’orientation de l’écran.
- **L’interface utilisateur** – taux approprié pour l’interface utilisateur générale.

## <a name="api"></a>API

- [Code source de magnétomètre](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [Documentation de magnétomètre API](xref:Xamarin.Essentials.Magnetometer)
