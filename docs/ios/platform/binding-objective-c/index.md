---
title: "Liaison iOS bibliothèques"
description: "Comment créer des bibliothèques natives iOS (et CocoaPods) accessible dans les applications Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-ios-libraries"></a>Liaison iOS bibliothèques

_Comment créer des bibliothèques natives iOS (et CocoaPods) accessible dans les applications Xamarin._

Suivez ces liens pour en savoir plus sur la liaison des bibliothèques de Objective-C et CocoaPods pour Xamarin.iOS et Xamarin.Mac :

- [**Vue d’ensemble** ](~/cross-platform/macios/binding/overview.md) -
  décrit le fonctionne de la liaison.
- [**Liaison Objective-C bibliothèques** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  obtenir des Instructions sur la façon de lier les bibliothèques Objective-C pour une utilisation dans les projets de Xamarin.
- [**Guide de référence de définition de type** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  décrit tous les attributs disponibles pour les auteurs de la liaison pour piloter le processus de génération de liaison.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objectif Sharpie est un outil de ligne de commande permettant de démarrer le premier test d’une liaison.
Il fonctionne en analysant les fichiers d’en-tête d’une bibliothèque native pour mapper l’API publique dans le [définition de liaison](~/cross-platform/macios/binding/objective-c-libraries.md) (processus qui est sinon effectué manuellement). Objectif Sharpie ne crée pas d’une liaison par lui-même, mais il peut vous aider à commencer !

Objectif Sharpie 3.0 a introduit la possibilité de lier directement des Cocoapods !

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Procédure pas à pas : liaison d’une bibliothèque de Objective-C iOS](walkthrough.md)

Cette page fournit une procédure pas à pas détaillées de création d’un projet de liaison iOS à l’aide de la source ouverte [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) projet Objective-C comme exemple. Le **InfColorPicker** bibliothèque fournit un contrôleur de vue réutilisables qui permettent à l’utilisateur de sélectionner une couleur en fonction de sa représentation sous forme de TSL, effectuer une sélection de couleur plus conviviaux.
Objectif Sharpie permet de faciliter le processus de liaison.



## <a name="related-links"></a>Liens associés

- [Liaison Objective-C](~/cross-platform/macios/binding/index.md)
- [Liaison de Mac](~/mac/platform/binding.md)
