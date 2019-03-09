---
title: Quels pilotes USB sont nécessaires pour déboguer Android sur Windows ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 85045967f5c63eb39c45f917b957d2a393a3a068
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670291"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Quels pilotes USB sont nécessaires pour déboguer Android sur Windows ?

## <a name="finding-usb-drivers"></a>Recherche de pilotes USB

Pour déboguer sur un appareil Android lors du développement de Windows ; Vous devez installer un pilote USB compatible. Le gestionnaire Android SDK inclut « Google USB Driver » par défaut, ce qui ajoute la prise en charge pour les périphériques Nexus comme décrit ici : [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

Autres périphériques nécessitent des pilotes USB spécifiquement publiés par le fabricant du périphérique. Certains liens correspondant aux fabricants plus courantes sont incluses dans ce guide : [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternatives

Selon le manfacturer, il peut être difficile à localiser le pilote USB exact nécessité. Des alternatives pour tester des applications Android développement dans Windows, y compris à l’aide d’un émulateur Android ou à l’aide des services de tests externes. Certaines interfaces incluent :

- [Application Center Test](https://docs.microsoft.com/appcenter/test-cloud/) - Cloud services s’exécutent sur des centaines d’appareils Android réels de test.

- [Émulateur Visual Studio pour Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Débogage sur l’émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md)

