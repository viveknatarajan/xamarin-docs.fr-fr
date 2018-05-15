---
title: Accélération matérielle de l’émulateur Android
description: Comment préparer votre ordinateur pour optimiser les performances de l’émulateur Google Android
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/10/2018
ms.openlocfilehash: b5c20eb9f40bb4c4981d6b60b9fd4bc75fd29336
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Accélération matérielle de l’émulateur Android

L’émulateur Google Android est extrêmement lent sans accélération matérielle. Vous pouvez considérablement améliorer les performances de l’émulateur Google Android en utilisant des images de matériel d’émulateur spéciales qui ciblent le matériel x86 et l’une des deux technologies de virtualisation :

1. **Microsoft Hyper-V et la plateforme d’hyperviseur** : Hyper-V est un composant de virtualisation disponible sur Windows 10 qui permet d’exécuter des systèmes informatiques virtualisés sur un hôte physique. Il s’agit de la technologie de virtualisation recommandée pour les images d’émulateur Google Android accélérées. Pour plus d’informations sur Hyper-V, consultez le [Guide d’Hyper-V sur Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).
2. **Intel HAXM (Hardware Accelerated Execution Manager)** : Il s’agit d’un moteur de virtualisation pour les ordinateurs exécutant des processeurs Intel. C’est le moteur de virtualisation recommandé pour les développeurs qui ne peuvent pas utiliser Hyper-V.

Android SDK Manager utilise automatiquement l’accélération matérielle quand elle est disponible. Il exécute une image d’émulateur spécifique pour un appareil virtuel **x86** (comme décrit dans [Configuration et utilisation ](~/android/deploy-test/debugging/android-sdk-emulator/index.md)).

## <a name="hyper-v-overview"></a>Vue d’ensemble d’Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> La prise en charge d’Hyper-V est actuellement en préversion.

Les développeurs qui utilisent Windows 10 (mise à jour d’avril 2018) sont vivement encouragés à utiliser Microsoft Hyper-V. Visual Studio Tools pour Xamarin permet aux développeurs de facilement tester et déboguer leurs applications Xamarin.Android quand un appareil Android n’est pas disponible ou n’est pas pratique à utiliser.

Pour commencer à utiliser Hyper-V et l’émulateur Google Android :

1. **Installer la mise à jour d’avril 2018 de Windows 10 (build 1803)** : Pour vérifier la version de Windows en cours d’exécution, cliquez dans la barre de recherche de Cortana et tapez **À propos de**. Sélectionnez **À propos de votre PC** dans les résultats de recherche. Faites défiler vers le bas la boîte de dialogue **À propos de** jusqu’à la section **Spécifications Windows**. La **Version** doit être au moins égale à 1803 :

    [![Spécifications Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Activer Hyper-V et la plateforme de l’hyperviseur Windows** : Dans la barre de recherche de Cortana, tapez **Activer ou désactiver des fonctionnalités Windows**.
   Faites défiler vers le bas la boîte de dialogue **Fonctionnalités Windows** et vérifiez que la **Plateforme de l’hyperviseur Windows** est activée.

    [![Hyper-V et plateforme de l’hyperviseur Windows activés](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Vous devez peut-être redémarrer Windows après l’activation d’Hyper-V et de la plateforme de l’hyperviseur Windows.

3. **Installer [Visual Studio 15.8 Preview 1](https://aka.ms/hyperv-emulator-dl)**  : Cette version de Visual Studio prend en charge l’IDE pour démarrer l’émulateur Google Android avec la prise en charge Hyper-V.

4. **Installer le package de l’émulateur Google Android 27.2.7 ou version ultérieure** : Pour installer ce package, accédez à **Outils > Android > Android SDK Manager** dans Visual Studio. Sélectionnez l’onglet **Outils** et vérifiez que la version du composant de l’émulateur Android est au moins égale à 27.2.7.

    [![Boîte de dialogue des kits Android SDK et outils](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Si la version de l’émulateur Android est inférieure à 27.3.1, effectuez l’étape de solution de contournement expliquée dans **Problèmes connus** (suivant).


### <a name="known-issues"></a>Problèmes connus

-   Si la version de l’émulateur est au moins égale à 27.2.7, mais inférieure à 27.3.1, la solution de contournement suivante est nécessaire pour utiliser Hyper-V :
    1.  Dans le dossier **C:\\Utilisateurs\\_nom_utilisateur_\\.android**, créez un fichier appelé **advancedFeatures.ini** s’il n’existe pas déjà.
    2.  Ajoutez la ligne suivante à **advancedFeatures.ini** :
        ```
        WindowsHypervisorPlatform = on
        ```

-   Les performances peuvent être réduites avec certains processeurs Intel et AMD.

-   Le temps de chargement de l’application Android peut être anormalement long pendant le déploiement.

-   Une erreur d’accès MMIO risque d’empêcher par intermittence le démarrage de l’émulateur Android. Un redémarrage de l’émulateur résout généralement le problème.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

La prise en charge d’Hyper-V nécessite Windows 10. Consultez les [exigences Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) pour plus d’informations.

-----

## <a name="haxm-overview"></a>Vue d’ensemble d’HAXM

HAXM est un moteur d’assistance matérielle à la virtualisation (hyperviseur) qui utilise la technologie de virtualisation (VT, Virtualization Technology) Intel pour accélérer l’émulation des applications Android sur un ordinateur hôte. Conjugué aux images de l’émulateur x86 Android fournies par Intel et au Gestionnaire Android SDK officiel, le Gestionnaire HAXM accélère l’émulation Android sur les systèmes Intel VT. 

Si vous développez sur un ordinateur équipé d’un processeur Intel prenant en charge VT, vous pouvez tirer parti de HAXM pour accélérer considérablement l’émulateur Google Android (si vous ne savez pas si votre processeur prend en charge VT, consultez [Déterminer si votre processeur prend en charge la technologie de virtualisation Intel](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Vous ne pouvez pas exécuter un émulateur accéléré par machine virtuelle dans une autre machine virtuelle telle qu’une machine virtuelle hébergée par VirtualBox, VMWare ou Docker. Vous devez exécuter l’émulateur Google Android [directement sur votre matériel système](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Avant d’utiliser l’émulateur Google Android pour la première fois, vérifiez que HAXM est installé et disponible pour l’émulateur Google Android.

### <a name="verifying-haxm-installation"></a>Vérification de l'installation d’HAXM

Vous pouvez vérifier qu’HAXM est disponible dans la fenêtre **Démarrage de l’émulateur Android** qui s’affiche au démarrage de l’émulateur. Pour démarrer l’émulateur Google Android, effectuez les étapes suivantes :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez le Gestionnaire d’émulateur Android en cliquant sur **Outils > Android > Gestionnaire d’émulateur Android** :

    [![Emplacement de l’élément de menu Gestionnaire d’émulateur Android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, HAXM n’est pas encore installé ou configuré correctement sur votre ordinateur :

    ![Boîte de dialogue Avertissement de performance indiquant qu’HAXM n’est pas prêt](hardware-acceleration-images/win/11-perf-warn.png)

   Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, consultez [Avertissements de performance](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) pour identifier la cause et résoudre le problème sous-jacent.

3. Sélectionnez une image **x86** (par exemple, **VisualStudio\_android-23\_x86\_phone**), cliquez sur **Démarrer**, puis cliquez sur **Lancer** :

    ![Démarrage de l’émulateur Google Android avec une image d’appareil virtuel par défaut](hardware-acceleration-images/win/02-start-default-avd.png)

4. Observez la boîte de dialogue **Démarrage de l’Émulateur Android** pendant le démarrage de l’émulateur. Si HAXM est installé, le message **HAX fonctionne et l’émulateur s’exécute en mode virtuel rapide** s’affiche, comme illustré dans cette capture d’écran :

    ![HAXM est indiqué comme étant disponible dans la boîte de dialogue Démarrage de l’Émulateur Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Si ce message ne s’affiche pas, HAXM n’est probablement pas installé. Par exemple, voici une capture d’écran d’un message qui peut s’afficher si HAXM n’est pas disponible :

    ![HAXM n’est pas disponible sur cette machine](hardware-acceleration-images/win/04-haxm-error.png)

   Si HAXM n’est pas disponible sur votre ordinateur, utilisez la procédure de la section suivante pour l’installer.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez le Gestionnaire d’émulateur Android en cliquant sur **Outils > Gestionnaire d’émulateur Google** :

    [![Emplacement de l’élément de menu Gestionnaire d’émulateur Android](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, HAXM n’est pas encore installé ou configuré correctement sur votre ordinateur :

    ![Boîte de dialogue Avertissement de performance indiquant qu’HAXM n’est pas prêt](hardware-acceleration-images/mac/04-avd-warning.png)

   Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, consultez [Avertissements de performance](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) pour identifier la cause et résoudre le problème sous-jacent.

3. Sélectionnez l’image **x86** (par exemple, **Android\_Accelerated\_x86**), cliquez sur **Démarrer**, puis cliquez sur **Lancer** :

    [![Démarrage de l’émulateur Google Android avec une image d’appareil virtuel par défaut](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Observez la boîte de dialogue **Démarrage de l’Émulateur Android** pendant le démarrage de l’émulateur. Si HAXM est installé, le message **HAX fonctionne et l’émulateur s’exécute en mode virtuel rapide** s’affiche, comme illustré dans cette capture d’écran :

    ![HAXM est indiqué comme étant disponible dans la boîte de dialogue Démarrage de l’Émulateur Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Si HAXM n’est pas disponible sur votre ordinateur (par exemple, si un message d’erreur comme _Vérifiez qu’Intel HAXM est correctement installé et utilisable_ s’affiche), utilisez la procédure décrite dans la section suivante pour installer HAXM.


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Installation d’HAXM

Si l’émulateur ne démarre pas, HAXM va vraisemblablement devoir être installé manuellement. Les paquets d’installation d’HAXM pour Windows et macOS sont disponibles sur la page [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager). Utilisez la procédure suivante pour télécharger et installer manuellement HAXM :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Depuis le site web Intel, téléchargez la dernière version du programme d’installation du [moteur de virtualisation HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) pour Windows. En téléchargeant le programme d’installation d’HAXM directement depuis le site web Intel, vous avez l’assurance d’utiliser la version la plus récente.

   Vous pouvez également utiliser le Gestionnaire du kit SDK pour télécharger le programme d’installation d’HAXM (dans le Gestionnaire du kit SDK, cliquez sur **Outils > Bonus > Intel x86 Emulator Accelerator (HAXM installer)**). Le kit Android SDK télécharge généralement le programme d’installation d’HAXM à l’emplacement suivant :

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   Notez que le Gestionnaire du kit SDK n’installe pas HAXM, il télécharge simplement le programme d’installation d’HAXM à l’emplacement ci-dessus. Vous devez encore le lancer manuellement.

2. Exécutez **intelhaxm-android.exe** pour démarrer le programme d’installation d’HAXM. Acceptez les valeurs par défaut dans les boîtes de dialogue du programme d’installation :

   ![Fenêtre Intel Hardware Accelerated Execution Manager (HAXM)](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Accélération matérielle et processeurs AMD

Étant donné que l’émulateur Android de Google prend actuellement en charge l’accélération matérielle AMD [uniquement sur Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), l’accélération matérielle n’est pas disponible pour les ordinateurs AMD exécutant Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Depuis le site web Intel, téléchargez la dernière version du programme d’installation du [moteur de virtualisation HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) pour macOS.

2. Exécutez le programme d’installation de HAXM. Acceptez les valeurs par défaut dans les boîtes de dialogue du programme d’installation :

   [![Fenêtre Intel Hardware Accelerated Execution Manager Setup](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Liens associés

* [Exécuter des applications sur l’émulateur Android](https://developer.android.com/studio/run/emulator)
