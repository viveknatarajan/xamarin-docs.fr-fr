---
title: 'Xamarin.Essentials : Informations sur l’appareil'
description: La classe DeviceInfo fournit des informations générales sur l’appareil.
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

La classe **DeviceInfo** fournit des informations générales sur l’appareil.

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

La propriété **[DeviceInfo.Platform](xref:Xamarin.Essentials.DeviceInfo.Platform)** indique la famille de système d'exploitation actuellement utilisé, elle possède les constantes suivantes :

* **DeviceInfo.Platforms.iOS** : iOS
* **DeviceInfo.Platforms.Android** : Android
* **DeviceInfo.Platforms.UWP** : UWP
* **DeviceInfo.Platforms.Unsupported** : non pris en charge

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idiomes](xref:Xamarin.Essentials.DeviceInfo.Idioms)

La propriété **[DeviceInfo.Idiom](xref:Xamarin.Essentials.DeviceInfo.Idiom)** indique le type d'appareil actuellement utilisé, elle possède les constantes suivantes :

* **DeviceInfo.Idioms.Phone** – téléphone
* **DeviceInfo.Idioms.Tablet** – tablette PC
* **DeviceInfo.Idioms.Desktop** : ordinateur de bureau
* **DeviceInfo.Idioms.TV** – TV
* **DeviceInfo.Idioms.Unsupported** : non pris en charge

## <a name="device-type"></a>[Type d'appareil](xref:Xamarin.Essentials.DeviceInfo.DeviceType)

L'énumérateur **[DeviceType](xref:Xamarin.Essentials.DeviceInfo.DeviceType)** indique si l'application est lancée sur un appareil phyique ou un émulateur, il possède les valeurs possibles suivantes :

* **DeviceInfo.DeviceType.Physical** – Appareil physique
* **DeviceInfo.DeviceType.Virtual** – Emulateur

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS n’expose pas d’API permettant d'obtenir le nom précis de l’appareil iOS utilisé. Au lieu de cela, un identificateur de matériel est retourné, comme _iPhone10, 6_, qui fait référence à l’iPhone X. Aucun mappage de ces identificateurs n'est fourni par Apple, mais vous pouvez le retrouver sur [l’iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (source communautaire/non officielle).

--------------

## <a name="api"></a>API

- [Code source de DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Documentation de l’API de DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
