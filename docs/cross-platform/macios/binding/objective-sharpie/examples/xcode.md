---
title: Exemple du monde réel à l’aide d’un projet Xcode
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: a90d2fdee353057126fe804a4c20032dad1a3057
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="real-world-example-using-an-xcode-project"></a>Exemple du monde réel à l’aide d’un projet Xcode


**Cet exemple utilise le [bibliothèque POP à partir de Facebook](https://github.com/facebook/pop).**

Nouveau dans la version 3.0, objectif Sharpie prend en charge des projets Xcode en tant qu’entrée. Ces projets spécifient les fichiers d’en-tête approprié et les indicateurs de compilateur nécessaires pour compiler la bibliothèque native et donc nécessaires de lier de trop. Sélectionne le premier objectif Sharpie _cible_ et sa configuration par défaut d’un projet si ne pas invité à le faire autrement.

Avant de l’objectif Sharpie tente d’analyser les fichiers d’en-tête et de projet, il doit le générer. Les projets ont souvent des phases de build correctement la structure des fichiers d’en-tête pour la consommation externe et l’intégration, il est donc préférable de toujours générer le projet complet avant de tenter de le lier.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

