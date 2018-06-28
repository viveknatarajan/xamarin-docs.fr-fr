---
title: .NET incorporation sur Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067302"
---
# <a name="net-embedding-on-android"></a>.NET incorporation sur Android

Dans certains cas, vous souhaiterez ajouter une bibliothèque .NET de Xamarin à un projet Android natif existant. Pour ce faire, vous pouvez utiliser la [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/) outil pour activer votre bibliothèque .NET dans une bibliothèque native qui peut être incorporée dans une application Android native basée sur Java.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Configuration requise de Xamarin.Android

Pour Xamarin.Android travailler avec l’incorporation de .NET, vous devez les éléments suivants :

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure doit être installé.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) ou version ultérieure doit être installé.

-   **Kit de développement Java** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure doit être installé.


## <a name="using-embeddinator-4000"></a>À l’aide de Embeddinator-4000.

Pour utiliser une bibliothèque .NET dans un projet Android native, procédez comme suit :

1.  Créez un projet de bibliothèque Android c#.

2.  Installer [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Recherchez **Embeddinator-4000.exe** et ajoutez-le à votre **chemin d’accès**. Exemple :

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Sur l’assembly de bibliothèque, exécutez Embeddinator-4000. Exemple :

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Utilisez le fichier AAR généré dans un projet Java dans Android Studio.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Configuration requise de Xamarin.Android

Pour Xamarin.Android travailler avec l’incorporation de .NET, vous devez les éléments suivants :

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure doit être installé.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) ou version ultérieure doit être installé.

-   **Kit de développement Java** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure doit être installé.

-   **Mono** &ndash; [5.0 Mono](http://www.mono-project.com/download/) ou version ultérieure doit être installé (mono est installé avec Visual Studio pour Mac).


## <a name="using-embeddinator-4000"></a>À l’aide de Embeddinator-4000.

Pour utiliser une bibliothèque .NET dans un projet Android native, procédez comme suit :

1.  Créez un projet de bibliothèque Android c#.

2.  Installer [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Recherchez **Embeddinator-4000.exe** et ajoutez **mono** à votre chemin d’accès. Exemple :

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Sur l’assembly de bibliothèque, exécutez Embeddinator-4000. Exemple :

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Utilisez le fichier AAR généré dans un projet Java dans Android Studio.

-----

Options d’utilisation et de ligne de commande sont décrites dans le [Embeddinator-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) documentation.


## <a name="callbacks"></a>Rappels

En savoir plus sur [effectue des appels entre c# et Java](callbacks.md).

## <a name="samples"></a>Exemples

* [Exemple d’application Météo](https://github.com/jamesmontemagno/embeddinator-weather)
