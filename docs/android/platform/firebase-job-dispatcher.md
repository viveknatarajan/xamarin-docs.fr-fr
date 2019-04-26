---
title: Répartiteur de travail Firebase
description: Ce guide explique comment planifier le travail en arrière-plan à l’aide de la bibliothèque de répartiteur de travail Firebase à partir de Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 91bafbbdaee805ad128766bf0a770cb711597a85
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023348"
---
# <a name="firebase-job-dispatcher"></a>Répartiteur de travail Firebase

_Ce guide explique comment planifier le travail en arrière-plan à l’aide de la bibliothèque de répartiteur de travail Firebase à partir de Google._


## <a name="overview"></a>Vue d'ensemble

Un des meilleurs moyens de maintenir la réactivité à l’utilisateur une application Android est de garantir que le travail complexe ou à long terme est effectuée en arrière-plan. Toutefois, il est important que travail en arrière-plan ne sera pas un impact négatif sur l’expérience utilisateur avec l’appareil. 

Par exemple, une tâche en arrière-plan peut interroger un site Web à trois ou quatre minutes à interroger pour les modifications apportées à un jeu de données particulier. Cela semble sans gravité, toutefois, il aurait un impact désastreux sur l’autonomie. L’application sera à plusieurs reprises sortir de veille de l’appareil, élever l’UC à un état d’alimentation plus élevé, les signaux radio sous tension, vérifiez les demandes réseau et le traitement des résultats. Il est encore plus délicate, car l’appareil ne sera pas immédiatement hors tension et revenir à l’état inactif de faible puissance. Travail en arrière-plan mal planifiée peut conserver par inadvertance de l’appareil dans un état avec les spécifications power inutile et excessif. Cette activité innocente (interrogation un site Web) affichera l’appareil inutilisable dans une période relativement courte.

Android fournit les API suivantes pour aider à réaliser un travail en arrière-plan, mais par eux-mêmes qu’ils ne sont pas suffisantes pour la planification des travaux intelligent. 

* **[Services Intent](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; intention Services sont parfaits pour effectuer le travail, mais ils n’offrent aucun moyen pour planifier le travail.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; ces API permettent uniquement être planifiée, mais n’offrent aucun moyen pour effectuer le travail le travail. En outre, le AlarmManager autorise uniquement contraintes de temps en fonction, ce qui signifie que de déclencher une alarme à un moment donné ou après qu’un certain laps de temps s’est écoulé. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; JobSchedule l’est une excellente API qui fonctionne avec le système d’exploitation pour planifier des tâches. Toutefois, il est uniquement disponible pour les applications Android qui ciblent le niveau d’API 21 ou version ultérieure. 
* **[Récepteurs de diffusion](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; Android une application peut configurer des récepteurs de diffusion pour effectuer le travail en réponse à des événements système ou des objectifs. Toutefois, les récepteurs de diffusion ne fournissent aucun contrôle sur quand la tâche doit s’exécuter. Également les modifications dans le système d’exploitation Android limitera lorsque les récepteurs de diffusion fonctionnera, ou les types de travail qu’ils puissent répondre aux. 

Il existe deux éléments essentiels à l’exécution efficace de travail en arrière-plan (parfois appelé un _tâche en arrière-plan_ ou un _travail_) :

1. **Intelligemment planification du travail** &ndash; il est important que lorsqu’une application effectue un travail en arrière-plan qui il le fait un bon citoyen. Dans l’idéal, l’application ne doit pas demander qu’une tâche est exécutée. Au lieu de cela, l’application doit spécifier les conditions qui doivent être remplies pour la tâche peut s’exécuter, et ensuite planifier ce travail à exécuter lorsque les conditions sont remplies. Cela permet à Android effectuer le travail intelligemment. Par exemple, les demandes réseau peuvent être regroupés par lots pour s’exécuter en même temps pour profiter au maximum de surcharge impliquée avec mise en réseau.
2. **Qui encapsule le travail** &ndash; le code pour effectuer le travail d’arrière-plan doit être encapsulé dans un composant discret qui peut être exécuté indépendamment de l’interface utilisateur et sera relativement facile de replanification nécessaire si le travail ne se termine pour une raison quelconque.

Le répartiteur de travail Firebase est une bibliothèque à partir de Google qui fournit une API fluent pour simplifier la planification du travail en arrière-plan. Il est destiné à être le remplacement pour le Gestionnaire de Cloud de Google. Le répartiteur de travail Firebase se compose des API suivantes :

* Un `Firebase.JobDispatcher.JobService` est une classe abstraite qui doit être étendue avec la logique qui s’exécute dans la tâche en arrière-plan.
* Un `Firebase.JobDispatcher.JobTrigger` déclare lorsque le travail doit être démarré. Cela est généralement exprimée comme une fenêtre de temps, par exemple, attendez au moins 30 secondes avant de démarrer le travail, mais exécuter le travail dans les 5 minutes.
* Un `Firebase.JobDispatcher.RetryStrategy` contient des informations sur ce qui doit être effectué quand une tâche ne parvient pas à s’exécuter correctement. La stratégie de nouvelle tentative spécifie la durée d’attente avant d’essayer de réexécuter la tâche. 
* Un `Firebase.JobDispatcher.Constraint` est une valeur facultative qui décrit une condition qui doit être remplie pour que le travail puisse exécuter, telles que l’appareil se trouve sur un réseau sans compteur ou de chargement.
* Le `Firebase.JobDispatcher.Job` est une API qui unifie les API précédentes dans pour une unité de travail qui peut être planifiée par le `JobDispatcher`. Le `Job.Builder` classe est utilisée pour instancier un `Job`.
* Un `Firebase.JobDispatcher.JobDispatcher` utilise trois API précédentes pour planifier le travail avec le système d’exploitation et de fournir un moyen d’annuler les travaux, si nécessaire.

Pour planifier le travail avec le répartiteur de travail Firebase, une application Xamarin.Android doit encapsuler le code dans un type qui étend la `JobService` classe. `JobService` a trois méthodes de cycle de vie qui peuvent être appelés pendant la durée de vie de la tâche :

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Cette méthode est où le travail se produira et doit toujours être implémenté. Il s’exécute sur le thread principal. Cette méthode retournera `true` si fonctionne restant, ou `false` si le travail est effectué. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Il s’agit quand le travail est interrompu pour une raison quelconque. Elle doit retourner `true` si le travail doit être replanifié pour une utilisation ultérieure.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Cette méthode est appelée lorsque le `JobService` a fini n’importe quel travail asynchrone. 

Pour planifier un travail, l’application instanciera un `JobDispatcher` objet. Ensuite, une `Job.Builder` est utilisé pour créer un `Job` objet, qui est fourni pour le `JobDispatcher` qui essayez et planifier le travail à exécuter.

Ce guide explique comment ajouter le répartiteur de travail Firebase à une application Xamarin.Android et l’utiliser pour planifier le travail d’arrière-plan.

## <a name="requirements"></a>Configuration requise

Le répartiteur de travail Firebase requiert un niveau d’API Android 9 ou version ultérieure. La bibliothèque de répartiteur de travail Firebase s’appuie sur certains composants fournis par Google Play Services ; l’appareil doit avoir Google Play Services est installé.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>À l’aide de la bibliothèque de répartiteur de travail Firebase dans Xamarin.Android

Pour vous familiariser avec le répartiteur de travail Firebase, ajoutez d’abord le [Xamarin.Firebase.JobDispatcher NuGet package](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) au projet Xamarin.Android. Rechercher le Gestionnaire de Package NuGet pour la **Xamarin.Firebase.JobDispatcher** package (qui est toujours en version préliminaire).

Après avoir ajouté la bibliothèque de répartiteur de travail Firebase, créer un `JobService` classe et puis planifier son exécution avec une instance de la `FirebaseJobDispatcher`.


### <a name="creating-a-jobservice"></a>Création d’un JobService

Tout le travail effectué par la bibliothèque de répartiteur de travail Firebase doit être effectué dans un type qui étend la `Firebase.JobDispatcher.JobService` classe abstraite. Création d’un `JobService` est très similaire à la création d’un `Service` avec le framework Android : 

1. Étendre la `JobService` classe
2. Décorer la sous-classe avec le `ServiceAttribute`. Bien que pas strictement obligatoire, il est recommandé de définir explicitement la `Name` paramètre pour faciliter le débogage du `JobService`. 
3. Ajouter un `IntentFilter` pour déclarer le `JobService` dans le **AndroidManifest.xml**. Cela permet également la bibliothèque de répartiteur de travail Firebase localiser et appeler le `JobService`.

Le code suivant est un exemple de la plus simple `JobService` pour une application, à l’aide de la bibliothèque parallèle de tâches à effectuer certaines tâches de façon asynchrone :

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

Avant tout le travail peut être planifié, il est nécessaire de créer un `Firebase.JobDispatcher.FirebaseJobDispatcher` objet. Le `FirebaseJobDispatcher` est chargé de planifier un `JobService`. L’extrait de code suivant est une façon de créer une instance de la `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Dans l’extrait de code précédent, le `GooglePlayDriver` est la classe qui permet la `FirebaseJobDispatcher` interagir avec certaines des API de planifications dans Google Play Services sur l’appareil. Le paramètre `context` est n’importe quel Android `Context`, telle qu’une activité. Actuellement le `GooglePlayDriver` est le seul `IDriver` implémentation dans la bibliothèque de répartiteur de travail Firebase. 

La liaison de Xamarin.Android pour le répartiteur de travail Firebase fournit une méthode d’extension pour créer un `FirebaseJobDispatcher` à partir de la `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Une fois le `FirebaseJobDispatcher` a été instancié, il est possible de créer un `Job` et exécuter le code de la `JobService` classe. Le `Job` est créé par un `Job.Builder` de l’objet et sera abordé dans la section suivante.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Création d’un Firebase.JobDispatcher.Job avec la Job.Builder

Le `Firebase.JobDispatcher.Job` classe est chargée pour encapsuler les métadonnées nécessaires pour exécuter un `JobService`. Un`Job` contient des informations telles que les contraintes qui doivent être remplies avant que le travail peut être exécuté, si le `Job` est récurrente, ou des déclencheurs qui entraînent le travail à exécuter.  Comme strict minimum, un `Job` doit avoir un _balise_ (une chaîne unique qui identifie le travail à la `FirebaseJobDispatcher`) et le type de la `JobService` qui doit être exécutée. Le répartiteur de travail Firebase instanciera le `JobService` quand il est temps d’exécution du travail.  Un `Job` est créé à l’aide d’une instance de la `Firebase.JobDispatcher.Job.JobBuilder` classe. 

L’extrait de code suivant est un exemple plus simple de la création d’un `Job` à l’aide de la liaison Xamarin.Android :

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

Le `Job.Builder` effectue certaines vérifications de validation de base sur les valeurs d’entrée pour le travail. Une exception est levée si elle n’est pas possible pour le `Job.Builder` pour créer un `Job`.  Le `Job.Builder` créera un `Job` avec les valeurs par défaut suivantes :

* Un `Job`de _durée de vie_ (la durée pendant laquelle il sera programmée pour s’exécuter) est uniquement jusqu'à ce que l’appareil redémarre &ndash; une fois que l’appareil redémarre le `Job` est perdue.
* Un `Job` n’est pas périodique &ndash; il exécute qu’une seule fois.
* Un `Job` sera programmée pour s’exécuter dès que possible.
* La stratégie de nouvelle tentative par défaut pour un `Job` consiste à utiliser un _interruption exponentielle_ (discutées plus en détail ci-dessous dans la section [définissant un RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Planification d’un travail

Après avoir créé le `Job`, il doit être planifiée avec le `FirebaseJobDispatcher` avant son exécution. Il existe deux méthodes pour planifier un `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

La valeur retournée par `FirebaseJobDispatcher.Schedule` sera une des valeurs entières suivantes :

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; Le `Job` a été correctement planifiée.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Un problème inconnu s’est produite qui a empêché la `Job` d’être planifiés.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Non valide `IDriver` a été utilisé ou `IDriver` d’une certaine manière était indisponible. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; Le `Trigger` n’était pas prise en charge.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Le service n’est pas configuré correctement ou n’est pas disponible.
 
### <a name="configuring-a-job"></a>Configuration d’un travail

Il est possible de personnaliser un travail. Exemples de la façon dont un travail peut être personnalisé sont les suivants :

* [Passage de paramètres à un travail](#Passing_Parameters_to_a_Job) &ndash; A `Job` peut nécessiter des valeurs supplémentaires pour effectuer son travail, par exemple télécharger un fichier.
* [Définir des contraintes](#Setting_Constraints) &ndash; il peut être nécessaire exécuter uniquement une tâche lorsque certaines conditions sont remplies. Par exemple, exécuter uniquement un `Job` lorsque l’appareil est en charge. 
* [Spécifiez le moment un `Job` doit s’exécuter](#Setting_Job_Triggers) &ndash; le répartiteur de travail Firebase permet aux applications de spécifier une heure où le travail doit s’exécuter.  
* [Déclarer une stratégie de nouvelle tentative pour les travaux ayant échoués](#Setting_a_RetryStrategy) &ndash; A _stratégie de nouvelle tentative_ fournit des conseils pour le `FirebaseJobDispatcher` sur quoi faire avec `Jobs` qui ne pas se terminer. 

Chacune de ces rubriques est étudiées plus en détail dans les sections suivantes.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Passage de paramètres à un travail

Paramètres sont passés à une tâche en créant un `Bundle` qui est passé avec la `Job.Builder.SetExtras` méthode :

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

#### <a name="setting-constraints"></a>Définition des contraintes

Les contraintes peuvent aider à réduit les coûts ou batterie drainage sur l’appareil. Le `Firebase.JobDispatcher.Constraint` classe définit ces contraintes en tant que valeurs entières :

* `Constraint.OnUnmeteredNetwork` &ndash; Exécuter uniquement le travail quand l’appareil est connecté à un réseau sans compteur. Cela est utile pour empêcher l’utilisateur de frais liés aux données.
* `Constraint.OnAnyNetwork` &ndash; Exécuter le travail sur le réseau de l’appareil est connecté à. Si spécifié avec `Constraint.OnUnmeteredNetwork`, cette valeur est prioritaire.
* `Constraint.DeviceCharging` &ndash; Exécuter le travail uniquement lorsque l’appareil est en charge.

Les contraintes sont définies avec la `Job.Builder.SetConstraint` méthode : 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

Le `JobTrigger` fournit des conseils pour le système d’exploitation sur lorsque le travail doit démarrer. Un `JobTrigger` a un _l’exécution de fenêtre_ qui définit une heure planifiée quand le `Job` doit s’exécuter. La fenêtre d’exécution possède un _fenêtre Démarrer_ valeur et un _fenêtre Terminer_ valeur. La fenêtre de démarrage est le nombre de secondes que l’appareil doit attendre avant d’exécuter le travail et la valeur de fenêtre de fin est le nombre maximal de secondes à attendre avant d’exécuter le `Job`. 

Un `JobTrigger` peuvent être créés avec le `Firebase.Jobdispatcher.Trigger.ExecutionWindow` (méthode).  Par exemple `Trigger.ExecutionWindow(15,60)` signifie que le travail doit s’exécuter entre 15 et 60 secondes à partir de laquelle elle est planifiée. Le `Job.Builder.SetTrigger` méthode est utilisée pour 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

La valeur par défaut `JobTrigger` pour un travail est représenté par la valeur `Trigger.Now`, qui spécifie qu’une tâche est exécutée dès que possible après la planification...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Définition d’un RetryStrategy

Le `Firebase.JobDispatcher.RetryStrategy` est utilisé pour spécifier la quantité d’un délai un appareil doit utiliser avant d’essayer de réexécuter une tâche ayant échoué. Un `RetryStrategy` a un _stratégie_, qui définit quel algorithme de base de temps permet de replanifier le travail ayant échoué et une fenêtre de l’exécution qui spécifie une fenêtre dans laquelle la tâche doit être planifiée. Cela _fenêtre replanification_ est définie par deux valeurs. La première valeur est le nombre de secondes à attendre avant de replanifier le travail (la _interruption initiale_ valeur), et le deuxième nombre est le nombre maximal de secondes avant que la tâche doit s’exécuter (le _interruption maximale_valeur). 
 
Les deux types de stratégies de nouvelle tentative sont identifiés par ces valeurs int :

* `RetryStrategy.RetryPolicyExponential` &ndash; Un _interruption exponentielle_ stratégie augmentera la valeur initiale d’interruption exponentielle après chaque échec. La première fois qu’une tâche échoue, la bibliothèque attendra l’intervalle _initial spécifié avant de replanifier le travail &ndash; exemple 30 secondes. La deuxième fois que le travail échoue, la bibliothèque attend au moins 60 secondes avant d’essayer d’exécuter le travail. Une fois que le troisième Échec de la tentative, la bibliothèque de 120 secondes et ainsi de suite. La valeur par défaut `RetryStrategy` pour le répartiteur de travail Firebase bibliothèque est représentée par le `RetryStrategy.DefaultExponential` objet. Il a une interruption initiale de 30 secondes et une interruption maximale de 3 600 secondes.
* `RetryStrategy.RetryPolicyLinear` &ndash; Cette stratégie est un _interruption linéaire_ que le travail doit être replanifié pour s’exécuter à des intervalles définis (jusqu'à ce qu’elle réussit). Interruption linéaire est idéale pour le travail qui doit être effectuée dès que possible ou pour les problèmes rapidement résoudre eux-mêmes. La bibliothèque de répartiteur de travail Firebase définit un `RetryStrategy.DefaultLinear` qui a une fenêtre de replanification d’au moins 30 secondes et jusqu'à 3 600 secondes.

Il est possible de définir un personnalisé `RetryStrategy` avec la `FirebaseJobDispatcher.NewRetryStrategy` (méthode). Elle accepte trois paramètres :

1. `int policy` &ndash; Le _stratégie_ fait partie de la précédente `RetryStrategy` valeurs, `RetryStrategy.RetryPolicyLinear`, ou `RetryStrategy.RetryPolicyExponential`.
2. `int initialBackoffSeconds` &ndash; Le _interruption initiale_ est un délai, en secondes, qui est requis avant d’essayer de réexécuter la tâche. La valeur par défaut est 30 secondes. 
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

Il est possible d’annuler toutes les tâches qui ont été planifiées, ou simplement une tâche unique à l’aide la `FirebaseJobDispatcher.CancelAll()` méthode ou le `FirebaseJobDispatcher.Cancel(string)` (méthode) :

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Soit la méthode retourne une valeur entière :

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; La tâche a été annulée avec succès.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Une erreur a empêché l’en cours d’annulation de la tâche.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; Le `FirebaseJobDispatcher` est impossible d’annuler le travail car il n’est pas valide `IDriver` disponibles.

## <a name="summary"></a>Récapitulatif

Ce guide décrit comment utiliser le répartiteur de travail Firebase pour effectuer intelligemment le travail en arrière-plan. Il a expliqué comment encapsuler le travail à exécuter en tant qu’un `JobService` et comment utiliser le `FirebaseJobDispatcher` pour planifier ce travail, en spécifiant les critères d’un `JobTrigger` et comment les échecs doivent être gérés avec une `RetryStrategy`.


## <a name="related-links"></a>Liens associés

- [Xamarin.Firebase.JobDispatcher sur NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [-répartiteur de travail firebase sur GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Planification des travaux intelligente](https://developer.android.com/topic/performance/scheduling.html)
- [Android batterie et les optimisations de mémoire - Google d’e/s 2016 (vidéo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
