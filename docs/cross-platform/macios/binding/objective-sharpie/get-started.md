---
title: Prise en main
ms.topic: article
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 01c390af08e59f3b10888a183df7fa6758c2609c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started"></a>Prise en main

<style type="text/css"> .Terminal-bleu {couleur : rgb(10,96,254) ;} .terminal-vert {couleur : rgb(12,156,26) ;} .terminal à magenta {couleur : rgb(152,12,103) ;} </style>


> [!IMPORTANT]
> **Avertissement :** Sharpie objectif est un outil pour les développeurs Xamarin expérimentés possédant des connaissances avancées de Objective-C (et par extension, C). Avant de tenter de lier une bibliothèque Objective-C, vous aurez solides connaissances de la génération de la bibliothèque native sur la ligne de commande (et une bonne compréhension du fonctionne de la bibliothèque native).

<a name="installing" />

# <a name="installing-objective-sharpie"></a>Lors de l’installation Sharpie objectif

Objectif Sharpie est un outil de ligne de commande autonome pour Mac OS X 10.10 et les versions ultérieures et est _pas un produit entièrement prise en charge de Xamarin_. Elle doit uniquement être utilisée par les développeurs expérimentés pour vous aider à créer un projet de liaison à un 3e partie Objective-C bibliothèque.

Objectif Sharpie peut être téléchargé en tant qu’un programme d’installation du package de système d’exploitation X standard.
Exécuter le programme d’installation et suivez toutes les instructions à l’écran de l’Assistant installation :

- **Version actuelle : 3.4**
  - [Télécharger la dernière version](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Annonce de forum](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> 💡 **Conseil :** utiliser le `sharpie update` commande pour mettre à jour vers la version la plus récente.

# <a name="basic-walkthrough"></a>Procédure de base

Objectif Sharpie est un outil de ligne de commande fournie par Xamarin qui aide à créer les définitions requises pour lier une bibliothèque de Objective-C 3e partie à c#.
Même si vous utilisez Sharpie objectif, le développeur *sera* devez modifier les fichiers générés une fois objectif Sharpie terminée pour résoudre les problèmes qui ne peuvent pas être gérées automatiquement par l’outil.

Si possible, objectif Sharpie annoter les API avec lesquelles elle a certains doutes sur la façon de lier correctement (nombreuses constructions en code natif sont ambiguës).
Ces annotations apparaît sous la forme [ `[Verify]` attributs](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

La sortie de l’objectif Sharpie est une paire de fichiers - [ `ApiDefinition.cs` et `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -qui peut être utilisé pour créer un projet de liaison qui compile dans une bibliothèque, vous pouvez utiliser dans des applications Xamarin.

> [!IMPORTANT]
> Objectif Sharpie est fourni avec un **majeure** règle pour une utilisation correcte : vous devez absolument lui transmettre les arguments de ligne de commande du compilateur clang approprié afin de garantir l’analyse correcte. Car le Sharpie d’objectif de l’analyse de phase est simplement un outil [implémentée par rapport à l’API de libtooling clang](http://clang.llvm.org/docs/LibTooling.html).

Cela signifie que Sharpie d’objectif de toute la puissance de Clang (le compilateur C/objectif-C/C++ qui compile en fait de la bibliothèque native, que vous devez effectuer la liaison) et tous ses connaissances internes des fichiers d’en-tête pour la liaison.
Au lieu de convertir analysé [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) au code de l’objet, objectif Sharpie traduit l’AST a un c# liaison « squelette » approprié pour l’entrée à la `bmac` et `btouch` Xamarin les outils de liaison.

Si erreurs objectif Sharpie lors de l’analyse, cela signifie que clang ayant subi une erreur out pendant son analyse phase tente de construire l’AST, et vous devez déterminer pourquoi.

**NOUVEAU !** tentatives de la version 3.0 d’adresses cette complexité en prenant en charge des projets Xcode directement. Si une bibliothèque native a un projet Xcode valide, objectif Sharpie peut évaluer le projet pour une cible spécifiée et la configuration de déduire les fichiers d’en-tête d’entrée nécessaires et les indicateurs de compilateur.

Si aucun projet Xcode n’est disponible, vous devez être familiarisé avec le projet en déduire les fichiers d’en-tête d’entrée correcte, de chemins de recherche de fichier en-tête et d’autres indicateurs de compilateur nécessaires. Il est important de savoir que les indicateurs de compilateur utilisés pour générer la bibliothèque native sont identiques qui doit être passé à l’objectif Sharpie. Il s’agit d’un processus plus manuel et une qui nécessite un peu de connaissance de la compilation du code natif sur la ligne de commande avec la chaîne d’outils Clang.

**NOUVEAU !** version 3.0 introduit également un outil pour la liaison de facilement [CocoaPods](https://cocoapods.org) via la `sharpie pod` commande.
Si la bibliothèque que vous êtes intéressé est disponible en tant qu’un CocoaPod, nous vous recommandons de que commencer par une tentative de lier le CocoaPod avec objectif Sharpie (par opposition à la tentative de lier directement par rapport à la source).