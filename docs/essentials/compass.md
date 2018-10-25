---
title: 'Xamarin.Essentials : Boussole'
description: La classe Compass vous permet d'obtenir des informations sur l'orientation de l’appareil par rapport au nord magnétique.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c3fe98c384a87bdc08ce94e7537d1a6343767561
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353881"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials : Boussole

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Compass** vous permet d'obtenir des informations sur l'orientation de l’appareil par rapport au nord magnétique.

## <a name="using-compass"></a>Utilisation de **Compass**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de **boussole** fonctionne en appelant les méthodes `Start` et `Stop` pour démarrer et arrêter l'écoute.
L'événement `ReadingChanged` est mis à votre disposition pour surveiller l'évolution de l'orientation de l'appareil :

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occurred
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Informations d'implémentation de la plateforme 

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android ne fournit pas d’API pour la lecture de la boussole. Nous utilisons donc l’accéléromètre et le magnétomètre pour calculer le nord magnétique, comme recommandé par Google.

Dans de rares cas, vous verrez peut-être des résultats incohérents, car les capteurs ont besoin d’être étalonnés, ce qui implique le déplacement de votre appareil dans un mouvement décrivant un 8. La meilleure façon de faire cela est d'ouvrir Google Maps, d'appuyer sur le point de votre emplacement et de sélectionner **Calibrate boussole**/**Calibrer la boussole**.

Il est bon de noter que l'exécution simultanée de plusieurs capteurs via votre application peut affecter leur vitesse.

## <a name="low-pass-filter"></a>Filtres passe-bas

En raison de la façon dont la boussole Android met à jour ses valeurs il peut être nécessaire de lisser la sortie. Un _filtre passe bas_ peut être appliqué en fonction de la moyenne des sinus et des cosinus des angles et peut être activé en définissant la propriété `ApplyLowPassFilter` sur la classe `Compass` :

```csharp
Compass.ApplyLowPassFilter = true;
```

Cette fonctionnalité est uniquement appliquable sur la plateforme Android. Plus d’informations [ici](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>API

- [Code source boussole](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Documentation de l’API boussole](xref:Xamarin.Essentials.Compass)
