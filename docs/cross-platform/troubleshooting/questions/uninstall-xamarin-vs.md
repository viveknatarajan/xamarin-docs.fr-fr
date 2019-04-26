---
title: Comment effectuer une désinstallation complète de Xamarin pour Visual Studio ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61159621"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Comment effectuer une désinstallation complète de Xamarin pour Visual Studio ?


1.  À partir du Panneau de configuration de Windows, désinstallez les éléments suivants qui sont présents :

    -   Xamarin
    -   Xamarin pour Windows
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Xamarin pour Visual Studio

2.  Dans l’Explorateur, supprimez tous les fichiers restants à partir des dossiers d’extension Xamarin Visual Studio (toutes les versions, y compris les _Program Files_ et _Program Files (x86)_) :

    _C:\\Program Files\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\Extensions\\Xamarin_

3.  Supprimer MEF composant répertoire du cache de Visual Studio ainsi :

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    En fait, cette étape en lui-même est souvent suffisante pour résoudre les erreurs, par exemple :

    -   « Le package 'XamarinShellPackage' ne pas été chargé correctement »

    -   « Le fichier projet... ne peut pas être ouvert. Il est un sous-type de projet manquant »

    -   « Référence d’objet non définie sur une instance d’un objet.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   « Échec de SetSite pour package » (dans Visual Studio _ActivityLog.xml_)

    -   « Échec de LegacySitePackage pour package » (dans Visual Studio _ActivityLog.xml_)

    (Voir aussi le [vider le Cache composant MEF](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) extension Visual Studio.  Et consultez [bogue 40781, commentaire 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) pour un peu plus de contexte sur le problème en amont dans Visual Studio qui peuvent provoquer ces erreurs.)

4.  Vérifiez aussi dans le _VirtualStore_ fichiers de superposition de répertoire pour voir si Windows a stocké de la _Extensions\\Xamarin_ ou _ComponentModelCache_répertoires :

    _%LOCALAPPDATA%\\VirtualStore_

5.  Ouvrez l’Éditeur du Registre (`regedit`).

6.  Recherchez la clé suivante :

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  Recherchez et supprimez toutes les entrées qui correspondent à ce modèle :

    _C:\\Program Files\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\Extensions\\Xamarin_

8.  Recherchez cette clé :

    _HKEY\_CURRENT\_USER\\Software\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Supprimez les entrées qui semblent éventuellement liées à Xamarin.  Par exemple, celui de Voici qui permet de poser des problèmes dans les versions antérieures de Xamarin :

    _Mono.VisualStudio.Shell,1.0_

10. Ouvrez un administrateur `cmd.exe` invite de commandes, puis exécutez le `devenv /setup` et `devenv /updateconfiguration` commandes pour chaque version installée de Visual Studio.  Par exemple, pour Visual Studio 2015 :

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Redémarrez.

12. Réinstallez la version stable actuelle de Xamarin à l’aide de [visualstudio.com](https://visualstudio.com/xamarin/).

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Autres étapes de dépannage pour « package non chargés correctement »

Dans les cas où les étapes ci-dessus ne résolvent pas l’erreur « package non chargés correctement », voici quelques étapes supplémentaires.

1.  Créer un nouveau compte d’utilisateur Windows.

2.  Vérifiez si les extensions de Xamarin Visual Studio se chargent sans erreur pour le nouvel utilisateur.

3.  Si les extensions se chargement correctement, le problème est probablement causé par certains des paramètres stockés pour l’utilisateur d’origine :

    -   **Dans l’Explorateur de** – _% LocalAppData%\\Microsoft\\VisualStudio\\1\*.0_
    -   **Dans regedit** – _HKEY\_actuel\_utilisateur\\logiciel\\Microsoft\\VisualStudio\\1\*.0_
    -   **Dans regedit** – _HKEY\_actuel\_utilisateur\\logiciel\\Microsoft\\VisualStudio\\1\*.0\_Config_

4.  Si ces paramètres stockés apparaissent effectivement du problème, vous pouvez essayer de les sauvegarder et de les supprimer ensuite.
