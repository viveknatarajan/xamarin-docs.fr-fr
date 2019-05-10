---
title: Planificateur de travaux Android
description: Ce guide explique comment planifier le travail en arrière-plan à l’aide de l’API du Planificateur de travail Android.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: d69a96ee55b09ef9fcf1485ec34d986dd40e7662
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977969"
---
# <a name="android-job-scheduler"></a>Planificateur de travaux Android

_Ce guide explique comment planifier le travail en arrière-plan à l’aide de l’API du Planificateur de travail Android, qui est disponible sur les appareils Android exécutant Android 5.0 (niveau 21 d’API) et versions ultérieures._


## <a name="overview"></a>Vue d'ensemble 

Un des meilleurs moyens de maintenir la réactivité à l’utilisateur une application Android est de garantir que le travail complexe ou à long terme est effectuée en arrière-plan. Toutefois, il est important que travail en arrière-plan ne sera pas un impact négatif sur l’expérience utilisateur avec l’appareil. 

Par exemple, une tâche en arrière-plan peut interroger un site Web à trois ou quatre minutes à interroger pour les modifications apportées à un jeu de données particulier. Cela semble sans gravité, toutefois, il aurait un impact désastreux sur l’autonomie. L’application sera à plusieurs reprises sortir de veille de l’appareil, élever l’UC à un état d’alimentation plus élevé, les signaux radio sous tension, vérifiez les demandes réseau et le traitement des résultats. Il est encore plus délicate, car l’appareil ne sera pas immédiatement hors tension et revenir à l’état inactif de faible puissance. Travail en arrière-plan mal planifiée peut conserver par inadvertance de l’appareil dans un état avec les spécifications power inutile et excessif. Cette activité innocente (interrogation un site Web) affichera l’appareil inutilisable dans une période relativement courte.

Android fournit les API suivantes pour aider à réaliser un travail en arrière-plan, mais par eux-mêmes qu’ils ne sont pas suffisantes pour la planification des travaux intelligent. 

* **[Services Intent](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; intention Services sont parfaits pour effectuer le travail, mais ils n’offrent aucun moyen pour planifier le travail.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; ces API permettent uniquement être planifiée, mais n’offrent aucun moyen pour effectuer le travail le travail. En outre, le AlarmManager autorise uniquement contraintes de temps en fonction, ce qui signifie que de déclencher une alarme à un moment donné ou après qu’un certain laps de temps s’est écoulé. 
* **[Récepteurs de diffusion](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; Android une application peut configurer des récepteurs de diffusion pour effectuer le travail en réponse à des événements système ou des objectifs. Toutefois, les récepteurs de diffusion ne fournissent aucun contrôle sur quand la tâche doit s’exécuter. Également les modifications dans le système d’exploitation Android limitera lorsque les récepteurs de diffusion fonctionnera, ou les types de travail qu’ils puissent répondre aux. 

Il existe deux éléments essentiels à l’exécution efficace de travail en arrière-plan (parfois appelé un _tâche en arrière-plan_ ou un _travail_) :

1. **Intelligemment planification du travail** &ndash; il est important que lorsqu’une application effectue un travail en arrière-plan qui il le fait un bon citoyen. Dans l’idéal, l’application ne doit pas demander qu’une tâche est exécutée. Au lieu de cela, l’application doit spécifier les conditions qui doivent être remplies pour la tâche peut s’exécuter, et ensuite planifier ce travail avec le système d’exploitation qui effectue le travail lorsque les conditions sont remplies. Cela permet à Android exécuter le travail pour assurer une efficacité maximale sur l’appareil. Par exemple, les demandes réseau peuvent être regroupés par lots pour s’exécuter en même temps pour profiter au maximum de surcharge impliquée avec mise en réseau.
2. **Qui encapsule le travail** &ndash; le code pour effectuer le travail d’arrière-plan doit être encapsulé dans un composant discret qui peut être exécuté indépendamment de l’interface utilisateur et sera relativement facile de replanification nécessaire si le travail ne se termine pour une raison quelconque.

Le Planificateur de travaux Android est une infrastructure intégrée dans le système d’exploitation Android qui fournit une API fluent pour simplifier la planification du travail en arrière-plan.  Le Planificateur de travaux Android se compose des types suivants :

* Le `Android.App.Job.JobScheduler` est un service système qui est utilisé pour planifier, exécuter et Annuler si nécessaire, les travaux pour le compte d’une application Android.
* Un `Android.App.Job.JobService` est une classe abstraite qui doit être étendue avec la logique que le travail sera exécuté sur le thread principal de l’application. Cela signifie que le `JobService` est responsable de la manière dont le travail doit être exécutée de façon asynchrone.
* Un `Android.App.Job.JobInfo` objet contient les critères pour guider Android lorsque le travail doit s’exécuter.

Pour planifier le travail avec le Planificateur de travaux Android, une application Xamarin.Android doit encapsuler le code dans une classe qui étend la `JobService` classe. `JobService` a trois méthodes de cycle de vie qui peuvent être appelés pendant la durée de vie de la tâche :

* **bool OnStartJob (paramètres JobParameters)** &ndash; cette méthode est appelée par le `JobScheduler` pour effectuer le travail et s’exécute sur le thread principal de l’application. Il incombe à le `JobService` pour effectuer le travail de façon asynchrone et `true` si fonctionne restant, ou `false` si le travail est effectué.
    
    Lorsque le `JobScheduler` appelle cette méthode, il va de demande et de conserver un wakelock à partir d’Android pour la durée du travail. Lorsque le travail est terminé, il est la responsabilité de la `JobService` pour indiquer le `JobScheduler` de ce fait par appel le `JobFinished` (méthode) (décrit ci-dessous).

* **JobFinished (paramètres de JobParameters, bool needsReschedule)** &ndash; cette méthode doit être appelée par le `JobService` pour indiquer le `JobScheduler` que le travail est effectué. Si `JobFinished` n’est pas appelée, le `JobScheduler` ne supprimera pas le wakelock, à l’origine de décharger la batterie inutiles. 

* **bool OnStopJob (paramètres JobParameters)** &ndash; elle est appelée lorsque le travail est arrêté prématurément par Android. Elle doit retourner `true` si le travail doit être replanifié selon les critères de nouvelle tentative (voir plus en détail ci-dessous).

Il est possible de spécifier _contraintes_ ou _déclencheurs_ qui contrôlera quand une tâche peut ou doit s’exécuter. Par exemple, il est possible limiter une tâche afin qu’elle sera exécutée uniquement lorsque l’appareil est en charge ou pour démarrer une tâche lorsqu’une image est effectuée.

Ce guide explique en détail comment implémenter un `JobService` classe et à planifier le `JobScheduler`.

## <a name="requirements"></a>Configuration requise

Le Planificateur de travaux Android nécessite le niveau d’API Android 21 (Android 5.0) ou une version ultérieure. 

## <a name="using-the-android-job-scheduler"></a>À l’aide du Planificateur de travaux Android

Il existe trois étapes pour l’utilisation de l’API JobScheduler Android :

1. Implémenter un type JobService pour encapsuler le travail.
2. Utilisez un `JobInfo.Builder` objet à créer le `JobInfo` objet qui conserve les critères pour le `JobScheduler` pour exécuter le travail. 
3. Planifier le travail à l’aide `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implémenter un JobService

Tout le travail effectué par la bibliothèque du Planificateur de travaux Android doit être effectué dans un type qui étend la `Android.App.Job.JobService` classe abstraite. Création d’un `JobService` est très similaire à la création d’un `Service` avec le framework Android : 

1. Étendre la `JobService` classe.
2. Décorer la sous-classe avec le `ServiceAttribute` et définir le `Name` paramètre vers une chaîne qui se compose du nom du package et le nom de la classe (voir l’exemple suivant).
3. Définir le `Permission` propriété sur le `ServiceAttribute` à la chaîne `android.permission.BIND_JOB_SERVICE`.
4. Remplacer le `OnStartJob` méthode, en ajoutant le code pour effectuer le travail. Android appelle cette méthode sur le thread principal de l’application pour exécuter le travail. Travail prendra plus de temps que quelques millisecondes doivent être effectuées sur un thread pour éviter de bloquer l’application.
5. Lorsque le travail est terminé, le `JobService` doit appeler le `JobFinished` (méthode). Cette méthode est comment `JobService` indique le `JobScheduler` que le travail est effectué. Échec d’appel `JobFinished` entraîne la `JobService` placer des demandes inutiles sur l’appareil, raccourcir la durée de vie de la batterie. 
6. Il est judicieux également substituer la `OnStopJob` (méthode). Cette méthode est appelée par Android lorsque le travail est en cours d’arrêt avant d’être terminé et fournit le `JobService` avec la possibilité de supprimer correctement toutes les ressources. Cette méthode doit retourner `true` s’il est nécessaire de replanifier le travail, ou `false` si elle n’est pas souhaitable de réexécuter le travail.
   

Le code suivant est un exemple de la plus simple `JobService` pour une application, à l’aide de la bibliothèque parallèle de tâches à effectuer certaines tâches de façon asynchrone :

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Création d’un objet JobInfo pour planifier un travail

Applications Xamarin.Android n’instancient pas un `JobService` directement, au lieu de cela ils transmettra un `JobInfo` de l’objet à le `JobScheduler`. Le `JobScheduler` instanciera demandé `JobService` objet, la planification et l’exécution du `JobService` en fonction des métadonnées dans le `JobInfo`. Un `JobInfo` objet doit contenir les informations suivantes :

* **JobId** &ndash; il s’agit d’un `int` valeur qui est utilisé pour identifier une tâche à la `JobScheduler`. Toutes les tâches existantes met à jour de la réutilisation de cette valeur. La valeur doit être unique pour l’application. 
* **JobService** &ndash; ce paramètre est un `ComponentName` qui identifie explicitement le type qui le `JobScheduler` doit utiliser pour exécuter une tâche. 
  

Cette méthode d’extension montre comment créer un `JobInfo.Builder` avec un Android `Context`, telle qu’une activité :

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

Une fonctionnalité puissante du planificateur Android est la possibilité de contrôler quand une tâche s’exécute ou dans quelles conditions une tâche peut s’exécuter. Le tableau suivant décrit certaines des méthodes sur `JobInfo.Builder` qui permettent à une application influencer lors de l’exécution d’une tâche :  


|  Méthode | Description   |
|---|---|
| `SetMinimumLatency`  | Spécifie qu’un délai (en millisecondes) qui doit-elle être respecté avant une tâche est exécuté. |
| `SetOverridingDeadline`  | Déclare que la tâche doit s’exécuter avant l’expiration de ce délai (en millisecondes). |
| `SetRequiredNetworkType`  | Spécifie la configuration réseau requise pour un travail. |
| `SetRequiresBatteryNotLow` | Il peut s’exécuter uniquement lorsque l’appareil n’affiche pas un avertissement « batterie faible » à l’utilisateur. |
| `SetRequiresCharging` | Il peut s’exécuter uniquement lorsque le chargement de la batterie. |
| `SetDeviceIdle` | La tâche s’exécute quand l’appareil est occupé. |
| `SetPeriodic` | Spécifie que le travail doit être exécuté régulièrement. |
| `SetPersisted` | La tâche doit perisist pour tous les redémarrages de l’appareil. | 


Le `SetBackoffCriteria` fournit des conseils sur la durée pendant laquelle le `JobScheduler` doit attendre avant d’essayer de réexécuter une tâche. Les critères d’interruption se compose de deux parties : un délai en millisecondes (valeur par défaut de 30 secondes) et le type de d’interruption qui doit être utilisé (parfois appelé le _stratégie d’interruption_ ou le _stratégie de nouvelle tentative_) . Les deux stratégies sont encapsulées dans le `Android.App.Job.BackoffPolicy` enum :

* `BackoffPolicy.Exponential` &ndash; Une stratégie d’interruption exponentielle augmentera la valeur initiale d’interruption exponentielle après chaque échec. La première fois qu’une tâche échoue, la bibliothèque attendra l’intervalle initial qui est spécifié avant de replanifier le travail – exemple 30 secondes. La deuxième fois que le travail échoue, la bibliothèque attend au moins 60 secondes avant d’essayer d’exécuter le travail. Une fois que le troisième Échec de la tentative, la bibliothèque de 120 secondes et ainsi de suite. Valeur par défaut.
* `BackoffPolicy.Linear` &ndash; Cette stratégie est une interruption linéaire qui doit être de replanifier la tâche à exécuter à intervalles réguliers (jusqu'à ce qu’elle réussit). Interruption linéaire est idéale pour le travail qui doit être effectuée dès que possible ou pour les problèmes rapidement résoudre eux-mêmes. 

Pour plus d’informations sur Créer un `JobInfo` objet, veuillez lire [documentation de Google pour le `JobInfo.Builder` classe](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Passage de paramètres à un travail via le JobInfo

Paramètres sont passés à une tâche en créant un `PersistableBundle` qui est passé avec la `Job.Builder.SetExtras` méthode :

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

Le `PersistableBundle` est accessible à partir de la `Android.App.Job.JobParameters.Extras` propriété dans le `OnStartJob` méthode d’un `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Planification d’un travail

Pour planifier un travail, une application Xamarin.Android obtiendra une référence à la `JobScheduler` service système et appelez le `JobScheduler.Schedule` méthode avec le `JobInfo` objet qui a été créé à l’étape précédente. `JobScheduler.Schedule` est retournée immédiatement avec une des deux valeurs entières :

* **JobScheduler.ResultSuccess** &ndash; le travail a été planifié. 
* **JobScheduler.ResultFailure** &ndash; Impossible de planifier le travail. Cela est généralement dû à en conflit `JobInfo` paramètres.

Ce code est un exemple de planification d’un travail et d’avertir l’utilisateur des résultats de la tentative de planification :

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>Annulation d’une tâche

Il est possible d’annuler toutes les tâches qui ont été planifiées, ou simplement une tâche unique à l’aide la `JobsScheduler.CancelAll()` méthode ou le `JobScheduler.Cancel(jobId)` (méthode) :

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Récapitulatif

Ce guide décrit comment utiliser le Planificateur de travaux Android pour effectuer intelligemment le travail en arrière-plan. Il a expliqué comment encapsuler le travail à exécuter en tant qu’un `JobService` et comment utiliser le `JobScheduler` pour planifier ce travail, en spécifiant les critères d’un `JobTrigger` et comment les échecs doivent être gérés avec une `RetryStrategy`.

## <a name="related-links"></a>Liens associés

- [Planification des travaux intelligente](https://developer.android.com/topic/performance/scheduling.html)
- [Référence de l’API de JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Planification des tâches comme un pro avec JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android batterie et les optimisations de mémoire - Google d’e/s 2016 (vidéo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
