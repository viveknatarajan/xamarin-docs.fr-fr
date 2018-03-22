---
title: "iOS Backgrounding avec des tâches"
ms.topic: article
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ad75dfac55add7e03ffbdb910e0e62ebd0fd6c18
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="ios-backgrounding-with-tasks"></a>iOS Backgrounding avec des tâches

Pour effectuer la backgrounding sur iOS, le plus simple consiste à décomposer vos exigences backgrounding en tâches et exécuter les tâches en arrière-plan. Tâches sont une limite de temps stricte et obtiennent généralement environ 600 secondes (10 minutes) du temps de traitement une fois une application a été déplacé à l’arrière-plan sur iOS 6 et moins de 10 minutes sur iOS 7 +.

Tâches en arrière-plan peuvent être divisés en trois catégories :

1.  **Les tâches en arrière-plan-Safe** : appelée n’importe où dans le l’application où vous avez une tâche vous ne souhaitez pas interrompu l’application, entrez le l’arrière-plan.
1.  **DidEnterBackground tâches** - appelée pendant la `DidEnterBackground` méthode de cycle de vie d’application pour faciliter le nettoyage et l’enregistrement d’état.
1.  **En arrière-plan transferts (iOS 7 +)** -un type spécial de la tâche d’arrière-plan utilisé pour effectuer les transferts de réseau sur iOS 7. Contrairement aux tâches régulières, les transferts en arrière-plan n’ont pas une limite de temps prédéfinie.


Arrière-plan-safe et `DidEnterBackground` tâches sont sécurisés à utiliser sur iOS 6 et iOS 7, avec quelques différences mineures. Nous allons étudier ces deux types de tâches plus en détail.

## <a name="creating-background-safe-tasks"></a>Créer des tâches d’arrière-plan-Safe

Certaines applications contiennent des tâches qui ne doivent pas être interrompues par iOS doit état de modifier l’application. Un pour protéger ces tâches ne seront pas interrompus consiste à les enregistrer avec iOS en tant que tâches longues. Vous pouvez utiliser ce modèle de n’importe où dans votre application où vous souhaitez une tâche en cours d’interruption doit la placer utilisateur l’application en arrière-plan. Un candidat idéal pour ce modèle serait des tâches telles que de transmettre les informations d’inscription d’un nouvel utilisateur à votre serveur, ou de la vérification des informations de connexion.

L’extrait de code suivant illustre l’inscription d’une tâche à exécuter en arrière-plan :

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Le processus d’inscription des paires une tâche avec un identificateur unique, `taskID`et il encapsule ensuite dans la correspondance `BeginBackgroundTask` et `EndBackgroundTask` appels. Pour générer l’identificateur, nous effectuer un appel à la `BeginBackgroundTask` méthode sur le `UIApplication` de l’objet, et recommencez la tâche d’exécution longue, généralement sur un nouveau thread. Lorsque la tâche est terminée, nous appelons `EndBackgroundTask` et passez le même identificateur. Ceci est important car iOS mettra fin à l’application si une `BeginBackgroundTask` appel n’a pas une correspondance `EndBackgroundTask`.

> [!IMPORTANT]
> Les tâches en arrière-plan-safe peuvent s’exécuter sur le thread principal ou sur un thread d’arrière-plan, en fonction des besoins de l’application.


## <a name="performing-tasks-during-didenterbackground"></a>Exécution de tâches au cours de DidEnterBackground

En plus d’une tâche de longue sécurisée en arrière-plan, l’inscription peut servir à déclencher des tâches comme une application est mises en arrière-plan. e/s fournit une méthode d’événement dans le *AppDelegate* classe appelée `DidEnterBackground` qui peut être utilisé pour enregistrer l’état de l’application, enregistrer les données utilisateur et chiffrer le contenu sensible avant une application passe à l’arrière-plan. Une application a environ cinq secondes à retourner à partir de cette méthode, ou il est interrompu. Par conséquent, les tâches de nettoyage peuvent prendre plus de cinq secondes peuvent être appelées à partir d’à l’intérieur de la `DidEnterBackground` (méthode). Ces tâches doivent être appelés sur un thread distinct.

Le processus est presque identique à celui de l’inscription d’une tâche d’exécution longue. L’extrait de code suivant illustre ce comportement en action :

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Commençons par substitution de la `DidEnterBackground` méthode dans le `AppDelegate`, où nous enregistrons notre tâche via `BeginBackgroundTask` comme dans l’exemple précédent. Ensuite, nous générer un nouveau thread et d’effectuer la tâche de longue. Notez que la `EndBackgroundTask` est maintenant appelé à partir d’à l’intérieur de la tâche d’exécution longue, depuis le `DidEnterBackground` méthode ont déjà retournés.

> [!IMPORTANT]
> iOS utilise un [mécanisme d’horloge de surveillance](http://developer.apple.com/library/ios/qa/qa1693/_index.html) pour vous assurer que l’interface utilisateur d’une application reste réactive. Une application qui utilise trop de temps dans `DidEnterBackground` cesse de répondre dans l’interface utilisateur. Permet de lancer des tâches à exécuter en arrière-plan `DidEnterBackground` à retourner en temps voulu, que l’interface utilisateur reste réactive et empêche l’agent de surveillance de l’arrêt de l’application.


## <a name="handling-background-task-time-limits"></a>Délais de traitement en arrière-plan tâche

iOS place limite stricte sur une tâche en arrière-plan de longue durée peut s’exécuter et, si le `EndBackgroundTask` appel n’est pas effectué dans le délai imparti, l’application va être interrompue. En conservant les backgrounding temps restant et l’utilisation des gestionnaires d’expiration si nécessaire, nous pouvons éviter iOS arrêt de l’application.

### <a name="accessing-background-time-remaining"></a>L’accès à des temps d’arrière-plan restant

Si une application avec des tâches est déplacée à l’arrière-plan, les tâches enregistrées obtenez environ 600 secondes à s’exécuter. Nous pouvons vérifier combien de temps la tâche doit s’effectuer à l’aide de la méthode statique `BackgroundTimeRemaining` propriété de la `UIApplication` classe. Le code suivant donne la durée, en secondes, pendant laquelle la tâche en arrière-plan a quitté :

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Éviter l’arrêt de l’application avec les gestionnaires d’Expiration

En plus de ce qui donne accès à la `BackgroundTimeRemaining` propriété, iOS offre un moyen approprié pour gérer l’expiration du temps en arrière-plan à travers une **expiration du gestionnaire**. Il s’agit d’un bloc facultatif du code qui sera exécuté lorsque le temps alloué à une tâche est sur le point d’expirer. Appelle du code dans le Gestionnaire d’Expiration `EndBackgroundTask` et transmet l’ID de tâche, ce qui indique que l’application se comporte correctement et iOS empêche l’arrêt de l’application même si la tâche s’exécute en dehors de l’heure. `EndBackgroundTask` doit être appelé dans le Gestionnaire d’expiration, ainsi que dans le cours normal de l’exécution. 

Le Gestionnaire d’expiration est exprimé comme une fonction anonyme à l’aide d’une expression lambda, comme illustré ci-dessous :

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

Alors que les gestionnaires d’expiration ne sont pas requis pour l’exécution de code, vous devez toujours utiliser un gestionnaire d’expiration avec une tâche en arrière-plan.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Tâches en arrière-plan dans iOS 7 +

Un changement majeur dans iOS 7 en ce qui concerne les tâches en arrière-plan est pas comment les tâches sont implémentés, mais lorsqu’ils exécutent.

Rappelez-vous que préliminaire iOS 7, une tâche qui s’exécute en arrière-plan est 600 secondes pour terminer. L’une des raisons pour cette limite sont qu’une tâche en cours d’exécution en arrière-plan conservez l’appareil allumé pendant la durée de la tâche :

 [![](ios-backgrounding-with-tasks-images/ios6.png "Graphique de la tâche en conservant l’application iOS préliminaire éveillés 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

le traitement en arrière-plan iOS 7 est optimisé pour une plus grande autonomie. Dans iOS 7, backgrounding devient opportuniste : au lieu de garder l’appareil allumé, tâches respectent lors de l’appareil accède à la mise en veille et à la place effectuer leur traitement en segments lorsque l’appareil sort de veille pour gérer les appels téléphoniques, notifications, entrant des messages électroniques et autres interruptions courantes. Le diagramme suivant fournit un aperçu de la façon dont une tâche peut être rompue des :

 [![](ios-backgrounding-with-tasks-images/ios7.png "Graphique de la tâche divisée en blocs postérieur à iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Étant donné que la tâche d’exécution n’est pas continue de plus, les tâches qui effectuent des transferts réseau doivent être gérés différemment dans iOS 7. Les développeurs sont encouragés à utiliser le `NSURlSession` API pour gérer les transferts de réseau. La section suivante est une vue d’ensemble des transferts en arrière-plan.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Transferts en arrière-plan

Le principal des transferts en arrière-plan dans iOS 7 est la nouvelle `NSURLSession` API. `NSURLSession` permet de créer des tâches :

1.  Transfert de contenu via les interruptions réseau et de périphériques.
1.  Télécharger des fichiers volumineux ( *Service de transfert en arrière-plan* ).


Jetons un œil plus près comment cela fonctionne.

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` est une API puissante pour le transfert de contenu sur le réseau. Il fournit un ensemble d’outils pour gérer le transfert de données par les interruptions réseau et les modifications d’états d’application.

Le `NSURLSession` API crée une ou plusieurs sessions, qui à son tour générer tâches pour navette des blocs de données associées sur le réseau. Tâches exécuter de façon asynchrone pour transférer des données rapide et fiable. Étant donné que `NSURLSession` est asynchrone, chaque session requiert un bloc de gestionnaire d’achèvement pour laisser le système et l’application de savoir quand un transfert est terminé.

Pour effectuer un transfert réseau est valide sur préliminaire iOS 7 et post-iOS 7, vérifiez si un `NSURLSession` est disponible pour les transferts de file d’attente et une tâche en arrière-plan standard permet d’effectuer le transfert si elle n’est pas :

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> Éviter d’effectuer des appels de mettre à jour l’interface utilisateur de l’arrière-plan dans un code conforme 6 iOS, iOS 6 ne prend pas en charge les mises à jour de l’interface utilisateur en arrière-plan et mettra fin à l’application.


Le `NSURLSession` API comprend un ensemble complet de fonctionnalités pour gérer l’authentification, de gérer les transferts ayant échouées et de signaler les erreurs du client-side - mais pas côté serveur -. Il permet de pont que l’interruption de la tâche exécution introduites dans iOS 7 et prend également en charge pour le transfert de fichiers volumineux rapidement et de façon fiable. La section suivante traite de ce deuxième fonctionnalité.

### <a name="background-transfer-service"></a>Service de transfert en arrière-plan

Avant d’iOS 7, téléchargement de fichiers en arrière-plan n’étaient pas fiables. Tâches en arrière-plan obtenir une durée limitée à exécuter, mais le temps que nécessaire pour transférer un fichier varie avec le réseau et la taille du fichier. Dans iOS 7, nous pouvons utiliser un `NSURLSession` pour charger et télécharger des fichiers volumineux avec succès. Particulier `NSURLSession` type de session qui gère les transferts de fichiers volumineux en arrière-plan réseau est connu comme le *Service de transfert en arrière-plan*.

Transferts initiées à l’aide du Service de transfert en arrière-plan sont gérés par le système d’exploitation et fournissent des API pour gérer les erreurs et l’authentification. Étant donné que les transferts ne sont pas liés par une limite de temps arbitraire, ils peuvent être utilisés pour charger ou télécharger des fichiers volumineux, contenu dans l’arrière-plan et bien plus encore mise à jour automatique. Reportez-vous à la [procédure pas à pas transfert en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) pour plus d’informations sur la façon d’implémenter le Service.

Le Service de transfert en arrière-plan est souvent associé à extraire de l’arrière-plan ou de Notifications à distance permettent aux applications d’actualiser le contenu en arrière-plan. Dans les deux sections suivantes, nous introduisons le concept de l’inscription des applications entières à exécuter en arrière-plan sur iOS 6 et iOS 7.

