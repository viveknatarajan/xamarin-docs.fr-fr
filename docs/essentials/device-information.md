---
title: 'Xamarin.Essentials : Informations sur l’appareil'
description: Ce document décrit la classe DeviceInfo dans Xamarin.Essentials, qui fournit des informations sur l’appareil, l’application est en cours d’exécution.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 18fe081372cc190e5ead2045f36d63652f8702c3
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353800"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials : Informations sur l’appareil

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **DeviceInfo** fournit des informations sur l’appareil que l’application est en cours d’exécution.

## <a name="using-deviceinfo"></a>Utilisation de **DeviceInfo**

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

`DeviceInfo.DeviceType` met en corrélation une énumération pour déterminer si l’application s’exécute sur un appareil physique ou virtuel. Un appareil virtuel est un simulateur ou un émulateur.

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS n’expose pas d’une API pour les développeurs obtenir le nom de l’appareil iOS spécifique. Au lieu de cela, un identificateur de matériel est retourné comme _iPhone10, 6_ qui fait référence à l’iPhone X. Un mappage de ces identificateurs ne sont pas fournies par Apple, mais peut être trouvée sur [l’iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (une source non officielles source).

--------------

## <a name="api"></a>API

- [Code source de DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Documentation de l’API de DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
