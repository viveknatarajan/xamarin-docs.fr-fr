---
title: Xamarin.Mac registrar
description: Ce document décrit l’objectif du bureau d’enregistrement Xamarin.Mac et ses configurations d’utilisation différentes.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 4b70ac2271b23b54e7942fdc870e0f49548e6154
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_Ce document décrit l’objectif du bureau d’enregistrement Xamarin.Mac et ses configurations d’utilisation différentes._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Mac réduit l’écart entre l’environnement managé (.NET) et l’exécution de/Cocoa, ce qui permet des classes managées pour appeler les classes Objective-C non managés et être appelée lorsque les événements se produisent. Le travail requis pour effectuer cet « magique » est géré par le bureau d’enregistrement et est, en règle générale, masqué.

Il existe des conséquences sur les performances de l’enregistrement, en particulier sur le démarrage de l’application le temps, et comprendre un bit, ce qui se passe « sous le capot » peut parfois être utile.

## <a name="configurations"></a>Configurations

Fondamentalement, tâche du bureau d’enregistrement au démarrage peut être divisée en deux catégories :

- Analyse de chaque classe managée pour ceux qui dérivent de NSObject et collectez une liste des éléments à être exposés au runtime Objective-C.
- Enregistrer ces informations avec le runtime Objective-C.

Au fil du temps, les trois configurations autre bureau d’enregistrement ont été créées pour couvrir les cas d’utilisation différentes. Chacune a build différente et exécutez les conséquences de l’heure :

- **Bureau d’enregistrement dynamique** : lors du démarrage, utiliser la réflexion .NET pour chaque type chargé d’analyse, de déterminer la liste d’éléments appropriés et informe le runtime natif. Cette option ajoute aucun temps à la build, mais est très coûteuse à calculer lors du lancement (jusqu'à plusieurs secondes).
- **Bureau d’enregistrement statique** – lors de la génération, le jeu d’éléments à être enregistré et générer le code Objective-C pour gérer l’inscription de calcul. Ce code est appelé lors du démarrage pour inscrire rapidement tous les éléments. Ajoute qu'une pause significative pour la build mais peut coupe un temps considérable de démarrage de l’application.
- **« Partielle » statique** – une approche « hybride » plus récente qui place la plupart des avantages des deux. Depuis les exportations de **Xamarin.Mac.dll** constantes, enregistrez une bibliothèque précalculée pour gérer leur inscription et les lier que dans. Utiliser la réflexion pour gérer les bibliothèques utilisateur, mais en tant que bibliothèques utilisateur exporter beaucoup moins de types que les liaisons de plateforme cela s’avère souvent assez rapide. Un négligeable générer l’impact du temps et de réduire une grande majorité du « coût » dynamique.

Aujourd'hui statique partielle est la valeur par défaut pour la configuration Debug et statique est la valeur par défaut pour les configurations Release.

Il existe certains scénarios :

- Plug-ins chargement après le lancement avec les classes dérivées de NSObject
- Créés dynamiquement des instances de classe dérivant de NSObject

Lorsque le bureau d’enregistrement ne peut pas savoir qu’il doit inscrire un type au démarrage. Le `ObjCRuntime.Runtime.RegisterAssembly` méthode est fournie pour informer le bureau d’enregistrement qu’il possède des types supplémentaires à prendre en compte.
