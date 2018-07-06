---
title: Exemple du monde réel à l’aide d’un projet Xcode
description: Ce document décrit comment utiliser un projet Xcode comme une entrée directe d’objectif Sharpie, ce qui simplifie le processus de création de liaisons c# pour le code Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855245"
---
# <a name="real-world-example-using-an-xcode-project"></a>Exemple du monde réel à l’aide d’un projet Xcode

**Cet exemple utilise le [bibliothèque POP à partir de Facebook](https://github.com/facebook/pop).**

Nouveau dans la version 3.0, objectif Sharpie prend en charge les projets Xcode en tant qu’entrée. Ces projets spécifient les fichiers d’en-tête approprié et les indicateurs de compilateur nécessaires pour compiler la bibliothèque native et par conséquent nécessaires de lier de trop. Objectif Sharpie sélectionnera le premier _cible_ et sa configuration par défaut d’un projet si ne pas demandé de faire autrement.

Avant de l’objectif Sharpie tente d’analyser les fichiers d’en-tête et de projet, il doit le générer. Projets ont souvent des phases de la build qui seront structure correctement les fichiers d’en-tête pour une consommation externe et l’intégration, il est donc préférable de toujours générer le projet complet avant de tenter de lier.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>Liens associés

- [Cours de l’Université de Xamarin : Génération d’une bibliothèque de liaisons Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University cours : Générer une bibliothèque de liaisons Objective-C avec Sharpie objectif](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)