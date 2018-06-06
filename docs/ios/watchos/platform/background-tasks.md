---
title: watchOS les tâches en arrière-plan dans Xamarin
description: Ce document décrit comment utiliser des tâches en arrière-plan avec watchOS dans Xamarin, étudier les types de tâches en arrière-plan, l’utilisation des ressources, implémentation des tâches en arrière-plan, planification, meilleures pratiques et bien plus encore.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 5ab53d4aea32cf41c492e286c18cbe85a619889a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792045"
---
# <a name="watchos-background-tasks-in-xamarin"></a>watchOS les tâches en arrière-plan dans Xamarin

Avec watchOS 3, il existe trois manières principales qu’une application espion peut maintenir ses informations à jour : 

- En utilisant l’une des nouvelles plusieurs tâches en arrière-plan. 
- À un des ses Complications sur la face espion (donnant il temps supplémentaire pour mettre à jour). 
- Devoir le pin de l’utilisateur à l’application de la nouvelle station d’accueil (où son conservées en mémoire et de mises à jour fréquemment). 

## <a name="keeping-an-app-up-to-date"></a>Mise à jour une application permanente

Avant d’examiner toutes les méthodes, qu'un développeur peut conserver les données et l’Interface utilisateur d’une application watchOS actuel et mis à jour, cette section s’intéresser à un ensemble typique de modèles d’utilisation et la façon dont un utilisateur peut passer leur iPhone et leurs Apple Watch pendant la journée en fonction de  l’heure du jour et de l’activité qu’ils le font actuellement (par exemple, la commande).

Prenons l’exemple suivant :

[![](background-tasks-images/update00.png "Comment un utilisateur peut déplacer entre leur iPhone et leurs Apple Watch pendant la journée")](background-tasks-images/update00.png#lightbox)

1. Le matin, tout en un café, l’utilisateur parcourt toutes les informations en cours sur leur iPhone pendant plusieurs minutes.
2. Avant de quitter le café-restaurant, ils vérifient rapidement la météo avec le des complications sur leur face espion.
3. Avant le déjeuner, leur permet l’application de mappages sur l’iPhone trouver un restaurant proche et une réservation pour répondre à un client.
4. Lors de déplacements au restaurant, ils reçoivent une notification sur leur Apple Watch et avec un coup de œil, qu’ils savent que leur rendez-vous déjeuner est en retard.
5. Dans la soirée, ils utilisent l’application de mappages sur l’iPhone pour vérifier le trafic avant la commande accueil.
6. Sur la manière dont chez vous, ils reçoivent une notification iMessage en ce qui sur leurs Apple Watch leur demandant de récupéreront certains lait, ceux-ci utilisent la fonctionnalité de réponse rapide pour envoyer la réponse « OK ».

En raison de la « aperçu rapide » (moins de trois secondes) la nature de la façon dont un utilisateur souhaitant utilisez généralement une application Apple Watch, il n’est pas suffisamment de temps pour l’application d’extraire les informations souhaitées et mettre à jour son interface utilisateur avant de les afficher à l’utilisateur.

À l’aide de la nouvelle API Apple a inclus dans watchOS 3, l’application peut planifier une _actualisation en arrière-plan_ et avoir les informations souhaitées prêt avant que l’utilisateur le demande. Prenons l’exemple de la Complication météo ci-dessus :

[![](background-tasks-images/update01.png "Un exemple de la Complication météo")](background-tasks-images/update01.png#lightbox)

1. Les planifications de l’application d’être réveillés par le système à un moment donné. 
2. L’application récupère les informations que vous devez pour générer la mise à jour.
3. L’application régénère son Interface utilisateur pour refléter les nouvelles données.
4. Lorsque l’utilisateur coup au niveau de Complication l’application, elle contient des informations à jour sans que l’utilisateur de devoir attendre la mise à jour.

Comme indiqué ci-dessus, le système watchOS sort de l’application à l’aide d’une ou plusieurs tâches, dont il dispose d’un pool très limité disponible :

[![](background-tasks-images/update02.png "Le système watchOS sort de l’application à l’aide d’une ou plusieurs tâches")](background-tasks-images/update02.png#lightbox)

Apple suggérer tirer pleinement parti de cette tâche (car il est de ce type d’une ressource limitée à l’application) à maintenir jusqu'à ce que l’application a terminé le processus de mise à jour lui-même.

Le système ces offre des tâches en appelant la nouvelle `HandleBackgroundTasks` méthode de la `WKExtensionDelegate` déléguer. Exemple :

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

Lorsque l’application a terminé la tâche donnée, elle le retourne au système en le marquant terminée :

[![](background-tasks-images/update03.png "La tâche retourne au système en le marquant terminée")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Nouvelles tâches en arrière-plan

watchOS 3 introduit plusieurs tâches d’arrière-plan d’une application peut utiliser pour mettre à jour ses informations s’assurer qu’il possède le contenu de l’utilisateur a besoin avant qu’ils ouvrent l’application, telles que :

- **Actualisation de l’application d’arrière-plan** - le [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) tâche permet à l’application mettre à jour son état en arrière-plan. En général, cela inclut une autre tâche, telles que le téléchargement de contenu à partir d’internet en utilisant un [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Actualisation de la capture instantanée d’arrière-plan** - le [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) tâche permet à l’application mettre à jour son contenu et l’interface utilisateur avant que le système prend un instantané qui sera utilisé pour remplir la station d’accueil.
- **Connectivité d’espion d’arrière-plan** - le [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) tâche est démarrée pour l’application lorsqu’il reçoit des données d’arrière-plan de l’iPhone apparié.
- **URL de Session d’arrière-plan** - le [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) tâche est démarrée pour l’application lorsqu’un transfert en arrière-plan nécessite une autorisation ou qu’il se termine (correctement ou erreur).

Ces tâches seront abordées en détail dans les sections ci-dessous.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

Le `WKApplicationRefreshBackgroundTask` est une tâche générique qui peut être planifiée pour que l’application quitte le mode veille à une date ultérieure :

[![](background-tasks-images/update04.png "Un WKApplicationRefreshBackgroundTask quitte le mode veille à une date ultérieure")](background-tasks-images/update04.png#lightbox)

Dans l’exécution de la tâche, l’application peut effectuer n’importe quel type de traitement local, telles que mise à jour une chronologie des complications ou extraire des données requises avec une `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

Le système envoie un `WKURLSessionRefreshBackgroundTask` lorsque les données a terminé le téléchargement et prêt à être traité par l’application :

[![](background-tasks-images/update05.png "Le WKURLSessionRefreshBackgroundTask lorsque les données de téléchargement sont terminé")](background-tasks-images/update05.png#lightbox)

Une application ne reste pas en cours d’exécution pendant le téléchargement de données en arrière-plan. Au lieu de cela, l’application planifie la demande de données, puis elle est suspendue et le système gère le téléchargement des données, reawakening uniquement l’application lorsque le téléchargement est terminé.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

Dans watchOS 3, Apple a ajouté la station d’accueil où les utilisateurs peuvent épingler leurs applications favorites et y accéder rapidement. Lorsque l’utilisateur appuie sur le bouton sur l’Apple Watch, une galerie d’instantanés de l’application épinglé s’affiche. L’utilisateur peut passez gauche ou droite pour rechercher l’application souhaitée, puis cliquez sur l’application pour le lancer en remplaçant l’instantané avec l’interface de l’application en cours d’exécution.

[![](background-tasks-images/update06.png "Remplacement de l’instantané avec l’interface d’applications en cours d’exécution")](background-tasks-images/update06.png#lightbox)

Le système prend régulièrement des instantanés de l’interface utilisateur de l’application (en envoyant un `WKSnapshotRefreshBackgroundTask`) et utilise ces instantanés pour remplir la station d’accueil. watchOS permet à l’application pour mettre à jour son contenu et l’interface utilisateur avant la prise de cet instantané.

Les instantanés sont très importants watchOS 3 dans la mesure où ils fonctionnent comme des images de l’aperçu et le lancement de l’application. Si l’utilisateur se règle sur une application de la station d’accueil, sera développer en plein écran, entrez le premier plan et commencer à exécuter, il est impératif que l’instantané soit à jour :

[![](background-tasks-images/update07.png "Si l’utilisateur se règle sur une application de la station d’accueil, il se développe en plein écran")](background-tasks-images/update07.png#lightbox)

Là encore, le système émet un `WKSnapshotRefreshBackgroundTask` afin de pouvoir préparer l’application (par la mise à jour les données et l’interface utilisateur) avant l’instantané :

[![](background-tasks-images/update08.png "Préparer l’application en mettant à jour les données et l’interface utilisateur avant de l’instantané")](background-tasks-images/update08.png#lightbox)

Lorsque l’application marque le `WKSnapshotRefreshBackgroundTask` terminé, le système mettra automatiquement un instantané de l’interface utilisateur de l’application.

> [!IMPORTANT]
> Il est important de toujours planifier un ` WKSnapshotRefreshBackgroundTask` après l’application a reçu de nouvelles données et mis à jour son Interface utilisateur ou l’utilisateur ne voit pas les informations modifiées.




En outre, lorsque l’utilisateur reçoit une notification à partir de l’application et clique sur dessus afin de mettre l’application au premier plan, l’instantané doit être mis à jour, car il agit en tant que de l’écran d’installation :

[![](background-tasks-images/update09.png "L’utilisateur reçoit une notification à partir de l’application et clique sur dessus afin de mettre l’application au premier plan")](background-tasks-images/update09.png#lightbox)

S’il a été plus d’une heure dans la mesure où l’utilisateur interagisse avec une application watchOS, il sera en mesure de retourner à son état par défaut. L’état par défaut peut signifier différentes choses pour différentes applications et, en fonction de la conception d’une application, est peut-être pas un état par défaut du tout.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

Dans watchOS 3, Apple a intégré espion connectivité avec l’API d’actualisation en arrière-plan via la nouvelle `WKWatchConnectivityRefreshBackgroundTask`. Cette nouvelle fonctionnalité, une application iPhone peut de fournir les données actualisées en son équivalent application espion, pendant l’exécution de l’application watchOS en arrière-plan :

[![](background-tasks-images/update10.png "Une application iPhone peut offrir de nouvelles données à son homologue application espion, pendant l’exécution de l’application watchOS en arrière-plan")](background-tasks-images/update10.png#lightbox)

Lancement d’un Push de la Complication, contexte d’application, envoi d’un fichier ou de mise à jour des informations sur l’utilisateur à partir de l’application iPhone sera mise en éveil l’application de l’Apple Watch en arrière-plan.

Lorsque l’application watch quitte le mode veille un `WKWatchConnectivityRefreshBackgroundTask` elle devra utiliser les méthodes API standard pour recevoir les données de l’application iPhone.

[![](background-tasks-images/update11.png "Le flux de données WKWatchConnectivityRefreshBackgroundTask")](background-tasks-images/update11.png#lightbox)

1. Assurez-vous que la Session est activé.
2. Surveiller la nouvelle `HasContentPending` propriété tant que la valeur est `true`, l’application possède toujours des données à traiter. Comme avant, l’application doit conserver la tâche jusqu'à ce qu’il a terminé le traitement de toutes les données.
3. Lorsqu’il n’existe plus aucune donnée à traiter (`HasContentPending = false`), marque la tâche s’est terminée pour restaurer le système. Le cas contraire épuiser les runtime alloué en arrière-plan de l’application obtenue dans un rapport d’incident.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Le cycle de vie des API en arrière-plan

Placer tous les éléments de la nouvelle API de tâches en arrière-plan, un ensemble typique d’interactions se présente comme suit :

[![](background-tasks-images/update12.png "Le cycle de vie des API en arrière-plan")](background-tasks-images/update12.png#lightbox)

1. Tout d’abord, l’application watchOS planifie un tâche pour être en éveil en tant qu’un moment donné dans le futur en arrière-plan.
2. Quitte le mode veille par le système et envoi d’une tâche de l’application.
3. L’application traite la tâche pour terminer le travail a été requis.
4. Par conséquent de la tâche de traitement, l’application peut avoir besoin planifier des tâches à effectuer davantage de travail à l’avenir, telles que le téléchargement de contenu à l’aide de plus d’informations un `NSUrlSession`.
5. L’application marque la tâche s’est terminée et le retourne au système.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Utilisation de ressources de façon responsable

Il est essentiel qu’une application watchOS se comporte de façon responsable dans cet écosystème en limitant son drainage sur des ressources partagées du système.

Examinez le scénario suivant :

[![](background-tasks-images/update13.png "Une application watchOS limite son drainage sur des ressources partagées du système")](background-tasks-images/update13.png#lightbox)

1. L’utilisateur lance une application watchOS à 1 h 00.
2. L’application planifie une tâche pour sortir de veille et de télécharger le nouveau contenu en une heure à 2 h 00.
3. À 13:50 l’utilisateur ouvre l’application qui lui permet de mettre à jour ses données et l’interface utilisateur pour l’instant.
4. Au lieu de laisser la mise en éveil tâche l’application dans 10 minutes, l’application doit replanifier la tâche pour exécuter une heure plus tard, à 2 h 50.

Alors que chaque application est différente, Apple suggère de trouver des modèles d’utilisation, comme ceux indiqués ci-dessus, pour aider à préserver les ressources système.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Implémentation des tâches en arrière-plan

Par exemple, ce document utilisera l’application de sports MonkeySoccer factice qui signale les scores de football à l’utilisateur. 

Examinez le scénario classique d’utilisation suivantes :

[![](background-tasks-images/update14.png "Le scénario classique d’utilisation")](background-tasks-images/update14.png#lightbox)

Équipe de football Favoris de l’utilisateur en cours de lecture de correspondance big entre 7:00 à 9:00 PM afin que l’application doit attendre l’utilisateur à vérifier régulièrement le score et elle décide d’un intervalle de mise à jour de 30 minutes.

1. L’utilisateur ouvre l’application et il planifie une tâche de mise à jour en arrière-plan de 30 minutes plus tard. L’API de l’arrière-plan ne permet qu’un seul type de tâche à exécuter à un moment donné d’arrière-plan.
2. L’application reçoit la tâche et met à jour ses données et l’interface utilisateur, puis d’arrière-plan des planifications pour une autre tâche 30 minutes plus tard. Il est important que le développeur se souvient de planifier l’arrière-plan d’une autre tâche, ou l’application n’est jamais nouveau activée pour obtenir plus de mises à jour.
3. Là encore, l’application reçoit la tâche et met à jour ses données, met à jour son interface utilisateur et planifie l’arrière-plan d’une autre tâche 30 minutes plus tard.
4. Le même processus se répète à nouveau.
5. Arrière-plan de la dernière tâche est reçu et l’application à nouveau des mises à jour ses données et l’interface utilisateur. Dans la mesure où il s’agit le score final, qu'il ne planifier pour une nouvelle actualisation en arrière-plan. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Planification de mise à jour en arrière-plan

Étant donné le scénario ci-dessus, l’application MonkeySoccer peut utiliser le code suivant pour planifier pour une mise à jour en arrière-plan :

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Il crée un nouveau `NSDate` 30 minutes dans le moment où futures que l’application souhaite activé et crée un `NSMutableDictionary` pour contenir les détails de la tâche demandée. Le `ScheduleBackgroundRefresh` méthode de la `SharedExtension` est utilisé pour demander la tâche planifiée.

Le système renvoie un `NSError` si elle n’a pas pu planifier la tâche demandée.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Le traitement de la mise à jour

Ensuite, prenez une présentation détaillée dans la fenêtre de 5 minutes montrant les étapes requises pour mettre à jour le score :

[![](background-tasks-images/update15.png "La fenêtre de 5 minutes montrant les étapes requises pour mettre à jour le score")](background-tasks-images/update15.png#lightbox)

1. À 7 h 30:02 mis en éveil par le système et de l’application étant donnée l’arrière-plan de la mise à jour de tâche. Sa priorité est pour obtenir les derniers résultats à partir du serveur. Consultez [planification une NSUrlSession](#Scheduling-a-NSUrlSession) ci-dessous.
2. À 7 h 30:05, l’application termine la tâche d’origine, le système met l’application en veille et qu’il continue à télécharger les données demandées en arrière-plan.
3. Lorsque le système le téléchargement terminé, il crée une nouvelle tâche afin de sortir de l’application afin qu’il peut traiter les informations téléchargées. Consultez [la gestion des tâches en arrière-plan](#Handling-Background-Tasks) et [gestion de la fin de téléchargement](#Handling-the-Download-Completing) ci-dessous. 
4. L’application enregistre les informations de mise à jour et marque la tâche s’est terminée. Le développeur peut-être être tenté de mettre à jour l’Interface utilisateur de l’application à ce stade, Apple suggère la planification d’une tâche d’instantané pour gérer le traitement. Consultez [planification d’une mise à jour de l’instantané](#Scheduling-a-Snapshot-Update) ci-dessous.
5. L’application reçoit la tâche d’instantané, met à jour son Interface utilisateur et marque la tâche s’est terminée. Consultez [une mise à jour de l’instantané de la gestion des](#Handling-a-Snapshot-Update) ci-dessous.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Planification d’une NSUrlSession

Le code suivant peut être utilisé pour planifier le téléchargement des derniers résultats :

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

Il configure et crée un nouveau `NSUrlSession`, puis utilise cette session pour créer un nouveau téléchargement de tâches à l’aide de la `CreateDownloadTask` (méthode). Il appelle le `Resume` méthode de la tâche pour démarrer la session de téléchargement.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>Gestion des tâches d’arrière-plan

En remplaçant le `HandleBackgroundTasks` méthode de le `WKExtensionDelegate`, l’application peut gérer les tâches en arrière-plan entrant :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

Le `HandleBackgroundTasks` méthode parcourt toutes les tâches que le système a envoyé à l’application (dans `backgroundTasks`) recherchant une `WKUrlSessionRefreshBackgroundTask`. S’il existe, il rejoint la session et l’attache un `NSUrlSessionDownloadDelegate` pour gérer la fin de téléchargement (consultez [gestion de la fin de téléchargement](#Handling-the-Download-Completing) ci-dessous) :

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Il conserve un pointeur sur la tâche jusqu'à ce qu’il a terminé en l’ajoutant à une collection :

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Toutes les tâches envoyées à l’application doivent être terminée, pour n’importe quelle tâche non gérée, marquez-le terminée :

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Gestion de la fin du téléchargement

L’application MonkeySoccer utilise les éléments suivants `NSUrlSessionDownloadDelegate` délégué pour gérer la fin du téléchargement et de traiter les données demandées :

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

Lors de l’initialisation, il conserve un pointeur à la fois à la `ExtensionDelegate` et `WKRefreshBackgroundTask` qui le généré. Il remplace le `DidFinishDownloading` méthode pour gérer la fin du téléchargement. Utilise ensuite le `CompleteTask` méthode de le `ExtensionDelegate` pour informer le la tâche qu’elle est terminée et le supprimer de la collection de tâches en attente. Consultez [la gestion des tâches en arrière-plan](#Handling-Background-Tasks) ci-dessus.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Planification d’une mise à jour de l’instantané

Le code suivant peut être utilisé pour planifier une tâche d’instantané pour mettre à jour l’interface utilisateur avec les derniers résultats :

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Tout comme `ScheduleURLUpdateSession` méthode ci-dessus, il crée un `NSDate` pour lorsque l’application souhaite activé et crée un `NSMutableDictionary` pour contenir les détails de la tâche demandée. Le `ScheduleSnapshotRefresh` méthode de la `SharedExtension` est utilisé pour demander la tâche planifiée.

Le système renvoie un `NSError` si elle n’a pas pu planifier la tâche demandée.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Gestion d’une mise à jour de l’instantané

Pour gérer la tâche d’instantané, la `HandleBackgroundTasks` (méthode) (voir [la gestion des tâches d’arrière-plan](#Handling-Background-Tasks) ci-dessus) est modifiée pour ressembler à ce qui suit :

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

La méthode des tests pour le type de tâche en cours de traitement. S’il s’agit un `WKSnapshotRefreshBackgroundTask` accède à la tâche :

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

La méthode met à jour de l’Interface utilisateur, puis crée un `NSDate` d’informer le système lors de l’instantané sera obsolète. Il crée un `NSMutableDictionary` avec des informations utilisateur pour décrire la nouvelle capture instantanée et la marque la tâche d’instantané s’est terminée avec ces informations :

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

En outre, il indique également la tâche d’instantané que l’application ne revient pas à l’état par défaut (dans le premier paramètre). Les applications qui n’ont aucun concept d’un état par défaut doivent toujours définir cette propriété `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Travailler efficacement

Comme dans l’exemple précédent de la fenêtre de cinq minutes que l’application MonkeySoccer suivies pour mettre à jour ses résultats, en ne fonctionne efficacement à l’aide de la nouvelle watchOS 3 tâches en arrière-plan, l’application était uniquement active pour un total de 15 secondes : 

[![](background-tasks-images/update16.png "L’application n’était active pour un total de 15 secondes")](background-tasks-images/update16.png#lightbox)

Cela réduit l’impact de l’application sur les ressources de l’Apple Watch disponibles et la durée des batteries et permet également à l’application pour mieux fonctionner avec d’autres applications s’exécutant sur la surveillance.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Fonctionne de la planification

Lorsqu’une application watchOS 3 est en premier plan, il est toujours planifiée pour s’exécuter et peut effectuer n’importe quel type de traitement nécessaire, telles que les données de mise à jour ou redessiner son interface utilisateur. Lorsque l’application passe en arrière-plan, il est généralement interrompue par le système et toutes les opérations d’exécution sont interrompues. 

Pendant que l’application est en arrière-plan, il peut être ciblé par le système à exécuter rapidement une tâche spécifique. Par conséquent, watchOS 2, le système peut temporairement sortir du mode veille une application en arrière-plan pour effectuer les opérations de gestion de la notification de longue analyse ou mise à jour Complication de l’application. Dans watchOS 3, il existe plusieurs manières de nouveau une application peut être exécutée en arrière-plan.

Lorsqu’une application est en arrière-plan, le système impose des limites plusieurs dessus :

- Il ne reçoit que quelques secondes pour terminer une tâche donnée. Le système prend en considération non seulement la quantité de temps passé, mais également la quantité d’énergie du processeur l’application consomme pour dériver de cette limite.
- N’importe quelle application qui dépasse la limite est supprimée avec les codes d’erreur suivants :
    - **Processeur** -0xc51bad01
    - **Heure** -0xc51bad02
- Le système sera imposent des limites différentes en fonction du type de tâche en arrière-plan a demandé à l’application à exécuter. Par exemple, `WKApplicationRefreshBackgroundTask` et `WKURLSessionRefreshBackgroundTask` tâches figurent les runtimes légèrement plus sur les autres types de tâches en arrière-plan.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Les mises à jour de l’application et les complications

Outre les nouvelles tâches en arrière-plan qui Apple a ajouté à watchOS 3, Complications d’une application watchOS peuvent avoir un impact sur comment et quand l’application reçoit des mises à jour en arrière-plan.

Complications sont de petits éléments visuels qui fournissent des informations utiles en un coup de œil. En fonction de la face espion est sélectionnée, l’utilisateur a la possibilité de personnaliser un visage Espion avec un ou plusieurs des complications peuvent être fournies par une application de surveillance dans watchOS 3.

Si l’utilisateur inclut l’une des Complications de l’application sur leur face espion, il donne à l’application mis à jour suivantes avantages :

- Le système de conserver de l’application dans un prêt à lancer état, où il tente de lancer l’application en arrière-plan, il conserve en mémoire et laisse l’il temps supplémentaire pour mettre à jour.
- Complications sont garanti que les mises à jour au moins 50 par émission de données par jour.

Le développeur doit s’efforcent toujours créer des Complications attrayantes pour leurs applications pour convaincre l’utilisateur de les ajouter à leur face espion pour les raisons répertoriées ci-dessus.

Complications étaient watchOS 2, la principale manière qu’une application reçu d’exécution en arrière-plan. Dans watchOS 3, une application de la Complication sera toujours assurée de recevoir plusieurs mises à jour par heure, toutefois, il peut utiliser `WKExtensions` pour demander un runtime plus mettre à jour ses complications.

Examinons le code suivant pour mettre à jour la Complication à partir de l’application iPhone connecté :

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

Elle utilise le `RemainingComplicationUserInfoTransfers` propriété de la `WCSession` pour voir combien de haute priorité transfère l’application a quitté du jour, puis les mesures correctives en fonction de ce nombre. Si l’application commence à manquer transferts, il peut contenir lors de l’envoi des mises à jour mineures et envoyer des informations uniquement lorsqu’il existe une modification significative.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Planification et la station d’accueil

Dans watchOS 3, Apple a ajouté la station d’accueil où les utilisateurs peuvent épingler leurs applications favorites et y accéder rapidement. Lorsque l’utilisateur appuie sur le bouton sur l’Apple Watch, une galerie d’instantanés de l’application épinglé s’affiche. L’utilisateur peut passez gauche ou droite pour rechercher l’application souhaitée, puis cliquez sur l’application pour le lancer en remplaçant l’instantané avec l’interface de l’application en cours d’exécution.

[![](background-tasks-images/dock01.png "La station d’accueil")](background-tasks-images/dock01.png#lightbox)

Périodiquement, le système de prend des instantanés de l’interface utilisateur de l’application et utilise ces instantanés pour remplir les documents. watchOS permet à l’application pour mettre à jour son contenu et l’interface utilisateur avant la prise de cet instantané.

Les applications qui ont été épinglées à la station d’accueil peuvent s’attendre les éléments suivants :

- Ils recevront un minimum d’une mise à jour par heure. Cela inclut une tâche d’actualisation d’application et une tâche d’instantané.
- L’allocation de réserve de mise à jour est distribuée entre toutes les applications de la station d’accueil. Par conséquent, le nombre des applications de que l’utilisateur a été épinglé, les mises à jour plus le risque que chaque application reçoit.
- L’application est conservée en mémoire afin de l’application reprend rapidement lorsque sélectionné à partir de la station d’accueil.

L’application de dernière exécution de l’utilisateur sera considéré comme le _des derniers fichiers utilisés_ application et occuperaient le dernier emplacement dans la station d’accueil. À partir de là, il peut choisir de faire épingler définitivement à la station d’accueil. Le plus récemment utilisé sera traité comme toute autre application favorite l’utilisateur a déjà épinglé à la station d’accueil.

> [!IMPORTANT]
> Les applications qui ont été ajoutées uniquement à l’écran d’accueil pas reçoivent une planification régulière. Pour recevoir une planification régulière et en arrière-plan met à jour, une application _doit_ être ajouté à la station d’accueil.

Comme indiqué plus haut dans ce document, les instantanés sont très importants watchOS 3 dans la mesure où ils fonctionnent comme des images de l’aperçu et le lancement de l’application. Si l’utilisateur se règle sur une application de la station d’accueil, il développer en plein écran, entrez le premier plan et commencer à exécuter, il est impératif que l’instantané soit à jour.

Il peut arriver lorsque le système décide qu’il a besoin d’un instantané de frais de l’interface utilisateur de l’application. Dans ce cas, la demande de capture instantanée comptera pas par rapport au budget de runtime de l’application. Déclenchent une demande de capture instantanée système les éléments suivants :

- Une mise à jour de la chronologie Complication.
- Interaction de l’utilisateur avec la notification d’une application.
- Changement de premier plan à l’état de l’arrière-plan.
- Après l’heure qui suit l’état en arrière-plan, par conséquent, l’application peut retourner l’état par défaut.
- Lorsque watchOS premier démarrage.

<a name="Best-Practices" />

## <a name="best-practices"></a>Meilleures pratiques 

Apple suggère les meilleures pratiques suivantes lorsque vous travaillez avec des tâches en arrière-plan :

- Planification aussi souvent que l’application doit être mis à jour. Chaque fois que l’application s’exécute, il doit réévaluer ses besoins futurs et ajuster cette planification en fonction des besoins.
- Si le système envoie une tâche d’actualisation en arrière-plan et l’application ne nécessite pas une mise à jour, différer le travail jusqu'à ce qu’une mise à jour est nécessaire.
- Prenez en compte toutes les opportunités de runtime sont disponibles pour une application :
    - Activation de premier plan et d’ancrage.
    - Notifications.
    - Mises à jour de la complication.
    - Actualisations en arrière-plan.
- Utilisez `ScheduleBackgroundRefresh` pour l’exécution en arrière-plan à usage général telles que :
    - Interrogation du système pour plus d’informations.
    - Planifier la future `NSURLSessions` pour demander des données en arrière-plan. 
    - Transitions de temps connu.
    - Déclenchement des mises à jour de la Complication.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Meilleures pratiques d’instantané

Lorsque vous travaillez avec des mises à jour de l’instantané, Apple rend les suggestions suivantes :

- Invalident les instantanés uniquement lorsque nécessaire, par exemple, lorsqu’il existe un changement important de contenu.
- Éviter l’invalidation d’instantanés de haute fréquence. Par exemple, une application du minuteur ne doivent pas mettre à jour l’instantané chaque seconde, il ne doit être effectuée lorsque la minuterie est terminée.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>Flux de données d’application

Apple suggérer les éléments suivants pour l’utilisation des flux de données :

[![](background-tasks-images/update17.png "Diagramme de flux de données d’application")](background-tasks-images/update17.png#lightbox)

Un événement externe (par exemple, Espion connectivité) sort de l’application. Cela force l’application à mettre à jour le modèle de données (qui représente l’état actuel d’applications). Par conséquent de la modification du modèle de données l’application devez mettre à jour ses Complications, demande un nouvel instantané, de démarrer un arrière-plan `NSURLSession` pour extraire des données et planifier davantage en arrière-plan actualise.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Le cycle de vie d’application

En raison de la station d’accueil et la possibilité d’épingler des applications favorites à celui-ci, juge Apple que vous envisagez de déplacer des utilisateurs entre les applications beaucoup plus, beaucoup plus souvent, puis ils le faisaient dans watchOS 2. Par conséquent, l’application doit être prête à gérer cette modification et de déplacer rapidement entre les États de premier plan et d’arrière-plan.

Apple a les suggestions suivantes :

- Assurez-vous que l’application termine une tâche en arrière-plan dès que possible lors de l’entrée de l’activation de premier plan.
- Veillez à terminer toutes les tâches de premier plan avant d’entrer l’arrière-plan en appelant `NSProcessInfo.PerformExpiringActivity`.
- Lorsque vous testez une application dans le simulateur watchOS, aucun des budgets de tâche sont appliquées pour une application peut actualiser autant que nécessaire pour tester correctement une fonctionnalité.
- Testez toujours sur du vrai matériel Apple Watch pour vous assurer que l’application n’est pas en cours d’exécution après son budgets avant la publication à iTunes Connect.
- Apple suggère en conservant l’Apple Watch sur le chargeur lors de tests et le débogage.
- Assurez-vous que les à froid du lancement et la reprise d’une application sont informées testées.
- Vérifiez que toutes les tâches d’application sont en cours terminées.
- Faire varier le nombre d’applications qui sont épinglées dans la station d’accueil pour tester les meilleures et le pire cas de scénarios.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert les améliorations Qu'apple a apportées au watchOS et comment ils peuvent être utilisés pour maintenir une application espion à jour. Tout d’abord, il couvert tous du nouveau Apple de tâche en arrière-plan a ajouté dans watchOS 3. Ensuite, il couvert le cycle de vie des API en arrière-plan et comment implémenter des tâches en arrière-plan dans une application Xamarin de watchOS. Enfin, il couvert les planifications de travaux et donnez quelques-unes des meilleures pratiques.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
