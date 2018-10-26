---
title: Comment mettre à jour la version du Kit de développement Java (JDK) ?
description: Cet article explique comment mettre à jour la version du Kit de développement Java (JDK) sur Windows et Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: aa04d944f803dded0e9448de27ed7d5ced2efb54
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109183"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Comment mettre à jour la version du Kit de développement Java (JDK) ?

_Cet article explique comment mettre à jour la version du Kit de développement Java (JDK) sur Windows et Mac._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Android utilise le Kit de développement Java (JDK) pour intégrer le SDK Android pour la création d’applications Android et Android designer en cours d’exécution. Les dernières versions d’Android SDK (API 24 et versions ultérieures) requièrent JDK 8 (1.8). Vous pouvez aussi installer le [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md). Microsoft Mobile OpenJDK va remplacer JDK 8 pour le développement de Xamarin.Android.

Pour mettre à jour pour Microsoft Mobile OpenJDK, consultez [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md). Pour mettre à jour de JDK 8, procédez comme suit :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Télécharger JDK 8 (1.8) à partir de la [site Web d’Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Capture d’écran du JDK télécharger sur le site Web d’Oracle](update-jdk-images/image1.png)

2.  Choisir la version 64 bits pour autoriser le rendu des [des contrôles personnalisés](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) dans le Concepteur de Xamarin Android :

    ![Sélection du package JDK Windows x64 à télécharger à partir de la page de téléchargement JDK](update-jdk-images/image2.png)

3.  Exécutez le fichier .exe et installer le **outils de développement**:

    ![Installation des outils de développement dans le programme d’installation JDK](update-jdk-images/image3.png)

4.  Ouvrez Visual Studio et mise à jour le **emplacement du Java Development Kit** pour pointer vers le nouveau JDK sous **Outils > Options > Xamarin > Paramètres Android > emplacement du Java Development Kit**:

    [![Paramètre de chemin d’accès pour le JDK dans la page Paramètres Android](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Veillez à redémarrer Visual Studio après la mise à jour de l’emplacement.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1.  Télécharger JDK 8 (1.8) à partir de la [site Web d’Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Capture d’écran du JDK télécharger sur le site Web d’Oracle](update-jdk-images/image1.png)

2.  Ouvrez le fichier .dmg et exécutez le programme d’installation .pkg :

    ![Exécute le programme d’installation JDK sur macOS](update-jdk-images/image5.png)

Mac OS définira automatiquement la nouvelle version JDK en mettant à jour en tant que la valeur par défaut **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Vous pouvez Vérifiez ensuite que le **SDK Java (JDK)** emplacement est défini sur la valeur par défaut attendu **/usr** sous **Visual Studio pour Mac > Préférences > projets > emplacements SDK > Android > emplacements > emplacement du Kit de développement logiciel (JDK) Java**:

[![Définition de l’emplacement du JDK dans l’onglet emplacements Android](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

