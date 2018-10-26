---
title: Où puis-je définir mon emplacements du Kit Android SDK ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 11/16/2017
ms.openlocfilehash: c004fb7717f78750e7ac1e8dc1856a32ba808638
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119486"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Où puis-je définir mon emplacements du Kit Android SDK ?

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Dans Visual Studio, accédez à **Outils > Options > Xamarin > Paramètres Android** pour afficher et définir l’emplacement du Kit Android SDK :

[![Onglet emplacements exemple préférences](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

L’emplacement par défaut pour chaque chemin d’accès est comme suit :

- Emplacement du Kit de développement Java : 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Emplacement d’Android SDK : 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Emplacement du NDK Android : 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Notez que le numéro de version de la NDK peut varier. Par exemple, au lieu de **android-ndk-r13b**, il peut s’agir comme une version antérieure **android-ndk-r10e**.

Pour définir l’emplacement du SDK Android, entrez le chemin d’accès complet du répertoire du Kit Android SDK dans le **emplacement du Kit Android** boîte. Vous pouvez accédez à l’emplacement du Kit Android SDK dans l’Explorateur de fichiers, copiez le chemin d’accès à partir de la barre d’adresses et collez ce chemin d’accès dans le **emplacement du Kit Android** boîte.
Par exemple, si votre emplacement d’Android SDK est à **C:\\utilisateurs\\nom d’utilisateur\\AppData\\Local\\Android\\Sdk**, désactivez l’ancien chemin d’accès dans le  **Emplacement du Kit Android** , collez dans ce chemin d’accès, puis cliquez sur **OK**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Dans Visual Studio pour Mac, accédez à **Préférences > projets > emplacements SDK > Android**. Dans le **Android** , cliquez sur le **emplacements** onglet pour afficher et définir l’emplacement du SDK :

[![Onglet emplacements exemple préférences](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

L’emplacement par défaut pour chaque chemin d’accès est comme suit :

- Emplacement d’Android SDK : 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Emplacement du NDK Android : 

    **~/Library/Developer/Xamarin/Android-ndk/Android-ndk-r14b**

- Emplacement du Kit de développement logiciel (JDK) Java : 

    **/ usr**

Notez que le numéro de version de la NDK peut varier. Par exemple, au lieu de **android-ndk-r14b**, il peut s’agir comme une version antérieure **android-ndk-r10e**.

Pour définir l’emplacement du SDK Android, entrez le chemin d’accès complet du répertoire du Kit Android SDK dans le **emplacement du Kit Android** boîte. Vous pouvez sélectionner le dossier du Kit Android SDK dans le Finder, appuyez sur **CTRL +&#8984;+ I** pour afficher les informations de dossier, cliquez- glissez le chemin d’accès à droite de **où :**, copier, puis collez-le dans le **du Kit Android SDK Emplacement** zone le **emplacements** onglet. Par exemple, si votre emplacement d’Android SDK est à **~/Library/Developer/Android/Sdk**, désactivez l’ancien chemin d’accès dans le **emplacement du Kit Android** , collez dans ce chemin d’accès, puis cliquez sur **OK**.

-----
