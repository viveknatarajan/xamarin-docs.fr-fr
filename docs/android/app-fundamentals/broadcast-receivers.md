---
title: "Récepteur de diffusion de Xamarin.Android"
description: "Cette section explique comment utiliser un récepteur de diffusion."
ms.topic: article
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7ea280e11e4051b51da8c20eb9ac5a4e298547f3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="broadcast-receiver-in-xamarinandroid"></a>Récepteur de diffusion de Xamarin.Android

_Cette section explique comment utiliser un récepteur de diffusion._


## <a name="broadcast-receiver-overview"></a>Vue d’ensemble du récepteur de diffusion

A _récepteur de diffusion_ est un composant Android qui permet à une application répondre aux messages (un Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) qui sont diffusés par le système d’exploitation Android ou par une application. Diffusions suivent un _la publication-abonnement_ modèle &ndash; un événement provoque une diffusion à publier et reçus par les composants qui sont intéressés par l’événement. 

Android identifie deux catégories de diffusions :

* **Diffusion normale** &ndash; une diffusion normale est routée vers tous les récepteurs de diffusion inscrits dans un ordre indéterminé. Chaque récepteur reçoit l’intention un ordre non défini. 
* **Classés diffusion** &ndash; une diffusion ordonnée est remise à la fois pour les récepteurs inscrits. Lorsque l’intention est reçue, le récepteur de diffusion peut modifier l’objectif ou se termine par la diffusion.

Le récepteur de diffusion est une sous-classe de la `BroadcastReceiver` classe et qu’il doivent remplacer le [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) (méthode). Android exécutera `OnReceive` sur le thread principal, par conséquent, cette méthode doit être conçue pour s’exécuter rapidement. Soyez vigilant lors de la génération dynamique de threads dans `OnReceive` car Android peut terminer le processus lors de la méthode se termine. Si un récepteur de diffusion doit effectuer un travail en cours d’exécution longue, il est recommandé de planifier un _travail_ à l’aide de la `JobScheduler` ou _Firebase travail répartiteur_. Planification de travail avec un travail est abordée dans un repère distinct.

Un _filtre intention_ est utilisée pour inscrire un récepteur de diffusion afin que Android de router correctement les messages. Le filtre intention peut être spécifié lors de l’exécution (il est parfois appelée une _contexte inscrit un récepteur_ ou en tant que _l’enregistrement dynamique_) ou il peut être défini de manière statique dans le manifeste Android (un _manifeste-inscrit un récepteur_). Xamarin.Android fournit un attribut de c#, `IntentFilterAttribute`, qui s’inscriront statiquement le filtre intention (cela est décrit plus en détail plus loin dans ce guide). 

La principale différence entre le récepteur inscrit de manifeste et le récepteur du contexte enregistré est qu’un récepteur de contexte enregistré répond uniquement aux diffusions pendant l’exécution d’une application, tandis qu’un récepteur inscrit de manifeste peut répondre à diffuse même si l’application ne peut pas être exécuté.  

Il existe deux ensembles d’API pour la gestion d’un récepteur de diffusion et l’envoi des diffusions :

1. **`Context`** &ndash; La `Android.Content.Context` classe peut être utilisée pour inscrire un récepteur de diffusion qui va répondre aux événements système. Le `Context` est également utilisé pour publier les diffusions à l’échelle du système.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Il s’agit d’une API qui est disponible via le [package NuGet de v4 bibliothèque de prise en charge de Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Cette classe est utilisée pour conserver les diffusions et les récepteurs de diffusion isolés dans le contexte de l’application qui les utilise. Cette classe peut être utile pour empêcher les autres applications à partir de répondre aux diffusions uniquement par l’application d’envoyer des messages aux récepteurs privés.

Un récepteur de diffusion peut ne pas affiche de boîtes de dialogue, et il est fortement déconseillé pour démarrer une activité dans un récepteur de diffusion. Si un récepteur de diffusion doit notifier l’utilisateur, il doit publier une notification.

Il n’est pas possible de lier à ou démarrer un service à partir d’un récepteur de diffusion. 

Ce guide couvre la création d’un récepteur de diffusion et l’enregistrement de sorte qu’il peut recevoir des diffusions.

## <a name="creating-a-broadcast-receiver"></a>Création d’un récepteur de diffusion

Pour créer un récepteur de diffusion de Xamarin.Android, une application doit sous-classe le `BroadcastReceiver` de classe, avec orner le `BroadcastReceiverAttribute`et remplacez le `OnReceive` (méthode) :

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.
        
        String value = intent.GetStringExtra("key");
    }
}
```

Lorsque Xamarin.Android compile la classe, il met également à jour le AndroidManifest avec les métadonnées nécessaires pour inscrire le récepteur. Pour un récepteur de diffusion inscrits de manière statique, le `Enabled` correctement doit avoir la valeur `true`, sinon Android ne seront pas en mesure de créer une instance du récepteur.
 
Le `Exported` propriété contrôle si le récepteur de diffusion peut recevoir des messages de l’extérieur de l’application. Si la propriété n’est pas définie explicitement, la valeur par défaut de la propriété est déterminée par Android selon s’il existe des filtres-intention associés du récepteur de diffusion. Si au moins une intention-filtre pour le récepteur de diffusion, Android suppose que le `Exported` propriété est `true`. Si aucun filtre d’intention associé du récepteur de diffusion, puis Android suppose que la valeur est `false`. 

Le `OnReceive` méthode reçoit une référence à la `Intent` qui a été distribué au récepteur de diffusion. Cela rend est possible de l’expéditeur de l’intention de passer des valeurs pour le récepteur de diffusion.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statiquement l’inscription d’un récepteur de diffusion avec un filtre Intent

Quand un `BroadcastReceiver` est décorée avec le [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android ajoutera nécessaires `<intent-filter>` élément à la Android manifeste au moment de la compilation. L’extrait de code suivant est un exemple d’un récepteur de diffusion qui s’exécute quand un appareil a terminé le démarrage (si les autorisations Android appropriées ont été accordées par l’utilisateur) :

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

Il est également possible de créer un filtre intention qui répond aux objectifs personnalisés. Prenons l'exemple suivant : 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

### <a name="context-registering-a-broadcast-receiver"></a>Contexte à l’inscription d’un récepteur de diffusion 

Contexte de l’enregistrement d’un récepteur est effectuée en appelant le `RegisterReceiver` (méthode) et du récepteur de diffusion doivent être annulée par un appel à la `UnregisterReceiver` (méthode). Pour éviter la fuite des ressources, il est important annuler l’inscription du récepteur lorsqu’il n’est plus pertinente dans le contexte. Par exemple, un service peut diffuser une intention pour informer une activité mises à jour sont disponibles pour être affichées à l’utilisateur. Lorsque l’activité démarre, il s’inscrit pour ces modes. Lorsque l’activité est déplacée dans l’arrière-plan et n’est plus visible par l’utilisateur, elle doit annuler l’inscription du récepteur, car l’interface utilisateur permettant d’afficher les mises à jour n’est plus visible. L’extrait de code suivant est un exemple de comment enregistrer et annuler l’inscription d’un récepteur de diffusion dans le contexte d’une activité :

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()
        
        // Code omitted for clarity
    }
    
    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }
    
    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

Dans l’exemple précédent, lorsque l’activité est fournie au premier plan, il enregistre un récepteur de diffusion écouter pour une intention personnalisée à l’aide de la `OnResume` méthode du cycle de vie. Lorsque l’activité se trouve dans l’arrière-plan, le `OnPause()` méthode annule le récepteur.

## <a name="publishing-a-broadcast"></a>Publication d’une diffusion

Une diffusion est publiée en encapsulant un _action_ dans une intention et la distribution avec une des deux API : 

1. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Modes qui sont publiés avec le `LocalBroadcastManager` sera uniquement reçue par l’application ; ils ne seront pas routées vers d’autres applications. Cela doit être préféré, il fournit un niveau de sécurité supplémentaire en conservant les intentions de l’application actuelle, étant donné que tout est in-process il n’existe aucune surcharge associée aux appels interprocessus. Cet extrait de code montre comment une activité peut distribuer une intention à l’aide du `LocalBroadcastManager`:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
   ```

2. **[`Context.SendBroadcast`](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/) méthodes** &ndash; il existe plusieurs implémentations de cette méthode.
   Ces méthodes seront diffusés à l’intention de l’ensemble du système. Cela offre une grande souplesse mais signifie que les autres applications peuvent s’inscrire pour recevoir l’application. Cela peut poser un risque de sécurité potentiel. Les applications devront peut-être implémenter la sécurité d’addition pour assurer un accès non autorisé à l’intention. Cet extrait de code est un exemple montrant comment distribuer une intention en utilisant l’une de le `SendBroadcast` méthodes :

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```
        
> [!NOTE]
> Le LocalBroadcastManager est disponible via le [v4 de la bibliothèque de prise en charge de Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet Package. 

Cet extrait de code est un autre exemple d’envoi d’une diffusion à l’aide de la `Intent.SetAction` méthode pour identifier l’action :

```csharp 
Intent intent = new Intent();
intent.SetAction("com.xamarin.example.TEST");
intent.PutExtra("key", "value");
SendBroadcast(intent);
```



## <a name="related-links"></a>Liens associés

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Notifications locales dans Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android prend en charge la bibliothèque v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
