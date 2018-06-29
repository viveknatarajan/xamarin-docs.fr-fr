---
title: 'Xamarin.Essentials : Gyroscope'
description: La classe Gyroscope dans Xamarin.Essentials vous permet d’analyser le capteur de gyroscope du périphérique, qui mesure la rotation autour de trois axes de principal de l’appareil.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6b2df3b6c5061464a06730ad09cbbbfdbceeb247
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080476"
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

Si votre gestionnaire d’événements n’est pas garanti pour s’exécuter sur le thread d’interface utilisateur et si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, utilisez la [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) méthode à exécuter ce code sur le thread d’interface utilisateur.

## <a name="api"></a>API

- [Code source d’un gyroscope](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Documentation de l’API de gyroscope](xref:Xamarin.Essentials.Gyroscope)
