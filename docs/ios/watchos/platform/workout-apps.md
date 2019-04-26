---
title: watchOS applications séances d’entraînement dans Xamarin
description: Cet article aborde les améliorations Apple a apportées aux applications de séances d’entraînement dans watchOS 3 et comment les implémenter dans Xamarin.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 02db7dce6ba38b6c1e943ff189ff69efb7cc1c08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61083956"
---
# <a name="watchos-workout-apps-in-xamarin"></a>watchOS applications séances d’entraînement dans Xamarin

_Cet article aborde les améliorations Apple a apportées aux applications de séances d’entraînement dans watchOS 3 et comment les implémenter dans Xamarin._


Nouveau watchOS 3, séances d’entraînement associé à des applications ont la possibilité d’exécuter en arrière-plan sur l’Apple Watch et accéder aux données d’HealthKit. Son application iOS 10 en fonction de parent a également la possibilité de lancer l’application en fonction de watchOS 3 sans intervention de l’utilisateur.

Les rubriques suivantes sont traitées en détail :

## <a name="about-workout-apps"></a>Sur les applications de séances d’entraînement

Les utilisateurs d’applications de Fitness (pertinence) et les séances d’entraînement peuvent être hautement dédiés, réserver alloués à plusieurs heures de la journée leurs objectifs de contrôle d’intégrité et de pertinence (fitness). Par conséquent, qu’elles attendent des applications réactives et facile à utiliser qui précisément collectent et affichent des données et une intégration transparente avec contrôle d’intégrité de l’Apple.

Une application bien conçue de Fitness (pertinence) ou d’entraînement permet aux utilisateurs de leurs activités pour atteindre leurs objectifs de Fitness (pertinence) du graphique. À l’aide de l’Apple Watch, les applications de Fitness (pertinence) et les séances d’entraînement ont un accès instantané à un rythme cardiaque, détection d’avancement et l’activité de calories.

[![](workout-apps-images/workout01.png "Exemple d’application de Fitness (pertinence) et les séances d’entraînement")](workout-apps-images/workout01.png#lightbox)

Nouveau à watchOS 3, _en cours d’exécution en arrière-plan_ séances d’entraînement donne liés applications la possibilité de s’exécuter en arrière-plan sur l’Apple Watch et accéder aux données d’HealthKit.

Ce document présentent la fonctionnalité en cours d’exécution en arrière-plan, couvrent le cycle de vie des applications séances d’entraînement et montrent comment une application de séances d’entraînement peut contribuer à l’utilisateur _activité anneaux_ sur l’Apple Watch.

## <a name="about-workout-sessions"></a>Sur les Sessions d’entraînement

Le cœur de chaque application séances d’entraînement est un _séances d’entraînement Session_ (`HKWorkoutSession`) que l’utilisateur peut démarrer et arrêter. L’API de Session d’entraînement est facile à implémenter et fournit plusieurs avantages à une application d’entraînement, telles que :

- Mouvement et calories graver détection basée sur le Type d’activité.
- Contribution automatique à anneaux d’activité de l’utilisateur.
- Au cours d’une session, l’application affichera automatiquement chaque fois que l’utilisateur sort de l’appareil (soit en déclenchant leur poignet ou en interagissant avec l’Apple Watch).

## <a name="about-background-running"></a>Sur l’exécution de l’arrière-plan

Comme indiqué ci-dessus, avec watchOS 3 une application d’entraînement peut être configurée pour fonctionner en arrière-plan. À l’aide d’une application de l’exercice en cours d’exécution en arrière-plan peut traiter les données de capteurs de l’Apple Watch lors de l’exécution en arrière-plan. Par exemple, une application continue de surveiller le taux de cœur de l’utilisateur, même si elle n’est plus affiché sur l’écran.

En cours d’exécution en arrière-plan fournit également la possibilité de présenter des commentaires en direct à l’utilisateur à tout moment pendant une Session active de séances d’entraînement, comme l’envoi d’une alerte haptique pour informer l’utilisateur de progression actuelle.

En outre, en cours d’exécution en arrière-plan permet à l’application à mettre à jour rapidement son Interface utilisateur pour l’utilisateur dispose des dernières données quand ils œil rapidement à leurs Apple Watch.

Pour maintenir des performances élevées sur Apple Watch, une application watch à l’aide en cours d’exécution en arrière-plan doit limiter la quantité de travail en arrière-plan afin d’économiser la batterie. Si une application utilise trop de ressources processeur en arrière-plan, il peut obtenir interrompue par watchOS.

### <a name="enabling-background-running"></a>L’activation en cours d’exécution en arrière-plan

Pour permettre l’exécution de l’arrière-plan, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur l’application iPhone de l’Extension Watch Compagnon `Info.plist` fichier à ouvrir pour modification.
2. Basculez vers le **Source** vue : 

    [![](workout-apps-images/plist01.png "La vue de Source")](workout-apps-images/plist01.png#lightbox)
3. Ajoutez une nouvelle clé appelée `WKBackgroundModes` et définir le **Type** à `Array`: 

    [![](workout-apps-images/plist02.png "Ajoutez une nouvelle clé appelée WKBackgroundModes")](workout-apps-images/plist02.png#lightbox)
4. Ajouter un nouvel élément dans le tableau avec la **Type** de `String` et la valeur `workout-processing`: 

    [![](workout-apps-images/plist03.png "Ajouter un nouvel élément dans le tableau avec le Type de chaîne et une valeur de traitement des séances d’entraînement")](workout-apps-images/plist03.png#lightbox)
5. Enregistrez les modifications dans le fichier.

## <a name="starting-a-workout-session"></a>Démarrage d’une Session d’entraînement

Il existe trois étapes principales pour démarrer une Session de l’exercice :

[![](workout-apps-images/workout02.png "Les trois principales étapes pour démarrer une Session d’entraînement")](workout-apps-images/workout02.png#lightbox)

1. L’application doit demander une autorisation pour accéder aux données dans HealthKit.
2. Créer un objet de Configuration de l’exercice pour le type de l’exercice en cours de démarrage.
3. Créez et démarrez une Session d’entraînement à l’aide de la Configuration d’entraînement nouvellement créé.

### <a name="requesting-authorization"></a>Demandez une autorisation

Une application peut accéder aux données HealthKit de l’utilisateur, il doit demander et recevoir l’autorisation de l’utilisateur. Selon la nature de l’application d’entraînement, il peut être les types de requêtes suivants :

- Autorisation d’écrire des données :
    - Sommeil
- Autorisation de lire des données :
    - Énergie consommée
    - Distance
    - Rythme cardiaque  

Avant d’une application peut demander l’autorisation, il doit être configuré pour accéder à HealthKit.

Effectuez ce qui suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Entitlements.plist` pour l’ouvrir et le modifier.
2. Faites défiler vers le bas et vérifiez **HealthKit activer**: 

    [![](workout-apps-images/auth01.png "Cocher Activer HealthKit")](workout-apps-images/auth01.png#lightbox)
3. Enregistrez les modifications dans le fichier.
4. Suivez les instructions de la [ID d’application explicite et profil de provisionnement](~/ios/platform/healthkit.md) et [associant l’ID d’application et la mise en service de profil avec votre application Xamarin.iOS](~/ios/platform/healthkit.md) sections de la [présentation HealthKit](~/ios/platform/healthkit.md) article pour configurer correctement l’application.
5. Enfin, utilisez les instructions fournies dans le [Kit de contrôle d’intégrité de programmation](~/ios/platform/healthkit.md) et [demandant l’autorisation de l’utilisateur](~/ios/platform/healthkit.md) sections de la [présentation d’HealthKit](~/ios/platform/healthkit.md) article à la demande autorisation d’accéder à la banque de données de l’utilisateur HealthKit.

### <a name="setting-the-workout-configuration"></a>Définition de la Configuration de séances d’entraînement

Séances d’entraînement Sessions sont créées à l’aide d’un objet de Configuration d’entraînement (`HKWorkoutConfiguration`) qui spécifie le type d’entraînement (tel que `HKWorkoutActivityType.Running`) et l’emplacement d’entraînement (tel que `HKWorkoutSessionLocationType.Outdoor`) :

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Création d’un délégué de Session séances d’entraînement 

Pour gérer les événements qui peuvent se produire pendant une Session d’entraînement, l’application doit créer une instance de délégué de Session d’entraînement. Ajoutez une nouvelle classe au projet et baser issu de la `HKWorkoutSessionDelegate` classe. Pour l’exemple d’une exécution en extérieur, il peut ressembler à ce qui suit :

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

Cette classe crée plusieurs événements qui seront déclenchés en tant que l’état de la Session d’entraînement change (`DidChangeToState`) et que la Session d’entraînement échoue (`DidFail`). 

### <a name="creating-a-workout-session"></a>Création d’une Session d’entraînement

À l’aide de la Configuration de séances d’entraînement et les séances d’entraînement Session délégué créé ci-dessus pour créer une nouvelle Session d’entraînement et démarrez-le sur le magasin de l’utilisateur par défaut HealthKit :

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

Si l’application démarre cette Session d’entraînement et que l’utilisateur bascule vers leur cadran de montre, une icône verte « en cours d’exécution man » minuscule s’affiche au-dessus de la face :

[![](workout-apps-images/workout03.png "Une petite vert en cours d’exécution man icône affichée au-dessus de la face")](workout-apps-images/workout03.png#lightbox)

Si l’utilisateur appuie sur cette icône, ils seront alors renvoyé à l’application.

## <a name="data-collection-and-control"></a>Collecte de données et de contrôle

Une fois qu’une Session d’entraînement a été configurée et démarrée, l’application devra collecter des données sur la session (telles que le taux de cœur de l’utilisateur) et de contrôler l’état de la session :

[![](workout-apps-images/workout04.png "Diagramme de contrôle et de collecte de données")](workout-apps-images/workout04.png#lightbox)

1. **En observant les exemples** -l’application doit récupérer les informations d’HealthKit qui est traité et affiché à l’utilisateur.
2. **En observant les événements** -l’application doit répondre aux événements générés par HealthKit ou à partir de l’interface utilisateur de l’application (par exemple, l’utilisateur la suspension de l’exercice).
3. **Entrez l’état d’exécution** -la session a été démarrée et est en cours d’exécution.
4. **Entrez l’état suspendu** -l’utilisateur a interrompu la session active de séances d’entraînement et pouvez la redémarrer à une date ultérieure. L’utilisateur peut basculer entre les États en cours d’exécution et suspendus plusieurs fois dans une seule Session de séances d’entraînement.
5. **Terminer la Session de séances d’entraînement** : À tout moment l’utilisateur peut se terminer la Session d’entraînement ou il peut expirer et se terminent sur son propre s’il s’agissait d’un exercice limitée (par exemple, une exécution mile deux).

L’étape finale consiste à enregistrer les résultats de la Session d’entraînement au magasin de données de l’utilisateur HealthKit.

### <a name="observing-healthkit-samples"></a>En observant les exemples HealthKit

L’application doit ouvrir un _requête d’objet d’ancrage_ pour chacun des données HealthKit points qui l’intéressent, tels que le rythme cardiaque ou l’énergie active gravé. Pour chaque point de données en cours d’observation, un gestionnaire de mise à jour sera doivent être créés pour capturer les nouvelles données lorsqu’il est envoyé à l’application.

Ces points de données, l’application peut cumuler les totaux (par exemple, la distance d’exécution totale) et mettre à jour son Interface utilisateur en fonction des besoins. En outre, l’application peut avertir les utilisateurs lorsqu’ils ont atteint un objectif spécifique ou la réalisation, telles que les étapes suivant d’une exécution à la fin.

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

Il crée un prédicat pour définir la date de début qu’il souhaite obtenir des données pour l’utilisation de la `GetPredicateForSamples` (méthode). Il crée un ensemble d’appareils pour extraire des informations d’HealthKit d’à l’aide de la `GetPredicateForObjectsFromDevices` méthode, dans ce cas le local dans l’Apple Watch (`HKDevice.LocalDevice`). Les deux prédicats sont combinés dans un prédicat composée (`NSCompoundPredicate`) à l’aide de la `CreateAndPredicate` (méthode).

Un nouveau `HKAnchoredObjectQuery` est créé pour le point de données que vous le souhaitez (dans ce cas `HKQuantityTypeIdentifier.ActiveEnergyBurned` pour le point de données Active gravé d’énergie), aucune limite sur la quantité de données retournées (`HKSampleQuery.NoLimit`) et un gestionnaire de mise à jour est défini pour gérer le retour à l’application en cours de données à partir d’HealthKit. 

Le Gestionnaire de mise à jour sera appelé chaque fois que les données nouvelles sont fournies à l’application du point de données donné. Si aucune erreur n’est retournée, l’application peut en toute sécurité lire les données, vérifiez les calculs requis et mettre à jour son interface utilisateur en fonction des besoins.

Le code effectue une itération sur tous les exemples (`HKSample`) retournées dans le `addedObjects` de tableau et effectue un cast à un échantillon de quantité (`HKQuantitySample`). Ensuite, il obtient la valeur double de l’exemple comme un joule (`HKUnit.Joule`) et s’accumulent dans le total cumulé de l’énergie active consommée pour les entraînements et met à jour de l’Interface utilisateur.

### <a name="achieved-goal-notification"></a>Notification de l’objectif atteint

Comme mentionné ci-dessus, lorsque l’utilisateur atteint un objectif dans l’application d’entraînement (par exemple, la fin de la première mile d’une exécution), il peut envoyer retour haptique à l’utilisateur via le moteur Taptic. L’application doit également mettre à jour son interface utilisateur à ce stade, dans la mesure où l’utilisateur augmente plus que probable que leur poignet pour voir l’événement que vous y êtes invité aux commentaires.

Pour lire le retour haptique, utilisez le code suivant :

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>En observant les événements

Les événements sont des timestamps que l’application peut utiliser pour mettre en évidence certains points au cours de l’exercice de l’utilisateur. Certains événements sont créés directement par l’application et enregistrés dans l’exercice et certains événements seront créés automatiquement par HealthKit.

Pour observer les événements qui sont créés par HealthKit, l’application remplacera le `DidGenerateEvent` méthode de la `HKWorkoutSessionDelegate`:

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

- `HKWorkoutEventType.Lap` -Sont pour les événements qui rompent l’entraînement en portions d’une distance égale. Par exemple, pour marquer un horizon une piste lors de son exécution.
- `HKWorkoutEventType.Marker` -Sont des points arbitraires d’intérêt dans l’exercice. Par exemple, atteindre un point spécifique sur l’itinéraire d’une série d’articles de plein air.

Ces nouveaux types peuvent être créés par l’application et stockés dans les séances d’entraînement pour une utilisation ultérieure lors de la création de graphiques et des statistiques.

Pour créer un événement de marqueur, procédez comme suit :

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

Ce code crée une nouvelle instance d’un événement de marqueur (`HKWorkoutEvent`) et les enregistre dans une collection privée d’événements (qui sera plus tard être écrits dans la Session d’entraînement) et informe l’utilisateur de l’événement via HAPTIQUES.

### <a name="pausing-and-resuming-workouts"></a>Suspension et reprise de sommeil

À tout moment dans une session d’entraînement, l’utilisateur peut temporairement suspendre l’entraînement et reprendre ultérieurement. Par exemple, ils s’interrompe une exécution intérieur pour prendre une opération importante et de reprendre l’exécution après que l’appel est terminé.

L’interface utilisateur de l’application doit fournir un moyen pour suspendre et reprendre l’entraînement (en appelant HealthKit) afin que l’Apple Watch peut économiser de l’espace d’alimentation et de données tandis que l’utilisateur a suspendu son activité. En outre, l’application doit ignorer les nouveaux points de données qui peuvent être reçus lorsque la Session d’entraînement est dans un état suspendu.

HealthKit répondra pour suspendre et reprendre des appels en générant des événements de suspendre et reprendre. Lorsque la Session d’entraînement est suspendue, aucune donnée ou nouveaux événements ne sera être envoyée à l’application par HealthKit jusqu'à la reprise de la session.

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

Les événements de suspendre et reprendre qui doit être générés à partir d’HealthKit peuvent être gérés en substituant le `DidGenerateEvent` méthode de la `HKWorkoutSessionDelegate`:

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

Également nouveau à watchOS 3, sont le mouvement interrompu (`HKWorkoutEventType.MotionPaused`) et la reprise de mouvement (`HKWorkoutEventType.MotionResumed`) événements. Ces événements sont déclenchés automatiquement par HealthKit pendant un exercice en cours d’exécution lorsque l’utilisateur démarre et cesse de se déplacer.

Lorsque l’application reçoit un événement de mouvement est suspendu, il doit s’arrêter la collecte de données jusqu'à ce que l’utilisateur reprend le mouvement et la réception de l’événement mouvement reprend. Application doit être interrompue pas la session d’entraînement en réponse à un événement de mouvement en pause.

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

Lorsque l’utilisateur a terminé son séances d’entraînement, l’application doit à la fin de la Session séances d’entraînement et l’enregistrer dans la base de données HealthKit. Sommeil enregistrés dans HealthKit s’affichera automatiquement dans la liste d’activités séances d’entraînement.

Nouveau à iOS 10, cela inclut la liste de liste d’activité séances d’entraînement sur iPhone de l’utilisateur ainsi. Par conséquent, même si l’Apple Watch n’est pas à proximité, l’entraînement s’affiche sur le téléphone.

Sommeil qui inclure des exemples d’énergie met à jour anneau de déplacer de l’utilisateur dans l’application d’activités pour les applications tierces 3e peuvent contribuer maintenant aux objectifs de déplacement quotidiennes de l’utilisateur.

Les étapes suivantes sont nécessaires à la fin et enregistrer une Session de l’exercice :

[![](workout-apps-images/workout05.png "Fin et l’enregistrement du diagramme de Session séances d’entraînement")](workout-apps-images/workout05.png#lightbox)

1. L’application doit tout d’abord terminer la Session d’entraînement.
2. La Session d’entraînement est enregistrée dans HealthKit.
3. Ajouter des exemples (par exemple, l’énergie consommée ou à distance) à la Session de séances d’entraînement enregistré.

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

Cette opération réinitialise les capteurs de périphériques à leur mode normal. Après la fin de l’exercice par HealthKit, il recevra un rappel à la `DidChangeToState` méthode de la `HKWorkoutSessionDelegate`:

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

### <a name="saving-the-session"></a>La Session est enregistrée

Une fois que l’application a interrompu la Session de séances d’entraînement, il doit créer un entraînement (`HKWorkout`) et enregistrez-le (ainsi que les événements d’une) dans le magasin de données HealthKit (`HKHealthStore`) :

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

Ce code crée le nécessitent une quantité totale de l’énergie consommée et distance pour l’entraînement en tant que `HKQuantity` objets. Un dictionnaire de métadonnées qui définissent l’entraînement est créé et l’emplacement de l’exercice est spécifié :

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Un nouveau `HKWorkout` objet est créé avec le même `HKWorkoutActivityType` en tant que le `HKWorkoutSession`, les dates de début et de fin, la liste des événements (en cours accumulées dans les sections ci-dessus), l’énergie consommée, nombre total de distance et du dictionnaire de métadonnées. Cet objet est enregistré dans le Store de l’intégrité et les erreurs.  

### <a name="adding-samples"></a>Ajout d’exemples

Lorsque l’application enregistre un ensemble d’exemples dans un exercice, HealthKit génère une connexion entre les exemples et les séances d’entraînement lui-même afin que l’application peut interroger HealthKit à une date ultérieure pour tous les exemples associés à un entraînement donné. À l’aide de ces informations, l’application peut générer des graphiques à partir des données séances d’entraînement et les affichera graphiquement par rapport à une chronologie d’entraînement.

Pour une application à contribuer à déplacer anneau l’application de l’activité, il doit inclure des exemples d’énergie avec l’entraînement enregistré. En outre, les totaux de distance et d’énergie doivent correspondre à la somme des exemples de l’application associe à un entraînement enregistré.

Pour ajouter des exemples à un entraînement enregistrée, procédez comme suit :

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

Si vous le souhaitez, l’application peut calculer et créer un sous-ensemble plus petit d’échantillons ou d’un échantillon méga (couvrant la plage entière de l’entraînement) obtient ensuite associé à l’entraînement enregistré.

## <a name="workouts-and-ios-10"></a>Entraînements et iOS 10

Chaque application d’entraînement watchOS 3 a une application de base entraînement iOS 10 parent et nouvelles pour iOS 10, cette application iOS peut être utilisée pour démarrer les Use qui placer l’Apple Watch dans le Mode d’entraînement (sans intervention de l’utilisateur) et exécutez l’application watchOS en mode en cours d’exécution en arrière-plan (consultez [sur en arrière-plan en cours d’exécution](#about-background-running) ci-dessus pour plus d’informations).

Pendant l’exécution de l’application watchOS, il peut utiliser WatchConnectivity pour la messagerie et de communication avec l’application iOS de parent.

Examinons le fonctionnement de ce processus :

[![](workout-apps-images/workout06.png "iPhone et diagramme de communication de l’Apple Watch")](workout-apps-images/workout06.png#lightbox)

1. L’application iPhone crée un `HKWorkoutConfiguration` de l’objet et définit le Type d’entraînement et l’emplacement.
2. Le `HKWorkoutConfiguration` objet est envoyé à la version de l’Apple Watch de l’application et, si elle n’est pas déjà en cours d’exécution, il est démarré par le système.
3. Démarrage de l’application watchOS 3 à l’aide de passé dans la Configuration d’entraînement, une nouvelle Session d’entraînement (`HKWorkoutSession`).

> [!IMPORTANT]
> Pour l’application iPhone parente démarrer un exercice sur l’Apple Watch, l’application watchOS 3 doit avoir en cours d’exécution en arrière-plan est activée. Consultez [l’activation en cours d’exécution en arrière-plan](#enabling-background-running) ci-dessus pour plus d’informations.

Ce processus est très similaire au processus de démarrage d’une Session de séances d’entraînement dans l’application watchOS 3 directement. Sur l’iPhone, utilisez le code suivant :

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

Ce code permet de s’assurer que la version watchOS de l’application est installée et la version iPhone peut vous y connecter tout d’abord :

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Puis il crée un `HKWorkoutConfiguration` comme d’habitude et utilise le `StartWatchApp` méthode de la `HKHealthStore` à envoyer à l’Apple Watch et démarrer l’application et la Session d’entraînement.

Et sur l’application de surveillance du système d’exploitation, utilisez le code suivant dans le `WKExtensionDelegate`:

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

Il prend le `HKWorkoutConfiguration` et crée un nouveau `HKWorkoutSession` et attache une instance de la personnalisation `HKWorkoutSessionDelegate`. La Session d’entraînement est démarrée par rapport à HealthKit Health Store l’utilisateur.

## <a name="bringing-all-the-pieces-together"></a>Rassemble tous les éléments

Effectué toutes les informations présentées dans ce document, un watchOS 3 séances d’entraînement en application et son parent iOS 10 séances d’entraînement en peuvent inclure les éléments suivants :

1. **iOS 10 `ViewController.cs`**  -gère le démarrage d’une session de connectivité de surveillance et d’un exercice sur l’Apple Watch.
2. **watchOS 3 `ExtensionDelegate.cs`**  -gère la version watchOS 3 de l’application d’entraînement.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -personnalisé `HKWorkoutSessionDelegate` pour gérer les événements pour l’exercice.

> [!IMPORTANT]
> Le code indiqué dans les sections suivantes inclut uniquement les parties requises pour implémenter les fonctionnalités nouvelles, améliorées fournies aux applications de séances d’entraînement dans watchOS 3. Tout code de prise en charge et le code de présenter et mettre à jour de l’interface utilisateur n’est pas inclus, mais peuvent être facilement créées en suivant notre autre documentation watchOS.<p/>



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

Apple suggère à l’aide des meilleures pratiques suivantes lors de la conception et implémentation d’applications d’entraînement dans watchOS 3 et iOS 10 :

- Assurez-vous que l’application de séances d’entraînement watchOS 3 fonctionne toujours même lorsqu’il est impossible de se connecter à l’iPhone et la version iOS 10 de l’application.
- Utilisez HealthKit distance lorsque le GPS n’est pas disponible, car il est en mesure de générer des exemples de distance sans GPS.
- Autoriser l’utilisateur démarrer l’exercice à partir de l’Apple Watch ou l’iPhone.
- Autoriser l’application afficher les entraînements à partir d’autres sources (telles que les autres applications tierces 3e) dans ses vues des données historiques.
- Assurez-vous que l’application ne pas afficher supprimé sommeil dans les données historiques.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les améliorations Apple a apportées aux applications de séances d’entraînement dans watchOS 3 et comment les implémenter dans Xamarin.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Présentation d’HealthKit](~/ios/platform/healthkit.md)
