---
title: "Installation de Xamarin dans Visual Studio sur les systèmes Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b68e03251b83192bdc5836af6ea54446ddaad24a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Installation de Xamarin dans Visual Studio sur les systèmes Windows

Étant donné que Xamarin est désormais inclus gratuitement dans toutes les éditions de Visual Studio et ne nécessite pas de licence supplémentaire, vous pouvez utiliser le programme d’installation de Visual Studio pour télécharger et installer les outils Xamarin.

-   [Requirements](#requirements)
-   [Installation](#installation)
-   [Ajout de Xamarin à Visual Studio 2017](#vs2017)
-   [Ajout de Xamarin à Visual Studio 2015](#vs2015)
-   [Vérification de l’installation](#verifying)
-   [Étapes suivantes](#nextsteps)


<a name="requirements" />

# <a name="requirements"></a>Configuration requise

Les éléments suivants sont nécessaires à l’installation de Visual Studio Tools pour Xamarin :

1. Windows 7 ou une version ultérieure

2. Visual Studio 2015 ou 2017 (Community, Professional ou Enterprise)

3. Xamarin pour Visual Studio

Notez que Xamarin ne peut pas être utilisé avec les éditions Express de Visual Studio en raison de l’absence de prise en charge des plug-ins.

Pour plus d’informations sur les prérequis nécessaires à l’installation et à l’utilisation de Xamarin, consultez [Configuration requise](~/cross-platform/get-started/requirements.md).


<a name="installation" />

# <a name="installation"></a>Installation

Xamarin peut être installé dans le cadre d’une nouvelle installation de Visual Studio.
Pour ce faire, effectuez les étapes suivantes :

1. Téléchargez Visual Studio Community, Visual Studio Professional ou Visual Studio Enterprise dans la page [Visual Studio](https://www.visualstudio.com/vs/) (les liens de téléchargement se trouvent au bas de la page).

2. Double-cliquez sur le paquet téléchargé pour démarrer l’installation.

3. Sélectionnez la charge de travail **Développement mobile en .NET** dans l’écran d’installation : 

    [![Sélection de l’option Développement mobile en .NET dans l’écran Charges de travail](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png)

4. Une fois l’option **Développement mobile en .NET** sélectionnée, regardez le panneau **Résumé** sur la droite. Ici, vous pouvez désélectionner les options de développement mobile que vous ne souhaitez pas installer. Par défaut, toutes les options de la capture d’écran suivante sont installées (**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**, **Android NDK**, **Android SDK**, **Java SE Development Kit**, **Google Android Emulator**, **Prise en charge F#** et **Intel HAXM**) :

    ![Panneau Résumé répertoriant les options Xamarin à installer](windows-images/02-summary.png)

5. Lorsque vous êtes prêt à installer Visual Studio, cliquez sur le bouton **Installer** en bas à droite :

    ![Emplacement du bouton Installation](windows-images/03-click-install.png)

   Selon l’édition de Visual Studio que vous installez, le processus d’installation peut prendre un certain temps. Vous pouvez surveiller l’installation à l’aide des barres de progression :

    ![Capture d’écran des barres de progression pendant l’installation](windows-images/04-progress-bars.png)

6. Lorsque l’installation de Visual Studio est terminée, cliquez sur le bouton **Lancer** pour démarrer Visual Studio :

    ![Emplacement du bouton Lancer](windows-images/05-launch.png)


<a name="vs2017" />

## <a name="adding-xamarin-to-visual-studio-2017"></a>Ajout de Xamarin à Visual Studio 2017

Si Visual Studio 2017 est déjà installé, vous pouvez ajouter Xamarin en réexécutant le programme d’installation de Visual Studio pour modifier les charges de travail (pour plus d’informations, consultez [Modifier Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)). Ensuite, suivez les étapes répertoriées ci-dessus pour installer Xamarin.

Pour plus d’informations sur le téléchargement et l’installation de Visual Studio 2017, consultez [Téléchargement et installation de Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


<a name="vs2015" />

## <a name="adding-xamarin-to-visual-studio-2015"></a>Ajout de Xamarin à Visual Studio 2015

Pour ajouter Xamarin.Android à une installation existante de Visual Studio 2015, effectuez les étapes suivantes :

1. Cliquez avec le bouton droit sur le bouton **Démarrer** de Windows, puis sélectionnez **Programmes et fonctionnalités**.

2. Cliquez avec le bouton droit sur **Microsoft Visual Studio**, puis cliquez sur **Modifier**.

3. Lorsque la boîte de dialogue Visual Studio Installer s’affiche, cliquez sur le bouton **Modifier**.

4. Sous l’onglet **Fonctionnalités**, faites défiler la page jusqu’à atteindre **Développement multiplateforme pour appareils mobiles**. Cochez la case en regard de **C#/.NET (Xamarin)** :

    ![Ajout de C#/.NET Xamarin à Visual Studio 2015](windows-images/06-add-xamarin.png)

5. Cliquez sur le bouton **Mettre à jour** pour ajouter Xamarin à Visual Studio.


<a name="verifying" />

## <a name="verifying-installation"></a>Vérification de l’installation

Dans Visual Studio 2017, vous pouvez vérifier que Xamarin est installé en cliquant sur le menu **Aide**. Si Xamarin est installé, vous devriez voir l’élément de menu **Xamarin**, comme dans cette capture d’écran :

![Élément de menu Xamarin dans le menu Aide](windows-images/12-xamarin-menu-item.png)

Si vous utilisez une version antérieure de Visual Studio, vous pouvez cliquer sur **Aide > À propos de Microsoft Visual Studio**, puis faire défiler la liste des produits installés pour voir si Xamarin y figure :

![Écran des produits Visual Studio installés](windows-images/13-xamarin-is-installed.png)

Pour savoir où trouver les informations de version, consultez [Où trouver les informations de version et les journaux ?](~/cross-platform/troubleshooting/questions/version-logs.md).

<a name="nextsteps" />

# <a name="next-steps"></a>Étapes suivantes

L’installation de Visual Studio Tools pour Xamarin vous permet d’écrire du code pour vos applications. Toutefois, vous devrez procéder à une configuration supplémentaire pour générer et déployer vos applications sur un simulateur, un émulateur ou un appareil. Consultez les guides suivants pour terminer l’installation et commencer la création d’applications multiplateformes.

## <a name="ios"></a>iOS

Pour plus d’informations, consultez le guide [Installation de Xamarin.iOS sur un système Windows](~/ios/get-started/installation/windows/index.md). 

1. [Installer les outils Xamarin.iOS sur votre Mac](~/ios/get-started/installation/windows/index.md#installation)
2. [Configuration de votre Mac](~/ios/get-started/installation/windows/index.md#configuration)
3. [Configuration développeur iOS](~/ios/get-started/installation/windows/index.md#developersetup) (pour exécuter votre application sur l’appareil).
4. [Connexion de Visual Studio à votre Build Host Mac](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Remoted iOS Simulator](~/tools/ios-simulator.md)
6. [Introduction à Xamarin.iOS pour Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

## <a name="android"></a>Android

Pour plus d’informations, consultez le guide [Installation de Xamarin.Android sur un système Windows](~/android/get-started/installation/windows.md).

1. [Configuration de Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Utilisation de Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Émulateur SDK Android](~/android/get-started/installation/android-emulator/index.md)
4. [Configuration de l’appareil pour le développement](~/android/get-started/installation/set-up-device-for-development.md)
