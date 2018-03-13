---
title: "À l’aide de bibliothèques natives"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 7bd9a64ab7ea775688225ff5496773647174ebf8
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="using-native-libraries"></a>À l’aide de bibliothèques natives

Xamarin.Android prend en charge l’utilisation de bibliothèques natives via le mécanisme standard de PInvoke. Vous pouvez également regrouper des bibliothèques natives supplémentaires qui ne font pas partie du système d’exploitation dans votre .apk.

Pour déployer une bibliothèque native avec une application de Xamarin.Android, ajoutez la bibliothèque binaire au projet et définissez son **Action de génération** à **AndroidNativeLibrary**.

Pour déployer une bibliothèque native avec un projet de bibliothèque de Xamarin.Android, ajoutez la bibliothèque binaire au projet et définissez son **Action de génération** à **EmbeddedNativeLibrary**.

Notez que puisque Android prend en charge plusieurs Interfaces de binaire d’Application (ABIs), Xamarin.Android doit connaître le ABI la bibliothèque native est construite pour.
Vous pouvez faire cela de deux façons :

1.  Chemin d’accès « détection »
1.  En utilisant un `AndroidNativeLibrary/Abi` élément dans le fichier projet


Avec la détection de chemin, le nom du répertoire parent de la bibliothèque native est utilisé pour spécifier l’ABI ciblée par la bibliothèque. Par conséquent, si vous ajoutez `lib/armeabi/libfoo.so` au projet, puis l’ABI sera être « détection » en tant que `armeabi`.

Vous pouvez également modifier votre fichier projet pour spécifier explicitement l’ABI à utiliser :

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Pour plus d’informations sur l’utilisation de bibliothèques natives, consultez [interagir avec des bibliothèques natives](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2015"></a>Débogage de Code natif avec Visual Studio 2015

Si vous utilisez *Visual Studio 2015*, vous n’êtes pas obligé de modifier vos fichiers projet (comme décrit ci-dessus).
Vous pouvez générer et déboguer C++ à l’intérieur de votre solution de Xamarin.Android, simplement en ajoutant une référence de projet à C++ **bibliothèque partagée dynamique (Android)** projet.

Les développeurs Visual Studio C++ peuvent voir le [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) exemple pour essayer de déboguer C++ à partir de Visual Studio 2015 avec Xamarin ; et reportez-vous à notre [billet de blog](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) pour plus d’informations.



## <a name="related-links"></a>Liens associés

- [SanAngeles_NativeDebug (sample)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
