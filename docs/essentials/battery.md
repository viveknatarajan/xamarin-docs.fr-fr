---
title: 'Xamarin.Essentials : batterie'
description: Ce document décrit la classe de la batterie dans Xamarin.Essentials, ce qui vous permet de vérifier les informations de batterie de l’appareil et surveillez les modifications apportées.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6b87625b3305d0a9ec40593d8b3fe29eb551bbf4
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514308"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials : batterie

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **batterie** classe vous permet de vérifier les informations sur la pile et surveillez les modifications apportées de l’appareil.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **batterie** fonctionnalité de la configuration spécifique de plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le `Battery` autorisation est obligatoire et doit être configurée dans le projet Android. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

OU mettre à jour le manifeste Android :

Ouvrez le **AndroidManifest.xml** fichier sous le **propriétés** dossier et ajoutez le code suivant à l’intérieur de la **manifeste** nœud.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** trouver la **autorisations requises :** zone et vérifiez la **batterie** autorisation. Cela met automatiquement à jour le **AndroidManifest.xml** fichier.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requis.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requis.

-----

## <a name="using-battery"></a>À l’aide de la batterie

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Vérifiez les informations sur la pile actuelle :

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

Chaque fois qu’une propriétés de la batterie de modifier un événement est déclenché :

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

## <a name="platform-differences"></a>Différences de plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune différence de la plateforme.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Appareil doit être utilisé pour tester les API. 
* Retourne uniquement des `AC` ou `Battery` pour `PowerSource`.
* Pas possible d’annuler la vibration.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Retourne uniquement des `AC` ou `Battery` pour `PowerSource`.

-----

## <a name="api"></a>API

- [Code source de batterie](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Documentation de l’API de la batterie](xref:Xamarin.Essentials.Battery)
