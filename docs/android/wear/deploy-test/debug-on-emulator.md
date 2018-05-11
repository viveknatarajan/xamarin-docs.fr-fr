---
title: Déboguer Android usure sur un émulateur
description: Ces articles expliquent comment déboguer une application de Xamarin.Android usure sur un émulateur.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3dda1445815f747f484458388cd856c2f9705603
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="debug-android-wear-on-an-emulator"></a>Déboguer Android usure sur un émulateur

_Ces articles expliquent comment déboguer une application de Xamarin.Android usure sur un émulateur._

## <a name="debug-wear-on-emulator-overview"></a>Déboguer l’usure sur une vue d’ensemble de l’émulateur

Développement d’applications d’usure Android nécessite l’exécution de l’application, soit sur du matériel physique ou à l’aide d’un émulateur ou un simulateur. L’utilisation de matériel est la meilleure approche, mais elle n’est pas toujours la plus pratique. Dans de nombreux cas, il peut être plus simple et plus efficace pour simuler/émuler le matériel d’usure Android à l’aide d’un émulateur comme décrit ci-dessous. Si vous n’êtes pas encore familiarisé avec le processus de déploiement et l’exécution d’usure Android les applications, voir [usure Bonjour,](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-google-android-emulator"></a>Configurer l’émulateur Android de Google

Pour exécuter votre application usure sur un émulateur, vous devez installer l’émulateur Android du Kit de développement logiciel Android et configurez-le pour porter Android. Pour l’émulateur Kit de développement logiciel Android installation et la configuration des informations générales, consultez [émulateur Android de Google](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Lorsque vous créez un périphérique virtuel usure, sélectionnez un profil d’appareil Android d’usure (tel que **Android usure carré**). Pour améliorer les performances, utilisez l’usure **x86** CPU/ABI comme dans cet exemple :

[![Exemple de configuration de périphérique virtuel usure](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Lancez le périphérique virtuel usure 

Après avoir créé un appareil virtuel Android d’usure, vous pouvez la choisir dans le menu déroulant de périphérique dans l’IDE avant de commencer le débogage. Si votre appareil virtuel n’est pas disponible dans le menu déroulant du périphérique, vérifiez que votre projet est un Android *usure* de niveau projet d’application (pas un projet d’application Android) et que son niveau de l’API de la cible est définie sur la même API que l’appareil virtuel. Par exemple :

[![En choisissant un AVD porter dans le menu de périphérique Visual Studio](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Après le démarrage de l’émulateur Android, Xamarin.Android déploierez l’application d’usure à l’émulateur. L’émulateur exécute l’application avec l’image d’appareil virtuel configurée.

Ne pas être surpris de constater si vous voyez cette erreur (ou un autre écran spot) dans un premier temps. L’émulateur espion peut prendre un certain temps pour pouvoir démarrer : 

![Surveiller l’émulateur affiche simplement une minute...](debug-on-emulator-images/please-wait.png)

Il est possible de laisser l’émulateur s’exécuter ; il n’est pas nécessaire de le fermer et de le redémarrer chaque fois que l’application est exécutée.

 
## <a name="summary"></a>Récapitulatif
 
Ce guide explique comment configurer l’émulateur Android de Google pour le développement d’usure et de lancer un périphérique virtuel d’usure pour le débogage.
