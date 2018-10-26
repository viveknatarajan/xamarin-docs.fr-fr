---
title: Déboguer Android Wear sur un émulateur
description: Ces articles expliquent comment déboguer une application Xamarin.Android usure sur un émulateur.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 699fb3cc3a5730e8ab2c677feb7cdfbdcf106aeb
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120552"
---
# <a name="debug-android-wear-on-an-emulator"></a>Déboguer Android Wear sur un émulateur

_Ces articles expliquent comment déboguer une application Xamarin.Android usure sur un émulateur._

## <a name="debug-wear-on-emulator-overview"></a>Déboguer l’usure de présentation de l’émulateur

Développement d’applications Android Wear nécessite l’exécution de l’application, soit sur du matériel physique ou à l’aide d’un émulateur ou simulateur. L’utilisation de matériel est la meilleure approche, mais elle n’est pas toujours la plus pratique. Dans de nombreux cas, il peut être plus simple et plus rentable de simuler/émuler le matériel Android Wear à l’aide d’un émulateur comme décrit ci-dessous. Si vous n’êtes pas encore familiarisé avec le processus de déploiement et l’exécution d’applications de Android Wear, consultez [Wear Hello,](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-android-emulator"></a>Configurer l’émulateur Android

Pour exécuter votre application Wear sur un émulateur, vous devez installer l’émulateur Android SDK Android et configurez-le pour Android Wear. Pour global émulateur du Kit Android installation et configuration d’informations, consultez [configuration de l’émulateur Android](~/android/get-started/installation/android-emulator/index.md).

Lorsque vous créez un appareil virtuel usure, sélectionnez un profil d’appareil Android Wear (tel que **Android Wear carré**). Pour améliorer les performances, utilisez l’usure **x86** CPU/ABI comme indiqué dans cet exemple :

[![Exemple de configuration de périphérique virtuel Wear](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Lancer l’appareil virtuel Wear 

Une fois que vous avez créé un appareil virtuel Android Wear, vous pouvez le choisir dans le menu déroulant de périphérique dans l’IDE avant de commencer le débogage. Si votre appareil virtuel n’est pas disponible dans le menu déroulant du périphérique, vérifiez que votre projet est un Android *Wear* de niveau projet d’application (pas un projet d’application Android) et que son niveau d’API cible est définie sur la même API que l’appareil virtuel. Exemple :

[![En choisissant un AVD Wear dans le menu de périphérique de Visual Studio](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Après le démarrage de l’émulateur Android, Xamarin.Android déploiera l’application Wear à l’émulateur. L’émulateur exécute l’application avec l’image d’appareil virtuel configurée.

Ne soyez pas surpris si vous voyez ce (ou un autre écran INTERSTITIEL) dans un premier temps. L’émulateur espion peut prendre un certain temps à démarrer : 

![Regardez l’émulateur affiche une minute...](debug-on-emulator-images/please-wait.png)

Il est possible de laisser l’émulateur s’exécuter ; il n’est pas nécessaire de le fermer et de le redémarrer chaque fois que l’application est exécutée.

 
## <a name="summary"></a>Récapitulatif
 
Ce guide a expliqué comment configurer l’émulateur Android pour le développement de l’usure et lancer un appareil virtuel usure pour le débogage.
