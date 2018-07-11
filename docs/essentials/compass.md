---
title: 'Xamarin.Essentials : Compass'
description: Ce document décrit la classe Compass dans Xamarin.Essentials, ce qui vous permet d’analyser l’en-tête de l’appareil le nord magnétique.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947411"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials : Compass

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **boussole** classe vous permet de surveiller l’en-tête de l’appareil le nord magnétique.

## <a name="using-compass"></a>À l’aide de la boussole

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de boussole fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à la boussole. Toutes les modifications sont renvoyées via la `ReadingChanged` événement. Voici un exemple :

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
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
            // Some other exception has occured
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android ne fournit pas d’une API pour la récupération de la boussole. Nous allons utiliser l’accéléromètre et magnétomètre pour calculer le titre le nord magnétique, ce qui est recommandé par Google. 

Dans de rares cas, vous peut-être voir des résultats incohérents, car les capteurs besoin d’être étalonné, ce qui implique le déplacement de votre appareil dans un mouvement de la figure 8. La meilleure façon de faire cela doit ouvrir Google Maps, appuyez sur le point de votre emplacement et sélectionnez **Calibrate boussole**.

Sachez qui exécute plusieurs capteurs à partir de votre application en même temps peut régler la vitesse du capteur.

--------------

## <a name="api"></a>API

- [Code source boussole](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Documentation de l’API boussole](xref:Xamarin.Essentials.Compass)
