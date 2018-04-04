---
title: Comment mettre à jour la version du Kit de développement Java (JDK) ?
description: Cet article explique comment mettre à jour la version du Kit de développement Java (JDK) sur Windows et Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: dcfc5e406e60ac72fb1ca1e9cfb0395d17074b2c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Comment mettre à jour la version du Kit de développement Java (JDK) ?

_Cet article explique comment mettre à jour la version du Kit de développement Java (JDK) sur Windows et Mac._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Android utilise le Kit de développement Java (JDK) pour intégrer le SDK Android pour la création d’applications Android et en cours d’exécution du concepteur Android. Les dernières versions du SDK Android (API 24 et versions ultérieures) requièrent JDK 8 (1,8). Si vous n'avez pas encore mis à jour JDK 8, procédez comme suit pour installer et activer :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Télécharger le JDK 8 (1,8) à partir de la [site Web d’Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Capture d’écran de JDK télécharger sur le site Web d’Oracle](update-jdk-images/image1.png)

2.  Choisir la version 64 bits pour permettre le rendu des [contrôles personnalisés](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) dans le concepteur Xamarin Android :

    ![En sélectionnant le package JDK de Windows x64 à télécharger à partir de la page de téléchargement JDK](update-jdk-images/image2.png)

3.  Exécuter le .exe et installer le **outils de développement**:

    ![Installation des outils de développement dans le programme d’installation JDK](update-jdk-images/image3.png)

4.  Ouvrez Visual Studio et mise à jour la **l’emplacement du Kit de développement Java** pour pointer vers le nouveau JDK sous **Outils > Options > Xamarin > Paramètres Android > emplacement du Kit de développement Java > modification**:

    ![Paramètre de chemin d’accès pour le JDK dans la page de l’IDE, paramètres Android](update-jdk-images/image4.png)

Veillez à redémarrer Visual Studio après la mise à jour de l’emplacement.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Télécharger le JDK 8 (1,8) à partir de la [site Web d’Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Capture d’écran de JDK télécharger sur le site Web d’Oracle](update-jdk-images/image1.png)

2.  Ouvrez le fichier .dmg et exécutez le programme d’installation .pkg :

    ![Exécute le programme d’installation JDK sur macOS](update-jdk-images/image5.png)

Mac OS définira automatiquement la nouvelle version du JDK en mettant à jour en tant que la valeur par défaut **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Vous pouvez ensuite vérifier que la **Kit de développement Java (JDK)** emplacement est défini sur la valeur par défaut attendu **/usr** sous **Visual Studio pour Mac > Préférences > projets > emplacements du SDK > Android > Java Kit de développement logiciel (JDK) > emplacement**:

![Définition de l’emplacement du JDK dans la page emplacement du Kit de développement logiciel Android](update-jdk-images/image6.png)

-----

