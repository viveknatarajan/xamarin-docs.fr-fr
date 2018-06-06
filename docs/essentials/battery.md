---
title: 'Xamarin.Essentials : batterie'
description: Ce document décrit la classe de la batterie dans Xamarin.Essentials, ce qui vous permet de vérifier les informations sur la pile de l’appareil et surveillez les modifications apportées.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 35764b4c2270359a7c010e1186f882e236e17fd5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782084"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials : batterie

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **batterie** classe vous permet de contrôler l’appareil les informations de batterie et surveillez les modifications apportées.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **batterie** fonctionnalité de la configuration spécifique plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le `Battery` autorisation est requise et doit être configurée dans le projet Android. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajouter :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

OU mettre à jour le manifeste Android :

Ouvrez le **AndroidManifest.xml** fichier sous le **propriétés** dossier et ajoutez le code suivant à l’intérieur de la **manifeste** nœud.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

Ou cliquez avec le bouton droit sur le projet Anroid et ouvrez les propriétés du projet. Sous **manifeste Android** de trouver la **les autorisations requises :** zone et vérifiez la **batterie** autorisation. Met automatiquement à jour la **AndroidManifest.xml** fichier.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

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
    case BatteryPowerSource.Ac:
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

Chaque fois qu’une des propriétés de la batterie modifier un événement est déclenché :

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Différences de plateformes

| Plateforme | Différence |
| --- | --- |
| iOS | Appareil doit être utilisé pour tester l’API. |
| iOS | Seulement retournera CA ou la batterie pour PowerSource. |
| UWP | Seulement retournera CA ou la batterie pour PowerSource. |

## <a name="api"></a>API

- [Code source de batterie](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Documentation de l’API de la batterie](xref:Xamarin.Essentials.Battery)
