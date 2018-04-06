---
title: Exécution de l’émulateur du kit Android SDK
description: Guide pratique pour déboguer votre application avec l’émulateur du kit Android SDK
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 630520f88dd23d3860b5f42fbb9bc4eb35ca2c4b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="running-the-android-sdk-emulator"></a>Exécution de l’émulateur du kit Android SDK

Dans ce guide, vous allez apprendre comment lancer un appareil virtuel dans l’émulateur du kit Android SDK pour déboguer et tester votre application.

## <a name="using-a-pre-configured-virtual-device"></a>Utilisation d’un appareil virtuel préconfiguré

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio inclut des appareils virtuels préconfigurés qui s’affichent dans le menu déroulant des appareils. Par exemple, dans la capture d’écran suivante de Visual Studio 2017, plusieurs appareils virtuels préconfigurés sont disponibles :

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![Appareils virtuels](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

En règle générale, vous sélectionnez l’appareil virtuel **VisualStudio\_android-23\_x86\_phone** pour tester et déboguer une application pour téléphone. Si l’un de ces appareils virtuels préconfigurés répond à vos besoins (autrement dit, correspond au niveau d’API cible de votre application), passez directement à la section [Lancement de l’émulateur](#launching) pour commencer à exécuter votre application dans l’émulateur. (Si vous n’êtes pas encore familiarisé avec les niveaux d’API Android, consultez [Présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).)

Si votre projet Xamarin.Android utilise un niveau Framework cible qui n’est pas compatible avec les appareils virtuels disponibles, le menu déroulant répertorie les appareils virtuels inutilisables sous **Appareils non pris en charge**. Par exemple, dans le projet suivant, l’infrastructure cible est définie sur **Android 7.1 Nougat (API 25)**, laquelle n’est pas compatible avec les appareils virtuels **Android 6.0** qui sont fournis par défaut :

[![Appareil virtuel incompatible](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

Vous pouvez cliquer sur **Modifier la cible Android minimale** pour modifier la version d’Android minimale du projet afin qu’elle corresponde au niveau d’API des appareils virtuels disponibles. Vous pouvez également utiliser le **Gestionnaire d’émulateur Android** pour créer de nouveaux appareils virtuels qui prennent en charge votre niveau d’API cible, comme expliqué plus loin dans [Configuration des appareils virtuels](#virtualdevice). Avant de pouvoir configurer les appareils virtuels pour un nouveau niveau d’API, vous devez commencer par installer les images système correspondantes pour ce niveau d’API. Cela est expliqué dans la section suivante.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Visual Studio pour Mac inclut des appareils virtuels préconfigurés qui s’affichent dans le menu déroulant des appareils. Par exemple, dans la capture d’écran suivante, deux appareils virtuels préconfigurés sont disponibles :

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Appareils virtuels](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

En règle générale, vous sélectionnez l’appareil virtuel **Android\_Accelerated\_x86** pour tester et déboguer une application pour téléphone. Si cet appareil virtuel préconfiguré répond à vos besoins (autrement dit, correspond au niveau d’API cible de votre application), passez directement à [Lancement de l’émulateur](#launching) pour commencer à exécuter votre application dans l’émulateur. (Si vous n’êtes pas encore familiarisé avec les niveaux d’API Android, consultez [Présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="creating-custom-virtual-devices"></a>Création d’appareils virtuels personnalisés

Pour créer des périphériques virtuels personnalisés, vous devez utiliser le Gestionnaire d’appareils Android Xamarin ou le Gestionnaire d’émulateur Google hérité qui fait partie du kit Android SDK. Pour plus d’informations sur la création et la personnalisation d’appareils virtuels, consultez [Gestionnaire d’appareils Android Xamarin](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Si vous préférez utiliser le Gestionnaire d’émulateur Google hérité, consultez [Gestionnaire d’émulateur Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md).

Notez que si vous développez des applications pour Android 8.0 Oreo, vous devez utiliser le Gestionnaire d’appareils Android Xamarin.

<a name="launching" />

## <a name="launching-the-emulator"></a>Lancement de l'émulateur

En haut de l’IDE, un menu déroulant peut être utilisé pour sélectionner le mode **Debug** ou **Mise en production**. Si vous choisissez **Debug**, le débogueur est attaché au processus d’application en cours d’exécution dans l’émulateur. 

Après avoir choisi un appareil virtuel dans le menu déroulant des appareils, sélectionnez le mode **Debug** ou **Mise en production**, puis cliquez sur le bouton **Lire** pour exécuter l’application :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Modes Debug et Mise en production, bouton Lire](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Modes Debug et Mise en production, bouton Lire](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

Après le démarrage de l’émulateur Android, Xamarin.Android déploiera l’application sur l’émulateur. L’émulateur exécute l’application avec l’image d’appareil virtuel configurée. Une capture d’écran de l’émulateur du kit Android SDK est affichée ci-dessous (l’émulateur exécute une application vide appelée **MyApp**) :

![Émulateur exécutant une application vide](running-the-emulator-images/emulator-running.png)

Il est possible de laisser l’émulateur s’exécuter ; il n’est pas nécessaire de le fermer et de le redémarrer chaque fois que l’application est exécutée. Lors de la première exécution d’une application Xamarin.Android dans l’émulateur, le runtime partagé Xamarin.Android pour le niveau d’API ciblé est installé, suivi de l’application. L’installation du runtime peut prendre un certain temps, soyez donc patient. L’installation du runtime est réalisée uniquement lors du premier déploiement de l’application Xamarin.Android sur l’émulateur . Les déploiements suivants sont plus rapides, car seule l’application est copiée sur l’émulateur.

Pour plus d’informations sur l’utilisation de l’émulateur du kit Android SDK, consultez les rubriques suivantes relatives au développement Android :

-   [Navigation dans l’écran](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Réaliser des tâches de base dans l’émulateur](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Utilisation des contrôles étendus, des paramètres et de l’aide](https://developer.android.com/studio/run/emulator.html#extended)

