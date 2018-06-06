---
title: Vue d'ensemble de l'architecture
description: Ce document décrit l’architecture de classeurs de Xamarin, examiner comment l’agent interactif et un client interactif fonctionnent ensemble.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 3cd0d3e8cc47dd7fa43dfa0b910c9f09740fcc9a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794005"
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
