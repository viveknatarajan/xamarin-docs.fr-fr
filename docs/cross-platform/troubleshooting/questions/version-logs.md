---
title: Où puis-je trouver mes informations de version et les journaux ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 9234ee0553b2cc9376c0e4e39ffc0700deaacda1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Où puis-je trouver mes informations de version et les journaux ?

## <a name="outline"></a>Plan

- [Informations de version](#version-information)
    - Informations de version de Windows
    - Informations de version Mac
    - Outils de génération d’Outils, outils de plateforme, le Kit de développement logiciel Android
- [Journaux d’IDE et de programme d’installation](#ide-and-installer-logs)
    - [Journaux Windows](#windows-logs)
        - Xamarin Studio
        - Xamarin pour Visual Studio
        - Programme d’installation de Xamarin universel
        - Individuels `.msi` programmes d’installation, les fichiers journaux détaillés
        - Démarrage de Visual Studio, journaux détaillés
    - [Journaux Mac](#mac-logs)
        - Hôte de build
    - Visual Studio pour Mac
        - Xamarin Studio
        - Programme d’installation de Xamarin
- [Sortie de génération de commentaires](#verbose-build-output-logs)
- [Les journaux pour les applications Xamarin.Android et Xamarin.iOS de débogage](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat journaux
    - Simulateur iOS se connecte (Mac)
    - APPAREIL iOS se connecte (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Informations de version

Il s’agit généralement solution optimale consiste à envoyer sauvegarde toutes les informations à partir de la **copier les informations** boutons. Sinon, nous devrons souvent demander des informations supplémentaires. Par exemple, versions de système d’exploitation, la version Xcode, installé niveaux d’API Android, et la version .NET peut tous être importante pour aider à résoudre un problème.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Informations de version de Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Aide > sur > Afficher les détails > Copier les informations [button]**

#### <a name="visual-studio"></a>Visual Studio

**Aide > à propos de Microsoft Visual Studio > Copier les informations [button]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Informations de version Mac

#### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

**Visual Studio > à propos de Visual Studio > Afficher les détails > Copier les informations [button]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Outils de génération d’Outils, outils de plateforme, le Kit de développement logiciel Android

Ouvrez le Gestionnaire du Kit de développement logiciel Android et prendre une capture d’écran de haut **outils** section.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Capture d’écran du Gestionnaire de SDK Android > dossier Tools")

#### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

**Outils > ouvrez le Gestionnaire de SDK Android**

#### <a name="visual-studio"></a>Visual Studio

Sélectionnez l’icône de barre d’outils du Gestionnaire du SDK :

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Icône de barre d’outils Démarrer le Gestionnaire de SDK Android dans Visual Studio")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />Journaux d’IDE et de programme d’installation

Pour chaque emplacement du journal, veillez à compresser et attacher le dossier de la totalité du journal.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Journaux Windows

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools pour Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Comment obtenir les journaux d’installation de Visual Studio](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Programme d’installation « Universel » de Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Voici les journaux à partir de la `XamarinInstaller.exe` programme d’installation.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Individuels `.msi` programmes d’installation, les fichiers journaux détaillés

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Référence : [les Options de ligne de commande](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Démarrage de Visual Studio, journaux détaillés

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Référence : [/Log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Journaux Mac

Vous pouvez sélectionner le **accédez > accédez au dossier** menu d’élément dans le Finder, puis copiez et collez un de ces chemins d’accès dans la boîte de dialogue.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio pour Mac

`~/Library/Logs/VisualStudio/7.0` (ce nombre peut changer selon la version que vous utilisez.)

Ce dossier peut également être ouvert via « Help -> Open Log Directory ».

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (ce nombre peut changer selon la version que vous utilisez.)

Ce dossier peut également être ouvert via « Help -> Open Log Directory ».

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Programme d’installation « Universel » de Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Voici les journaux à partir de la `XamarinInstaller.dmg` programme d’installation.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Hôte de Build Xamarin

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Sortie de génération de commentaires

1.  Activer [diagnostic de sortie MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  Pour les applications iOS, également activer **mtouch verbose sortie** en ajoutant `-v -v -v -v` sous **propriétés du projet > Build iOS > Général [tab] > Options supplémentaires > les arguments supplémentaires mtouch**.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "Entrez les quatre tiret v dans le champ d’entrée d’arguments mtouch supplémentaires")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio pour Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "Entrez les quatre tiret v dans le champ d’entrée d’arguments mtouch supplémentaires")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  Nettoyez et regénérez le projet.

4.  Copiez et collez la sortie de génération à partir de l’IDE dans un fichier texte.
     - Visual Studio (Windows) : **vue > sortie > Afficher la sortie à partir de : Build**
     - Visual Studio pour Mac : **vue > remplit > erreurs > sortie de génération [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Les journaux pour les applications Xamarin.Android et Xamarin.iOS de débogage

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

**Vue > remplit > sortie de l’Application**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Icône de remplissage de la sortie des applications dans Visual Studio pour Mac")


(Notez que cet élément de menu s’affiche uniquement une fois que l’application a été lancée.)

### <a name="visual-studio"></a>Visual Studio

**Vue > sortie > Afficher la sortie à partir de : débogage**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Tampon de sortie indiquant l’option de débogage dans Visual Studio")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat journaux

Après l’exécution de la `adb` command, attacher précédent le **android_logcat.txt** fichier à partir de votre bureau. Ces instructions supposent que vous avez un seul périphérique attaché.

Voir aussi la [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) page.

#### <a name="visual-studio"></a>Visual Studio

1.  **Outils > Android > Démarrez invite de commandes Adb Android**
2.  Nettoyer le journal : `adb logcat -c`
3.  Reproduisez le problème.
4.  Le journal de sortie : `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

1.  **Outils > Ouvrir l’invite de commandes SDK Android**
2.  Nettoyer le journal : `adb logcat -c`
3.  Reproduisez le problème.
4.  Le journal de sortie : `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />Simulateur iOS se connecte (Mac)

*   Pour accéder au journal système, sélectionnez **Déboguer > Ouvrir le journal système...**  dans l’application de simulateur iOS.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "Affichage de l’option Ouvrir le journal système de menu Déboguer")

*   Pour afficher les rapports d’incidents à partir du simulateur, ouvrez Console.app et accédez à `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />APPAREIL iOS se connecte (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

**Vue > remplit > iOS journal de l’appareil**

#### <a name="xcode"></a>Xcode 

**Fenêtre > appareils > ${DeviceName}**

Rapports d’incidents sont disponibles sous les **afficher les journaux de périphérique** bouton. Le journal système pour le périphérique s’affiche au bas de la fenêtre de la flèche de divulgation d’informations <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />.

#### <a name="xcode-5"></a>Xcode 5

**Fenêtre > organisateur > appareils [tab] > ${DeviceName}**
