---
title: 'Xamarin.Essentials : Informations sur l’appareil'
description: Ce document décrit la classe DeviceInfo dans Xamarin.Essentials, qui fournit des informations sur l’appareil, l’application est en cours d’exécution.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b7246afca19607ef2f70288d4643696f4ac35d52
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831485"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials : Informations sur l’appareil

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **DeviceInfo** classe fournit des informations sur l’appareil que l’application est en cours d’exécution.

## <a name="using-deviceinfo"></a>À l’aide de DeviceInfo

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Les informations suivantes sont exposées via l’API :

```csharp
// Device Model (SMG-950U)
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

`DeviceInfo.Platform` correspond à une chaîne constante qui mappe au système d’exploitation. Les valeurs peuvent être vérifiées avec la `Platforms` classe :

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** : Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** : non pris en charge

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idiomes](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` met en corrélation une chaîne constante qui correspond au type d’appareil, l’application est en cours d’exécution. Les valeurs peuvent être vérifiées avec la `Idioms` classe :

- **DeviceInfo.Idioms.Phone** – téléphone
- **DeviceInfo.Idioms.Tablet** – Tablet PC
- **DeviceInfo.Idioms.Desktop** : bureau
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** : non pris en charge

## <a name="device-type"></a>Type d'appareil

`DeviceInfo.DeviceType` met en corrélation une énumération pour déterminer si l’application est en cours d’exécution sur physique ou virtuel appareil. Un appareil virtuel est un simulateur ou un émulateur.

## <a name="api"></a>API

- [Code source de DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Documentation de l’API de DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
