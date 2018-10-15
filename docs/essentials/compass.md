---
title: 'Xamarin.Essentials : Compass'
description: Ce document décrit la classe Compass dans Xamarin.Essentials, ce qui vous permet d’analyser l’en-tête de l’appareil le nord magnétique.
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
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials : Compass

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Compass** vous permet de surveiller l’en-tête de l’appareil le nord magnétique.

## <a name="using-compass"></a>Utilisation de **Compass**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de boussole fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à la boussole. Toutes les modifications sont renvoyées via la `ReadingChanged` événement. Voici un exemple :

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

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android ne fournit pas d’une API pour la récupération de la boussole. Nous allons utiliser l’accéléromètre et magnétomètre pour calculer le titre le nord magnétique, ce qui est recommandé par Google.

Dans de rares cas, vous peut-être voir des résultats incohérents, car les capteurs besoin d’être étalonné, ce qui implique le déplacement de votre appareil dans un mouvement de la figure 8. La meilleure façon de faire cela doit ouvrir Google Maps, appuyez sur le point de votre emplacement et sélectionnez **Calibrate boussole**.

Sachez qui exécute plusieurs capteurs à partir de votre application en même temps peut régler la vitesse du capteur.

## <a name="low-pass-filter"></a>Filtres passe-bas

En raison de la façon la boussole Android valeurs sont mis à jour et calculés il peut être nécessaire pour lisser les valeurs. A _faible passer filtre_ peuvent être appliquées que la moyenne des sinus et le cosinus valeurs des angles et peut être activée en définissant le `ApplyLowPassFilter` propriété sur le `Compass` classe :

```csharp
Compass.ApplyLowPassFilter = true;
```

Cela est uniquement appliqué sur la plateforme Android. Plus d’informations peut être lu [ici](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>API

- [Code source boussole](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Documentation de l’API boussole](xref:Xamarin.Essentials.Compass)
