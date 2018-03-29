---
title: Mise en route avec Java
ms.topic: article
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 06df925525c7bc01963274e9e0d5edfe7bb70613
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2018
---
# <a name="getting-started-with-java"></a>Mise en route avec Java


Il s’agit de la page de démarrage pour Java, qui couvre les principes de base pour toutes les plateformes prises en charge.

## <a name="requirements"></a>Spécifications

Pour utiliser .NET incorporation avec Java, vous devez :

* Java version 1.8 ou ultérieure
* [Mono 5.0](http://www.mono-project.com/download/)

Pour Mac :
* Xcode 8.3.2 ou version ultérieure

Pour Windows :
* Visual Studio 2017, avec prise en charge C++
* Windows 10 SDK

Pour Android :
* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) ou version ultérieure
* [Android Studio 3.x](https://developer.android.com/studio/index.html) avec Java 1.8

Vous pouvez utiliser [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) pour modifier et compiler votre code c#.

> [!NOTE]
> Les versions antérieures de Xcode, Visual Studio, Xamarin.Android, Android Studio et Mono _peut_ le travail, mais non testé et non pris en charge.

## <a name="installation"></a>Installation

Incorporation de .NET est actuellement disponible sur [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```csharp
nuget install Embeddinator-4000
```
Cela place `Embeddinator-4000.exe` dans le `packages/Embeddinator-4000/tools` active.

En outre, vous pouvez générer Embeddinator à partir de la source, consultez notre [référentiel git](https://github.com/mono/Embeddinator-4000/) et [contribuant](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) document pour obtenir des instructions.

## <a name="platforms"></a>Plateformes

Java est actuellement dans un état d’aperçu pour macOS, Windows et Android.

La plateforme est sélectionnée en passant le `--platform=<platform>` un argument de ligne de commande pour l’embeddinator. Actuellement `macOS`, `Windows`, et `Android` sont pris en charge.

### <a name="macos-and-windows"></a>macOS et Windows

Pour le développement, doit être en mesure d’utiliser n’importe quel IDE Java qui prend en charge Java 1.8. Vous pouvez même utiliser Android Studio pour cela si vous le souhaitez, [ici](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Vous pouvez utiliser la sortie du fichier JAR comme vous le feriez pour n’importe quel fichier jar de Java standard.

### <a name="android"></a>Android

Vérifiez que vous sont déjà configuré pour développer des applications Android avant de tenter d’en créer un à l’aide de Embeddinator. Le [suivant instructions](~/tools/dotnet-embedding/get-started/java/android.md) supposent que vous avez déjà créé et déployé une application Android à partir de votre ordinateur.

Android Studio est recommandée pour le développement, mais les autres environnements IDE doit fonctionner en tant que la prise en charge pour le [format de fichier AAR](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>informations supplémentaires

* [Mise en route sur Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Rappels sur Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Recherche Android préliminaire](~/tools/dotnet-embedding/android/index.md)
* [Limitations de Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Contribue au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
