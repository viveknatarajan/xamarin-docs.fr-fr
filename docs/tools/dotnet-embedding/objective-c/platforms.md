---
title: Plateformes objective-C
description: Ce document décrit les différentes plateformes .NET incorporation peut cibler lorsque vous travaillez avec le code Objective-C. Elle explique macOS, iOS, tvOS et watchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 6eeb776959d1a2a37d67bfae6603971d0e5a22b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793889"
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
