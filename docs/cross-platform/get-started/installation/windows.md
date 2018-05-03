---
title: Installation de Xamarin dans Visual Studio 2017
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: 0f1c014f316ff4f3eb7341fa9815475175d11937
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Installation de Xamarin dans Visual Studio 2017

<a name="requirements" />

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont nécessaires à l’installation de Xamarin dans Visual Studio 2017 :

1. Windows 7 ou une version ultérieure

2. Visual Studio 2017 (Community, Professional ou Enterprise)

3. Xamarin pour Visual Studio

Pour plus d’informations sur les prérequis nécessaires à l’installation et à l’utilisation de Xamarin, consultez [Configuration requise](~/cross-platform/get-started/requirements.md).

<a name="installation" />

## <a name="installation"></a>Installation

Xamarin peut être installé dans le cadre d’une nouvelle installation de Visual Studio 2017.
Pour ce faire, effectuez les étapes suivantes :

1. Téléchargez Visual Studio 2017 Community, Visual Studio Professional ou Visual Studio Enterprise dans la page [Visual Studio](https://www.visualstudio.com/vs/) (les liens de téléchargement se trouvent au bas de la page).

2. Double-cliquez sur le paquet téléchargé pour démarrer l’installation.

3. Sélectionnez la charge de travail **Développement mobile en .NET** dans l’écran d’installation : 

    [![Sélection de l’option Développement mobile en .NET dans l’écran Charges de travail](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Une fois l’option **Développement mobile en .NET** sélectionnée, regardez le panneau **Résumé** sur la droite. Ici, vous pouvez désélectionner les options de développement mobile que vous ne souhaitez pas installer. Par défaut, toutes les options de la capture d’écran suivante sont installées (**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**, **Android NDK**, **Android SDK**, **Java SE Development Kit**, **Google Android Emulator**, **Prise en charge F#** et **Intel HAXM**) :

    ![Panneau Résumé répertoriant les options Xamarin à installer](windows-images/02-summary.png)

5. Quand vous êtes prêt à installer Visual Studio 2017, cliquez sur le bouton **Installer** en bas à droite :

    ![Emplacement du bouton Installation](windows-images/03-click-install.png)

   Selon l’édition de Visual Studio 2017 que vous installez, le processus d’installation peut prendre un certain temps. Vous pouvez surveiller l’installation à l’aide des barres de progression :

    ![Capture d’écran des barres de progression pendant l’installation](windows-images/04-progress-bars.png)

6. Quand l’installation de Visual Studio 2017 est terminée, cliquez sur le bouton **Lancer** pour démarrer Visual Studio :

    ![Emplacement du bouton Lancer](windows-images/05-launch.png)

<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Ajout de Xamarin à Visual Studio 2017

Si Visual Studio 2017 est déjà installé, vous pouvez ajouter Xamarin en réexécutant le programme d’installation de Visual Studio 2017 pour modifier les charges de travail (pour plus d’informations, consultez [Modifier Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)). Ensuite, suivez les étapes répertoriées ci-dessus pour installer Xamarin.

Pour plus d’informations sur le téléchargement et l’installation de Visual Studio 2017, consultez [Téléchargement et installation de Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### <a name="verifying-installation"></a>Vérification de l’installation

Dans Visual Studio 2017, vous pouvez vérifier que Xamarin est installé en cliquant sur le menu **Aide**. Si Xamarin est installé, vous devriez voir l’élément de menu **Xamarin**, comme dans cette capture d’écran :

![Élément de menu Xamarin dans le menu Aide](windows-images/12-xamarin-menu-item.png)

Si vous utilisez une version antérieure de Visual Studio, vous pouvez cliquer sur **Aide > À propos de Microsoft Visual Studio**, puis faire défiler la liste des produits installés pour voir si Xamarin y figure :

![Écran des produits Visual Studio installés](windows-images/13-xamarin-is-installed.png)

Pour savoir où trouver les informations de version, consultez [Où trouver les informations de version et les journaux ?](~/cross-platform/troubleshooting/questions/version-logs.md).

<a name="nextsteps" />

## <a name="next-steps"></a>Étapes suivantes

L’installation de Xamarin dans Visual Studio 2017 vous permet d’écrire du code pour vos applications. Toutefois, vous devrez procéder à une configuration supplémentaire pour générer et déployer vos applications sur un simulateur, un émulateur ou un appareil. Consultez les guides suivants pour terminer l’installation et commencer la création d’applications multiplateformes.

### <a name="ios"></a>iOS

Pour plus d’informations, consultez le guide [Installation de Xamarin.iOS sur un système Windows](~/ios/get-started/installation/windows/index.md). 

1. [Installer Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Connecter Visual Studio à votre hôte de build Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [Configuration développeur iOS](~/ios/get-started/installation/device-provisioning/index.md) (nécessaire pour exécuter votre application sur l’appareil)
5. [Remoted iOS Simulator](~/tools/ios-simulator.md)
6. [Introduction à Xamarin.iOS pour Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Pour plus d’informations, consultez le guide [Installation de Xamarin.Android sur un système Windows](~/android/get-started/installation/windows.md).

1. [Configuration de Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Utilisation de Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Émulateur SDK Android](~/android/get-started/installation/android-emulator/index.md)
4. [Configuration de l’appareil pour le développement](~/android/get-started/installation/set-up-device-for-development.md)
