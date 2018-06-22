---
title: Quelle version de Xamarin.Android prise en charge l’interface Lollipop ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764463"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Quelle version de Xamarin.Android prise en charge l’interface Lollipop ?

**Remarque :** ce guide a été écrit à l’origine pour la version préliminaire L Android.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) prise en charge Android L aperçu.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) prise en charge l’interface Lollipop Android.

Xamarin uniquement activement prend en charge que la version stable actuelle des outils Xamarin. Les informations ci-dessous sont fournies « en tant que-est » pour les versions antérieures des outils. Pour plus d’informations sur les versions de Xamarin, consultez [ici](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>« Manquant android.jar pour niveau 21 d’API » dans l’aperçu de L Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le message d’erreur suivant (ou similaire) peut s’afficher :

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Ce message signifie que la plateforme du kit SDK Android pour API 21 de niveau n’est pas installée. Effectuer l’installation dans le Gestionnaire de SDK Android (Outils > Open Android SDK Manager...), ou modifier votre projet Xamarin.Android pour cibler une version d’API qui est installée.

Il existe quelques solutions de contournement pour résoudre ce problème :

1. Modifiez votre projet afin qu’il cible API 19 ou inférieure.

2. Renommer votre dossier android-21 21-android vers android-L. (Au mieux, ne doit être utilisée comme solution temporaire, et il peut ne pas fonctionne très bien du tout.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Rétrograder temporairement vers la version d’évaluation au niveau d’API Android 21 « L » [1] :

    1.  Supprimer le **%LocalAppData%\\Android\\android-sdk\\plateformes\\android-21** 
    2.  Extraire [1] dans **C:\\utilisateurs\\<username>\\AppData\\Local\\Android\\android-sdk\\plateformes** pour créer un **android-l** dossier.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le message d’erreur suivant (ou similaire) peut s’afficher :

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Cela signifie que la plateforme du kit SDK Android pour API 21 de niveau n’est pas installée. Effectuer l’installation dans le Gestionnaire de SDK Android (Outils > Gestionnaire de kit de développement logiciel), ou modifier votre projet Xamarin.Android pour cibler une version d’API qui est installée.

Il existe quelques solutions de contournement pour résoudre ce problème :

1. Modifiez votre projet afin qu’il cible API 19 ou inférieure.

2. Renommer votre dossier android-21 21-android vers android-L. (Au mieux, ne doit être utilisée comme solution temporaire, et il peut ne pas fonctionne très bien du tout.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Rétrograder temporairement vers la version d’évaluation au niveau d’API Android 21 « L » [1] :

    1.  Delete **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  Extraire [1] dans **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** pour créer un **android-l** dossier.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
