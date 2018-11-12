---
title: 'Xamarin.Essentials : batterie'
description: Ce document décrit la classe Battery de Xamarin.Essentials, qui vous permet d’accéder aux informations sur la batterie de l’appareil et de superviser les changements.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6a14c939064538a405a1fe64061e0bb2e903fedd
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675430"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials : batterie

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **Battery** vous permet d’accéder aux informations sur la batterie de l’appareil et de superviser les changements.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

Pour accéder à la fonctionnalité de **batterie**, la configuration suivante spécifique à la plateforme est obligatoire.

# <a name="androidtabandroid"></a>[Android](#tab/android)

L’autorisation `Battery` est obligatoire, et doit être configurée dans le projet Android. Vous pouvez l’ajouter des façons suivantes :

Ouvrez le fichier **AssemblyInfo.cs** sous le dossier **Propriétés** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

OU mettez à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** sous le dossier **Propriétés** et ajoutez ce qui suit dans le nœud du **manifeste**.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Vous pouvez également cliquer avec le bouton droit sur le projet Android, et ouvrir les propriétés du projet. Sous **Manifeste Android**, recherchez la zone **Autorisations nécessaires**, puis cochez l’autorisation **Battery**. Cela entraîne la mise à jour automatique du fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-battery"></a>Utilisation de la batterie

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Examinez les informations actuelles relatives à la batterie :

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

Chaque fois que l’une des propriétés de la batterie change, un événement est déclenché :

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(object sender, BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune différence entre les plateformes.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Vous devez utiliser l’appareil pour tester les API. 
* Seul `AC` ou `Battery` est retourné pour `PowerSource`.
* Impossible d’annuler la vibration.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Seul `AC` ou `Battery` est retourné pour `PowerSource`.

-----

## <a name="api"></a>API

- [Code source de la fonctionnalité de batterie](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Documentation sur l’API de batterie](xref:Xamarin.Essentials.Battery)
