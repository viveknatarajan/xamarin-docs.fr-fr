---
title: Prise en main de Objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 832ad2e6d462b39df7fa4a45739e97f7a7d6e5b5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-objective-c"></a>Prise en main de Objective-C

Il s’agit de la page de démarrage pour Objective-C, qui couvre les principes de base pour toutes les plateformes prises en charge.


## <a name="requirements"></a>Configuration requise

Pour utiliser .NET incorporation avec Objective-C, vous devez un Mac en cours d’exécution :

* macOS 10.12 (Sierra) ou version ultérieure
* Xcode 8.3.2 ou version ultérieure
* [Mono 5.0](http://www.mono-project.com/download/)

Vous pouvez installer [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) pour modifier et compiler votre code c#.


Remarques :

* Les versions antérieures de macOS, Xcode et Mono _peut_ le travail, mais non testé et non pris en charge ;
* Génération de code peut être effectuée sur Windows, mais il n’est possible de compiler sur un ordinateur Mac où Xcode est installé ;


## <a name="installation"></a>Installation

L’étape suivante consiste à télécharger et installer l’incorporation de .NET sur votre Mac.

* [Package](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [Notes de publication](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

En guise d’alternative, il est possible de le générer à partir de notre [référentiel git](https://github.com/mono/Embeddinator-4000/tree/objc), consultez la [contribuant](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) document pour obtenir des instructions.

Le programme d’installation est un programme d’installation standard pkg en fonction :

![Présentation du programme d’installation](images/install1.png)
![programme d’installation installe le Type](images/install2.png)
![résumé du programme d’installation](images/install3.png)

Une fois installée via le programme d’installation, une fois que vous démarrez une nouvelle session de Terminal Server, vous pouvez utiliser la `objcgen` commande.
Dans le cas contraire, vous pouvez toujours exécuter l’outil via son chemin d’accès absolu : `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`.

## <a name="platforms"></a>Plateformes

Objective-C est un langage qui est couramment utilisé pour écrire des applications pour Mac OS, iOS, tvOS et watchOS ; et l’embeddinator prend en charge toutes les plateformes. Utilisation de chaque plateforme implique des différences clés et ces types sont expliqués [ici](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Création d’une application macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) est plus simple, car elle n’implique pas autant d’étapes supplémentaires, telles que la configuration d’identité, les profils provisining, simulateurs et appareils. Il est conseillé de commencer par le document macOS précédant celle pour iOS.

### <a name="ios--tvos"></a>iOS / tvOS

Assurez-vous que vous sont déjà configuré pour développer des applications iOS avant d’essayer de créer un à l’aide de l’embeddinator. Le [suivant instructions](~/tools/dotnet-embedding/get-started/objective-c/ios.md) supposent que vous avez déjà créé et déployé une application iOS de votre ordinateur.

Prise en charge de tvOS est analogue au fonctionnement des e/s, en utilisant des projets de tvOS dans l’IDE (Visual Studio et Xcode) au lieu des projets iOS.

> [!NOTE]
> Remarque : La prise en charge de watchOS sera disponible dans une version ultérieure et sera très similaire pour iOS/tvOS.


## <a name="further-reading"></a>informations supplémentaires

* [Fonctionnalités d’incorporation de .NET spécifiques pour Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Meilleures pratiques pour Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Limitations de l’incorporation de .NET](~/tools/dotnet-embedding/limitations.md)
* [Contribue au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
* [Plateformes cibles](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>Liens associés

- [Exemple de météo (iOS et Mac OS)](https://github.com/jamesmontemagno/embeddinator-weather)
