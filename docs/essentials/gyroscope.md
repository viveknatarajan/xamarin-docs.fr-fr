---
title: 'Xamarin.Essentials : Gyroscope'
description: La classe Gyroscope dans Xamarin.Essentials vous permet d’analyser le capteur de gyroscope du périphérique, qui mesure la rotation autour de trois axes de principal de l’appareil.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2f2961c6cb78293891e186e7e0f749a7aa2fb8fc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783013"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials : Gyroscope

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **Gyroscope** classe vous permet de surveiller les capteurs gyroscope du périphérique qui est de pivoter autour de trois axes de principal de l’appareil.

## <a name="using-gyroscope"></a>À l’aide d’un Gyroscope

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité d’un Gyroscope fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à la gyroscope. Toutes les modifications sont envoyées par le biais de la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(GyroscopeChangedEventArgs e)
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur.](xref:Xamarin.Essentials.SensorSpeed)

- **Plus rapide** : obtenir les données de capteur aussi rapidement que possible (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Jeu** – taux approprié pour les jeux (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Normal** – taux par défaut approprié pour les modifications d’orientation de l’écran.
- **L’interface utilisateur** – taux approprié pour l’interface utilisateur générale.

## <a name="api"></a>API

- [Code source d’un gyroscope](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Documentation de l’API de gyroscope](xref:Xamarin.Essentials.Gyroscope)
