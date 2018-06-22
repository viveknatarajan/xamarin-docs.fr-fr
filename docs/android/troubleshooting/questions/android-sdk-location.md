---
title: Où puis-je définir mes emplacements du SDK Android ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 11/16/2017
ms.openlocfilehash: e7af6635aeec72a9a0e0c01eeb6eb0a77d2a1d7b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764006"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Où puis-je définir mes emplacements du SDK Android ?

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dans Visual Studio, accédez à **Outils > Options > Xamarin > Paramètres Android** pour afficher et définir l’emplacement du SDK Android :

[![Onglet emplacements exemple préférences](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

L’emplacement par défaut pour chaque chemin d’accès est comme suit :

- Emplacement du Kit de développement Java : 

    **C:\\fichiers programme\\Java\\jdk1.8.0_131**

- Emplacement du Kit de développement logiciel Android : 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Emplacement du NDK Android : 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Notez que le numéro de version de la NDK peut-être varier. Par exemple, au lieu de **android-ndk-r13b**, il peut être une version antérieure, tel que **android-ndk-r10e**.

Pour définir l’emplacement du SDK Android, entrez le chemin d’accès complet du répertoire du SDK Android dans le **emplacement du Kit de développement logiciel Android** boîte. Vous pouvez naviguer vers l’emplacement du SDK Android dans l’Explorateur de fichiers, copiez le chemin d’accès à partir de la barre d’adresses et coller ce chemin d’accès dans le **emplacement du Kit de développement logiciel Android** boîte.
Par exemple, si votre emplacement du SDK Android se trouve **C:\\utilisateurs\\nom d’utilisateur\\AppData\\Local\\Android\\Sdk**, désactivez l’ancien chemin d’accès dans le  **Emplacement du Kit de développement logiciel Android** , collez dans ce chemin d’accès, puis cliquez sur **OK**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans Visual Studio pour Mac, accédez à **Préférences > projets > emplacements du SDK > Android**. Dans le **Android** , cliquez sur le **emplacements** onglet pour afficher et définir l’emplacement du Kit de développement logiciel :

[![Onglet emplacements exemple préférences](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

L’emplacement par défaut pour chaque chemin d’accès est comme suit :

- Emplacement du Kit de développement logiciel Android : 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Emplacement du NDK Android : 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Emplacement du Kit de développement logiciel (JDK) Java : 

    **/usr**

Notez que le numéro de version de la NDK peut-être varier. Par exemple, au lieu de **android-ndk-r14b**, il peut être une version antérieure, tel que **android-ndk-r10e**.

Pour définir l’emplacement du SDK Android, entrez le chemin d’accès complet du répertoire du SDK Android dans le **emplacement du Kit de développement logiciel Android** boîte. Vous pouvez sélectionner le dossier SDK Android dans l’outil de recherche, appuyez sur **CTRL +&#8984;+ I** pour afficher les informations de dossier, cliquez et faites glisser de chemin d’accès à droite de **où :**, copier, puis collez-le dans le **du SDK Android Emplacement** zone le **emplacements** onglet. Par exemple, si votre emplacement du SDK Android se trouve **~/Library/Developer/Android/Sdk**, désactivez l’ancien chemin d’accès dans le **emplacement du Kit de développement logiciel Android** , collez dans ce chemin d’accès, puis cliquez sur **OK**.

-----
