---
title: 'Xamarin.Essentials : informations sur l’appareil'
description: Ce document décrit la classe DeviceInfo de Xamarin.Essentials, qui fournit des informations sur l’appareil où l’application s’exécute.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 530b04446703d78452357b2c9f9089e59ebf6e6c
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674811"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials : informations sur l’appareil

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **DeviceInfo** fournit des informations sur l’appareil où l’application s’exécute.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-deviceinfo"></a>Utilisation de DeviceInfo

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Les informations suivantes sont exposées via l’API :

```csharp
// Device Model (SMG-950U, iPhone10,6)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[Plateformes](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` est mis en corrélation avec une chaîne constante mappée au système d’exploitation. Vous pouvez vérifier les valeurs avec la classe `Platforms` :

- **DeviceInfo.Platforms.iOS** - iOS
- **DeviceInfo.Platforms.Android** - Android
- **DeviceInfo.Platforms.UWP** - UWP
- **DeviceInfo.Platforms.Unsupported** - Non pris en charge

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idiomes](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` est mis en corrélation avec une chaîne constante mappée au type d’appareil sur lequel l’application s’exécute. Vous pouvez vérifier les valeurs avec la classe `Idioms` :

- **DeviceInfo.Idioms.Phone** - Téléphone
- **DeviceInfo.Idioms.Tablet** - Tablette
- **DeviceInfo.Idioms.Desktop** - Poste de travail
- **DeviceInfo.Idioms.TV** - TV
- **DeviceInfo.Idioms.Unsupported** - Non pris en charge

## <a name="device-type"></a>Type d'appareil

`DeviceInfo.DeviceType` est mis en corrélation avec une énumération pour déterminer si l’application s’exécute sur un appareil physique ou virtuel. Un appareil virtuel est un simulateur ou un émulateur.

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre de la plateforme

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS n’expose pas d’API permettant aux développeurs d’obtenir le nom de l’appareil iOS spécifique. À la place, un identificateur matériel est retourné, par exemple _iPhone10,6_, qui fait référence à l’iPhone X. Apple ne fournit pas le mappage de ces identificateurs. Toutefois, vous pouvez le trouver sur le [Wiki de l’iPhone](https://www.theiphonewiki.com/wiki/Models) (source non officielle).

--------------

## <a name="api"></a>API

- [Code source de DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Documentation sur l’API de DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
