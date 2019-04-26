---
title: Backgrounding dans Xamarin.iOS
description: Arrière-plan de traitement ou backgrounding consiste à laisser les applications à effectuer des tâches en arrière-plan pendant qu’une autre application est en cours d’exécution au premier plan. Ce guide constitue une introduction à iOS de traitement en arrière-plan.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: a4f5112b6e77ab6e00453c19c766d1e905df1144
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946655"
---
# <a name="backgrounding-in-xamarinios"></a>Backgrounding dans Xamarin.iOS

_Arrière-plan de traitement ou backgrounding consiste à laisser les applications à effectuer des tâches en arrière-plan pendant qu’une autre application est en cours d’exécution au premier plan. Ce guide constitue une introduction à iOS de traitement en arrière-plan._

Backgrounding dans les applications mobiles est fondamentalement différente de celle le concept traditionnel du mode multitâche sur le bureau. Ordinateurs de bureau ont une variété de ressources disponibles pour une application, y compris l’écran, de puissance et de mémoire. Les applications sont en mesure de s’exécuter côte à côte et restent performante et utilisable. Sur un appareil mobile, les ressources sont beaucoup plus limitées. Il est difficile d’afficher plusieurs applications sur un petit écran, et plusieurs applications en cours d’exécution à pleine vitesse serait décharger la batterie. Backgrounding est un constante compromis entre permettant aux applications les ressources pour exécuter les tâches en arrière-plan, qu'ils ont besoin pour effectuer correctement et que l’application foregrounded et l’appareil reste réactive. IOS et Android sont de backgrounding, mais ils le gérer de manière très différente.

Dans iOS, backgrounding est reconnu comme un état de l’application, et applications sont déplacées vers et depuis l’état d’arrière-plan en fonction du comportement de l’application et l’utilisateur. iOS offre également plusieurs options pour associer une application à exécuter en arrière-plan, y compris en demandant au système d’exploitation pour le moment de terminer une tâche importante, fonctionne comme un type d’application d’arrière-plan-nécessaire connu, et l’actualisation du contenu d’une application à désigné intervalles.

Dans ce guide et accompagnant les procédures pas à pas, nous allons apprendre à effectuer des tâches de l’application en arrière-plan. Nous couvrent les concepts clés et les meilleures pratiques et puis examiner la création d’une application réelle qui reçoit des mises à jour de l’emplacement en arrière-plan.

## <a name="contents"></a>Sommaire

1.  [Introduction au backgrounding dans iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Démo du cycle de vie d’une application](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniques de backgrounding iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Procédures pas à pas : Backgrounding dans iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Conseils de backgrounding iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons introduit les différentes façons d’effectuer le traitement en arrière-plan dans iOS. Nous avons couvert iOS États de l’Application et examiné le rôle joué par dans le cycle de vie des applications iOS de backgrounding. En outre, nous avons appris comment nous pouvons enregistrer des tâches ou des applications entières pour fonctionner en arrière-plan dans iOS. Enfin, nous avons renforcé notre compréhension de backgrounding dans iOS en créant des applications qui effectuent des mises à jour en arrière-plan.



## <a name="related-links"></a>Liens associés

- [Backgrounding sur Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (sample)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Emplacement (exemple)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Simple transfert en arrière-plan (exemple)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS exécution en arrière-plan](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
