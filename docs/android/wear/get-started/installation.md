---
title: Installation et configuration
description: Cet article assiste les étapes d’installation et les détails de configuration requises pour préparer votre ordinateur et les périphériques pour le développement d’usure Android. À la fin de cet article, vous devez posséder un Xamarin.Android usure installation intégrée dans Visual Studio pour Mac et/ou de Microsoft Visual Studio, et vous serez prêt à commencer à créer votre première application Xamarin.Android usure.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 86ed62075f5c39eaf7b0e348020f4a9361764727
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="setup-and-installation"></a>Installation et configuration

_Cet article assiste les étapes d’installation et les détails de configuration requises pour préparer votre ordinateur et les périphériques pour le développement d’usure Android. À la fin de cet article, vous devez posséder un Xamarin.Android usure installation intégrée dans Visual Studio pour Mac et/ou de Microsoft Visual Studio, et vous serez prêt à commencer à créer votre première application Xamarin.Android usure._

## <a name="requirements"></a>Spécifications

Les éléments suivants requis pour créer des applications en fonction de Xamarin d’usure Android :

-   **Visual Studio ou Visual Studio pour Mac** &ndash; vous si vous utilisez Visual Studio, Visual Studio Professional 2015 ou version ultérieure êtes requis.

-   **Xamarin.Android** &ndash; Xamarin.Android 4,17 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac.

-   **Kit de développement logiciel Android** -Kit de développement logiciel Android 5.0.1 (API 21) ou version ultérieure doit être installé via le Gestionnaire de kit de développement logiciel Android.

-   **Kit de développement Java** &ndash; le développement Xamarin Android nécessite [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) si vous êtes le développement pour le niveau de l’API 24 ou supérieure (JDK 1.8 prend également en charge les niveaux d’API antérieures à 24).

Vous pouvez continuer à utiliser [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) si vous êtes le développement en particulier pour les API de niveau 23 ou une version antérieure.

> [!IMPORTANT]
> Xamarin.Android ne prend pas en charge JDK 9.

## <a name="installation"></a>Installation

Après avoir installé Xamarin.Android, procédez comme suit afin que vous êtes prêt à générer et tester des applications d’usure Android : 

1.  Installez les outils et Kit de développement logiciel Android requis.
2.  Configurer un appareil de test.
3.  Créer votre première application usure Android.

Ces étapes sont décrites dans les sections suivantes.


### <a name="install-android-sdk-and-tools"></a>Installer les outils et Kit de développement logiciel Android 

Lancer le **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Comment lancer le Gestionnaire de SDK Android dans Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Comment lancer le Gestionnaire de SDK Android dans Visual Studio pour Mac](installation-images/xs/sdk-menu.png)

-----


Vérifiez que le Kit de développement logiciel Android suivant et des outils :

* V outils du Kit de développement logiciel Android 24.0.0 ou une version ultérieure, et
* Android 4.4W (API20), ou
* Android 5.0.1 (API21) ou une version ultérieure.

Si vous n’avez pas le Kit de développement logiciel et des outils plus récentes, téléchargez les outils du Kit de développement logiciel requis *et* les bits de l’API (vous devrez peut-être faire défiler un bit pour rechercher les &ndash; la sélection de l’API est indiquée ci-dessous) : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Capture d’écran de l’exemple SDK Manager de l’activation 5.0.1 Android composants](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Capture d’écran de l’exemple de kit de développement logiciel gestionnaire d’activation Android 4.4 et 5.0.1 composants](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Configuration

Avant de pouvoir utiliser votre application de test, vous devez configurer un émulateur d’usure Android ou un appareil Android d’usure réel. 


### <a name="android-wear-emulator"></a>Émulateur d’usure Android

Avant de pouvoir utiliser un émulateur d’usure Android, vous devez configurer un Android usure Android Virtual Device (AVD) à l’aide de la **Gestionnaire de l’émulateur Google**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Comment lancer le Gestionnaire d’émulateur Android à partir de Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Comment lancer le Gestionnaire d’émulateur Android à partir de Visual Studio pour Mac](installation-images/xs/emulator-menu.png)

-----

Pour plus d’informations sur la configuration d’un émulateur d’usure Android, consultez [déboguer Android porter sur un émulateur](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Appareil d’usure Android

Si vous avez un appareil Android d’usure comme un Smartwatch usure Android, vous pouvez déboguer l’application sur cet appareil au lieu d’utiliser un émulateur. Pour plus d’informations sur le développement avec un périphérique d’usure, consultez [déboguer sur un appareil usure](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>Créer votre première application Android usure

Suivez les [Bonjour, l’usure](~/android/wear/get-started/hello-wear.md) instructions pour créer votre première application de surveillance.


## <a name="packaging-your-app"></a>Empaquetage de votre application

Les applications Android usure sont toujours distribuées avec une application de téléphone Android d’accompagnement. 

Lorsque vous ajoutez à votre application d’usure Android en tant que référence à votre application Android principale, il est automatiquement considéré comme un projet Android d’usure et génère tous les XML et les métadonnées nécessaires pour vous. En outre, il vérifie que les numéros de version et package correspondent donc vous pouvez facilement expédier vos applications vers Google Play. 

Pour plus d’informations sur l’empaquetage d’applications d’usure, consultez [fonctionne avec l’empaquetage](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Liens associés

- [SkeletonWear (exemple)](https://developer.xamarin.com/samples/SkeletonWear/)
