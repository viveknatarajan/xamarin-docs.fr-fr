---
title: Objectif Sharpie
description: Cette section fournit une introduction à l’objectif Sharpie, outil de ligne de commande de Xamarin permettent d’automatiser le processus de création d’une liaison à une bibliothèque Objective-C
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 4341aa7d2ea0bec88e9c36c1804c5a909fd6fd5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="objective-sharpie"></a>Objectif Sharpie

_Cette section fournit une introduction à l’objectif Sharpie, outil de ligne de commande de Xamarin permettent d’automatiser le processus de création d’une liaison à une bibliothèque Objective-C_

- [Vue d’ensemble](#overview) & [historique](#history)
- [Prise en main](get-started.md)
- [Outils et commandes](tools.md)
- [Fonctionnalités](platform/index.md)
- [Exemples](examples/index.md)
- [Procédure pas à pas terminée](~/ios/platform/binding-objective-c/walkthrough.md)
- [Historique des versions](releases.md)

## <a name="overview"></a>Vue d'ensemble

Objectif Sharpie est un outil de ligne de commande permettant de démarrer le premier test d’une liaison.
Il fonctionne en analysant les fichiers d’en-tête d’une bibliothèque native pour mapper l’API publique dans le [définition de liaison](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (processus qui a été effectué précédemment manuellement).

Objectif Sharpie utilise Clang analyse en-tête des fichiers, la liaison est exact et complet que possible. Cela peut réduire considérablement le temps et les efforts que nécessaires pour produire une liaison de qualité.

> [!IMPORTANT]
> Objectif Sharpie est un outil pour les développeurs Xamarin expérimentés possédant des connaissances avancées de Objective-C (et par extension, C). Avant de tenter de lier une bibliothèque Objective-C, vous aurez solides connaissances de la génération de la bibliothèque native sur la ligne de commande (et une bonne compréhension du fonctionne de la bibliothèque native).

## <a name="history"></a>Historique

Nous avons en constante évolution et à l’aide de l’objectif Sharpie en interne à Xamarin pour les trois dernières années. Inscrit à la puissance d’objectif Sharpie, API introduits dans Xamarin.iOS et Xamarin.Mac depuis iOS 8, Mac OS X 10.10 et watchOS 2.0 ont été amorcés entièrement avec Sharpie d’objectif. Xamarin s’appuie fortement sur objectif Sharpie en interne pour la création de ses propres produits.

Toutefois, objectif Sharpie est un outil très avancé qui nécessite une connaissance approfondie de Objective-C et C, comment utiliser le compilateur clang sur la ligne de commande et des bibliothèques natives généralement de la manière dont sont regroupés. En raison de cette barre élevée, il nous semble dont une interface graphique utilisateur Assistant définit les attentes incorrects, et par conséquent, objectif Sharpie est actuellement disponible uniquement comme un outil de ligne de commande.

## <a name="related-links"></a>Liens associés

- [Téléchargement de Sharpie objectif](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Procédure pas à pas : Liaison d’une bibliothèque Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Liaison de bibliothèques Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Détails de liaison](~/cross-platform/macios/binding/overview.md)
- [Guide de référence de Types de liaison](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin pour les développeurs Objective-C](~/ios/get-started/objective-c-developers/index.md)
