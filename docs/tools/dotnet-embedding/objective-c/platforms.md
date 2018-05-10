---
title: Plateformes objective-C
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8c48a14e5d58f95bf93363e0b354fcf3afd94b44
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="objective-c-platforms"></a>Plateformes objective-C

Incorporation de .NET peuvent cibler différentes plateformes lors de la génération de code Objective-C :

* macOS
* iOS
* tvOS
* watchOS [ne pas encore implémentée]

La plateforme est sélectionnée en passant le `--platform=<platform>` un argument de ligne de commande à l’incorporation de .NET.

Lors de la génération pour iOS, tvOS et watchOS plateformes, .NET incorporation créera toujours une infrastructure qui incorpore Xamarin.iOS, étant donné que Xamarin.iOS contient beaucoup de code de prise en charge de runtime qui est requis sur ces plateformes.

Toutefois, lors de la génération pour la plate-forme macOS, il est possible de choisir si le framework généré doit incorporer Xamarin.Mac ou non. Il est possible de n'incorporer pas Xamarin.Mac si l’assembly dépendant ne fait pas référence Xamarin.Mac.dll (directement ou indirectement), cette option est sélectionnée en passant `--platform=macOS` à l’outil .NET incorporation.

Si l’assembly dépendant contient une référence à Xamarin.Mac.dll, il est nécessaire incorporer Xamarin.Mac, et en outre l’embeddinator doit connaître le framework cible à utiliser.

Il existe trois infrastructures de cible Xamarin.Mac possibles : `modern` (précédemment appelé `mobile`), `full` et `system` (la différence entre chacun d’eux est décrit dans Xamarin.Mac [framework cible] [ 1] documentation), et chacun est sélectionné en passant `--platform=macOS-modern`, `--platform=macOS-full` ou `--platform=macOS-system` à l’outil .NET incorporation.

[1]: ~/mac/platform/target-framework.md
