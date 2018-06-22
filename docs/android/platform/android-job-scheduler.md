---
title: Planificateur de travaux Android
description: Ce guide explique comment planifier le travail en arrière-plan à l’aide de l’API du Planificateur de travaux Android.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2018
ms.locfileid: "30005685"
---
# <a name="android-job-scheduler"></a>Planificateur de travaux Android

_Ce guide explique comment planifier le travail en arrière-plan à l’aide de l’API du Planificateur de travaux Android, qui est disponible sur les appareils Android exécutant Android 5.0 (niveau 21 d’API) et versions ultérieures._


## <a name="overview"></a>Vue d'ensemble 

Un des meilleurs moyens d’éviter à une application Android réactive à l’utilisateur est de garantir que le travail complex ou à long terme est effectué en arrière-plan. Toutefois, il est important que travail d’arrière-plan ne sera pas un impact négatif sur l’expérience utilisateur avec l’appareil. 

Par exemple, une tâche en arrière-plan peut interroger un site Web à trois ou quatre minutes à interroger pour les modifications apportées à un jeu de données. Cela peut paraître inoffensifs, toutefois, cela aurait un impact désastreux sur l’autonomie. L’application sera à plusieurs reprises sortir le périphérique, élever l’UC pour un état d’alimentation plus élevé, radios sous tension, vérifiez les demandes réseau et ensuite le traitement des résultats. Il s’aggrave, car l’appareil ne sera pas immédiatement hors tension et revenir à l’état inactif de faible puissance. Travail d’arrière-plan mal planifiée peut par inadvertance conserver l’appareil dans un état d’alimentation inutile et excessif. Cette activité apparemment innocente (interrogation un site Web) rend le périphérique inutilisable dans une période relativement courte.

Android fournit les API suivantes pour aider à effectuer le travail en arrière-plan, mais par elles-mêmes ou qu’ils ne sont pas suffisantes pour la planification du travail intelligent. 

* **[Services intentionnels](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; intention de Services sont très utiles pour effectuer le travail, mais elles n’offrent aucun moyen de planifier le travail.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; ces API permettent uniquement être planifiée, mais ne fournissent aucun moyen pour effectuer le travail le travail. Permet également de, le AlarmManager uniquement les contraintes de temps en fonction, ce qui signifie que de déclencher une alarme à un moment donné ou après qu’une certaine période de temps écoulé. 
* **[Récepteurs](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; Android une application peut le programme d’installation pour effectuer un travail en réponse à des événements système ou des modes de récepteurs de diffusion. Toutefois, les récepteurs de diffusion ne fournissent pas quand la tâche doit s’exécuter n’importe quel contrôle. Également les modifications dans le système d’exploitation Android limitera lorsque les récepteurs de diffusion fonctionnent, ou les types de travail qu’ils puissent répondre aux. 

Il existe deux principales fonctionnalités pour effectuer efficacement les tâches en arrière-plan (parfois appelé un _tâche en arrière-plan_ ou un _travail_) :

1. **Intelligemment et planification du travail** &ndash; il est important que lorsqu’une application effectue un travail en arrière-plan qui il le fait un bon citoyen. Dans l’idéal, l’application ne doit pas demander qu’un travail est exécutée. Au lieu de cela, l’application doit spécifier les conditions qui doivent être remplies pour lorsque la tâche peut s’exécuter et ensuite planifier cette tâche avec le système d’exploitation qui effectue le travail lorsque les conditions sont remplies. Cela permet d’exécuter le travail pour assurer une efficacité maximale sur l’appareil Android. Par exemple, les demandes réseau peuvent être regroupés par lots pour s’exécuter en même temps pour rendre l’utilisation maximale de traitement liés à la mise en réseau.
2. **Encapsuler le travail** &ndash; le code pour effectuer le travail d’arrière-plan doit être encapsulé dans un composant séparé qui s’exécute indépendamment de l’interface utilisateur et relativement facile à prévoir si le travail ne se termine pour une raison quelconque.

Le Planificateur de travaux Android est une infrastructure intégrée dans le système d’exploitation Android qui fournit une API fluent pour simplifier la planifier le travail en arrière-plan.  Le Planificateur de travaux Android se compose des types suivants :

* Le `Android.App.Job.JobScheduler` est un service système qui est utilisé pour planifier, exécuter et Annuler si nécessaire, les travaux pour le compte d’une application Android.
* Un `Android.App.Job.JobService` est une classe abstraite qui doit être étendue avec la logique qui vont exécuter le travail sur le thread principal de l’application. Cela signifie que le `JobService` est responsable de la façon dont le travail doit être exécutée de façon asynchrone.
* Un `Android.App.Job.JobInfo` objet contient les critères pour guider Android lorsque la tâche doit s’exécuter.

Pour planifier le travail avec le Planificateur de travaux Android, une application de Xamarin.Android doit encapsuler le code dans une classe qui étend la `JobService` classe. `JobService` a trois méthodes de cycle de vie qui peut être appelée pendant la durée de vie de la tâche :

* **bool OnStartJob (paramètres JobParameters)** &ndash; cette méthode est appelée par le `JobScheduler` pour effectuer le travail et s’exécute sur le thread principal de l’application. Il incombe à le `JobService` pour exécuter de façon asynchrone le travail et `true` s’il existe de travail restant, ou `false` si le travail est effectué.
    
    Lorsque le `JobScheduler` appelle cette méthode, il demande et conserver un wakelock à partir d’Android pour la durée du travail. Lorsque la tâche est terminée, il est responsable de la `JobService` pour indiquer le `JobScheduler` de ce fait par l’appel de la `JobFinished` (méthode) (décrit ci-dessous).

* **JobFinished (paramètres de JobParameters, bool needsReschedule)** &ndash; cette méthode doit être appelée par le `JobService` pour indiquer le `JobScheduler` que le travail est effectué. Si `JobFinished` n’est pas appelée, le `JobScheduler` ne supprimera pas le wakelock, à l’origine d’une décharge inutiles de la batterie. 

* **bool OnStopJob (paramètres JobParameters)** &ndash; celle-ci est appelée lorsque le travail est arrêté prématurément par Android. Elle doit retourner `true` si le travail doit être replanifié selon les critères de nouvelle tentative (décrites plus en détail ci-dessous).

Il est possible de spécifier _contraintes_ ou _déclencheurs_ afin de pouvoir contrôler quand une tâche peut ou doit s’exécuter. Par exemple, il est possible limiter une tâche afin qu’elle sera exécutée uniquement lorsque l’appareil est en cours de chargement ou pour démarrer une tâche lorsqu’une image est effectuée.

Ce guide explique en détail comment implémenter un `JobService` classe et à planifier la `JobScheduler`.

## <a name="requirements"></a>Spécifications

Du Planificateur de travaux Android nécessite le niveau de l’API Android 21 (Android 5.0) ou une version ultérieure. 

## <a name="using-the-android-job-scheduler"></a>À l’aide du Planificateur de travaux Android

Il existe trois étapes pour l’utilisation de l’API JobScheduler Android :

1. Implémenter un type JobService pour encapsuler le travail.
2. Utilisez un `JobInfo.Builder` objet à créer le `JobInfo` objet qui conserve les critères pour le `JobScheduler` pour exécuter le travail. 
3. Planifier le travail à l’aide de `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implémenter un JobService

Tout le travail effectué par la bibliothèque du Planificateur de travaux Android doit être effectué dans un type qui étend la `Android.App.Job.JobService` classe abstraite. Création d’un `JobService` est très similaire à la création d’un `Service` avec l’infrastructure Android : 

1. Étendre la `JobService` classe.
2. Décorer la sous-classe avec la `ServiceAttribute` et définir le `Name` paramètre à une chaîne qui se compose du nom du package et le nom de la classe (voir l’exemple suivant).
3. Définir le `Permission` propriété sur le `ServiceAttribute` à la chaîne `android.permission.BIND_JOB_SERVICE`.
4. Remplacer la `OnStartJob` méthode, en ajoutant le code pour effectuer le travail. Android appelle cette méthode sur le thread principal de l’application pour exécuter le travail. Travail prendra plus de temps que quelques millisecondes doivent être exécutées sur un thread afin d’éviter le blocage de l’application.
5. Lorsque le travail est effectué, le `JobService` doit appeler le `JobFinished` (méthode). Cette méthode est la `JobService` indique le `JobScheduler` que le travail est effectué. Échec d’appel `JobFinished` entraîne la `JobService` placer des demandes inutiles sur l’appareil, réduire la durée de vie de batterie. 
6. Il est judicieux d’également substituer la `OnStopJob` (méthode). Cette méthode est appelée par Android lorsque la tâche est en cours d’arrêt avant qu’il ait terminé et fournit le `JobService` une possibilité de supprimer correctement toutes les ressources. Cette méthode doit retourner `true` s’il est nécessaire de replanifier le travail, ou `false` s’il n’est pas desireable exécuter à nouveau la tâche.
   

Le code suivant est un exemple de la plus simple `JobService` pour une application, à l’aide de la bibliothèque parallèle de tâches à exécuter de façon asynchrone des tâches :

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Création d’un JobInfo pour planifier un travail

Xamarin.Android applications n’instancient pas un `JobService` directement, au lieu de cela ils passera un `JobInfo` de l’objet à le `JobScheduler`. Le `JobScheduler` instancie demandé `JobService` objet, la planification et l’exécution du `JobService` selon les métadonnées dans le `JobInfo`. A `JobInfo` objet doit contenir les informations suivantes :

* **JobId** &ndash; il s’agit d’un `int` valeur utilisée pour identifier une tâche à la `JobScheduler`. Tous les travaux existants met à jour de la réutilisation de cette valeur. La valeur doit être unique pour l’application. 
* **JobService** &ndash; ce paramètre est un `ComponentName` qui identifie de manière explicite le type qui le `JobScheduler` doit utiliser pour exécuter une tâche. 
  

Cette méthode d’extension montre comment créer un `JobInfo.Builder` avec un Android `Context`, comme une activité :

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

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Une fonctionnalité puissante du planificateur Android est la possibilité de contrôler quand une tâche s’exécute ou dans quelles conditions une tâche peut s’exécuter. Le tableau suivant décrit quelques-unes des méthodes sur `JobInfo.Builder` qui permettent à une application influencer lors de l’exécution d’une tâche :  


|  Méthode | Description   |
|---|---|
| `SetMinimumLatency`  | Spécifie qu’un délai (en millisecondes) qui doit-elle être respectée avant une tâche est exécuté. |
| `SetOverridingDeadline`  | Déclare que la tâche doit être exécutée avant ce délai (en millisecondes). |
| `SetRequiredNetworkType`  | Spécifie la configuration réseau requise pour un travail. |
| `SetRequiresBatteryNotLow` | Il peut s’exécuter uniquement lorsque l’appareil n’affiche un avertissement « batterie » à l’utilisateur. |
| `SetRequiresCharging` | Il peut s’exécuter uniquement lorsque la charge de la batterie. |
| `SetDeviceIdle` | La tâche sera exécutée lorsque le périphérique est occupé. |
| `SetPeriodic` | Spécifie que le travail doit être exécuté régulièrement. |
| `SetPersisted` | La tâche doit perisist entre les redémarrages de l’appareil. | 


Le `SetBackoffCriteria` fournit des conseils sur la durée pendant laquelle le `JobScheduler` doit attendre avant d’essayer de réexécuter une tâche. Pour les critères d’interruption en deux parties : un délai en millisecondes (valeur par défaut de 30 secondes) et le type d’interruption qui doit être utilisé (parfois appelé le _stratégie d’interruption_ ou le _stratégie de nouvelle tentative_) . Les deux stratégies sont encapsulées dans le `Android.App.Job.BackoffPolicy` enum :

* `BackoffPolicy.Exponential` &ndash; Une stratégie d’interruption exponentielle augmentera la valeur initiale d’interruption exponentielle après chaque défaillance. La première fois qu’une tâche échoue, la bibliothèque attendra l’intervalle initial spécifié avant de replanifier le travail – exemple 30 secondes. La deuxième fois que le travail échoue, la bibliothèque attend au moins 60 secondes avant d’essayer d’exécuter le travail. Une fois que le troisième Échec de la tentative, la bibliothèque de 120 secondes et ainsi de suite. Valeur par défaut.
* `BackoffPolicy.Linear` &ndash; Cette stratégie est une interruption linéaire qui doit être de replanifier la tâche à exécuter à des intervalles définis (jusqu'à ce qu’il réussit). Interruption linéaire est parfaitement adaptée pour le travail qui doit être exécutée dès que possible, ou pour les problèmes qui se résoudra rapidement. 

Pour plus d’informations sur Créer un `JobInfo` objet, veuillez lire [documentation de Google pour le `JobInfo.Builder` classe](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Passage de paramètres à un travail via le JobInfo

Les paramètres sont passés à un travail en créant un `PersistableBundle` qui lui est passé avec la `Job.Builder.SetExtras` méthode :

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

Pour planifier un travail, une application de Xamarin.Android obtenez une référence à la `JobScheduler` service système et appelez le `JobScheduler.Schedule` méthode avec le `JobInfo` objet qui a été créé à l’étape précédente. `JobScheduler.Schedule` est retournée immédiatement avec une des deux valeurs entières :

* **JobScheduler.ResultSuccess** &ndash; le travail a été planifiée. 
* **JobScheduler.ResultFailure** &ndash; Impossible de planifier le travail. Cela est généralement dû à en conflit `JobInfo` paramètres.

Ce code est un exemple de planification d’une tâche et informer l’utilisateur des résultats de la tentative de planification :

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

Il est possible d’annuler tous les travaux qui ont été planifiées ou simplement une tâche unique à l’aide de la `JobsScheduler.CancelAll()` (méthode) ou `JobScheduler.Cancel(jobId)` (méthode) :

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Récapitulatif

Ce guide décrit comment utiliser le Planificateur de travaux Android pour effectuer le travail intelligent en arrière-plan. Il décrit comment encapsuler le travail à exécuter en tant qu’un `JobService` et comment utiliser le `JobScheduler` planification de ce travail, en spécifiant les critères d’un `JobTrigger` et comment les échecs doivent être gérés avec un `RetryStrategy`.

## <a name="related-links"></a>Liens associés

- [Planification de travaux intelligente](https://developer.android.com/topic/performance/scheduling.html)
- [Référence de l’API de JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Planification des tâches comme un pro avec JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android batterie et les optimisations de la mémoire - Google d’e/s 2016 (vidéo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert - Xamarin University](https://www.youtube.com/watch?v=aSjBBPYjelE)