---
title: Vue d'ensemble de l'architecture
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 04ed545955b10ed95a15ac0820e4a99b697e0c3e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="architecture-overview"></a>Vue d'ensemble de l'architecture

Les classeurs Xamarin propose deux composants principaux qui doivent fonctionner conjointement avec l’autre : _Agent_ et _Client_.

## <a name="interactive-agent"></a>Agent interactif

Le composant Agent est un assembly spécifique à la plateforme petit qui s’exécute dans le contexte d’une application .NET.

Les classeurs Xamarin fournit des applications de « vides » avant génération pour plusieurs plateformes, telles qu’iOS, Android, Mac et WPF. Ces applications hôtes explicitement l’agent.

Lors de l’inspection dynamique (inspecteur Xamarin), l’agent est injecté via le débogueur de l’IDE dans une application existante dans le cadre du développement régulière & débogage de flux de travail.

## <a name="interactive-client"></a>Client interactif

Le client est un shell natif (/ Cocoa sur Mac, WPF sur Windows) qui héberge une surface de navigateur web pour la présentation de l’interface de classeur/REPL. À partir d’un point de vue du Kit de développement logiciel, toutes les intégrations de client sont implémentées dans JavaScript et CSS.

Le client est chargé (via Roslyn) pour compiler le code source dans des assemblys de petits et de les transmettre à l’exécution de l’agent connecté. Résultats de l’exécution sont envoyées au client pour le rendu. Chaque cellule dans un classeur génère un assembly qui fait référence à l’assembly de la cellule précédente.

Un agent peut s’exécuter sur n’importe quel type de plate-forme .NET et a accès à quoi que ce soit dans l’application en cours d’exécution, doivent veiller à sérialiser les résultats de manière indépendant de la plateforme.