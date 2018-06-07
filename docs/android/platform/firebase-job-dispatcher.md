---
title: Répartiteur de travail firebase
description: Ce guide explique comment planifier le travail en arrière-plan à l’aide de la bibliothèque Firebase travail répartiteur à partir de Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 06/05/2018
ms.openlocfilehash: 0d512342f1c978c84341d20c298a9fa750800d84
ms.sourcegitcommit: 5db075bdd0b62d5d1d1567c267303a6a1888c8f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806801"
---
# <a name="firebase-job-dispatcher"></a>Répartiteur de travail firebase

_Ce guide explique comment planifier le travail en arrière-plan à l’aide de la bibliothèque Firebase travail répartiteur à partir de Google._


## <a name="overview"></a>Vue d'ensemble

Un des meilleurs moyens d’éviter à une application Android réactive à l’utilisateur est de garantir que le travail complex ou à long terme est effectué en arrière-plan. Toutefois, il est important que travail d’arrière-plan ne sera pas un impact négatif sur l’expérience utilisateur avec l’appareil. 

Par exemple, une tâche en arrière-plan peut interroger un site Web à trois ou quatre minutes à interroger pour les modifications apportées à un jeu de données. Cela peut paraître inoffensifs, toutefois, cela aurait un impact désastreux sur l’autonomie. L’application sera à plusieurs reprises sortir le périphérique, élever l’UC pour un état d’alimentation plus élevé, radios sous tension, vérifiez les demandes réseau et ensuite le traitement des résultats. Il s’aggrave, car l’appareil ne sera pas immédiatement hors tension et revenir à l’état inactif de faible puissance. Travail d’arrière-plan mal planifiée peut par inadvertance conserver l’appareil dans un état d’alimentation inutile et excessif. Cette activité apparemment innocente (interrogation un site Web) rend le périphérique inutilisable dans une période relativement courte.

Android fournit les API suivantes pour aider à effectuer le travail en arrière-plan, mais par elles-mêmes ou qu’ils ne sont pas suffisantes pour la planification du travail intelligent. 

* **[Services intentionnels](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; intention de Services sont très utiles pour effectuer le travail, mais elles n’offrent aucun moyen de planifier le travail.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; ces API permettent uniquement être planifiée, mais ne fournissent aucun moyen pour effectuer le travail le travail. Permet également de, le AlarmManager uniquement les contraintes de temps en fonction, ce qui signifie que de déclencher une alarme à un moment donné ou après qu’une certaine période de temps écoulé. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; JobSchedule l’est une excellente API qui fonctionne avec le système d’exploitation pour planifier des travaux. Toutefois, il est uniquement disponible pour les applications Android qui ciblent le niveau 21 d’API ou une version ultérieure. 
* **[Récepteurs](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; Android une application peut le programme d’installation pour effectuer un travail en réponse à des événements système ou des modes de récepteurs de diffusion. Toutefois, les récepteurs de diffusion ne fournissent pas quand la tâche doit s’exécuter n’importe quel contrôle. Également les modifications dans le système d’exploitation Android limitera lorsque les récepteurs de diffusion fonctionnent, ou les types de travail qu’ils puissent répondre aux. 

Il existe deux principales fonctionnalités pour effectuer efficacement les tâches en arrière-plan (parfois appelé un _tâche en arrière-plan_ ou un _travail_) :

1. **Intelligemment et planification du travail** &ndash; il est important que lorsqu’une application effectue un travail en arrière-plan qui il le fait un bon citoyen. Dans l’idéal, l’application ne doit pas demander qu’un travail est exécutée. Au lieu de cela, l’application doit spécifier les conditions qui doivent être remplies pour lorsque la tâche peut s’exécuter et ensuite planifier ce travail à exécuter lorsque les conditions sont remplies. Ainsi effectuer le travail intelligemment Android. Par exemple, les demandes réseau peuvent être regroupés par lots pour s’exécuter en même temps pour rendre l’utilisation maximale de traitement liés à la mise en réseau.
2. **Encapsuler le travail** &ndash; le code pour effectuer le travail d’arrière-plan doit être encapsulé dans un composant séparé qui s’exécute indépendamment de l’interface utilisateur et relativement facile à prévoir si le travail ne se termine pour une raison quelconque.

Le répartiteur de travail Firebase est une bibliothèque à partir de Google qui fournit une API fluent pour simplifier la planifier le travail en arrière-plan. Elle est destinée à être la solution de remplacement pour le Gestionnaire de Cloud de Google. Le répartiteur de travail Firebase se compose des API suivantes :

* A `Firebase.JobDispatcher.JobService` est une classe abstraite qui doit être étendue avec la logique qui s’exécutera dans la tâche en arrière-plan.
* A `Firebase.JobDispatcher.JobTrigger` déclare lorsque le travail doit être démarré. Cela est généralement exprimée comme une fenêtre de temps, par exemple, patientez au moins 30 secondes avant de démarrer le travail, mais exécuter le travail dans les 5 minutes.
* A `Firebase.JobDispatcher.RetryStrategy` contient des informations sur l’action à entreprendre en cas d’échec d’une tâche pour s’exécuter correctement. La stratégie de nouvelle tentative Spécifie le délai d’attente avant d’essayer de réexécuter la tâche. 
* A `Firebase.JobDispatcher.Constraint` est une valeur facultative qui décrit une condition qui doit être remplie pour permettre d’exécuter la tâche, telle que l’appareil est sur un réseau sans compteur ou de chargement.
* Le `Firebase.JobDispatcher.Job` est une API qui unifie les API précédentes dans pour un unité de travail qui peut être planifié par le `JobDispatcher`. Le `Job.Builder` classe est utilisée pour instancier un `Job`.
* A `Firebasee.JobDispatcher.JobDispatcher` utilise les API de trois précédentes pour planifier le travail avec le système d’exploitation et pour fournir un moyen d’annuler les travaux, si nécessaire.

Pour planifier le travail avec le répartiteur de travail Firebase, une application de Xamarin.Android doit encapsuler le code dans un type qui étend la `JobService` classe. `JobService` a trois méthodes de cycle de vie qui peut être appelée pendant la durée de vie de la tâche :

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Cette méthode est où le travail se produit et qu’il doit toujours être implémenté. Il s’exécute sur le thread principal. Cette méthode retournera `true` s’il existe de travail restant, ou `false` si le travail est effectué. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Cela est appelé quand le travail est interrompu pour une raison quelconque. Elle doit retourner `true` si le travail doit être replanifié pour une date ultérieure.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Cette méthode est appelée lorsque le `JobService` a terminé tout travail asynchrone. 

Pour planifier un travail, l’application instancie un `JobDispatcher` objet. Ensuite, une `Job.Builder` est utilisé pour créer un `Job` objet, qui est fourni pour le `JobDispatcher` qui essayez et planifier le travail à exécuter.

Ce guide explique comment ajouter le répartiteur de travail Firebase à une application de Xamarin.Android et l’utiliser pour planifier le travail d’arrière-plan.

## <a name="requirements"></a>Configuration requise

Le répartiteur de travail Firebase requiert un niveau d’API Android 9 ou version ultérieure. La bibliothèque de répartiteur du travail Firebase s’appuie sur certains composants fournis par les Services Google Play ; l’appareil doit avoir les Services Google Play installé.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>À l’aide de la bibliothèque de répartiteur du travail Firebase dans Xamarin.Android

Pour commencer le répartiteur de travail Firebase, ajoutez d’abord le [package NuGet de Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) pour le projet Xamarin.Android. Rechercher le Gestionnaire de Package NuGet pour la **Xamarin.Firebase.JobDispatcher** package (qui est toujours en version préliminaire).

Après avoir ajouté la bibliothèque Firebase travail répartiteur, créer un `JobService` classe et ensuite planifier son exécution avec une instance de la `FirebaseJobDispatcher`.


### <a name="creating-a-jobservice"></a>Création d’un JobService

Tout le travail effectué par la bibliothèque de répartiteur du travail Firebase doit être effectué dans un type qui étend la `Firebase.JobDispatcher.JobService` classe abstraite. Création d’un `JobService` est très similaire à la création d’un `Service` avec l’infrastructure Android : 

1. Étendre la `JobService` classe
2. Décorer la sous-classe avec le `ServiceAttribute`. Bien que pas strictement obligatoire, il est recommandé de définir explicitement la `Name` paramètre pour aider au débogage du `JobService`. 
3. Ajouter un `IntentFilter` pour déclarer le `JobService` dans les **AndroidManifest.xml**. Cela permet également la bibliothèque de répartiteur du travail Firebase localiser et appeler le `JobService`.

Le code suivant est un exemple de la plus simple `JobService` pour une application, à l’aide de la bibliothèque parallèle de tâches à exécuter de façon asynchrone des tâches :

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Création d’un FirebaseJobDispatcher

Avant tout le travail peut être planifié, il est nécessaire de créer un `Firebase.JobDispatcher.FirebaseJobDispatcher` objet. Le `FirebaseJobDispatcher` est chargé de planifier un `JobService`. L’extrait de code suivant est une méthode de création d’une instance de la `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Dans l’extrait de code précédent, le `GooglePlayDriver` est la classe qui vous aide à la `FirebaseJobDispatcher` interagir avec certains des API de planification dans les Services Google Play sur l’appareil. Le paramètre `context` est n’importe quel Android `Context`, comme une activité. Actuellement le `GooglePlayDriver` est le seul `IDriver` mise en œuvre dans la bibliothèque Firebase travail répartiteur. 

La liaison de Xamarin.Android pour le répartiteur de travail Firebase fournit une méthode d’extension pour créer un `FirebaseJobDispatcher` à partir de la `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Une fois la `FirebaseJobDispatcher` a été instancié, il est possible de créer un `Job` et exécuter le code de la `JobService` classe. Le `Job` est créé par un `Job.Builder` de l’objet et sera abordé dans la section suivante.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Création d’un Firebase.JobDispatcher.Job avec la Job.Builder

Le `Firebase.JobDispatcher.Job` classe est chargée pour encapsuler les métadonnées nécessaires pour exécuter un `JobService`. A`Job` contient des informations telles que les contraintes qui doivent être remplies avant que le travail peut être exécuté, si la `Job` est récurrente, ou des déclencheurs qui entraînent la tâche à exécuter.  En tant qu’un minimum, un `Job` doit avoir un _balise_ (une chaîne unique qui identifie le travail à la `FirebaseJobDispatcher`) et le type de la `JobService` qui doit être exécutée. Le répartiteur de travail Firebase instancie le `JobService` lorsqu’il est temps d’exécution du travail.  A `Job` est créé à l’aide d’une instance de la `Firebase.JobDispatcher.Job.JobBuilder` classe. 

L’extrait de code suivant est un exemple simple de la création d’un `Job` à l’aide de la liaison de Xamarin.Android :

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

Le `Job.Builder` effectue des vérifications de validation de base sur les valeurs d’entrée pour la tâche. Une exception est levée si il est impossible pour le `Job.Builder` pour créer un `Job`.  Le `Job.Builder` créera un `Job` avec les valeurs par défaut suivantes :

* A `Job`de _durée de vie_ (la durée pendant laquelle elle est planifiée pour être exécuté) est uniquement jusqu'à ce que l’appareil redémarre &ndash; une fois que l’appareil redémarre le `Job` est perdue.
* A `Job` n’est pas récurrente &ndash; il exécute qu’une seule fois.
* A `Job` est programmé pour s’exécuter dès que possible.
* La stratégie de nouvelle tentative par défaut pour un `Job` consiste à utiliser un _interruption exponentielle_ (décrite plus en détail ci-dessous dans la section sur les [définissant un RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Planification d’un travail

Après avoir créé le `Job`, elle doit être planifiée avec le `FirebaseJobDispatcher` avant son exécution. Il existe deux méthodes pour planifier un `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

La valeur retournée par `FirebaseJobDispatcher.Schedule` est une des valeurs entières suivantes :

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; Le `Job` a été planifiée avec succès.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Un problème inconnu s’est produite qui a empêché la `Job` d’être planifiés.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Non valide `IDriver` a été utilisé ou `IDriver` était une certaine manière indisponible. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; Le `Trigger` n’était pas prise en charge.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Le service n’est pas configuré correctement ou n’est pas disponible.
 
### <a name="configuring-a-job"></a>Configuration d’un travail

Il est possible de personnaliser une tâche. Voici quelques exemples de la façon dont un travail peut-être être personnalisé :

* [Passage de paramètres à un travail](#Passing_Parameters_to_a_Job) &ndash; A `Job` peut nécessiter des valeurs supplémentaires à effectuer leur travail, par exemple le téléchargement de fichier.
* [Définir des contraintes](#Setting_Constraints) &ndash; il peut être nécessaire exécuter uniquement une tâche lorsque certaines conditions sont remplies. Par exemple, ne s’exécutent un `Job` lorsque l’appareil est en cours de chargement. 
* [Spécifiez à quel moment un `Job` doit s’exécuter](#Setting_Job_Triggers) &ndash; le répartiteur de travail Firebase permet aux applications de spécifier une heure quand la tâche doit s’exécuter.  
* [Déclarez une stratégie de nouvelle tentative pour les travaux ayant échouées](#Setting_a_RetryStrategy) &ndash; A _stratégie de nouvelle tentative_ fournit des conseils pour la `FirebaseJobDispatcher` sur quoi faire avec `Jobs` qui ne parviennent pas à. 

Chacune de ces rubriques est étudiées plus en détail dans les sections suivantes.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-jarameters-to-a-job"></a>Jarameters passage à un travail

Les paramètres sont passés à un travail en créant un `Bundle` qui lui est passé avec la `Job.Builder.SetExtras` méthode :

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Le `Bundle` est accessible à partir de la `IJobParameters.Extras` propriété sur le `OnStartJob` méthode :

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Définition de contraintes

Les contraintes peuvent aider à réduit les coûts ou batterie drainage sur l’appareil. La `Firebase.JobDispatcher.Constraint` classe définit ces contraintes en tant que valeurs entières :

* `Constraint.OnUnmeteredNetwork` &ndash; Exécuter uniquement le travail lorsque l’appareil est connecté à un réseau sans compteur. Cela est utile pour empêcher l’utilisateur de la comptabilisation des frais de données.
* `Constraint.OnAnyNetwork` &ndash; Exécuter le travail sur tout réseau de l’appareil est connecté à. S’il est spécifié avec `Constraint.OnUnmeteredNetwork`, cette valeur est prioritaire.
* `Constraint.DeviceCharging` &ndash; Exécuter le travail uniquement lorsque l’appareil est en cours de chargement.

Les contraintes sont définies avec la `Job.Builder.SetConstraint` méthode : 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

Le `JobTrigger` fournit des conseils pour le système d’exploitation sur laquelle la tâche doit commencer. A `JobTrigger` a un _l’exécution de fenêtre_ qui définit une heure planifiée lorsque la `Job` doit s’exécuter. La fenêtre d’exécution a un _démarrer fenêtre_ valeur et un _fenêtre Terminer_ valeur. La fenêtre de démarrage est le nombre de secondes que l’appareil doit attendre avant d’exécuter le travail et la valeur de fenêtre de fin est le nombre maximal de secondes à attendre avant d’exécuter le `Job`. 

A `JobTrigger` peuvent être créés avec le `Firebase.Jobdispatcher.Trigger.ExecutionWindow` (méthode).  Par exemple `Trigger.ExecutionWindow(15,60)` signifie que la tâche doit s’exécuter entre 15 et 60 secondes à partir de laquelle elle est planifiée. Le `Job.Builder.SetTrigger` (méthode) est utilisée pour 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

La valeur par défaut `JobTrigger` pour une tâche est représentée par la valeur `Trigger.Now`, ce qui indique qu’une tâche est exécutée dès que possible après la planification...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Définition d’un RetryStrategy

Le `Firebase.JobDispatcher.RetryStrategy` est utilisé pour spécifier la quantité de retard un appareil doit utiliser avant d’essayer de réexécuter une tâche ayant échoué. A `RetryStrategy` a un _stratégie_, qui définit quel algorithme de base de temps permet de RE-planifier le travail ayant échoué et une fenêtre de l’exécution qui spécifie une fenêtre dans laquelle la tâche doit être planifiée. Cela _fenêtre replanification_ est définie par deux valeurs. La première valeur est le nombre de secondes à attendre avant de replanifier le travail (la _interruption initiale_ valeur), et le deuxième nombre est le nombre maximal de secondes avant que la tâche doit s’exécuter (le _interruption maximale_valeur). 
 
Les deux types de stratégies de nouvelle tentative sont identifiés par ces valeurs int :

* `RetryStrategy.RetryPolicyExponential` &ndash; Un _interruption exponentielle_ stratégie augmente la valeur initiale d’interruption exponentielle après chaque défaillance. La première fois qu’une tâche échoue, la bibliothèque attendra l’intervalle _initial spécifié avant de replanifier le travail &ndash; exemple 30 secondes. La deuxième fois que le travail échoue, la bibliothèque attend au moins 60 secondes avant d’essayer d’exécuter le travail. Une fois que le troisième Échec de la tentative, la bibliothèque de 120 secondes et ainsi de suite. La valeur par défaut `RetryStrategy` pour le répartiteur de travail Firebase bibliothèque est représentée par la `RetryStrategy.DefaultExponential` objet. Il a une interruption initiale de 30 secondes et une interruption maximale de 3 600 secondes.
* `RetryStrategy.RetryPolicyLinear` &ndash; Cette stratégie est un _interruption linéaire_ que la tâche doit être replanifiée pour s’exécuter à des intervalles définis (jusqu'à ce qu’il réussit). Interruption linéaire est parfaitement adaptée pour le travail qui doit être exécutée dès que possible, ou pour les problèmes qui se résoudra rapidement. La bibliothèque de répartiteur du travail Firebase définit un `RetryStrategy.DefaultLinear` qui a une fenêtre de replanification d’au moins 30 secondes et jusqu'à 3 600 secondes.

Il est possible de définir une personnalisée `RetryStrategy` avec la `FirebaseJobDispatcher.NewRetryStrategy` (méthode). Elle accepte trois paramètres :

1. `int policy` &ndash; Le _stratégie_ fait partie de la précédente `RetryStrategy` valeurs, `RetryStrategy.RetryPolicyLinear`, ou `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; Le _interruption initiale_ est un délai, en secondes, qui est requis avant d’essayer de réexécuter la tâche. La valeur par défaut est 30 secondes. 
3. `int maximumBackoffSeconds` &ndash; Le _interruption maximale_ valeur déclare le nombre maximal de secondes du délai avant d’essayer de réexécuter la tâche. La valeur par défaut est 3 600 secondes. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Annulation d’une tâche

Il est possible d’annuler tous les travaux qui ont été planifiées ou simplement une tâche unique à l’aide de la `FirebaseJobDispatcher.CancelAll()` (méthode) ou `FirebaseJobDispatcher.Cancel(string)` (méthode) :

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Soit la méthode retourne une valeur entière :

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; Le travail a été annulé.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Une erreur a empêché le travail de l’annulation.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; Le `FirebaseJobDispatcher` ne peut pas annuler la tâche car il n’est pas valide `IDriver` disponibles.

## <a name="summary"></a>Récapitulatif

Ce guide décrit comment utiliser le répartiteur de travail Firebase pour effectuer un travail en arrière-plan intelligemment. Il décrit comment encapsuler le travail à exécuter en tant qu’un `JobService` et comment utiliser le `FirebaseJobDispatcher` planification de ce travail, en spécifiant les critères d’un `JobTrigger` et comment les échecs doivent être gérés avec un `RetryStrategy`.


## <a name="related-links"></a>Liens associés

- [Xamarin.Firebase.JobDispatcher sur NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase-travail-répartiteur sur GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Liaison de Xamarin.Firebase.JobDispatcher](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Planification de travaux intelligente](https://developer.android.com/topic/performance/scheduling.html)
- [Android batterie et les optimisations de la mémoire - Google d’e/s 2016 (vidéo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
