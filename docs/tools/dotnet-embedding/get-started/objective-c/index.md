---
title: Prise en main de Objective-C
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 0c2d92f52000bbc6d9d4ea3b07112795aa98bd0a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-objective-c"></a>Prise en main de Objective-C

Il s’agit de la page de démarrage pour Objective-C, qui couvre les principes de base pour toutes les plateformes prises en charge.

## <a name="requirements"></a>Spécifications

Pour utiliser .NET incorporation avec Objective-C, vous devez un Mac en cours d’exécution :

* macOS 10.12 (Sierra) ou version ultérieure
* Xcode 8.3.2 ou version ultérieure
* [Mono 5.0](http://www.mono-project.com/download/)

Vous pouvez installer [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) pour modifier et compiler votre code c#.

> [!NOTE]
> * Les versions antérieures de macOS, Xcode et Mono _peut_ le travail, mais non testé et non pris en charge
> * Génération de code peut être effectuée sur Windows, mais il n’est possible de compiler sur un ordinateur Mac où Xcode est installé

## <a name="installing-net-embedding-from-nuget"></a>L’installation de .NET incorporation de NuGet

Suivez ces [instructions](~/tools/dotnet-embedding/get-started/install/install.md) pour installer et configurer l’incorporation de .NET pour votre projet.

Exemple d’un appel de commande est répertorié dans le [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) et [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) guides de mise en route.

## <a name="platforms"></a>Plateformes

Objective-C est un langage qui est couramment utilisé pour écrire des applications pour Mac OS, iOS, tvOS et watchOS ; .NET incorporation prend en charge toutes les plateformes. Utilisation de chaque plateforme implique certaines [différences clés et ces sont expliquées ici](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Création d’une application macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) est plus simple, car elle n’implique pas autant d’étapes supplémentaires, telles que la configuration d’identité, les profils provisining, simulateurs et appareils. Il est conseillé de commencer par le document macOS précédant celle pour iOS.

### <a name="ios--tvos"></a>iOS / tvOS

Vérifiez que vous sont déjà configuré pour développer des applications iOS avant de tenter d’en créer un à l’aide de l’incorporation de .NET. Le [suivant instructions](~/tools/dotnet-embedding/get-started/objective-c/ios.md) supposent que vous avez déjà créé et déployé une application iOS de votre ordinateur.

Prise en charge de tvOS est analogue au fonctionnement des e/s, en utilisant des projets de tvOS dans l’IDE (Visual Studio et Xcode) au lieu des projets iOS.

> [!NOTE]
> Prise en charge de watchOS seront disponibles dans une version ultérieure et sera très similaire pour iOS/tvOS.

## <a name="further-reading"></a>Informations supplémentaires

* [Fonctionnalités d’incorporation de .NET spécifiques pour Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Meilleures pratiques pour Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Limitations de l’incorporation de .NET](~/tools/dotnet-embedding/limitations.md)
* [Contribue au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
* [Plateformes cibles](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Liens connexes

- [Exemple de météo (iOS et Mac OS)](https://github.com/jamesmontemagno/embeddinator-weather)
