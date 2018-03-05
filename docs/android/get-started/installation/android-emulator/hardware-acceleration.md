---
title: "Accélération matérielle de l’émulateur Android"
description: "Guide pratique pour préparer votre ordinateur afin d’optimiser les performances de l’émulateur du kit Android SDK"
ms.topic: article
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: 53dc85cab94bdf692e088d7c6eea6916d283ba84
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Accélération matérielle de l’émulateur Android

Dans la mesure où l’émulateur du kit Android SDK est extrêmement lent sans accélération matérielle, il est vivement recommandé d’utiliser le Gestionnaire d'exécution avec accélération matérielle (HAXML) d’Intel pour améliorer de façon significative les performances de l’émulateur du kit Android SDK.

<a name="haxm-overview" />

## <a name="haxm-overview"></a>Vue d’ensemble d’HAXM

HAXM est un moteur d’assistance matérielle à la virtualisation (hyperviseur) qui utilise la technologie de virtualisation (VT, Virtualization Technology) Intel pour accélérer l’émulation des applications Android sur un ordinateur hôte. Conjugué aux images de l’émulateur x86 Android fournies par Intel et au Gestionnaire Android SDK officiel, le Gestionnaire HAXM accélère l’émulation Android sur les systèmes Intel VT. Si vous développez sur un ordinateur équipé d’un processeur Intel prenant en charge VT, vous pouvez tirer parti du Gestionnaire HAXM pour accélérer considérablement l’émulateur du kit Android SDK (si vous ne savez pas si votre processeur prend en charge VT, consultez [Déterminer si votre processeur prend en charge la technologie de virtualisation Intel](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

L’émulateur du kit Android SDK utilise automatiquement HAXM lorsque celui-ci est disponible. Lorsque vous sélectionnez un appareil virtuel **x86** (comme décrit dans [Configuration et utilisation](~/android/deploy-test/debugging/android-sdk-emulator/index.md)), cet appareil utilise HAXM pour l’accélération matérielle. Avant d’utiliser l’émulateur du kit Android SDK pour la première fois, il est judicieux de vérifier qu’HAXM est installé et disponible pour l’émulateur.

> [!NOTE]
> **Remarque :** Vous ne pouvez pas exécuter HAXM sur une machine virtuelle.

<a name="verify-haxm" />

## <a name="verifying-haxm-installation"></a>Vérification de l'installation d’HAXM

Vous pouvez vérifier qu’HAXM est disponible dans la fenêtre **Démarrage de l’émulateur Android** qui s’affiche au démarrage de l’émulateur. Pour démarrer l’émulateur du kit Android SDK, effectuez les étapes suivantes :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez le Gestionnaire d’émulateur Android en cliquant sur **Outils > Android > Gestionnaire d’émulateur Android** :

    [![Emplacement de l’élément de menu Gestionnaire d’émulateur Android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png)

2. Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, HAXM n’est pas encore installé ou configuré correctement sur votre ordinateur :

    ![Boîte de dialogue Avertissement de performance indiquant qu’HAXM n’est pas prêt](hardware-acceleration-images/win/11-perf-warn.png)

   Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, consultez [Avertissements de performance](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) pour identifier la cause et résoudre le problème sous-jacent.

3. Sélectionnez une image **x86** (par exemple, **VisualStudio\_android-23\_x86\_phone**), cliquez sur **Démarrer**, puis cliquez sur **Lancer** :

    ![Démarrage de l’émulateur du kit SDK Android avec une image d’appareil virtuel par défaut](hardware-acceleration-images/win/02-start-default-avd.png)

4. Observez la boîte de dialogue **Démarrage de l’Émulateur Android** pendant le démarrage de l’émulateur. Si HAXM est installé, le message **HAX fonctionne et l’émulateur s’exécute en mode virtuel rapide** s’affiche, comme illustré dans cette capture d’écran :

    ![HAXM est indiqué comme étant disponible dans la boîte de dialogue Démarrage de l’Émulateur Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Si ce message ne s’affiche pas, HAXM n’est probablement pas installé. Par exemple, voici une capture d’écran d’un message qui peut s’afficher si HAXM n’est pas disponible :

    ![HAXM n’est pas disponible sur cette machine](hardware-acceleration-images/win/04-haxm-error.png)

   Si HAXM n’est pas disponible sur votre ordinateur, utilisez la procédure de la section suivante pour l’installer.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez le Gestionnaire d’émulateur Android en cliquant sur **Outils > Gestionnaire d’émulateur Google** :

    [![Emplacement de l’élément de menu Gestionnaire d’émulateur Android](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png)

2. Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, HAXM n’est pas encore installé ou configuré correctement sur votre ordinateur :

    ![Boîte de dialogue Avertissement de performance indiquant qu’HAXM n’est pas prêt](hardware-acceleration-images/mac/04-avd-warning.png)

   Si une boîte de dialogue **Avertissement de performance** similaire à la suivante s’affiche, consultez [Avertissements de performance](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) pour identifier la cause et résoudre le problème sous-jacent.

3. Sélectionnez l’image **x86** (par exemple, **Android\_Accelerated\_x86**), cliquez sur **Démarrer**, puis cliquez sur **Lancer** :

    [![Démarrage de l’émulateur du kit SDK Android avec une image d’appareil virtuel par défaut](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png)

3. Observez la boîte de dialogue **Démarrage de l’Émulateur Android** pendant le démarrage de l’émulateur. Si HAXM est installé, le message **HAX fonctionne et l’émulateur s’exécute en mode virtuel rapide** s’affiche, comme illustré dans cette capture d’écran :

    ![HAXM est indiqué comme étant disponible dans la boîte de dialogue Démarrage de l’Émulateur Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Si HAXM n’est pas disponible sur votre ordinateur (par exemple, si un message d’erreur comme _Vérifiez qu’Intel HAXM est correctement installé et utilisable_ s’affiche), utilisez la procédure décrite dans la section suivante pour installer HAXM.


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>Installation d’HAXM

Si l’émulateur ne démarre pas, HAXM va vraisemblablement devoir être installé manuellement. Les paquets d’installation d’HAXM pour Windows et macOS sont disponibles sur la page [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager). Utilisez la procédure suivante pour télécharger et installer manuellement HAXM :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Depuis le site web Intel, téléchargez la dernière version du programme d’installation du [moteur de virtualisation HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) pour Windows. En téléchargeant le programme d’installation d’HAXM directement depuis le site web Intel, vous avez l’assurance d’utiliser la version la plus récente.

   Vous pouvez également utiliser le Gestionnaire du kit SDK pour télécharger le programme d’installation d’HAXM (dans le Gestionnaire du kit SDK, cliquez sur **Outils > Bonus > Intel x86 Emulator Accelerator (HAXM installer)**). Le kit Android SDK télécharge généralement le programme d’installation d’HAXM à l’emplacement suivant :

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   Notez que le Gestionnaire du kit SDK n’installe pas HAXM, il télécharge simplement le programme d’installation d’HAXM à l’emplacement ci-dessus. Vous devez encore le lancer manuellement.

2. Exécutez **intelhaxm-android.exe** pour démarrer le programme d’installation d’HAXM. Acceptez les valeurs par défaut dans les boîtes de dialogue du programme d’installation :

   ![Fenêtre Intel Hardware Accelerated Execution Manager (HAXM)](hardware-acceleration-images/win/05-haxm-installer.png)

Si la boîte de dialogue d’erreur suivante s’affiche (_Cet ordinateur ne prend pas en charge la technologie de virtualisation Intel (VT-x) ou il est utilisé exclusivement par Hyper-V_), Hyper-V doit être désactivé avant de pouvoir installer HAXM :

![HAXM ne peut pas être installé en raison d’un conflit Hyper-V](hardware-acceleration-images/win/06-cant-install-haxm.png)

La section suivante explique comment désactiver Hyper-V.

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Désactivation d'Hyper-V

Si vous utilisez Windows avec Hyper-V activé, vous devez le désactiver et redémarrer votre ordinateur pour installer et utiliser HAXM. Vous pouvez désactiver Hyper-V à partir du Panneau de configuration en effectuant les étapes suivantes :

1. Dans la zone de recherche Windows, entrez **Programmes et**, puis cliquez sur **Programmes et fonctionnalités** dans les résultats de la recherche.

2. Dans la boîte de dialogue **Programmes et fonctionnalités** du Panneau de configuration, cliquez sur **Activer ou désactiver des fonctionnalités Windows** :

    ![Activer ou désactiver des fonctionnalités Windows](hardware-acceleration-images/win/07-turn-windows-features.png)

3. Décochez **Hyper-V** et redémarrez l’ordinateur :

    ![Désactivation d’Hyper-V dans la boîte de dialogue Fonctionnalités de Windows](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

Vous pouvez également utiliser l’applet de commande Powershell suivante pour désactiver Hyper-V :

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM et Microsoft Hyper-V ne peuvent pas être actifs en même temps. Malheureusement, il n’existe à ce jour aucun moyen de basculer entre Hyper-V et HAXM sans avoir à redémarrer votre ordinateur. Si vous souhaitez utiliser l’[émulateur Visual Studio pour Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (qui dépend d’Hyper-V), vous ne pouvez pas utiliser l’émulateur du kit Android SDK sans redémarrer l’ordinateur. Il est possible d’utiliser à la fois Hyper-V et HAXM en créant une configuration de démarrage multiple comme expliqué dans [Création d’une entrée de démarrage « sans hyperviseur »](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

Dans certains cas, la procédure ci-dessus ne permet pas de désactiver Hyper-V si Device Guard et Credential Guard sont activés. Si vous ne parvenez pas à désactiver Hyper-V (ou s’il semble être désactivé mais que l’installation d’HAXM échoue toujours), utilisez la procédure de la section suivante pour désactiver Device Guard et Credential Guard.

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Désactivation de Device Guard

Device Guard et Credential Guard peuvent empêcher la désactivation d’Hyper-V sur les ordinateurs Windows. Ce problème apparaît généralement sur les ordinateurs joints au domaine qui sont configurés et contrôlés par une organisation propriétaire.
Sur Windows 10, effectuez les étapes suivantes pour savoir si **Device Guard** est en cours d’exécution :

1. Dans la **zone de recherche Windows**, tapez **Informations système** pour démarrer l’application **Informations système**.

2. Dans **Résumé système**, vérifiez si **Sécurité basée sur la virtualisation de Device Guard** est présent et **En cours d’exécution** :

   [![Device Guard est présent et en cours d’exécution](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png)

Si Device Guard est activé, effectuez les étapes suivantes pour le désactiver :

1. Vérifiez qu’**Hyper-V** est désactivé (sous **Activer ou désactiver des fonctionnalités Windows**) comme décrit dans la section précédente.

2. Dans la zone de recherche Windows, tapez **gpedit** et sélectionnez **Modifier la stratégie de groupe** dans les résultats de la recherche. Cette opération lance l’**Éditeur de stratégie de groupe locale**.

3. Dans l’**Éditeur de stratégie de groupe locale**, accédez à **Configuration ordinateur > Modèles d’administration > Système > Device Guard** :

   [![Device Guard dans l’Éditeur de stratégie de groupe locale](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png)

4. Modifiez **Activer la sécurité basée sur la virtualisation** en le passant à **Désactivé** (comme illustré ci-dessus) et quittez l’**Éditeur de stratégie de groupe locale**.

5. Dans la zone de recherche Windows, tapez **cmd**. Cliquez avec le bouton droit sur **Invite de commandes** dans les résultats de la recherche et sélectionnez **Exécuter en tant qu’administrateur**.

6. Copiez et collez les commandes suivantes dans la fenêtre d’invite de commandes (si lecteur **Z:** est utilisé, choisissez une lettre de lecteur non utilisée) :

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Redémarrez votre ordinateur. Dans l’écran de démarrage, une invite similaire à la suivante doit s’afficher :

   **Voulez-vous désactiver Credential Guard ?**

   Appuyez sur la touche indiquée pour désactiver Credential Guard, comme il vous est demandé.

8. Une fois que l’ordinateur a redémarré, vérifiez de nouveau qu’Hyper-V est désactivé (comme décrit dans les étapes précédentes).

Si Hyper-V n’est toujours pas désactivé, les stratégies de votre ordinateur joint au domaine peuvent vous empêcher de désactiver Device Guard ou Credential Guard. Dans ce cas, vous pouvez demander une dérogation à votre administrateur de domaine afin que vous soyez autorisé à ne pas utiliser Credential Guard. Ou bien, vous pouvez utiliser un ordinateur qui n’est pas joint au domaine pour utiliser HAXM.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Depuis le site web Intel, téléchargez la dernière version du programme d’installation du [moteur de virtualisation HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) pour macOS.

2. Exécutez le programme d’installation de HAXM. Acceptez les valeurs par défaut dans les boîtes de dialogue du programme d’installation :

   [![Fenêtre Intel Hardware Accelerated Execution Manager Setup](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png)

-----
