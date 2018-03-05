---
title: "Déboguer sur un appareil d’usure"
description: "Cet article explique comment déboguer une application de Xamarin.Android usure sur un périphérique d’usure."
ms.topic: article
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 5ec627fad1695bab8d05d75a5089fe849ea2fd75
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="debug-on-a-wear-device"></a>Déboguer sur un appareil d’usure

_Cet article explique comment déboguer une application de Xamarin.Android usure sur un périphérique d’usure._


## <a name="overview"></a>Vue d'ensemble

Si vous avez un appareil Android d’usure comme un Smartwatch usure Android, vous pouvez exécuter l’application sur l’appareil au lieu d’utiliser un émulateur. (Si vous n’êtes pas encore familiarisé avec le processus de déploiement et l’exécution d’usure Android les applications, voir [usure Bonjour,](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Préparer l’usure du périphérique :

Utilisez les étapes suivantes pour activer le débogage sur l’appareil Android d’usure :

1.  Ouvrez le **paramètres** menu sur l’appareil Android d’usure.

2.  Faites défiler vers le bas du menu et appuyez sur **sur**.

3.  Cliquez sur le numéro de build 7 heures.

4.  Sur le **paramètres** menu, appuyez sur **Developer Options**.

5.  Vérifiez que **ADB débogage** est activé.


## <a name="debugging-over-usb"></a>Débogage en mode USB

Si votre appareil d’usure dispose d’un port USB, vous pouvez connecter l’appareil d’usure à votre ordinateur, déployer sur celle-ci et exécution/débogage l’application comme vous le feriez avec un téléphone Android (pour plus d’informations, consultez [déboguer sur un appareil](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Le débogage sur Bluetooth

Si votre appareil d’usure ne dispose pas d’un port USB, vous pouvez déployer l’application sur le périphérique d’usure par Bluetooth par le routage de la sortie du débogage de l’application sur un téléphone Android qui est connecté à votre ordinateur. 

### <a name="prepare-your-phone"></a>Préparer votre téléphone

Pour préparer votre téléphone pour l’établissement de connexions Bluetooth sur l’appareil usure, procédez comme suit : 

1.  Si vous ne le n'avez pas déjà fait, configurez les votre téléphone pour le développement de Xamarin.Android comme expliqué dans [définir appareil pour le développement](~/android/get-started/installation/set-up-device-for-development.md).

2.  Téléchargez et installez la version gratuite [usure Android](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) application à partir de Google Play Store.

### <a name="connect-the-device"></a>Connectez l’appareil

Utilisez les étapes suivantes pour connecter votre appareil d’usure à votre téléphone :

1.  Sur le téléphone qui sera agir en tant que Bluetooth intermédiaire (configuré ci-dessus), démarrez l’application d’usure Android. 

2.  Appuyez sur la **paramètres** icône.

3.  Activer **le débogage sur Bluetooth**. Vous devez voir l’état suivant affiché sur l’écran de l’application d’usure Android :

        Host: disconnected
        Target: connected

4.  Connectez le téléphone à votre ordinateur en mode USB. Sur votre ordinateur, entrez les commandes suivantes :

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Si le port 4444 n’est pas disponible, vous pouvez utiliser n’importe quel autre port disponible auquel vous avez accès. 

    **Remarque**: Si vous redémarrez Visual Studio ou Visual Studio pour Mac, vous devez exécuter ces commandes pour configurer une connexion à l’appareil d’usure.

5.  Lorsque l’appareil usure vous y invite, confirmez que vous autorisez **ADB débogage**. Dans l’application d’usure Android, vous devez voir l’état à modifier à :

        Host: connected
        Target: connected

6.  Après avoir effectué les étapes ci-dessus, en cours d’exécution `adb devices` indique l’état de téléphone et l’appareil Android d’usure :

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

À ce stade, vous pouvez déployer votre application à l’appareil d’usure.

<a name="screenshots"/>

### <a name="taking-screenshots"></a>Effectuer des captures d’écran

Vous pouvez effectuer une capture d’écran de l’appareil d’usure en entrant la commande suivante : 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Copier la capture d’écran sur votre ordinateur en entrant la commande suivante :

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Supprimer la capture d’écran sur l’appareil en entrant la commande suivante :

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Désinstaller une application

Vous pouvez désinstaller une application à partir de l’appareil d’usure en entrant la commande suivante :

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Par exemple, pour supprimer l’application avec le nom du package `com.xamarin.weartest`, entrez la commande suivante :

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Pour plus d’informations sur le débogage des appareils Android d’usure par Bluetooth, consultez [le débogage sur Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Débogage d’une application d’usure avec une application de téléphone mobile

Les applications Android usure sont empaquetées avec une application de téléphone Android d’accompagnement pour la distribution sur Google Play (pour plus d’informations, consultez [fonctionne avec l’empaquetage](~/android/wear/deploy-test/packaging.md)). Toutefois, vous toujours développez l’application d’usure et son application Compagnon séparément. Quand vous publiez votre application via le Google Play Store, l’application d’usure sera être empaquetée avec l’application auxiliaire et automatiquement installée si possible.

Pour déboguer l’application d’usure avec une application auxiliaire : 

1.  Générez et déployez l’application auxiliaire au numéro de téléphone.

2.  Cliquez sur le projet d’usure et définissez-le comme projet de démarrage par défaut.

3.  Déployer le projet usure sur l’appareil portable.

4.  Exécuter et déboguer l’application d’usure sur l’appareil.

 
## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment configurer un appareil Android d’usure pour le débogage d’usure à partir de Visual Studio via Bluetooth et explique comment déboguer une application d’usure avec une application de téléphone mobile. Il également des conseils de débogage courants pour le débogage d’une application d’usure Bluetooth.
