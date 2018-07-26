---
title: Introduction au Backgrounding dans iOS
description: 'Ce document décrit backgrounding dans iOS : États de l’application, les méthodes de cycle de vie des applications et actualisation d’application en arrière-plan.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 804a99817f664989bbac67a4c662357f4ee628c5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242275"
---
# <a name="introduction-to-backgrounding-in-ios"></a>Introduction au Backgrounding dans iOS

iOS régule très étroitement de traitement en arrière-plan et propose trois approches pour l’implémenter :

-  **Inscrire une tâche en arrière-plan** -si une application a besoin terminer une tâche importante, elle peut demander à iOS éviter d’interrompre la tâche lorsque l’application passe en arrière-plan. Par exemple, une application peut avoir besoin terminer la connexion d’un utilisateur ou terminer le téléchargement d’un fichier volumineux.
-  **Enregistrer comme Application nécessaire à l’arrière-plan** -une application peut s’inscrire comme un type spécifique d’application qui a connu, les exigences de backgrounding spécifiques, tels que *Audio* , *VoIP* ,  *Accessoire externe* , *Newsstand* , et *emplacement* . Ces applications sont autorisées en arrière-plan continue des privilèges de traitement tant qu’ils exécutent des tâches qui se trouvent dans les paramètres du type d’application inscrits.
-  **Activer les mises à jour d’arrière-plan** -Applications peuvent déclencher des mises à jour en arrière-plan avec *région surveillance* ou en écoutant les *emplacement des modifications significatives* . À compter d’iOS 7, les applications peuvent également s’inscrire pour mettre à jour le contenu de l’arrière-plan à l’aide *récupération en arrière-plan* ou *Notifications à distance* .


## <a name="application-states-and-application-delegate-methods"></a>États de l’application et les méthodes de délégué d’Application

Avant de nous plonger dans le code pour iOS de traitement en arrière-plan, nous devons comprendre comment backgrounding a une incidence sur le cycle de vie d’une application iOS.

Le cycle de vie des applications iOS est une collection d’états de l’application et les méthodes de déplacement entre eux. Une application effectue la transition entre États, selon le comportement de l’utilisateur et les exigences de backgrounding de l’application. Le déplacement est illustré par le diagramme suivant :

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Diagramme des États de l’application et les méthodes de délégué d’Application")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Ne pas en cours d’exécution** -l’application n’a pas encore été lancée sur l’appareil.
-  **En cours d’exécution/actif** -l’application se trouve sur l’écran et exécute le code au premier plan.
-  **Inactif** -l’application est interrompue par un appel téléphonique entrant, texte ou autre interruption.
-  **Lancé en arrière-plan** -l’application se déplace dans l’arrière-plan et poursuit l’exécution de code d’arrière-plan.
-  **Suspended** : si l’application n’a pas de code à exécuter en arrière-plan, ou si tout le code est terminé, l’application sera *Suspended* par le système d’exploitation. Les processus d’une application suspendue ne sont maintenu actif, mais l’application est incapable d’exécuter du code dans cet état.
-  **Retourner à pas en cours d’exécution/de l’arrêt (Rare)** : parfois, le processus d’application est détruit, et l’application revienne à la *pas en cours d’exécution* état. Cela se produit dans les situations de mémoire insuffisante, ou si l’utilisateur ferme manuellement l’application.


Depuis l’introduction de la prise en charge du multitâche, iOS rarement met fin à des applications inactives et conserve à la place de leurs processus *Suspended* en mémoire. En conservant un processus d’application actif garantit que l’application est lancée rapidement la prochaine fois que l’utilisateur l’ouvre. Cela signifie également que les applications peuvent se déplacer librement à partir de la *Suspended* état dans le *Backgrounded* état sans dessiner sur les ressources système. iOS 7 exploite cette fonctionnalité avec les nouvelles API qui permettent aux applications de suspendre des tâches en arrière-plan lorsque l’appareil se met en veille, contenu de mise à jour directement à partir de l’arrière-plan sans intervention de l’utilisateur et bien plus encore. Nous allons aborder les nouvelles API dans [Techniques de Backgrounding iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Méthodes de cycle de vie des applications

Lorsqu’une application change d’état, iOS indique à l’application via des méthodes d’événement dans le `AppDelegate` classe :

-  `OnActivated` -C’est appelée la première fois que l’application est lancée, et chaque fois que l’application revient au premier plan. Il s’agit de l’endroit où placer le code qui doit s’exécuter chaque fois que l’application est ouverte.
-  `OnResignActivation` -Si l’utilisateur reçoit une interruption tel qu’un texte ou d’un appel téléphonique, cette méthode est appelée et l’application est temporairement désactivé. L’utilisateur doit accepter l’appel téléphonique, l’application sera être transmise à l’arrière-plan.
-  `DidEnterBackground` -Appelée lorsque l’application passe à l’état backgrounded, cette méthode donne à une application environ cinq secondes pour vous préparer à l’abandon possible. Utilisez cette fois pour enregistrer les tâches et les données utilisateur et supprimer les informations sensibles à partir de l’écran.
-  `WillEnterForeground` -Lorsqu’un utilisateur renvoie à une application lancé en arrière-plan ou suspendue et il démarre au premier plan, `WillEnterForeground` est appelée. Il s’agit le temps de préparer l’application en premier plan par réalimentation tout état enregistré pendant `DidEnterBackground` .  `OnActivated` sera appelée immédiatement après la fin de cette méthode.
-  `WillTerminate` -L’application est arrêtée, et son processus est détruit. Cette méthode est appelée uniquement si le multitâche n’est pas disponible sur l’appareil ou la version du système d’exploitation, si la mémoire est insuffisante ou si l’utilisateur ferme manuellement une application backgrounded. Notez que les applications suspendues arrêtées n’appellera pas `WillTerminate` .


Le diagramme suivant illustre la façon dont les États de l’application et les méthodes de cycle de vie s’imbriquent :

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Ce diagramme illustre la façon dont les États de l’application et les méthodes de cycle de vie s’imbriquent")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Contrôles utilisateur pour Backgrounding dans iOS

iOS 7 a introduit plusieurs fonctionnalités pour donner aux utilisateurs davantage de contrôle sur l’état backgrounded d’une application. Le sélecteur d’application et le paramètre Actualiser l’application arrière-plan affectent le cycle de vie d’Application.

### <a name="app-switcher"></a>Sélecteur d’application

Le sélecteur d’application est une fonctionnalité importante du contrôle introduite dans iOS 7. Il est lancé par double-clic avec le **accueil** bouton et affiche les applications dont les processus sont actifs :

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Déplacement entre des applications à l’aide du sélecteur d’application")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

À l’aide du sélecteur d’application, les utilisateurs peuvent faire défiler des captures instantanées de toutes les applications suspendues et lancé en arrière-plan. En appuyant sur une application, il démarre au premier plan. Faire glisser des supprime l’application de l’arrière-plan, met fin à son processus. Nous allons examiner plus en détail le sélecteur d’application dans le [iOS démonstration de cycle de vie d’Application](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) dans la section suivante.

> [!IMPORTANT]
> Le sélecteur d’application n’affiche pas de différence entre les applications suspendues et lancé en arrière-plan.



### <a name="background-app-refresh-settings"></a>Paramètres d’actualisation de l’application en arrière-plan

iOS 7 augmente utilisateur de contrôler le cycle de vie des applications en permettant aux utilisateurs de refuser de backgrounding pour les applications [inscrit pour le traitement en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Cela n’empêche pas l’exécution des tâches en arrière-plan d’applications*.

Les utilisateurs peuvent modifier ce paramètre en accédant à <span class="uiitem">Paramètres > Général > Actualiser l’application arrière-plan</span> et modifier les privilèges backgrounding pour une application sélectionnée. Si l’application d’actualisation en arrière-plan est définie à off, l’application sera suspendue immédiatement lors de l’entrée de l’arrière-plan et a empêché d’effectuer tout traitement en arrière-plan :

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Paramètres d’actualisation de l’application en arrière-plan")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Les développeurs peuvent vérifier l’état de l’Application d’actualisation en arrière-plan avec le `BackgroundRefreshStatus` API. Pour obtenir un exemple, reportez-vous à la [Recipe (Recette) du paramètre d’actualisation en arrière-plan vérifier](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting).

Nous avons couvert les principes fondamentaux de l’iOS cycle de vie des applications et les fonctionnalités permettant de contrôler le cycle de vie d’Application. Ensuite, nous allons voir le cycle de vie des applications iOS en action.

