---
title: Accélération matérielle pour les performances de l’émulateur
description: Cet article explique comment utiliser les fonctionnalités d’accélération matérielle de votre ordinateur pour optimiser les performances de l’émulateur Android.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: c2bef2c614d4cc0655deb9732ccefec223a8318a
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848465"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Accélération matérielle pour les performances de l’émulateur

_Cet article explique comment utiliser les fonctionnalités d’accélération matérielle de votre ordinateur pour optimiser les performances de l’émulateur Android._

## <a name="overview"></a>Vue d'ensemble

Visual Studio permet aux développeurs de facilement tester et déboguer leurs applications Xamarin.Android à l’aide de l’émulateur Android quand un appareil Android n’est pas disponible ou n’est pas pratique à utiliser.
Toutefois, l’émulateur Android s’exécute trop lentement si l’accélération matérielle n’est pas disponible sur l’ordinateur qui l’exécute. Vous pouvez considérablement améliorer les performances de l’émulateur Android en utilisant des images d’appareils virtuels spéciales qui ciblent le matériel x86 conjointement avec l’une des deux technologies de virtualisation :

1. **Microsoft Hyper-V et la Plateforme de l’hyperviseur**. Hyper-V est une fonctionnalité de virtualisation de Windows qui permet d’exécuter des systèmes informatiques virtualisés sur un ordinateur hôte physique. Il s’agit de la technologie de virtualisation recommandée pour accélérer l’émulateur Android. Pour plus d’informations sur Hyper-V, consultez [Hyper-V sur Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Intel Hardware Accelerated Execution Manager (HAXM)**. 
   HAXM est un moteur de virtualisation pour les ordinateurs exécutant des processeurs Intel.
   Il s’agit du moteur de virtualisation recommandé pour les développeurs qui ne peuvent pas exécuter Hyper-V.

L’émulateur Android utilise automatiquement l’accélération matérielle si les critères suivants sont satisfaits :

-   L’accélération matérielle est disponible et activée sur l’ordinateur de développement.

-   L’émulateur exécute une image d’émulateur créée spécifiquement pour un appareil virtuel **x86**.

Pour plus d’informations sur le lancement et le débogage avec l’émulateur Android, consultez [Débogage sur l’émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> La prise en charge d’Hyper-V est actuellement en préversion.

Nous encourageons vivement les développeurs qui utilisent Windows 10 (mise à jour d’avril 2018 ou version ultérieure) à utiliser Microsoft Hyper-V afin d’accélérer l’émulateur Android. Pour utiliser l’émulateur Android avec Hyper-V :

1. **Appliquez la mise à jour d’avril 2018 de Windows 10 (build 1803) ou ultérieur**.
   Pour vérifier la version de Windows en cours d’exécution, cliquez dans la barre de recherche de Cortana et tapez **À propos de**. Sélectionnez **À propos de votre PC** dans les résultats de recherche. Faites défiler vers le bas la boîte de dialogue **À propos de** jusqu’à la section **Spécifications Windows**. La **Version** doit être au moins égale à 1803 :

    [![Spécifications Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Activez la Plateforme de l’hyperviseur Windows**.
   Dans la barre de recherche de Cortana, tapez **Activer ou désactiver des fonctionnalités Windows**.
   Faites défiler vers le bas la boîte de dialogue **Fonctionnalités Windows** et vérifiez que la **Plateforme de l’hyperviseur Windows** est activée.

    [![Plateforme de l’hyperviseur Windows activée](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   L’activation de l’option **Plateforme de l’hyperviseur Windows** active automatiquement Hyper-V. Il est préférable de redémarrer Windows après avoir apporté cette modification.

3. **Installez [Visual Studio 15.8 Preview 1 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/)**.
   Cette version de Visual Studio prend en charge l’IDE pour l’exécution de l’émulateur Android avec Hyper-V.
 
4. **Installez le package de l’émulateur Android 27.2.7 ou ultérieur**. Pour installer ce package, accédez à **Outils > Android > Android SDK Manager** dans Visual Studio. Sélectionnez l’onglet **Outils** et vérifiez que la version de l’émulateur Android est au moins égale à 27.2.7. Vérifiez également que la version d’Android SDK Tools est 26.1.1 ou ultérieure :

    [![Boîte de dialogue des kits Android SDK et outils](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Si la version de l’émulateur est au moins égale à 27.2.7, mais inférieure à 27.3.1, la solution de contournement suivante est nécessaire pour utiliser Hyper-V :

    1.  Dans le dossier **C:\\Utilisateurs\\_nom_utilisateur_\\.android**, créez un fichier nommé **advancedFeatures.ini** (s’il n’existe pas).

    2.  Ajoutez la ligne suivante à **advancedFeatures.ini** :
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Problèmes connus

-   Si vous ne parvenez pas à effectuer la mise à jour vers la version 27.2.7 ou ultérieure de l’émulateur après avoir mis à jour vers une préversion de Visual Studio, vous devrez peut-être installer directement le [programme d’installation de préversion](http://aka.ms/hyperv-emulator-dl) pour activer les nouvelles versions de l’émulateur.

-   Les performances peuvent être réduites avec certains processeurs Intel et AMD.

-   Le temps de chargement de l’application Android peut être anormalement long pendant le déploiement.

-   Une erreur d’accès MMIO risque d’empêcher par intermittence le démarrage de l’émulateur Android. Un redémarrage de l’émulateur résout généralement le problème.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

La prise en charge d’Hyper-V nécessite Windows 10. Consultez les [exigences Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) pour plus d’informations.

-----

## <a name="haxm"></a>HAXM

HAXM est un moteur d’assistance matérielle à la virtualisation (hyperviseur) qui utilise la technologie de virtualisation (VT, Virtualization Technology) Intel pour accélérer l’émulation des applications Android sur un ordinateur hôte. Conjugué aux images de l’émulateur x86 Android fournies par Intel, HAXM accélère l’émulation Android sur les systèmes Intel VT.

Si vous développez sur un ordinateur équipé d’un processeur Intel prenant en charge VT, vous pouvez tirer parti de HAXM pour accélérer considérablement l’émulateur Android (si vous ne savez pas si votre processeur prend en charge VT, consultez [Mon processeur prend-il en charge la technologie de virtualisation Intel ?](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Vous ne pouvez pas exécuter un émulateur accéléré par machine virtuelle dans une autre machine virtuelle telle qu’une machine virtuelle hébergée par VirtualBox, VMWare ou Docker. Vous devez exécuter l’émulateur Android [directement sur votre matériel système](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Avant d’utiliser l’émulateur Android pour la première fois, vérifiez que HAXM est installé et disponible pour l’émulateur Android.

### <a name="verifying-haxm-installation"></a>Vérification de l'installation d’HAXM

Vous pouvez vérifier qu’HAXM est disponible dans la fenêtre **Démarrage de l’émulateur Android** qui s’affiche au démarrage de l’émulateur. Pour démarrer l’émulateur Android, effectuez les étapes suivantes :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Android Device Manager en cliquant sur **Outils > Android > Android Device Manager** :

    [![Emplacement de l’élément de menu Android Device Manager](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, HAXM n’est pas encore installé ou configuré correctement sur votre ordinateur :

    ![Boîte de dialogue Avertissement de performance indiquant qu’HAXM n’est pas prêt](hardware-acceleration-images/win/11-perf-warn.png)

   Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, consultez [Avertissements de performance](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) pour identifier la cause et résoudre le problème sous-jacent.

3. Sélectionnez une image **x86** (par exemple, **VisualStudio\_android-23\_x86\_phone**) et cliquez sur **Démarrer** :

    ![Démarrage de l’émulateur Android avec une image d’appareil virtuel par défaut](hardware-acceleration-images/win/02-start-default-avd.png)

4. Observez la boîte de dialogue **Démarrage de l’Émulateur Android** pendant le démarrage de l’émulateur. Si HAXM est installé, le message **HAX fonctionne et l’émulateur s’exécute en mode virtuel rapide** s’affiche, comme illustré dans cette capture d’écran :

    ![HAXM est indiqué comme étant disponible dans la boîte de dialogue Démarrage de l’Émulateur Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Si ce message ne s’affiche pas, HAXM n’est probablement pas installé. Par exemple, voici une capture d’écran d’un message qui peut s’afficher si HAXM n’est pas disponible :

    ![HAXM n’est pas disponible sur cette machine](hardware-acceleration-images/win/04-haxm-error.png)

   Si HAXM n’est pas disponible sur votre ordinateur, utilisez la procédure de la section suivante pour l’installer.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Android Device Manager en cliquant sur **Outils > Device Manager** :

    [![Emplacement de l’élément de menu Android Device Manager](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, HAXM n’est pas encore installé ou configuré correctement sur votre ordinateur :

    ![Boîte de dialogue Avertissement de performance indiquant qu’HAXM n’est pas prêt](hardware-acceleration-images/mac/04-avd-warning.png)

   Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, consultez [Avertissements de performance](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) pour identifier la cause et résoudre le problème sous-jacent.

3. Sélectionnez l’image **x86** (par exemple, **Android\_Accelerated\_x86**) et cliquez sur **Lire** :

    [![Démarrage de l’émulateur Android avec une image d’appareil virtuel par défaut](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

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

Étant donné que l’émulateur Android prend actuellement en charge l’accélération matérielle AMD [uniquement sur Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), l’accélération matérielle n’est pas disponible pour les ordinateurs AMD exécutant Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Depuis le site web Intel, téléchargez la dernière version du programme d’installation du [moteur de virtualisation HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) pour macOS.

2. Exécutez le programme d’installation de HAXM. Acceptez les valeurs par défaut dans les boîtes de dialogue du programme d’installation :

   [![Fenêtre Intel Hardware Accelerated Execution Manager Setup](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Liens associés

* [Exécuter des applications sur l’émulateur Android](https://developer.android.com/studio/run/emulator)
