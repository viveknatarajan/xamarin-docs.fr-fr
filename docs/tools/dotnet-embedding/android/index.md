---
title: .NET incorporation sur Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: lobrien
ms.author: laobri
ms.date: 06/15/2018
ms.openlocfilehash: 6917267896cff796af4e5cff095720eaeccc7652
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215532"
---
# <a name="net-embedding-on-android"></a>.NET incorporation sur Android

Dans certains cas, vous souhaiterez sans doute ajouter une bibliothèque .NET de Xamarin à un projet Android natif existant. Pour ce faire, vous pouvez utiliser la [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/) outil pour transformer votre bibliothèque .NET en une bibliothèque native qui peut être incorporée dans une application Android native basée sur Java.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Exigences de Xamarin.Android

Pour Xamarin.Android travailler avec l’incorporation de .NET, vous devez les éléments suivants :

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure doit être installé.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) ou version ultérieure doit être installé.

-   **Kit de développement Java** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure doit être installé.


## <a name="using-embeddinator-4000"></a>À l’aide de Embeddinator-4000

Pour utiliser une bibliothèque .NET dans un projet Android natif, utilisez les étapes suivantes :

1.  Créer un C# projet de bibliothèque Android.

2.  Installer [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Recherchez **Embeddinator-4000.exe** et ajoutez-le à votre **chemin d’accès**. Exemple :

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Exécutez Embeddinator-4000 sur l’assembly de bibliothèque. Exemple :

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Utilisez le fichier AAR généré dans un projet Java dans Android Studio.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Exigences de Xamarin.Android

Pour Xamarin.Android travailler avec l’incorporation de .NET, vous devez les éléments suivants :

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure doit être installé.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) ou version ultérieure doit être installé.

-   **Kit de développement Java** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure doit être installé.

-   **Mono** &ndash; [5.0 Mono](https://www.mono-project.com/download/) ou version ultérieure doit être installé (mono est installé avec Visual Studio pour Mac).


## <a name="using-embeddinator-4000"></a>À l’aide de Embeddinator-4000

Pour utiliser une bibliothèque .NET dans un projet Android natif, utilisez les étapes suivantes :

1.  Créer un C# projet de bibliothèque Android.

2.  Installer [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Recherchez **Embeddinator-4000.exe** et ajoutez **mono** à votre chemin d’accès. Exemple :

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Exécutez Embeddinator-4000 sur l’assembly de bibliothèque. Exemple :

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Utilisez le fichier AAR généré dans un projet Java dans Android Studio.

-----

Options d’utilisation et de ligne de commande sont décrites dans le [Embeddinator-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) documentation.


## <a name="callbacks"></a>Rappels

En savoir plus sur [des appels entre C# et Java](callbacks.md).

## <a name="samples"></a>Exemples

* [Exemple d’application Météo](https://github.com/jamesmontemagno/embeddinator-weather)
