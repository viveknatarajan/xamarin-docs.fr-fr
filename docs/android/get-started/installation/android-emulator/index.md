---
title: Installation de l’émulateur Google Android
description: Vous pouvez exécuter l’émulateur Google Android dans diverses configurations pour simuler différents appareils. Ce guide explique comment préparer l’émulateur Android pour tester votre application.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732529"
---
# <a name="google-android-emulator-setup"></a>Installation de l’émulateur Google Android

_Ce guide explique comment préparer l’émulateur Google Android pour tester votre application._


## <a name="overview"></a>Vue d'ensemble

Vous pouvez exécuter l’émulateur Google Android dans diverses configurations pour simuler différents appareils. Chaque configuration est appelée _appareil virtuel_. Quand vous déployez et testez votre application sur l’émulateur, vous sélectionnez un appareil virtuel préconfiguré ou personnalisé qui simule un appareil physique Android tel qu’un téléphone Nexus ou Pixel.

Les sections ci-dessous décrivent comment accélérer l’émulateur Google Android pour optimiser les performances, comment utiliser Android Device Manager pour créer et personnaliser des appareils virtuels et comment personnaliser les propriétés de profil d’un appareil virtuel. De plus, une section de dépannage décrit les problèmes d’installation courants et les solutions de contournement.

## <a name="sections"></a>Sections

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Accélération matérielle pour les performances de l’émulateur](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Guide pratique pour préparer votre ordinateur afin d’optimiser les performances de l’émulateur Android.
Comme l’émulateur Google Android peut être extrêmement lent sans accélération matérielle, nous vous recommandons d’activer l’accélération matérielle sur votre ordinateur avant d’utiliser cet émulateur.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Gestion des appareils virtuels avec Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)

Guide pratique pour utiliser Android Device Manager afin de créer et de personnaliser des appareils virtuels.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Modification des propriétés d’appareil virtuel Android](~/android/get-started/installation/android-emulator/device-properties.md)

Guide pratique pour utiliser Android Device Manager afin de modifier les propriétés de profil d’un appareil virtuel Android.

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Résolution des problèmes d’installation d’émulateur](~/android/get-started/installation/android-emulator/troubleshooting.md)

Guide pratique pour diagnostiquer et corriger les problèmes liés à Android Device Manager qui peuvent se produire quand vous configurez l’émulateur Android.


Après avoir configuré l’émulateur Android, consultez [Débogage avec l’émulateur Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md) pour plus d’informations sur le lancement de l’émulateur et son utilisation afin de tester et déboguer votre application.


> [!NOTE]
> À compter de la version **26.0.1** d’Android SDK Tools, Google ne prend plus en charge les gestionnaires d’AVD/de SDK, au profit de ses nouveaux outils d’interface de ligne de commande (CLI). En raison de ce changement, les Gestionnaires d’appareils/de SDK Xamarin sont dorénavant utilisés à la place des Gestionnaires d’appareils/de SDK Google pour Android Tools 26.0.1 et versions ultérieures. Pour plus d’informations sur l’utilisation de Xamarin SDK Manager, consultez [Installation du kit Android SDK](~/android/get-started/installation/android-sdk.md).

