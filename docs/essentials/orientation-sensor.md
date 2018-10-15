---
title: 'Xamarin.Essentials : Capteur d’orientation'
description: La classe OrientationSensor vous permet de surveiller l’orientation d’un appareil dans l’espace à trois dimensions.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: a15338795424885882ed9c86288342d196f6fda2
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353813"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials : Capteur d’orientation

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **OrientationSensor** vous permet de surveiller l’orientation d’un appareil dans un espace tridimensionnel.

> [!NOTE]
> Cette classe est pour déterminer l’orientation d’un appareil dans l’espace 3D. Si vous avez besoin déterminer si le périphérique vidéo d’affichage est en mode portrait ou paysage, utilisez le `Orientation` propriété de la `ScreenMetrics` objet disponible à partir de la [ `DeviceDisplay` ](device-display.md) classe.

## <a name="using-orientationsensor"></a>Utilisation de **OrientationSensor**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Le `OrientationSensor` est activé en appelant le `Start` (méthode) pour surveiller les modifications apportées à l’orientation de l’appareil et désactivé en appelant le `Stop` (méthode). Toutes les modifications sont renvoyées via la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(object sender, OrientationSensorChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` lectures sont retournés sous la forme d’un [ `Quaternion` ](xref:System.Numerics.Quaternion) qui décrit l’orientation de l’appareil en fonction des deux systèmes de coordonnées 3D :

L’appareil (généralement un téléphone ou tablette) dispose d’un système de coordonnées 3D avec les axes suivants :

- La valeur positive à droite de l’affichage en mode portrait, les points de l’axe X.
- L’axe Y positif pointe vers le haut de l’appareil en mode portrait.
- L’axe Z positif pointe en dehors de l’écran.

Le système de coordonnées 3D de la terre a les axes suivants :

- La valeur positive axe des X est la tangente à la surface de la terre et pointe east.
- L’axe Y positif est également tangent à la surface de la terre et nord de points.
- L’axe Z positif est perpendiculaire à la surface de la terre et pointe vers le haut.

Le `Quaternion` décrit la rotation du système de coordonnées de l’appareil par rapport à système de coordonnées de la terre.

Un `Quaternion` valeur est très étroitement liée à la rotation autour d’un axe. Si un axe de rotation est le vecteur normalisé (un<sub>x</sub>, un<sub>y</sub>, un<sub>z</sub>), et l’angle de rotation est Θ, puis (X, Y, Z, W) sont des composants du quaternion :

(un<sub>x</sub>·sin(Θ/2), un<sub>y</sub>·sin(Θ/2), un<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Il s’agit des systèmes de coordonnées droite, donc avec le curseur de la main droite référencée dans le sens positif de l’axe de rotation, la courbe des doigts indiquent la direction de rotation d’angle positif.

Exemples :

* Lorsque l’appareil se trouve à plat sur une table avec son écran vers le haut, avec le bord supérieur de l’appareil (en mode portrait) pointant vers le nord, les deux systèmes de coordonnées sont alignées. Le `Quaternion` valeur représente le quaternion identité (0, 0, 0, 1). Tous les rotations peuvent être analysées par rapport à cette position.

* Lorsque l’appareil se trouve à plat sur une table avec son écran vers le haut et le haut de l’appareil (en mode portrait) pointant vers l’ouest, le `Quaternion` valeur est (0, 0, 0,707, 0,707). L’appareil a été pivotée de 90 degrés autour de l’axe Z de la terre.

* Lorsque l’appareil est maintenu verticalement afin que le haut (en mode portrait) pointe vers le ciel et l’arrière de l’appareil est confrontée Nord, l’appareil a été pivotée de 90 degrés autour de l’axe X. Le `Quaternion` valeur est (0,707, 0, 0, 0,707).

* Si l’appareil est positionné à son bord gauche est donc sur une table, et points de la partie supérieure du Nord, l’appareil a été pivotée &ndash;90 degrés autour de l’axe Y (ou 90 degrés autour de l’axe Y négatif). Le `Quaternion` valeur est (0,-0.707, 0, 0,707).

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Code source de OrientationSensor](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [Documentation de l’API de OrientationSensor](xref:Xamarin.Essentials.OrientationSensor)
