---
title: Backgrounding
description: "Arrière-plan de traitement ou backgrounding est le processus de laisser les applications à effectuer des tâches en arrière-plan pendant une autre application s’exécute dans le premier plan. Ce guide constitue une introduction aux e/s de traitement en arrière-plan."
ms.topic: article
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 2bba7c0908fb78ca199cc654adad645afaf47a02
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="backgrounding"></a>Backgrounding

_Arrière-plan de traitement ou backgrounding est le processus de laisser les applications à effectuer des tâches en arrière-plan pendant une autre application s’exécute dans le premier plan. Ce guide constitue une introduction aux e/s de traitement en arrière-plan._

Backgrounding dans les applications mobiles est fondamentalement différente que le concept traditionnel de multitâche sur le bureau. Les ordinateurs de bureau disposent de ressources disponibles pour une application, y compris l’espace d’écran, la puissance et la mémoire. Les applications sont en mesure de s’exécuter côte à côte et reste performant et utilisable. Sur un appareil mobile, les ressources sont beaucoup plus limités. Il est difficile d’afficher plusieurs applications sur un petit écran et plusieurs applications en cours d’exécution à pleine vitesse serait décharger la batterie. Backgrounding est un compromis constant entre accorder les ressources pour exécuter des tâches d’arrière-plan qu’ils requièrent pour effectuer correctement des applications et que l’application foregrounded et le périphérique reste réactive. IOS et Android sont de backgrounding, mais ils le gérer de manière très différente.

Dans iOS, backgrounding est reconnu comme un état de l’application et les applications sont déplacées vers et depuis l’état de l’arrière-plan en fonction du comportement de l’application et de l’utilisateur. iOS offre également plusieurs options pour associer une application à exécuter en arrière-plan, y compris en demandant le système d’exploitation de temps pour terminer une tâche importante, fonctionne comme un type de l’application connue nécessaire à l’arrière-plan, et l’actualisation du contenu d’une application à désigné intervalles.

Dans ce guide et accompagnant les procédures pas à pas, nous allons apprendre à effectuer les tâches de l’application en arrière-plan. Nous couvrent les concepts clés et les meilleures pratiques et puis passer par la création d’une application du monde réel qui reçoit des mises à jour de l’emplacement en arrière-plan.

## <a name="contents"></a>Sommaire

1.  [Introduction à Backgrounding dans iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Démonstration de cycle de vie d’application](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniques de Backgrounding iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Procédures pas à pas : Backgrounding dans iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS Backgrounding Guide](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons présenté les différentes façons d’effectuer un traitement en arrière-plan dans iOS. Nous couverts iOS États de l’Application et examiner le rôle backgrounding est lu dans le cycle de vie des applications iOS. En outre, nous l’avons vu comment nous pouvons enregistrer des tâches ou des applications entières à fonctionner en arrière-plan dans iOS. Enfin, nous avons renforcé notre compréhension de backgrounding sur iOS en générant des applications qui effectuent des mises à jour en arrière-plan.



## <a name="related-links"></a>Liens associés

- [Backgrounding sur Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (sample)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Emplacement (exemple)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Simple transfert en arrière-plan (exemple)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [Exécution de l’arrière-plan d’iOS](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
