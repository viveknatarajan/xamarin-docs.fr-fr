---
title: Mise en route avec Objective-C
description: Ce document décrit comment commencer à utiliser l’incorporation de .NET avec Objective-C. Il aborde la configuration requise, l’installation de l’incorporation de .NET à partir de NuGet et les plateformes prises en charge.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 92b11db2ee566bcd9f3f8a3ee3771e163a47589b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670218"
---
# <a name="getting-started-with-objective-c"></a>Mise en route avec Objective-C

Il s’agit de la page de démarrage pour Objective-C, qui couvre les notions de base pour toutes les plateformes prises en charge.

## <a name="requirements"></a>Spécifications

Pour utiliser l’incorporation de .NET avec Objective-C, vous aurez besoin d’un Mac en cours d’exécution :

* macOS 10.12 (Sierra) ou version ultérieure
* Xcode 8.3.2 ou version ultérieure
* [Mono 5.0](https://www.mono-project.com/download/)

Vous pouvez installer [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/) pour modifier et compiler votre C# code.

> [!NOTE]
> * Les versions antérieures de macOS, Xcode et Mono _peut_ fonctionne, mais non testés et non pris en charge
> * Génération de code peut être effectuée sur Windows, mais il est uniquement possible de le compiler sur un ordinateur Mac où Xcode est installé

## <a name="installing-net-embedding-from-nuget"></a>Installation d’incorporation de .NET à partir de NuGet

Suivez ces [instructions](~/tools/dotnet-embedding/get-started/install/install.md) pour installer et configurer .NET incorporation pour votre projet.

Exemple d’un appel de commande est répertorié dans le [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) et [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) guides de démarrage.

## <a name="platforms"></a>Plateformes

Objective-C est un langage qui est couramment utilisé pour écrire des applications pour macOS, iOS, tvOS et watchOS ; incorporation .NET prend en charge toutes ces plateformes. Utilisation de chaque plateforme implique certaines [principales différences et ceux-ci sont expliquées ici](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Création d’une application macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) est le plus simple, car elle n’implique pas autant d’étapes supplémentaires, telles que la configuration d’identité, des profils d’approvisionnement, des simulateurs et des appareils. Vous êtes invité à démarrer avec le document de macOS avant celui pour iOS.

### <a name="ios--tvos"></a>iOS / tvOS

Vérifiez que vous êtes déjà configuré pour développer des applications iOS avant d’essayer d’en créer un à l’aide de l’incorporation de .NET. Le [suivant instructions](~/tools/dotnet-embedding/get-started/objective-c/ios.md) supposent que vous avez déjà créé et déployé une application iOS à partir de votre ordinateur.

Prise en charge de tvOS est analogue au fonctionnement d’iOS en utilisant simplement les projets tvOS dans l’IDE (Visual Studio et Xcode) au lieu des projets iOS.

> [!NOTE]
> Prise en charge de watchOS sera disponible dans une version ultérieure et sera très similaire à iOS/tvOS.

## <a name="further-reading"></a>Informations supplémentaires

* [Fonctionnalités d’incorporation de .NET spécifiques et Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Meilleures pratiques pour Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Limitations de l’incorporation de .NET](~/tools/dotnet-embedding/limitations.md)
* [Contribution au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
* [Plateformes cibles](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Liens connexes

- [Exemple de météo (iOS et macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
