---
title: Exemple du monde réel à l’aide d’un projet Xcode
description: Ce document décrit comment utiliser un projet Xcode comme une entrée directe d’objectif Sharpie, ce qui simplifie le processus de création C# liaisons au code Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ccfc2f1760d8971e2d824cf65344fa2a5e158c12
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978366"
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

