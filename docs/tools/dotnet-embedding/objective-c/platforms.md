---
title: Plateformes objective-C
description: Ce document décrit les différentes plateformes .NET incorporation pouvant cibler lorsque vous travaillez avec le code Objective-C. Il aborde macOS, iOS, tvOS et watchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 8091fb4e8328f61f1471d061b51b4735de3c089c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230722"
---
# <a name="objective-c-platforms"></a>Plateformes objective-C

Incorporation .NET peut cibler différentes plateformes lors de la génération de code Objective-C :

* macOS
* iOS
* tvOS
* watchOS [ne pas encore implémenté]

La plateforme est sélectionnée en passant le `--platform=<platform>` argument de ligne de commande à l’incorporation de .NET.

Lorsque vous générez pour iOS, tvOS et watchOS des plateformes, incorporation .NET crée toujours une infrastructure qui incorpore Xamarin.iOS, étant donné que Xamarin.iOS contient beaucoup de code de prise en charge de runtime qui est requis sur ces plateformes.

Toutefois, lors de la génération pour la plateforme macOS, il est possible de choisir si le framework généré doit incorporer Xamarin.Mac ou non. Il est possible de ne pas incorporer Xamarin.Mac si l’assembly dépendant ne fait pas référence Xamarin.Mac.dll (directement ou indirectement), et que cette option est sélectionnée en passant `--platform=macOS` à l’outil d’incorporation de .NET.

Si l’assembly dépendant contient une référence à Xamarin.Mac.dll, il est nécessaire incorporer Xamarin.Mac, et en outre l’embeddinator doit savoir quel framework cible à utiliser.

Il existe trois frameworks de cibles de Xamarin.Mac possibles : `modern` (précédemment appelé `mobile`), `full` et `system` (la différence entre chacun d’eux est décrit dans Xamarin.Mac [framework cible] [ 1] documentation), et chacun est sélectionné en passant `--platform=macOS-modern`, `--platform=macOS-full` ou `--platform=macOS-system` à l’outil d’incorporation de .NET.

[1]: ~/mac/platform/target-framework.md
