---
title: Extensions Visual Studio manquantes après l’installation
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 3e3d426e7b00725eafeba139de5bc46d416c368a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61344222"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Extensions Visual Studio manquantes après l’installation

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Message d’erreur : Ce projet est incompatible avec l’édition actuelle de Visual Studio

Vérifiez que Visual Studio 2017 (Community, Professional ou Enterprise) ou plus récent est installé.

Voir aussi le [configuration requise de Windows](~/cross-platform/get-started/requirements.md#windows-requirements).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Correction possible 1 : Modifier l’installation de s’assurer que les extensions Visual Studio sont installées.

Dans certaines situations, le programme d’installation de Xamarin peut automatiquement désactiver les options d’installation pour les extensions de Visual Studio. Si c’est la cause du problème, installez les extensions de Visual Studio manquantes à l’aide du programme d’installation **modification** commande. Par exemple, pour installer les extensions pour Visual Studio 2013 :

1. Ouvrez le Windows **programmes et fonctionnalités** Control Panel.

2. Bouton droit sur le **Xamarin** entrée, puis sélectionnez **modification**.

3. Cliquez sur **suivant**, puis **modification**.

4. Assurez-vous que le **Xamarin pour Visual Studio 2013** option est définie à installer :

    ![](missing-vs-extensions-images/installer.png "Activer Xamarin pour l’option d’installation de Visual Studio 2013")

5. Parcourez le reste de l’Assistant installation.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Correction possible 2 : Demandez à Visual Studio pour configurer les extensions à nouveau

1. Vérifiez si les extensions de Xamarin ont été copiées dans le dossier d’extensions Visual Studio :

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Si les extensions sont installées correctement (pour la version 3.1.228), il y aura 60 éléments dans le dossier :


    ![](missing-vs-extensions-images/folder.png "Liste de contenu du dossier 'Xamarin\3.1.228.0' dans l’Explorateur")

2. Une fois que vous avez confirmé que ce dossier sont correctes, indiquer à Visual Studio pour essayer de configurer les extensions à nouveau :

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Correction possible 3 : Essayez de réinstaller fraîche de Xamarin

1.  À partir du Panneau de configuration de Windows, désinstallez les éléments suivants qui sont présents :

    *   Xamarin

    *   Xamarin pour Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Xamarin pour Visual Studio

2.  Dans l’Explorateur, supprimez tous les fichiers restants à partir des dossiers d’extension Xamarin Visual Studio (toutes les versions, y compris les **Program Files** et **Program Files (x86)**) :

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Vérifiez également dans le répertoire « VirtualStore » pour voir si toutes les copies de tous les répertoires de l’extension de « superposition » peut être :

    `%LOCALAPPDATA%\VirtualStore`

4.  Ouvrez l’Éditeur du Registre (regedit).

5.  Recherchez cette clé :

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Recherchez et supprimez toutes les entrées qui correspondent à ce modèle :

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Recherchez cette clé :

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Supprimez les entrées qui semblent éventuellement liées à Xamarin. Par exemple, celui de Voici qui permet de poser des problèmes dans les versions antérieures de Xamarin :

    _Mono.VisualStudio.Shell,1.0_

9.  Redémarrez.

10.  Réinstallez la version stable actuelle de Xamarin à partir de [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Correction possible 4 : Réparer l’installation de Visual Studio

1.  Ouvrez le Windows **programmes et fonctionnalités** Control Panel.

2.  Cliquez avec le bouton droit sur l’entrée de Microsoft Visual Studio pertinentes, puis sélectionnez **modification**

3.  Cliquez sur le **réparation** bouton dans la boîte de dialogue Visual Studio qui s’ouvre.
