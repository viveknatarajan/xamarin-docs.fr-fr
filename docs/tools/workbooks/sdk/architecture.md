---
title: Vue d'ensemble de l'architecture
description: Ce document décrit l’architecture de Xamarin Workbooks, en examinant le fonctionnement conjoint l’agent interactive et le client interactive.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 352e8d0191512184ffd7d82fa0dfa0bc79fa24ca
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107259"
---
# <a name="architecture-overview"></a>Vue d'ensemble de l'architecture

Xamarin Workbooks comprend deux composants principaux qui doivent travailler conjointement avec eux : _Agent_ et _Client_.

## <a name="interactive-agent"></a>Agent interactif

Le composant Agent est un petit assembly spécifique à la plateforme qui s’exécute dans le contexte d’une application .NET.

Xamarin Workbooks fournit avant génération d’applications « vide » pour un nombre de plateformes, notamment iOS, Android, Mac et WPF. Ces applications hôtes explicitement l’agent.

Pendant l’inspection en direct (Xamarin Inspector), l’agent est injecté via le débogueur de l’IDE dans une application existante dans le cadre du développement régulière & débogage de flux de travail.

## <a name="interactive-client"></a>Client interactif

Le client est un shell natif qui héberge une surface de navigateur web pour présenter l’interface de classeur/REPL (Cocoa sur Mac, WPF sur Windows). À partir d’un point de vue du Kit de développement logiciel, toutes les intégrations de client sont implémentées dans JavaScript et CSS.

Le client est chargé (via Roslyn) pour compiler le code source dans des assemblys de petite et de les transmettre à l’agent connecté pour l’exécution. Résultats de l’exécution sont envoyées au client pour le rendu. Chaque cellule dans un classeur génère un assembly qui référence l’assembly de la cellule précédente.

Comme un agent peut s’exécuter sur n’importe quel type de plate-forme .NET et a accès à quoi que ce soit dans l’application en cours d’exécution, doivent veiller à sérialiser les résultats de manière indépendante de la plateforme.
