---
title: 'Xamarin.Essentials : OrientationSensor'
description: La classe OrientationSensor permet de contrôler l’orientation d’un périphérique dans un espace à trois dimensions.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080460"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials : OrientationSensor

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **OrientationSensor** classe vous permet de surveiller l’orientation d’un périphérique dans un espace tridimensionnel.

> [!NOTE]
> Cette classe est pour déterminer l’orientation d’un périphérique dans un espace 3D. Si vous avez besoin déterminer si du vidéo périphérique affichage est en mode portrait ou paysage, utilisez le `Orientation` propriété de la `ScreenMetrics` objet disponible à partir de la [ `DeviceDisplay` ](device-display.md) classe.

## <a name="using-orientationsensor"></a>À l’aide de OrientationSensor

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Le `OrientationSensor` est activé en appelant le `Start` méthode pour surveiller les modifications apportées à l’orientation du périphérique et désactivée en appelant le `Stop` (méthode). Toutes les modifications sont envoyées par le biais de la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
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

`OrientationSensor` lectures sont retournés sous la forme d’un [ `Quaternion` ](xref:System.Numerics.Quaternion) qui décrit l’orientation de l’appareil selon les deux systèmes de coordonnées 3D :

Le périphérique (généralement un téléphone ou tablette) possède un système de coordonnées 3D avec les axes suivants :

- Le nombre positif X des points d’axe à droite de l’affichage en mode portrait.
- L’axe Y positif pointe vers le haut de l’appareil en mode portrait.
- L’axe Z positif pointe en dehors de l’écran.

Le système de coordonnées 3D du monde a les axes suivants :

- Le nombre positif axe X est la tangente à la surface de la terre et pointe east.
- L’axe Y positif est également tangente à la surface de la terre et points Nord.
- L’axe Z positif est perpendiculaire à la surface de la terre et pointe vers le haut.

Le `Quaternion` décrit la rotation du périphérique système de coordonnées par rapport à système de coordonnées de la terre.

A `Quaternion` valeur est étroitement liée à la rotation autour d’un axe. Si un axe de rotation est le vecteur normalisé (un<sub>x</sub>, un<sub>y</sub>, un<sub>z</sub>), et l’angle de rotation est Θ, puis (X, Y, Z, W) sont des composants du quaternion :

(un<sub>x</sub>·sin(Θ/2), un<sub>y</sub>·sin(Θ/2), un<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Il s’agit des systèmes de coordonnées droite, donc avec le curseur de la partie droite désigné dans la direction positive de l’axe de rotation, la courbe des doigts indiquer la direction de rotation pour les valeurs positives.

Exemples :

* Lorsque l’appareil se trouve à plat sur une table avec l’écran vers le haut, le haut de l’appareil (en mode portrait) pointant vers le nord, les deux systèmes de coordonnées sont alignées. Le `Quaternion` valeur représente le quaternion identité (0, 0, 0, 1). Tous les rotations peuvent être analysées par rapport à cette position.

* Lorsque l’appareil se trouve à plat sur une table avec l’écran vers l’et le haut de l’appareil (en mode portrait) pointant vers l’ouest, la `Quaternion` valeur est (0, 0, 0,707, 0,707). L’appareil a été pivote de 90 degrés autour de l’axe Z du monde.

* Lorsque l’appareil est maintenu exclusivement afin que la partie supérieure (en mode portrait) pointe vers le ciel et l’arrière de l’appareil fait face Nord, l’appareil a été pivotée de 90 degrés autour de l’axe X. Le `Quaternion` est de valeur (0,707, 0, 0, 0,707).

* Si l’appareil est placé à son bord gauche est donc sur une table et haut pointe nord, l’appareil a été pivoté &ndash;90 degrés autour de l’axe des Y (ou 90 degrés autour de l’axe Y négatif). Le `Quaternion` valeur est (0,-0.707, 0, 0,707).

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur.](xref:Xamarin.Essentials.SensorSpeed)

- **Plus rapide** : obtenir les données de capteur aussi rapidement que possible (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Jeu** – taux approprié pour les jeux (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Normal** – taux par défaut approprié pour les modifications d’orientation de l’écran.
- **L’interface utilisateur** – taux approprié pour l’interface utilisateur générale.

Si votre gestionnaire d’événements n’est pas garanti pour s’exécuter sur le thread d’interface utilisateur et si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, utilisez la [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) méthode à exécuter ce code sur le thread d’interface utilisateur.

## <a name="api"></a>API

- [Code source de OrientationSensor](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [Documentation de l’API de OrientationSensor](xref:Xamarin.Essentials.OrientationSensor)
