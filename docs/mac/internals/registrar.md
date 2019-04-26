---
title: Xamarin.Mac registrar
description: Ce document décrit l’objectif du bureau d’enregistrement de Xamarin.Mac et son statique partielle, statique et dynamique (hybride) des configurations de l’utilisation.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 21e1a2c6ae5a9ad8b6acf520851ea92a340da887
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032495"
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_Ce document décrit l’objectif du bureau d’enregistrement de Xamarin.Mac et sa configuration de l’utilisation diffère._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Mac comble le fossé entre le monde géré (.NET) et le runtime de Cocoa, ce qui permet des classes managées appeler les classes non managées de Objective-C et être rappelé lorsqu’événements se produisent. Le travail requis pour effectuer cette « magiques » est géré par le bureau d’enregistrement et est, en règle générale, masqué.

Impacte les performances de l’enregistrement, en particulier sur le démarrage de l’application le temps, et comprendre un peu de ce qui se passe « en coulisse » peut parfois être utile.

## <a name="configurations"></a>Configurations

Fondamentalement, travail du bureau d’enregistrement au démarrage peut être divisée en deux catégories :

- Analyse de chaque classe managée pour ceux dérivant NSObject et collectez une liste des éléments à être exposés au runtime Objective-C.
- Enregistrer ces informations avec le runtime Objective-C.

Au fil du temps, les trois configurations autre bureau d’enregistrement ont été créées pour couvrir différents cas d’usage. Chacune a build différente et exécutez les conséquences de l’heure :

- **Bureau d’enregistrement dynamique** – lors du démarrage, utilisez la réflexion .NET pour chaque type chargé d’analyse, de déterminer la liste d’éléments appropriés et informer le runtime natif. Cette option ajoute l’heure zéro à la build, mais est très coûteuse à calculer pendant le lancement (jusqu'à plusieurs secondes).
- **Bureau d’enregistrement statique** : lors de la génération, le jeu d’éléments d’être inscrit et générer du code Objective-C pour gérer l’inscription de calcul. Ce code est appelé lors du démarrage à inscrire rapidement tous les éléments. Ajoute qu'une pause importante pour la build, mais peut coupe une quantité significative de temps de démarrage de l’application.
- **« Partielle » statique** – une approche « hybride » plus récente qui offre la plupart des avantages des deux. Depuis les exportations de **Xamarin.Mac.dll** sont constants, enregistrez une bibliothèque précalculée pour gérer leur inscription et de lier que dans. Utiliser la réflexion pour gérer les bibliothèques utilisateur, mais en tant que bibliothèques utilisateur exporter beaucoup moins de types que les liaisons de plate-forme il s’agit souvent assez rapide. Un négligeable générer l’impact du temps et réduit une grande majorité de « coût » dynamique.

Aujourd'hui statique partielle est la valeur par défaut pour la configuration Debug et statique est la valeur par défaut pour les configurations Release.

Il existe certains scénarios :

- Plug-ins chargés après le lancement avec les classes dérivées de NSObject
- Créé dynamiquement des instances de classe dérivant de NSObject

où le bureau d’enregistrement ne peut pas savoir qu’il doit inscrire un type au démarrage. Le `ObjCRuntime.Runtime.RegisterAssembly` méthode est fournie pour informer le bureau d’enregistrement qu’il a des types supplémentaires à prendre en compte.
