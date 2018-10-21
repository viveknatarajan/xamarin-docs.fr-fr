---
title: 'Xamarin.Essentials : Batterie'
description: La classe Battery vous permet d'obternir et de surveiller les informations sur la batterie de l’appareil.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6b87625b3305d0a9ec40593d8b3fe29eb551bbf4
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514308"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials : Batterie

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Battery** vous permet d'obternir et de surveiller les informations sur la batterie de l’appareil.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **Battery** quelques étapes de configurations spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

L'autorisation `Battery` est obligatoire et doit être configurée dans le projet Android. Elle peut être ajoutée comme suit :

Ouvrez le fichier **AssemblyInfo.cs** sous le dossier **propriétés** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

OU mettez à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** sous le dossier **propriétés** et ajoutez le code suivant à l’intérieur du nœud **manifest**.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** trouvez la zone "**autorisations requises :**" et cochez l'autorisation **batterie**. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-battery"></a>Utilisation de **Battery**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Vérifiez les informations sur la batterie :

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

L'événement `BatteryChanged` est mis à votre disposition pour surveiller l'évolution de l'état de la batterie :

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

Aucune spécificité pour cette plateforme.

# <a name="iostabios"></a>[iOS](#tab/ios)

* L'appareil doit être allumé pour accéder aux API.
* Retourne uniquement des `AC` ou `Battery` pour `PowerSource`.
* Impossible d’annuler la vibration.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Retourne uniquement des `AC` ou `Battery` pour `PowerSource`.

-----

## <a name="api"></a>API

- [Code source de batterie](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Documentation de l’API de la batterie](xref:Xamarin.Essentials.Battery)
