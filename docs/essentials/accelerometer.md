---
title: 'Xamarin.Essentials : accéléromètre'
description: La classe de l’accéléromètre dans Xamarin.Essentials vous permet d’analyser le capteur d’accéléromètre du périphérique, ce qui indique l’accélération de l’appareil dans un espace tridimensionnel.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8229a372659e7918457a9d2f358b871e1a3f5978
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080455"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials : accéléromètre

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **accéléromètre** classe vous permet de surveiller les capteurs accéléromètre du périphérique qui indique l’accélération de l’appareil dans un espace tridimensionnel.

## <a name="using-accelerometer"></a>À l’aide d’accéléromètre

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de l’accéléromètre fonctionne en appelant le `Start` et `Stop` méthodes pour écouter les modifications apportées à l’accélération. Toutes les modifications sont envoyées par le biais de la `ReadingChanged` événement. Voici un exemple d’utilisation :

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
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

Lectures de l’accéléromètre sont rapportées dans G. Un G est une unité de par voie sèche forcer égale à celle exercée par champ gravitationnel de la terre (9,81 m/s ^ 2).

Le système de coordonnées est défini par rapport à l’écran du téléphone dans son orientation par défaut. Les axes ne sont pas permutées lorsque l’orientation du périphérique écran change.

L’axe des X est horizontale et pointe vers la droite, l’axe des Y est vertical pointant vers le haut et l’axe Z pointe vers l’extérieur de la face avant de l’écran. Dans ce système, coordonnées derrière l’écran ont des valeurs Z négatifs.

Exemples :

* Lorsque l’appareil se trouve à plat sur une table et est placé sur le côté gauche vers la droite, la valeur de l’accélération de x est un nombre positive.

* Lorsque l’appareil se trouve à plat sur une table, la valeur de l’accélération est + 1.00 G ou (+ 9,81 m/s ^ 2), ce qui correspond à l’accélération de l’appareil (0 m/s ^ 2) moins la force de gravité (-9,81 m/s ^ 2) et normalisé comme G.

* Lorsque l’appareil se trouve à plat sur une table et est transmise vers le ciel avec une accélération d’un m/s ^ 2, la valeur de l’accélération est égale à un + 9.81 qui correspondent à l’accélération de l’appareil (+ m/s ^ 2) moins la force de gravité (-9,81 m/s ^ 2) et normalisée dans G. 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur.](xref:Xamarin.Essentials.SensorSpeed)

- **Plus rapide** : obtenir les données de capteur aussi rapidement que possible (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Jeu** – taux approprié pour les jeux (ne pas retourne toujours sur le thread d’interface utilisateur).
- **Normal** – taux par défaut approprié pour les modifications d’orientation de l’écran.
- **L’interface utilisateur** – taux approprié pour l’interface utilisateur générale.

Si votre gestionnaire d’événements n’est pas garanti pour s’exécuter sur le thread d’interface utilisateur et si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, utilisez la [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) méthode à exécuter ce code sur le thread d’interface utilisateur.

## <a name="api"></a>API

- [Code source d’accéléromètre](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Documentation de l’accéléromètre API](xref:Xamarin.Essentials.Accelerometer)
