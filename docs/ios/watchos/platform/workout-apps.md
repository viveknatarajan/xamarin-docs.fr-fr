---
title: "Applications d’entraînement"
description: "Cet article décrit les améliorations Apple a apportées aux applications d’entraînement dans watchOS 3 et comment les implémenter dans Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 2282a340811d9932f9df3a1343b22ffc35247e54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="workout-apps"></a>Applications d’entraînement

_Cet article décrit les améliorations Apple a apportées aux applications d’entraînement dans watchOS 3 et comment les implémenter dans Xamarin._


Nouveau watchOS 3, exercices relatifs applications ont la possibilité d’exécuter en arrière-plan sur l’Apple Watch et accéder aux données de HealthKit. Son application iOS 10 en fonction de parent a également la possibilité de lancer l’application en fonction de watchOS 3 sans intervention de l’utilisateur.

Les rubriques suivantes sont traitées en détail :

## <a name="about-workout-apps"></a>Sur les applications d’entraînement

Les utilisateurs d’applications à l’adéquation et entraînement peuvent être très dédiés, réserver alloués à plusieurs heures de la journée vers leurs objectifs de contrôle d’intégrité et d’adéquation. Par conséquent, ils attendent des applications réactives, facile à utiliser qui précisément collectent et affichent des données et une intégration transparente avec contrôle d’intégrité d’Apple.

Une application bien conçue d’aptitude ou entraînement permet aux utilisateurs de leurs activités afin d’atteindre les objectifs de l’adéquation du graphique. À l’aide de l’Apple Watch, les applications à l’adéquation et entraînement ont un accès instantané à un rythme cardiaque, détection de calories avancement et l’activité.

[![](workout-apps-images/workout01.png "Exemple d’application à l’adéquation et entraînement")](workout-apps-images/workout01.png#lightbox)

Vous débutez avec watchOS 3, _en cours d’exécution en arrière-plan_ donne entraînement liées applications la possibilité de s’exécuter en arrière-plan sur l’Apple Watch et accéder aux données de HealthKit.

Ce document présentent la fonctionnalité en cours d’exécution en arrière-plan, couvrent le cycle de vie des applications entraînement et montrent comment une application d’entraînement peut contribuer à l’utilisateur _activité anneaux_ sur l’Apple Watch.

## <a name="about-workout-sessions"></a>Sur les Sessions d’entraînement

Le cœur de toutes les applications entraînement est un _entraînement Session_ (`HKWorkoutSession`) que l’utilisateur peut démarrer et arrêter. L’API de Session d’entraînement est facile à implémenter et offre plusieurs avantages pour une application d’entraînement, telles que :

- Animation et calories graver détection basée sur le Type d’activité.
- Contribution automatique à anneaux d’activité de l’utilisateur.
- Au cours d’une session, l’application affichera automatiquement chaque fois que l’utilisateur sort de l’appareil (soit en déclenchant leur poignet ou d’interagir avec l’Apple Watch).

## <a name="about-background-running"></a>Sur l’exécution de l’arrière-plan

Comme indiqué ci-dessus, watchOS 3 une application d’entraînement peut être définie pour s’exécuter en arrière-plan. À l’aide d’une application d’entraînement en cours d’exécution en arrière-plan peut traiter les données de capteurs de l’Apple Watch lors de l’exécution en arrière-plan. Par exemple, une application continue de surveiller le rythme cardiaque de l’utilisateur, même si elle n’est plus affichée sur l’écran.

En cours d’exécution en arrière-plan fournit également la possibilité de présenter des commentaires en direct à l’utilisateur à tout moment pendant une Session d’entraînement active, telles que l’envoi d’une alerte tactile pour informer l’utilisateur de progression actuelle.

En outre, en cours d’exécution en arrière-plan permet à l’application à mettre à jour rapidement son Interface utilisateur pour l’utilisateur dispose des dernières données lorsqu’ils œil rapidement leurs Apple Watch.

Pour maintenir des performances élevées sur Apple Watch, une application de surveillance à l’aide d’en cours d’exécution en arrière-plan doit limiter la quantité de travail en arrière-plan pour préserver la batterie. Si une application utilise le processeur excessif en arrière-plan, il peut obtenir interrompu par watchOS.

### <a name="enabling-background-running"></a>L’activation en cours d’exécution en arrière-plan

Pour permettre l’exécution de l’arrière-plan, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur l’application de l’Extension de surveillance accompagnement iPhone `Info.plist` fichier à ouvrir pour le modifier.
2. Basculez vers le **Source** vue : 

    [![](workout-apps-images/plist01.png "La vue de Source")](workout-apps-images/plist01.png#lightbox)
3. Ajouter une nouvelle clé nommée `WKBackgroundModes` et définir le **Type** à `Array`: 

    [![](workout-apps-images/plist02.png "Ajouter une nouvelle clé nommée WKBackgroundModes")](workout-apps-images/plist02.png#lightbox)
4. Ajouter un nouvel élément au tableau avec la **Type** de `String` et la valeur `workout-processing`: 

    [![](workout-apps-images/plist03.png "Ajouter un nouvel élément au tableau avec le Type de chaîne et une valeur de traitement d’entraînement")](workout-apps-images/plist03.png#lightbox)
5. Enregistrez les modifications dans le fichier.

## <a name="starting-a-workout-session"></a>Démarrage d’une Session d’entraînement

Il existe trois étapes principales pour démarrer une Session d’entraînement :

[![](workout-apps-images/workout02.png "Les trois étapes principales pour démarrer une Session d’entraînement")](workout-apps-images/workout02.png#lightbox)

1. L’application doit demander une autorisation pour accéder aux données dans HealthKit.
2. Créer un objet de Configuration d’entraînement pour le type d’entraînement en cours de démarrage.
3. Créez et démarrez une Session d’entraînement à l’aide de la Configuration d’entraînement nouvellement créé.

### <a name="requesting-authorization"></a>Demande d’autorisation

Une application peut accéder aux données HealthKit de l’utilisateur, il doit demander et recevoir l’autorisation de l’utilisateur. Selon la nature de l’application d’entraînement il risque de rendre les types de requêtes suivants :

- Autorisation d’écrire des données :
    - Workouts
- Autorisation de lire des données :
    - Énergie consommée
    - Distance
    - Rythme cardiaque  

Avant d’une application peut demander l’autorisation, il doit être configuré pour accéder à HealthKit.

Effectuez ce qui suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Entitlements.plist` pour l’ouvrir et le modifier.
2. Faites défiler vers le bas et vérifiez **HealthKit d’activer**: 

    [![](workout-apps-images/auth01.png "Case Activer HealthKit")](workout-apps-images/auth01.png#lightbox)
3. Enregistrez les modifications dans le fichier.
4. Suivez les instructions de la [explicite ID d’application et de profil de préparation](~/ios/platform/healthkit.md) et [associant les ID d’application et la configuration de profil avec votre application Xamarin.iOS](~/ios/platform/healthkit.md) sections de la [présentation HealthKit](~/ios/platform/healthkit.md) article pour correctement configurer l’application.
5. Enfin, suivez les instructions de la [Kit de contrôle d’intégrité de programmation](~/ios/platform/healthkit.md) et [demandant l’autorisation de l’utilisateur](~/ios/platform/healthkit.md) sections de la [présentation HealthKit](~/ios/platform/healthkit.md) de l’article à la demande autorisation d’accès au magasin de données de l’utilisateur HealthKit.

### <a name="setting-the-workout-configuration"></a>Définition de la Configuration d’entraînement

Sessions d’entraînement sont créées à l’aide d’un objet de Configuration d’entraînement (`HKWorkoutConfiguration`) qui spécifie le type d’entraînement (tel que `HKWorkoutActivityType.Running`) et l’emplacement d’entraînement (tel que `HKWorkoutSessionLocationType.Outdoor`) :

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Création d’un délégué de Session d’entraînement 

Pour gérer les événements qui peuvent se produire pendant une Session d’entraînement, l’application doit créer une instance de délégué de Session d’entraînement. Ajouter une nouvelle classe au projet et de baser la `HKWorkoutSessionDelegate` classe. Pour l’exemple d’une série d’extérieur, il peut se présenter comme suit :

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

Cette classe crée plusieurs événements qui seront déclenchés en tant que l’état de la Session d’entraînement change (`DidChangeToState`) et si la Session d’entraînement échoue (`DidFail`). 

### <a name="creating-a-workout-session"></a>Création d’une Session d’entraînement

À l’aide de la Configuration d’entraînement et entraînement Session délégué créé ci-dessus pour créer une nouvelle Session d’entraînement et démarrez-le sur le magasin de l’utilisateur par défaut HealthKit :

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Si l’application démarre cette Session d’entraînement et l’utilisateur bascule leur face espion, une icône verte « running man » minuscule s’affichera au-dessus de l’image :

[![](workout-apps-images/workout03.png "Une petite verte en cours d’exécution man icône affichée au-dessus de la face")](workout-apps-images/workout03.png#lightbox)

Si l’utilisateur appuie sur cette icône, elles seront alors renvoyé à l’application.

## <a name="data-collection-and-control"></a>Le contrôle et la collecte de données

Une fois qu’une Session d’entraînement a été configurée et démarrée, l’application doit collecter des données sur la session (par exemple, le rythme cardiaque de l’utilisateur) et de contrôler l’état de la session :

[![](workout-apps-images/workout04.png "Collecte de données et le schéma de contrôle")](workout-apps-images/workout04.png#lightbox)

1. **Observation d’exemples** -l’application doit récupérer les informations de HealthKit qui est traité et affiché à l’utilisateur.
2. **Observation des événements** -l’application doit répondre aux événements générés par HealthKit ou à partir de l’interface utilisateur de l’application (par exemple, l’utilisateur suspension effectuer).
3. **Entrez l’état d’exécution** -la session a été démarrée et est en cours d’exécution.
4. **Entrez l’état suspendu** -l’utilisateur a interrompu la session d’entraînement en cours et qu’il puisse le redémarrer ultérieurement. L’utilisateur peut basculer entre les États en cours d’exécution et suspendus plusieurs fois dans une même Session d’entraînement.
5. **Terminer la Session d’entraînement** - à tout moment l’utilisateur peut mettre fin à la Session d’entraînement ou il peut expirer et se terminent à sa propre s’il s’agissait d’un entraînement limitée (par exemple, une série de deux miles).

La dernière étape consiste à enregistrer les résultats de la Session d’entraînement dans le magasin de données de l’utilisateur HealthKit.

### <a name="observing-healthkit-samples"></a>En observant les exemples HealthKit

L’application doit ouvrir un _requête d’objet d’ancrage_ pour chacun des données HealthKit points qui l’intéressent, telles que le rythme cardiaque ou énergie active gravé. Pour chaque point de données observé, devez un gestionnaire de mise à jour doit être créé pour capturer les nouvelles données telle qu’elle est envoyée à l’application.

Ces points de données, l’application peut cumuler les totaux (par exemple, la distance d’exécution total) et mettre à jour son Interface utilisateur en fonction des besoins. En outre, l’application peut avertir les utilisateurs lorsqu’ils ont atteint un objectif spécifique ou la réalisation, telles que les miles suivant d’une exécution à la fin.

Examinons l’exemple de code suivant :

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

Il crée un prédicat pour définir la date de début qu’il souhaite obtenir des données pour l’utilisation de la `GetPredicateForSamples` (méthode). Il crée un ensemble d’appareils pour extraire des informations de HealthKit d’à l’aide de la `GetPredicateForObjectsFromDevices` (méthode), dans ce cas l’Apple Watch local uniquement (`HKDevice.LocalDevice`). Les deux prédicats sont combinés dans un prédicat composés (`NSCompoundPredicate`) à l’aide de la `CreateAndPredicate` (méthode).

Un nouveau `HKAnchoredObjectQuery` est créé pour le point de données souhaité (dans ce cas `HKQuantityTypeIdentifier.ActiveEnergyBurned` pour le point de données Active gravé d’énergie), aucune limite n’est imposée sur la quantité de données retournées (`HKSampleQuery.NoLimit`) et un gestionnaire de mise à jour est défini pour gérer le retour à l’application en cours de données à partir de HealthKit. 

Le Gestionnaire de mise à jour sera appelé à tout moment de nouvelles données sont remises à l’application pour le point de données spécifique. Si aucune erreur n’est retourné, l’application peut sans risque lire les données, effectuer les calculs requis et mettre à jour son interface utilisateur en fonction des besoins.

Le code effectue une itération sur tous les exemples (`HKSample`) retournées dans le `addedObjects` de tableau et les convertit en un exemple de quantité (`HKQuantitySample`). Ensuite, il obtient la valeur double de l’échantillon sous la forme d’un joule (`HKUnit.Joule`) et accumule dans le total cumulé d’énergie active gravé pour effectuer et met à jour de l’Interface utilisateur.

### <a name="achieved-goal-notification"></a>Notification de l’objectif atteint

Comme mentionné ci-dessus, lorsque l’utilisateur atteint un objectif dans l’application d’entraînement (par exemple, la fin de la première miles d’une exécution), il peut envoyer des commentaires tactiles à l’utilisateur via le moteur Taptic. L’application doit également mettre à jour son interface utilisateur à ce stade, car l’utilisateur génère plus que probable que leur poignet pour voir l’événement à l’évaluation.

Pour lire les commentaires tactile, utilisez le code suivant :

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Observation des événements

Les événements sont les horodateurs de l’application peut utiliser pour mettre en évidence certains points au cours des exercices de l’utilisateur. Certains événements sont créés directement par l’application et enregistrés dans les exercices et certains événements seront créés automatiquement par HealthKit.

Pour observer les événements qui sont créés par HealthKit, l’application remplace le `DidGenerateEvent` méthode de la `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple a ajouté les nouveaux types d’événements suivants dans watchOS 3 :

- `HKWorkoutEventType.Lap` -Sont des événements qui empêche d’effectuer en portions d’une distance égale. Par exemple, pour le marquage d’une visite guidée une piste lors de son exécution.
- `HKWorkoutEventType.Marker` -Sont dans les exercices d’arbitraires points d’intérêt. Par exemple, atteindre un point spécifique sur l’itinéraire d’une exécution extérieur.

Ces nouveaux types peuvent être créées par l’application et stockées dans les exercices pour une utilisation ultérieure lors de la création de graphiques et les statistiques.

Pour créer un marqueur d’événements, procédez comme suit :

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Ce code crée une nouvelle instance d’un événement marqueur (`HKWorkoutEvent`) et l’enregistre dans une collection privée d’événements (qui sera ultérieurement écrit à la Session d’entraînement) et avertit l’utilisateur de l’événement via HAPTIQUES.

### <a name="pausing-and-resuming-workouts"></a>Suspension et reprise de sommeil

À tout moment dans une session d’entraînement, l’utilisateur peut temporairement suspendre l’entraînement et reprendre ultérieurement. Par exemple, ils risque de s’interrompre une exécution intérieur pour effectuer un appel important et reprendre l’exécution une fois l’appel terminé.

L’interface utilisateur de l’application doit fournir un moyen pour suspendre et reprendre l’entraînement (en appelant HealthKit) afin que l’Apple Watch peut économiser de l’espace d’alimentation et de données pendant que l’utilisateur a suspendu son activité. En outre, l’application doit ignorer les nouveaux points de données qui peuvent être reçus lorsque la Session d’entraînement est dans un état suspendu.

HealthKit répondra pour suspendre et reprendre des appels de générer des événements de suspendre et reprendre. Lorsque la Session d’entraînement est suspendue, aucune donnée ou nouveaux événements ne sera être envoyée à l’application par HealthKit jusqu'à la reprise de la session.

Pour suspendre et reprendre une Session d’entraînement, utilisez le code suivant :

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

Les événements de suspendre et reprendre qui seront générées à partir de HealthKit peuvent être gérés en substituant le `DidGenerateEvent` méthode de la `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>Événements de mouvement

Autre nouveauté watchOS 3, sont le mouvement interrompu (`HKWorkoutEventType.MotionPaused`) et la reprise de mouvement (`HKWorkoutEventType.MotionResumed`) événements. Ces événements sont déclenchés automatiquement par HealthKit pendant un entraînement en cours d’exécution lorsque l’utilisateur démarre et arrête le déplacement.

Lorsque l’application reçoit un événement de mouvement interrompu, il doit s’arrêter la collecte de données jusqu'à ce que l’utilisateur reprend le déplacement et la réception de l’événement de mouvement reprend. Application de l’application doit être interrompue pas la session d’entraînement en réponse à un événement de mouvement interrompu.

> [!IMPORTANT]
> Les événements de mouvement en pause et reprise de mouvement sont uniquement pris en charge pour le Type d’activité RunningWorkout (`HKWorkoutActivityType.Running`).

Là encore, ces événements peuvent être gérés en substituant le `DidGenerateEvent` méthode de la `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Fin et l’enregistrement de la Session d’entraînement

Lorsque l’utilisateur a terminé leur entraînement, l’application devez terminer la Session d’entraînement actuel et l’enregistrer dans la base de données HealthKit. Sommeil enregistrés dans HealthKit s’affichera automatiquement dans la liste d’activités entraînement.

Nouveau pour iOS 10, cela inclut la liste de liste d’activités entraînement sur iPhone de l’utilisateur ainsi. Par conséquent, même si l’Apple Watch n’est pas à proximité, effectuer s’affiche sur le téléphone.

Sommeil qui inclure des exemples d’énergie met à jour anneau de déplacement de l’utilisateur dans l’application d’activités pour les applications tierces 3e peuvent désormais contribuer à objectifs déplacement quotidiennes de l’utilisateur.

Les étapes suivantes sont nécessaires pour terminer et enregistrer une Session d’entraînement :

[![](workout-apps-images/workout05.png "Fin et l’enregistrement du schéma de Session d’entraînement")](workout-apps-images/workout05.png#lightbox)

1. Tout d’abord, l’application doit terminer la Session d’entraînement.
2. La Session d’entraînement est enregistrée dans HealthKit.
3. Ajouter des exemples (par exemple, de l’énergie consommée ou à distance) à la Session d’entraînement enregistrée.

### <a name="ending-the-session"></a>Fin de la Session

Pour terminer la Session d’entraînement, appelez le `EndWorkoutSession` méthode de la `HKHealthStore` en passant le `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Ceci va réinitialiser les capteurs de périphériques à leur mode normal. Après la fin d’effectuer par HealthKit, il reçoit un rappel à la `DidChangeToState` méthode de la `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>L’enregistrement de la Session

Une fois que l’application a terminé la Session d’entraînement, il doit créer un entraînement (`HKWorkout`) et l’enregistrer (avec des événements) dans le magasin de données HealthKit (`HKHealthStore`) :

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

Ce code crée le besoin de quantité totale d’énergie gravé et à distance pour effectuer en tant que `HKQuantity` objets. Un dictionnaire de métadonnées qui définissent l’entraînement est créé et l’emplacement de l’entraînement est spécifié :

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Un nouveau `HKWorkout` objet est créé avec le même `HKWorkoutActivityType` en tant que le `HKWorkoutSession`, les dates de début et de fin, la liste des événements (en cours accumulées dans les sections ci-dessus), l’énergie gravé, nombre total de distance et du dictionnaire de métadonnées. Cet objet est enregistré dans le magasin de contrôle d’intégrité et les erreurs.  

### <a name="adding-samples"></a>Ajout d’exemples

Lorsque l’application enregistre un ensemble d’exemples à Use, HealthKit génère une connexion entre les échantillons et effectuer lui-même, afin que l’application peut interroger HealthKit à une date ultérieure pour tous les exemples associés à un entraînement donné. À l’aide de ces informations, l’application peut générer des graphiques à partir des données d’entraînement et tracer les contre une chronologie d’entraînement.

Pour une application afin de contribuer à anneau de déplacer l’application de l’activité, elle doit inclure exemples énergie avec l’entraînement enregistré. En outre, les totaux de distance et d’énergie doivent correspondre à la somme de d’échantillons de l’application associe un entraînement enregistré.

Pour ajouter des échantillons à Use enregistrée, procédez comme suit :

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

Si vous le souhaitez, l’application peut calculer et créer un sous-ensemble plus petit d’échantillons ou d’un échantillon méga (la plage entière d’effectuer la répartition) obtient ensuite associé à l’entraînement enregistré.

## <a name="workouts-and-ios-10"></a>Sommeil et iOS 10

Chaque application d’entraînement watchOS 3 a une application de base entraînement iOS 10 parent et nouvelles pour iOS 10, cette application iOS peut être utilisée pour démarrer les Use qui placer l’Apple Watch dans le Mode d’entraînement (sans intervention de l’utilisateur) et exécutez l’application watchOS en mode en cours d’exécution en arrière-plan (consultez [sur en arrière-plan en cours d’exécution](#About-Background-Running) ci-dessus pour plus d’informations).

Pendant l’exécution de l’application watchOS, elle peut utiliser WatchConnectivity pour la communication avec l’application iOS de parent et de messagerie.

Examinons le fonctionnement de ce processus :

[![](workout-apps-images/workout06.png "iPhone et schéma de communication de l’Apple Watch")](workout-apps-images/workout06.png#lightbox)

1. L’application iPhone crée un `HKWorkoutConfiguration` de l’objet et définit le Type d’entraînement et l’emplacement.
2. Le `HKWorkoutConfiguration` objet est envoyé à la version de l’Apple Watch de l’application et, si elle n’est pas déjà en cours d’exécution, il est démarré par le système.
3. Démarrage de l’application watchOS 3 à l’aide de l’élément passé dans la Configuration d’entraînement, une nouvelle Session d’entraînement (`HKWorkoutSession`).

> [!IMPORTANT]
> Pour l’application iPhone parent démarrer un entraînement sur l’Apple Watch, l’application watchOS 3 doit avoir en cours d’exécution en arrière-plan activés. Consultez [l’activation en cours d’exécution en arrière-plan](#Enabling-Background-Running) ci-dessus pour plus d’informations.

Ce processus est très similaire au processus de démarrage d’une Session d’entraînement dans l’application watchOS 3 directement. Sur l’iPhone, utilisez le code suivant :

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

Ce code vérifie que la version watchOS de l’application est installée et que la version de l’iPhone peut se connecter en premier :

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Puis il crée un `HKWorkoutConfiguration` comme d’habitude et utilise le `StartWatchApp` méthode de le `HKHealthStore` à envoyer à l’Apple Watch et démarrer l’application et la Session d’entraînement.

Et sur l’application de la surveillance du système d’exploitation, utilisez le code suivant dans le `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Il prend le `HKWorkoutConfiguration` et crée un nouveau `HKWorkoutSession` et l’attache une instance personnalisé `HKWorkoutSessionDelegate`. La Session d’entraînement est lancée sur le magasin de contrôle d’intégrité de l’utilisateur HealthKit.

## <a name="bringing-all-the-pieces-together"></a>Regrouper toutes les pièces

En utilisant toutes les informations présentées dans ce document, une application d’entraînement en fonction de watchOS 3 et son application d’entraînement en fonction d’iOS 10 parent peuvent inclure les éléments suivants :

1. **iOS 10 `ViewController.cs`**  -gère le démarrage d’une session de la connectivité d’espion et use sur l’Apple Watch.
2. **watchOS 3 `ExtensionDelegate.cs`**  -gère la version watchOS 3 de l’application d’entraînement.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -personnalisé `HKWorkoutSessionDelegate` pour gérer les événements pour effectuer.

> [!IMPORTANT]
> Le code présenté dans les sections suivantes inclut uniquement les parties requises pour implémenter les nouvelles fonctionnalités améliorées de fourni aux applications d’entraînement dans watchOS 3. Prise en charge tout le code et le code de présenter et mettre à jour l’interface utilisateur n’est pas inclus, mais peuvent facilement être créés en suivant notre autre documentation watchOS.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

Le `ViewController.cs` fichier dans la version d’iOS 10 parent de l’application d’entraînement comprend le code suivant :

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

Le `ExtensionDelegate.cs` fichier dans la version watchOS 3 de l’application d’entraînement comprend le code suivant :

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

Le `OutdoorRunDelegate.cs` fichier dans la version watchOS 3 de l’application d’entraînement comprend le code suivant :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>Meilleures pratiques

Apple suggère les meilleures pratiques suivantes lors de la conception et implémentation d’applications d’entraînement dans watchOS 3 et iOS 10 à l’aide de :

- Assurez-vous que l’application d’entraînement watchOS 3 fonctionne toujours même lorsqu’il est impossible de se connecter à l’iPhone et la version iOS 10 de l’application.
- Utilisez HealthKit distance lorsque GPS n’est pas disponible, car il est en mesure de générer des exemples de distance sans GPS.
- Autoriser l’utilisateur démarrer l’effectuer à partir de l’Apple Watch ou l’iPhone.
- Autoriser l’application à afficher sommeil provenant d’autres sources (telles que les autres applications de partie 3) dans ses vues des données historiques.
- Assurez-vous que l’application ne pas afficher supprimé sommeil dans les données historiques.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les améliorations Apple a apportées aux applications d’entraînement dans watchOS 3 et comment les implémenter dans Xamarin.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Introduction aux HealthKit](~/ios/platform/healthkit.md)
