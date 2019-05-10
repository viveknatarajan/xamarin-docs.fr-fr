---
title: Prise en main Sharpie objectif
description: Ce document fournit une vue d’ensemble de Sharpie objectif, l’outil utilisé pour automatiser la création de C# liaisons au code Objective-C.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 4fb5e503a82a2b666bf6f8d7d7166475e94546e7
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978399"
---
# <a name="getting-started-with-objective-sharpie"></a>Prise en main Sharpie objectif

> [!IMPORTANT]
> Objectif Sharpie est un outil pour les développeurs expérimentés de Xamarin possédant des connaissances avancées de Objective-C (et par extension, C). Avant de tenter de lier une bibliothèque Objective-C, vous devez acquérir des connaissances solides de la création de la bibliothèque native sur la ligne de commande (et une bonne compréhension du fonctionne de la bibliothèque native).

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Installation Sharpie objectif

Objectif Sharpie est actuellement un outil de ligne de commande autonome pour Mac OS X 10.10 et versions ultérieures et _pas un produit entièrement prise en charge de Xamarin_. Il doit uniquement être utilisé par les développeurs expérimentés pour aider à créer un projet de liaison à un 3e partie bibliothèque Objective-C.

Sharpie objectif peut être téléchargé en tant qu’un programme d’installation du package de système d’exploitation X standard.
Exécutez le programme d’installation et de suivre toutes les invites à l’écran à partir de l’Assistant installation :

- **Version actuelle : 3.4**
  - [Télécharger la dernière version](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Annonce de forum](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Utilisez le `sharpie update` commande pour mettre à jour vers la dernière version.

## <a name="basic-walkthrough"></a>Procédure de base

Objectif Sharpie est un outil de ligne de commande fourni par Xamarin qui aide à créer les définitions requises pour lier une bibliothèque de Objective-C 3ème partie à C#.
Même si vous utilisez Sharpie objectif, le développeur *sera* devez modifier les fichiers générés une fois objectif Sharpie terminée pour résoudre les problèmes qui ne peuvent pas être gérées automatiquement par l’outil.

Si possible, objectif Sharpie annote les API avec lesquelles elle a des doutes sur la façon de lier correctement (nombreuses constructions dans le code natif sont ambiguës).
Ces annotations apparaît sous la forme [ `[Verify]` attributs](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

L’objectif Sharpie génère une paire de fichiers - [ `ApiDefinition.cs` et `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -qui peut être utilisé pour créer un projet de liaison qui compile dans une bibliothèque, vous pouvez utiliser dans les applications Xamarin.

> [!IMPORTANT]
> Objectif Sharpie est fourni avec un **majeure** règle pour une utilisation correcte : vous devez absolument lui transmettre les arguments de ligne de commande du compilateur clang correct afin de garantir l’analyse correcte. Il s’agit, car le Sharpie objectif phase d’analyse est simplement un outil [implémentée par rapport à l’API de libtooling clang](http://clang.llvm.org/docs/LibTooling.html).

Cela signifie qu’objectif Sharpie a toute la puissance de Clang (le compilateur C/Objective-C/C++ qui en fait compile la bibliothèque native, que vous devez effectuer la liaison) et tous ses connaissances interne des fichiers d’en-tête pour la liaison.
Au lieu de convertir le texte analysé [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree) au code de l’objet, objectif Sharpie traduit l’AST pour un C# liaison « structurer » approprié pour l’entrée à la `bmac` et `btouch` Xamarin les outils de liaison.

Si objectif Sharpie échoue lors de l’analyse, cela signifie que clang erronées lors de son analyse phase tente de construire l’AST, et vous devez identifier la raison.

**NOUVEAU !** tentatives de la version 3.0 d’adresses cette complexité en prenant en charge les projets Xcode directement. Si une bibliothèque native a un projet Xcode valide, objectif Sharpie peut évaluer le projet pour une cible spécifiée et la configuration de déduire les fichiers d’en-tête d’entrée nécessaires et les indicateurs de compilateur.

Si aucun projet Xcode n’est disponible, vous devez être familiarisé avec le projet en déduire les fichiers d’en-tête d’entrée correcte, de chemins de recherche de fichier en-tête et d’autres indicateurs de compilateur nécessaire. Il est important de savoir que les indicateurs de compilateur utilisés pour générer la bibliothèque native sont identiques qui doivent être passés à un objectif Sharpie. Il s’agit d’un processus plus manuel et celle qui nécessite un peu de vous êtes familiarisé avec la compilation du code natif sur la ligne de commande avec la chaîne d’outils Clang.

**NOUVEAU !** version 3.0 introduit également un outil pour la liaison facilement [CocoaPods](https://cocoapods.org) via la `sharpie pod` commande.
Si la bibliothèque que vous êtes intéressé est disponible en tant qu’un CocoaPod, nous vous recommandons de que commencer par essayer de lier le CocoaPod avec objectif Sharpie (et non pas tenter de lier directement par rapport à la source).
