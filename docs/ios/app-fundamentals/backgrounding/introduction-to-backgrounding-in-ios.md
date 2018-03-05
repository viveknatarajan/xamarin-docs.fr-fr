---
title: "Introduction à Backgrounding dans iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e46520122b54fabfe27f73370e2ab736a349f7f4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-backgrounding-in-ios"></a>Introduction à Backgrounding dans iOS

iOS régule étroitement de traitement en arrière-plan et offre trois approches pour l’implémentation :

-  **Enregistrer une tâche en arrière-plan** -si une application a besoin effectuer une tâche importante, il peut demander iOS éviter d’interrompre la tâche lors de l’application passe en arrière-plan. Par exemple, une application peut avoir besoin terminer la connexion d’un utilisateur ou terminer le téléchargement d’un fichier volumineux.
-  **Enregistrer comme Application nécessaire à l’arrière-plan** -une application peut enregistrer comme un type spécifique d’application a connu, exigences backgrounding spécifiques, tels que *Audio* , *VoIP* ,  *Accessoire externe* , *kiosque* , et *emplacement* . Ces applications sont autorisées en arrière-plan continue à traiter des privilèges tant qu’ils effectuent des tâches qui se trouvent dans les paramètres du type d’application inscrit.
-  **Activer les mises à jour en arrière-plan** -Applications peuvent déclencher des mises à jour en arrière-plan avec *région analyse* ou en écoutant les *modifications d’emplacement importantes* . À partir d’iOS 7, les applications peuvent également inscrire pour mettre à jour le contenu de l’arrière-plan à l’aide de *arrière-plan Fetch* ou *des Notifications à distance* .


## <a name="application-states-and-application-delegate-methods"></a>États de l’application et les méthodes de délégué d’Application

Avant d’étudier le code pour iOS de traitement en arrière-plan, nous devons comprendre comment backgrounding affecte le cycle de vie d’une application iOS.

Le cycle de vie des applications iOS est une collection d’états de l’application et des méthodes pour se déplacer entre eux. Une application effectue des transitions entre États basées sur le comportement de l’utilisateur et les exigences backgrounding de l’application. Le déplacement est illustré dans le diagramme suivant :

 [ ![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Diagramme des États de l’application et les méthodes de délégué d’Application")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png)

-  **Ne pas en cours d’exécution** -l’application n’a pas encore été lancée sur le périphérique.
-  **En cours d’exécution/Active** -l’application est sur l’écran et exécute le code au premier plan.
-  **Inactif** -l’application est interrompue par un appel téléphonique entrant, le texte ou autres interruption.
-  **Backgrounded** -l’application passe en arrière-plan et poursuit l’exécution de code en arrière-plan.
-  **Suspended** - si l’application n’a pas de code à exécuter en arrière-plan, ou si tout le code est terminée, l’application sera *Suspended* par le système d’exploitation. Les processus d’une application suspendue reste actif, mais l’application ne parvient pas à exécuter n’importe quel code dans cet état.
-  **Retourner à pas en cours d’exécution ou des arrêts (Rare)** : parfois, le processus d’application est détruit, et l’application revienne à la *pas en cours d’exécution* état. Cela se produit dans les situations de mémoire insuffisante, ou si l’utilisateur ferme manuellement l’application.


Depuis l’introduction de prise en charge multitâche, iOS rarement met fin à des applications inactives et conserve à la place de leurs processus *Suspended* en mémoire. En conservant les processus d’une application active garantit que l’application lance rapidement la prochaine fois que l’utilisateur l’ouvre. Cela signifie également que les applications peuvent déplacer librement à partir de la *Suspended* état dans le *Backgrounded* état sans dessiner des ressources système. iOS 7 exploite cette fonctionnalité avec les nouvelles API qui permettre aux applications d’interrompre les tâches en arrière-plan lorsque l’appareil est mis en veille, mise à jour le contenu directement à partir de l’arrière-plan sans intervention de l’utilisateur et bien plus encore. Nous allons aborder les nouvelles API de [Techniques de Backgrounding iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Méthodes de cycle de vie des applications

Quand une application modifie l’état, iOS indique à l’application via les méthodes d’événement dans le `AppDelegate` classe :

-  `OnActivated` -Ceci est appelée la première fois que l’application est lancée, et chaque fois que l’application revient au premier plan. Il s’agit de l’endroit où placer le code qui doit s’exécuter chaque fois que l’application est ouverte.
-  `OnResignActivation` -Si l’utilisateur reçoit une interruption tel qu’un texte ou d’un appel téléphonique, cette méthode est appelée et l’application est temporairement désactivée. L’utilisateur doit accepter l’appel téléphonique, l’application sera adressée à l’arrière-plan.
-  `DidEnterBackground` -Appelée lorsque l’application passe à l’état backgrounded, cette méthode donne à une application environ cinq secondes pour préparer à l’arrêt possible. Utilisez cette fois pour enregistrer les tâches et les données utilisateur et supprimer des informations sensibles de l’écran.
-  `WillEnterForeground` -Lorsqu’un utilisateur retourne à une application backgrounded ou suspendue et qu’il démarre au premier plan, `WillEnterForeground` est appelée. Il s’agit de la durée de préparation de l’application de prendre de premier plan par réalimentation tout état enregistré pendant `DidEnterBackground` .  `OnActivated` sera appelée immédiatement après la fin de cette méthode.
-  `WillTerminate` -L’application s’arrête et la destruction de son processus. Cette méthode est appelée uniquement si les travaux multitâches n’est pas disponible sur l’appareil ou la version du système d’exploitation, si la mémoire est insuffisante ou si l’utilisateur ferme manuellement une application backgrounded. Notez que des applications suspendues interrompues n’appellera pas `WillTerminate` .


Le diagramme suivant illustre la façon dont les États de l’application et les méthodes de cycle de vie s’articulent :

 [ ![](introduction-to-backgrounding-in-ios-images/image2.png "Ce diagramme illustre la façon dont les États de l’application et les méthodes de cycle de vie s’articulent")](introduction-to-backgrounding-in-ios-images/image2.png)

## <a name="user-controls-for-backgrounding-in-ios"></a>Contrôles utilisateur pour Backgrounding dans iOS

iOS 7 a introduit plusieurs fonctionnalités permettant aux utilisateurs plus de contrôle sur l’état de backgrounded d’une application. Le sélecteur de l’application et le paramètre d’actualisation en arrière-plan application affectent le cycle de vie d’Application.

### <a name="app-switcher"></a>Sélecteur d’application

Le sélecteur de l’application est une fonctionnalité de contrôle importantes introduite dans iOS 7. Il est lancé par un double-clic avec le **accueil** bouton et affiche les applications dont les processus sont actives :

 [ ![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Déplacement entre des applications à l’aide du sélecteur de l’application")](introduction-to-backgrounding-in-ios-images/app-switcher-.png)

À l’aide du sélecteur de l’application, les utilisateurs peuvent faire défiler des captures instantanées de toutes les applications backgrounded ou suspendues. En appuyant sur une application, il démarre au premier plan. Le glissement de supprime l’application de l’arrière-plan, met fin à son processus. Nous allons étudier le sélecteur d’application dans le [iOS démonstration de cycle de vie d’Application](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) dans la section suivante.

> [!IMPORTANT]
> **Remarque**: le sélecteur de l’application n’affiche pas de différence entre les applications backgrounded ou suspendues.



### <a name="background-app-refresh-settings"></a>Paramètres d’actualisation de l’application en arrière-plan

iOS 7 augmente le contrôle de l’utilisateur sur le cycle de vie des applications en permettant aux utilisateurs de refuser backgrounding pour les applications [inscrit pour le traitement en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Cela n’empêche pas l’exécution des tâches en arrière-plan d’applications*.

Les utilisateurs peuvent modifier ce paramètre en accédant à <span class="uiitem">Paramètres > Général > Actualiser l’application arrière-plan</span> et modifier les privilèges backgrounding pour une application sélectionnée. Si l’application d’actualisation en arrière-plan est définie à off, l’application est suspendue immédiatement lors de l’entrée de l’arrière-plan et empêchée tout le traitement en arrière-plan :

 [ ![](introduction-to-backgrounding-in-ios-images/settings-.png "Paramètres d’actualisation de l’application en arrière-plan")](introduction-to-backgrounding-in-ios-images/settings-.png)

Les développeurs peuvent vérifier l’état de l’Application d’actualisation en arrière-plan avec le `BackgroundRefreshStatus` API. Pour obtenir un exemple, reportez-vous à la [recette de paramètre d’actualisation en arrière-plan vérifier](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/).

Nous avons couvert les principes fondamentaux de l’iOS cycle de vie des applications et les fonctionnalités pour contrôler le cycle de vie d’Application. Ensuite, vous allez voir le cycle de vie des applications iOS en action.

