---
title: Liaison de bibliothèques d’iOS
description: Ce document décrit comment créer des liaisons c# en code Objective-C, ce qui permet d’utiliser les bibliothèques natives et CocoaPods dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 75c8f9a2eea080c3da031366b314d94929080811
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855219"
---
# <a name="binding-ios-libraries"></a>Liaison de bibliothèques d’iOS

Suivez ces liens pour en savoir plus sur la liaison de bibliothèques Objective-C et CocoaPods pour Xamarin.iOS et Xamarin.Mac :

- [**Vue d’ensemble** ](~/cross-platform/macios/binding/overview.md) -
  décrit le fonctionne de la liaison.
- [**Liaison de bibliothèques Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  obtenir des Instructions sur la façon de lier des bibliothèques Objective-C à utiliser dans les projets Xamarin.
- [**Guide de référence de définition de type** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  décrit tous les attributs disponibles pour les auteurs de liaison pour piloter le processus de génération de liaison.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objectif Sharpie est un outil de ligne de commande pour aider à démarrer la première passe d’une liaison.
Il fonctionne en analysant les fichiers d’en-tête d’une bibliothèque native pour mapper l’API publique dans le [définition de liaison](~/cross-platform/macios/binding/objective-c-libraries.md) (un processus qui est sinon effectué manuellement). Objectif Sharpie ne crée pas d’une liaison par lui-même, mais il peut vous aider à commencer !

Objectif Sharpie 3.0 a introduit la possibilité de lier directement les Cocoapods !

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Procédure pas à pas : liaison d’une bibliothèque Objective-C iOS](walkthrough.md)

Cette page fournit une procédure de création d’un projet de liaison iOS à l’aide de l’open source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) projet Objective-C comme exemple. Le **InfColorPicker** bibliothèque fournit un contrôleur d’affichage réutilisable permettant aux utilisateurs de sélectionner une couleur en fonction de sa représentation sous forme de TSL, effectuer une sélection de couleur plus conviviales.
Objectif Sharpie permet de faciliter le processus de liaison.

## <a name="xamarin-university-lightning-lecture"></a>Xamarin University Lightning Lecture

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS liaisons en C/C++, par [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Liens associés

- [Liaison Objective-C](~/cross-platform/macios/binding/index.md)
- [Liaison du Mac](~/mac/platform/binding.md)
- [Cours de l’Université de Xamarin : Génération d’une bibliothèque de liaisons Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University cours : Générer une bibliothèque de liaisons Objective-C avec Sharpie objectif](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)