---
title: Résolution des problèmes de l’émulateur Google Android
description: Comment identifier et résoudre les problèmes de l’émulateur Google Android
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/04/2018
ms.openlocfilehash: 001fc21a519a251715d24b43acfdd4251b5fbc91
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="google-android-emulator-troubleshooting"></a>Résolution des problèmes de l’émulateur Google Android

Cet article décrit les messages d’avertissement et les problèmes les plus courants liés à l’émulateur Google Android (et leurs solutions).
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>avertissements liés aux performances

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

À compter de Visual Studio 2017 version 15.4, une boîte de dialogue d’avertissement sur les performances peut s’afficher lorsque vous déployez votre application pour la première fois sur l’émulateur du kit Android SDK. Ces boîtes de dialogue d’avertissement sont expliquées ci-dessous.

### <a name="computer-does-not-contain-an-intel-procesor"></a>L’ordinateur ne contient pas de processeur Intel

![L’ordinateur ne contient pas de processeur Intel](troubleshooting-images/01-no-intel-processor.png)

Si cette boîte de dialogue s’affiche, votre ordinateur n’est pas équipé d’un processeur Intel, lequel est requis pour l’accélération de l’émulateur du kit Android SDK. Si votre ordinateur n’est pas équipé d’un processeur Intel, nous recommandons d’utiliser un appareil Android physique pour le développement.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V est installé ou actif

![Hyper-V est installé ou actif](troubleshooting-images/02-hyper-v-active.png)

Si cette boîte de dialogue s’affiche, Hyper-V est installé ou actif et doit être désactivé. La section [Désactivation d’Hyper-V](#disable-hyperv) explique comment résoudre ce problème.

### <a name="haxm-is-not-installed"></a>Le Gestionnaire HAXM n'est pas installé

![Le Gestionnaire HAXM n'est pas installé](troubleshooting-images/03-haxm-not-installed.png)

Cette boîte de dialogue indique que votre ordinateur est équipé d’un processeur Intel, qu’Hyper-V est désactivé, mais que le Gestionnaire HAXM n’est pas installé.
La section [Installation du Gestionnaire HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) décrit la procédure d’installation du Gestionnaire HAXM.

### <a name="haxm-process-not-running"></a>Le processus HAXM n’est pas en cours d’exécution

![Le processus HAXM n’est pas en cours d’exécution](troubleshooting-images/04-haxm-process-not-running.png)

Cette boîte de dialogue s’affiche si votre ordinateur est équipé d’un processeur Intel, Hyper-V est désactivé, le Gestionnaire HAXM Intel est installé, mais que le processus HAXM n’est pas en cours d’exécution. Pour résoudre ce problème, ouvrez une fenêtre d’invite de commandes et entrez la commande suivante :

```cmd
sc query intelhaxm
```

Si le processus HAXM est en cours d’exécution, la sortie de la commande doit être similaire à ce qui suit :

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


Si **STATE** n’est pas défini sur **RUNNING**, consultez [How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) pour résoudre le problème.


### <a name="other-failures"></a>Autres échecs

![Autres échecs](troubleshooting-images/05-other-failure.png)

Cette boîte de dialogue s’affiche si votre ordinateur est équipé d’un processeur Intel, Hyper-V est désactivé, Intel HAXM est installé, le processus HAXM est en cours d’exécution, mais que le démarrage de l’émulateur échoue pour une raison inconnue.
Pour résoudre cette erreur, consultez [How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Désactivation des avertissements liés aux performances

Si vous préférez ne pas afficher les avertissements liés aux performances, vous pouvez les désactiver. Dans Visual Studio, cliquez sur **Outils > Options > Xamarin > Paramètres Android** et désactivez l’option **Avertir si l'accélération AVD n'est pas prise en charge (HAXM)**  :

[![Désactivation des avertissements d’accélération AVD](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

À compter de Visual Studio pour Mac build 7.2 (build 559), une boîte de dialogue d’avertissement sur les performances peut s’afficher quand vous déployez votre application pour la première fois sur l’émulateur Google Android. Ces boîtes de dialogue d’avertissement sont expliquées ci-dessous.

### <a name="haxm-is-not-installed"></a>Le Gestionnaire HAXM n'est pas installé

![Le Gestionnaire HAXM n'est pas installé](troubleshooting-images/03-haxm-not-installed.png)

Cette boîte de dialogue indique qu’HAXM n’est pas installé.
La section [Installation du Gestionnaire HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) décrit la procédure d’installation du Gestionnaire HAXM.

### <a name="haxm-process-not-running"></a>Le processus HAXM n’est pas en cours d’exécution

![Le processus HAXM n’est pas en cours d’exécution](troubleshooting-images/04-haxm-process-not-running.png)

Cette boîte de dialogue s’affiche si le processus HAXM n’est pas en cours d’exécution. Pour plus d’informations sur la résolution de ce problème, consultez [How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="other-failures"></a>Autres échecs

![Autres échecs](troubleshooting-images/05-other-failure.png)

Cette boîte de dialogue s’affiche si l’émulateur ne parvient pas à démarrer pour une raison inconnue. Pour résoudre cette erreur, consultez [How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

-----


## <a name="solutions-to-common-problems"></a>Solutions aux problèmes courants

De nombreux problèmes courants de l’émulateur Google Android peuvent être résolus en changeant la configuration de votre ordinateur ou en installant des logiciels supplémentaires. Les sections suivantes décrivent ces problèmes et fournissent des solutions.


### <a name="deployment-issues"></a>Problèmes de déploiement

Si un message d’erreur indiquant que l’installation de l’APK sur l’émulateur a échoué ou que l’exécution d’Android Debug Bridge (**adb**) a échoué, vérifiez que le kit Android SDK peut se connecter à votre émulateur. Pour ce faire, effectuez les étapes suivantes :

1. Lancez l’émulateur à partir du **Gestionnaire des appareil virtuels Android (AVD)** (sélectionnez votre appareil virtuel et cliquez sur **Démarrer**).

2. Ouvrez une invite de commandes et accédez au dossier d’installation d’**adb**. Par exemple, sur Windows, il peut s’agir du dossier : **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**.

3. Tapez la commande suivante :

   ```shell
   adb devices
   ```

4. Si l’émulateur est accessible à partir du kit Android SDK, l’émulateur doit apparaître dans la liste des appareils connectés. Exemple :

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Si l’émulateur n’apparaît pas dans cette liste, démarrez le **Gestionnaire du kit Android SDK**, appliquez les mises à jour, puis essayez à nouveau de lancer l’émulateur.



### <a name="haxm-issues"></a>Problèmes liés à HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Si l’émulateur Google Android ne démarre pas correctement, il s’agit généralement de problèmes liés à HAXM. Les problèmes liés à HAXM sont souvent le résultat de conflits avec d’autres technologies de virtualisation, de paramètres incorrects ou d’un pilote HAXM obsolète.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>Conflits de virtualisation HAXM

HAXM peut entrer en conflit avec d’autres technologies qui utilisent la virtualisation, comme Hyper-V, Windows Device Guard et certains logiciels antivirus :

- **Hyper-V** &ndash; Si vous utilisez Windows avec Hyper-V activé, suivez la procédure de la section [Désactivation d’Hyper-V](#disable-hyperv).

- **Device Guard** &ndash; Device Guard et Credential Guard peuvent empêcher la désactivation d’Hyper-V sur les ordinateurs Windows. Pour désactiver Device Guard et Credential Guard, consultez [Désactivation de Device Guard](#disable-devguard).

- **Logiciel antivirus** &ndash; Si vous exécutez un logiciel antivirus qui utilise la virtualisation à assistance matérielle (comme Avast), désactivez ou désinstallez ce logiciel, redémarrez et essayez à nouveau de démarrer l’émulateur du kit Android SDK.


#### <a name="incorrect-bios-settings"></a>Paramètres du BIOS incorrects

Si vous utilisez HAXM sur un PC Windows, HAXM ne fonctionne pas sauf si la technologie de virtualisation (Intel VT-x) est activée dans le BIOS. Si VT-x est désactivé, une erreur semblable à la suivante s’affiche quand vous essayez de démarrer l’émulateur Google Android :

**Cet ordinateur répond à la configuration requise pour HAXM, mais la technologie Intel de virtualisation (VT-x) n’est pas activée.**

Pour corriger cette erreur, démarrez l’ordinateur dans le BIOS, activez VT-x et SLAT (Second Level Address Translation), puis redémarrez l’ordinateur dans Windows.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Si l’émulateur Google Android ne démarre pas correctement, il s’agit généralement de problèmes liés à HAXM. Les problèmes liés à HAXM sont souvent le résultat de conflits avec d’autres technologies de virtualisation, de paramètres incorrects ou d’un pilote HAXM obsolète. Essayez de réinstaller le pilote HAXM, en utilisant la procédure décrite dans [Installation d’HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Désactivation d'Hyper-V

Si vous utilisez Windows avec Hyper-V activé, vous devez le désactiver et redémarrer votre ordinateur pour installer et utiliser HAXM. Vous pouvez désactiver Hyper-V à partir du Panneau de configuration en effectuant les étapes suivantes :

1. Dans la zone de recherche Windows, entrez **Programmes et**, puis cliquez sur **Programmes et fonctionnalités** dans les résultats de la recherche.

2. Dans la boîte de dialogue **Programmes et fonctionnalités** du Panneau de configuration, cliquez sur **Activer ou désactiver des fonctionnalités Windows** :

    ![Activer ou désactiver des fonctionnalités Windows](troubleshooting-images/win/07-turn-windows-features.png)

3. Décochez **Hyper-V** et redémarrez l’ordinateur :

    ![Désactivation d’Hyper-V dans la boîte de dialogue Fonctionnalités de Windows](troubleshooting-images/win/08-uncheck-hyper-v.png)

Vous pouvez également utiliser l’applet de commande Powershell suivante pour désactiver Hyper-V :

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM et Microsoft Hyper-V ne peuvent pas être actifs en même temps. Malheureusement, il n’existe à ce jour aucun moyen de basculer entre Hyper-V et HAXM sans avoir à redémarrer votre ordinateur. Si vous souhaitez utiliser l’[émulateur Visual Studio pour Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (qui dépend d’Hyper-V), vous ne pouvez pas utiliser l’émulateur du kit Android SDK sans redémarrer l’ordinateur. Il est possible d’utiliser à la fois Hyper-V et HAXM en créant une configuration de démarrage multiple comme expliqué dans [Création d’une entrée de démarrage « sans hyperviseur »](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

Dans certains cas, la procédure ci-dessus ne permet pas de désactiver Hyper-V si Device Guard et Credential Guard sont activés. Si vous ne parvenez pas à désactiver Hyper-V (ou s’il semble être désactivé mais que l’installation d’HAXM échoue toujours), utilisez la procédure de la section suivante pour désactiver Device Guard et Credential Guard.

<a name="disable-devguard" />

#### <a name="disabling-device-guard"></a>Désactivation de Device Guard

Device Guard et Credential Guard peuvent empêcher la désactivation d’Hyper-V sur les ordinateurs Windows. Ce problème apparaît généralement sur les ordinateurs joints au domaine qui sont configurés et contrôlés par une organisation propriétaire.
Sur Windows 10, effectuez les étapes suivantes pour savoir si **Device Guard** est en cours d’exécution :

1. Dans la **zone de recherche Windows**, tapez **Informations système** pour démarrer l’application **Informations système**.

2. Dans **Résumé système**, vérifiez si **Sécurité basée sur la virtualisation de Device Guard** est présent et **En cours d’exécution** :

   [![Device Guard est présent et en cours d’exécution](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Si Device Guard est activé, effectuez les étapes suivantes pour le désactiver :

1. Vérifiez qu’**Hyper-V** est désactivé (sous **Activer ou désactiver des fonctionnalités Windows**) comme décrit dans la section précédente.

2. Dans la zone de recherche Windows, tapez **gpedit** et sélectionnez **Modifier la stratégie de groupe** dans les résultats de la recherche. Cette opération lance l’**Éditeur de stratégie de groupe locale**.

3. Dans l’**Éditeur de stratégie de groupe locale**, accédez à **Configuration ordinateur > Modèles d’administration > Système > Device Guard** :

   [![Device Guard dans l’Éditeur de stratégie de groupe locale](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

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


# <a name="visual-studiotabvsmac"></a>[Visual Studio](#tab/vsmac)

Hyper-V n’est pas disponible sur OS X ou macOS.

-----

