---
title: Une liaison de bibliothèques de Mac pour Xamarin.Mac
description: Ce document liens vers des guides qui décrivent comment travailler avec des liaisons dans une application de Xamarin.Mac, notamment des exemples de code et objectif Sharpie Objective-C.
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/13/2017
ms.openlocfilehash: 05602401c1408b016b371fc00b65f2d3db4c667d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792214"
---
# <a name="binding-mac-libraries-for-xamarinmac"></a>Une liaison de bibliothèques de Mac pour Xamarin.Mac

Suivez ces liens pour en savoir plus sur la liaison des bibliothèques Objective-C sur Xamarin.Mac :

- [**Vue d’ensemble** ](~/cross-platform/macios/binding/overview.md) -
  décrit le fonctionne de la liaison.
- [**Liaison Objective-C bibliothèques** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  obtenir des Instructions sur la façon de lier les bibliothèques Objective-C pour une utilisation dans les projets de Xamarin.
- [**Guide de référence de définition de type** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  décrit tous les attributs disponibles pour les auteurs de la liaison pour piloter le processus de génération de liaison.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objectif Sharpie est un outil de ligne de commande permettant de démarrer le premier test d’une liaison.
Il fonctionne en analysant les fichiers d’en-tête d’une bibliothèque native pour mapper l’API publique dans le [définition de liaison](~/cross-platform/macios/binding/binding-types-reference.md) (processus qui est sinon effectué manuellement). Objectif Sharpie ne crée pas d’une liaison par lui-même, mais il peut vous aider à commencer !

## <a name="examples"></a>Exemples

Reportez-vous à la [exemple de XMBindingExample Mac](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample) pour apprendre à créer une liaison Mac à l’aide de projets de la liaison.

## <a name="related-links"></a>Liens associés

- [Liaison Objective-C](~/cross-platform/macios/binding/index.md)
- [Liaison des bibliothèques d’e/s](~/ios/platform/binding-objective-c/index.md)
