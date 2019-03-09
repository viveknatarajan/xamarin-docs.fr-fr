---
title: Utilisation de bibliothèques natives
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1195685db9e85e7fba006272ef300e22d47d1fa6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666356"
---
# <a name="using-native-libraries"></a>Utilisation de bibliothèques natives

Xamarin.Android prend en charge l’utilisation de bibliothèques natives via le mécanisme standard de PInvoke. Vous pouvez également regrouper des bibliothèques natives supplémentaires qui ne font pas partie du système d’exploitation dans votre fichier .apk.

Pour déployer une bibliothèque native avec une application Xamarin.Android, ajoutez la bibliothèque binaire au projet et définissez son **Action de génération** à **AndroidNativeLibrary**.

Pour déployer une bibliothèque native avec un projet de bibliothèque de Xamarin.Android, ajoutez la bibliothèque binaire au projet et définissez son **Action de génération** à **EmbeddedNativeLibrary**.

Notez que dans la mesure où Android prend en charge plusieurs Interfaces de binaire d’Application (ABI), Xamarin.Android doivent savoir pour quelle ABI la bibliothèque native est destiné.
Vous pouvez faire cela de deux façons :

1.  Chemin d’accès « détection »
1.  En utilisant un `AndroidNativeLibrary/Abi` élément dans le fichier projet


Avec la détection de chemin, le nom du répertoire parent de la bibliothèque native est utilisé pour spécifier l’ABI ciblée par la bibliothèque. Par conséquent, si vous ajoutez `lib/armeabi/libfoo.so` au projet, puis l’ABI sera « détectée » en tant que `armeabi`.

Vous pouvez également modifier votre fichier projet pour spécifier explicitement l’ABI à utiliser :

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Pour plus d’informations sur l’utilisation de bibliothèques natives, consultez [assurer l’interopérabilité avec les bibliothèques natives](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2017"></a>Débogage du Code natif avec Visual Studio 2017

Si vous utilisez *Visual Studio 2017* ou version ultérieure, vous n’êtes pas obligé de modifier vos fichiers projet, comme décrit ci-dessus.
Vous pouvez générer et déboguer C++ à l’intérieur de votre solution de Xamarin.Android en ajoutant une référence de projet à C++ **bibliothèque partagée dynamique (Android)** projet. 

Pour déboguer le code C++ natif dans votre projet, procédez comme suit :

1. Double-cliquez sur le projet **propriétés** et sélectionnez le **Options Android** page.
2. Faites défiler jusqu'à **options de débogage**.
3. Dans le **débogueur** menu déroulant, sélectionnez **C++** (au lieu de la valeur par défaut **.Net (Xamarin)**).

Visual Studio C++ les développeurs peuvent voir le [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) échantillonner pour essayer de déboguer C++ à partir de Visual Studio 2017 avec Xamarin ; et reportez-vous à notre [billet de blog](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) pour plus d’informations.



## <a name="related-links"></a>Liens associés

- [SanAngeles_NativeDebug (sample)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Développement d’Applications natives Xamarin Android](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
