---
title: Configuration de l’émulateur Android
description: Cette section décrit comment préparer l’émulateur Google Android pour tester une application. Elle explique comment accélérer l’émulateur pour optimiser les performances et vous montre comment utiliser un gestionnaire d’émulateur pour créer et personnaliser des appareils virtuels.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 215e298068b7a3a23b2e469e923f172c8303bbcb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-setup"></a>Configuration de l’émulateur Android

_Cette section décrit comment préparer l’émulateur Google Android pour tester une application. Elle montre comment accélérer l’émulateur pour optimiser les performances, et comment utiliser un gestionnaire d’émulateur pour créer et personnaliser des appareils virtuels._


## <a name="overview"></a>Vue d'ensemble

L’émulateur du kit Android SDK Google peut être exécuté dans diverses configurations pour simuler différents appareils. Chacune de ces configurations est créée comme _appareil virtuel_. Dans ce guide, vous allez apprendre à accélérer l’émulateur Android pour optimiser les performances et à utiliser le Gestionnaire d’émulateurs Android Xamarin ou le Gestionnaire d’émulateurs hérité de Google pour créer des appareils virtuels.


> [!NOTE]
> À compter de la version **26.0.1** d’Android SDK Tools, Google ne prend plus en charge les gestionnaires d’AVD/de SDK, au profit de ses nouveaux outils d’interface de ligne de commande (CLI). En raison de ce changement, les Gestionnaires d’appareils/de SDK Xamarin sont dorénavant utilisés à la place des Gestionnaires de SDK/d’émulateur Google pour Android Tools 26.0.1 et ultérieur. (Pour plus d’informations sur l’utilisation du Gestionnaire Xamarin SDK, consultez [Installation du kit Android SDK](~/android/get-started/installation/android-sdk.md).


## <a name="sections"></a>Sections

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Accélération matérielle](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Comment préparer votre ordinateur pour optimiser les performances de l’émulateur Google Android. Comme l’émulateur Google Android peut être extrêmement lent sans accélération matérielle, nous vous recommandons d’activer l’accélération matérielle sur votre ordinateur avant d’utiliser l’émulateur.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Gestionnaire d’appareils Android Xamarin](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Comment utiliser Xamarin Android Device Manager pour créer et personnaliser les appareils virtuels de l’émulateur Google Android. Le **Gestionnaire d’appareils Android Xamarin**, actuellement en préversion, est destiné à remplacer le Gestionnaire d’émulateur Google hérité. Si vous ciblez Android Oreo 8.0 ou ultérieur, vous devez utiliser le Gestionnaire d’appareils Android Xamarin à la place du Gestionnaire d’émulateur Google.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Gestionnaire d’émulateur Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

Comment utiliser le gestionnaire d’émulateur Google hérité pour créer et personnaliser les appareils virtuels de l’émulateur Google Android. Vous pouvez continuer d’exécuter l’émulateur Android Google avec le Gestionnaire d’émulateur Google d’origine en continuant d’utiliser Android Tools 25.2.5 ou inférieur.

Après avoir configuré l’émulateur du kit Android SDK, consultez [Émulateur Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md) pour plus d’informations sur le lancement de l’émulateur et son utilisation afin de tester et déboguer votre application.
