---
title: Récepteurs de diffusion de Xamarin.Android
description: Cette section explique comment utiliser un récepteur de diffusion.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 6b2e316eaf67e51801be4fcd670e80ec81c8ff08
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935397"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Récepteurs de diffusion de Xamarin.Android

_Cette section explique comment utiliser un récepteur de diffusion._

## <a name="broadcast-receiver-overview"></a>Vue d’ensemble du récepteur de diffusion

A _récepteur de diffusion_ est un composant Android qui permet à une application répondre aux messages (un Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) qui sont diffusés par le système d’exploitation Android ou par une application. Diffusions suivent un _la publication-abonnement_ modèle &ndash; un événement provoque une diffusion à publier et reçus par les composants qui sont intéressés par l’événement. 

Android identifie deux types de diffusions :

* **Diffusion explicite** &ndash; ces types de diffusions ciblent une application spécifique. L’utilisation la plus courante d’une diffusion explicite est une activité de démarrage. Un exemple d’une diffusion explicite lorsqu’une application a besoin pour composer un numéro de téléphone ; Il distribue une intention qui cible l’application téléphonique sur Android et passe le long du numéro de téléphone à composer. Android puis achemine l’intention de l’application téléphonique.
* **Diffusion implicite** &ndash; ces diffusions sont distribuées à toutes les applications sur l’appareil. Est un exemple d’une diffusion implicite le `ACTION_POWER_CONNECTED` intention. Ce mode est publié chaque fois Qu'android détecte que la batterie sur l’appareil est en cours de chargement. Android achemine cette intention à toutes les applications qui ont inscrit pour cet événement.

Le récepteur de diffusion est une sous-classe de la `BroadcastReceiver` type et il doivent remplacer le [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) (méthode). Android exécutera `OnReceive` sur le thread principal, par conséquent, cette méthode doit être conçue pour s’exécuter rapidement. Soyez vigilant lors de la génération dynamique de threads dans `OnReceive` car Android peut terminer le processus lors de la méthode se termine. Si un récepteur de diffusion doit effectuer un travail en cours d’exécution longue, il est recommandé de planifier un _travail_ à l’aide de la `JobScheduler` ou _Firebase travail répartiteur_. Planification de travail avec un travail est abordée dans un repère distinct.

Un _filtre intention_ est utilisée pour inscrire un récepteur de diffusion afin que Android de router correctement les messages. Le filtre intention peut être spécifié lors de l’exécution (il est parfois appelée une _contexte inscrit un récepteur_ ou en tant que _l’enregistrement dynamique_) ou il peut être défini de manière statique dans le manifeste Android (un _manifeste-inscrit un récepteur_). Xamarin.Android fournit un attribut de c#, `IntentFilterAttribute`, qui s’inscriront statiquement le filtre intention (cela est décrit plus en détail plus loin dans ce guide). À compter de 8.0 Android, il n’est pas possible pour une application à inscrire statiquement pour une diffusion implicite.

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

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statiquement l’inscription d’un récepteur de diffusion avec un filtre intention

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

Les applications qui ciblent 8.0 Android (API niveau 26) ou version ultérieure ne peut pas inscrire statiquement pour une diffusion implicite. Les applications peuvent toujours statiquement inscrire pour une diffusion explicite. Il existe une petite liste de diffusions implicites qui sont exempts de cette restriction. Ces exceptions sont décrites dans le [implicite des Exceptions de diffusion](https://developer.android.com/guide/components/broadcast-exceptions.html) guide dans la documentation d’Android. Les applications qui sont intéressées par des diffusions implicites doivent faire à l’aide de façon dynamique le `RegisterReceiver` (méthode). Cela est décrit ci-après.

### <a name="context-registering-a-broadcast-receiver"></a>Contexte à l’inscription d’un récepteur de diffusion

Contexte de l’enregistrement (également appelée inscription dynamique) d’un récepteur est effectuée en appelant le `RegisterReceiver` (méthode) et du récepteur de diffusion doivent être annulée par un appel à la `UnregisterReceiver` (méthode). Pour éviter la fuite des ressources, il est important annuler l’inscription du récepteur lorsqu’il n’est plus pertinente pour le contexte (activité ou service). Par exemple, un service peut diffuser une intention pour informer une activité mises à jour sont disponibles pour être affichées à l’utilisateur. Lorsque l’activité démarre, il s’inscrit pour ces modes. Lorsque l’activité est déplacée dans l’arrière-plan et n’est plus visible par l’utilisateur, elle doit annuler l’inscription du récepteur, car l’interface utilisateur permettant d’afficher les mises à jour n’est plus visible. L’extrait de code suivant est un exemple de comment enregistrer et annuler l’inscription d’un récepteur de diffusion dans le contexte d’une activité :

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

Une diffusion peut-être être publiée dans toutes les applications installées sur l’appareil de création d’un objet d’intention et la distribution avec la `SendBroadcast` ou `SendOrderedBroadcast` (méthode).  

1. **Les méthodes Context.SendBroadcast** &ndash; il existe plusieurs implémentations de cette méthode.
   Ces méthodes seront diffusés à l’intention de l’ensemble du système. Récepteurs de diffusion thatwill recevoir l’intention dans un ordre indéterminé. Cela offre une grande souplesse mais signifie qu’il est possible pour les autres applications inscrire et recevoir l’intention. Cela peut poser un risque de sécurité potentiel. Les applications devront peut-être implémenter la sécurité d’addition pour empêcher tout accès non autorisé. Une solution possible consiste à utiliser le `LocalBroadcastManager` qui distribue uniquement les messages au sein de l’espace privé de l’application. Cet extrait de code est un exemple montrant comment distribuer une intention en utilisant l’une de le `SendBroadcast` méthodes :

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    Cet extrait de code est un autre exemple d’envoi d’une diffusion à l’aide de la `Intent.SetAction` méthode pour identifier l’action :

    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; cette méthode est très semblable à `Context.SendBroadcast`, la différence étant que l’intention sera publié simultanément à des destinataires, dans l’ordre que les recievers ont été enregistrés.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

Le [v4 de la bibliothèque de prise en charge de Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) fournit une classe d’assistance appelée [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). Le `LocalBroadcastManager` est destinée aux applications que vous ne souhaitez pas envoyer ou recevoir des diffusions à partir d’autres applications sur l’appareil. Le `LocalBroadcastManager` publient des messages dans le contexte de l’application et uniquement pour les récepteurs de diffusion qui sont inscrits avec uniquement le `LocalBroadcastManager`. Cet extrait de code est un exemple d’inscription d’un récepteur de diffusion avec `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Autres applications sur l’appareil ne peut pas recevoir les messages qui sont publiés avec le `LocalBroadcastManager`. Cet extrait de code montre comment distribuer une intention à l’aide du `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>Liens associés

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Intention de](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Notifications locales dans Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android prend en charge la bibliothèque v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
