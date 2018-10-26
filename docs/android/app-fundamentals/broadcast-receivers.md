---
title: Récepteurs de diffusion dans Xamarin.Android
description: Cette section explique comment utiliser un récepteur de diffusion.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 51bd3dd4c27dce19344f7660c31a0d4e741e1ad4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121137"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Récepteurs de diffusion dans Xamarin.Android

_Cette section explique comment utiliser un récepteur de diffusion._

## <a name="broadcast-receiver-overview"></a>Vue d’ensemble du récepteur de diffusion

Un _récepteur de diffusion_ est un composant Android qui permet à une application répondre aux messages (un Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) qui sont diffusés par le système d’exploitation Android ou par une application. Suivent des diffusions un _de publication-abonnement_ modèle &ndash; un événement provoque une diffusion à être publiées et reçus par les composants qui sont intéressés par l’événement.

Android identifie deux types de diffusions :

* **Diffusion explicite** &ndash; ces types de diffusions ciblent une application spécifique. L’utilisation la plus courante d’une diffusion explicite consiste à démarrer une activité. Un exemple d’une diffusion explicite lorsqu’une application doit composer un numéro de téléphone ; Il dépêchera une intention qui cible l’application de téléphone sur Android et passe le long du numéro de téléphone à composer. Android va ensuite l’acheminer l’intention de l’application de téléphone.
* **Diffusion implicite** &ndash; ces diffusions sont envoyées à toutes les applications sur l’appareil. Un exemple d’une diffusion implicite est le `ACTION_POWER_CONNECTED` intention. Cette intention est publiée chaque fois qu’Android détecte que la batterie sur l’appareil est en charge. Android achemine cette intention à toutes les applications qui ont inscrit pour cet événement.

Le récepteur de diffusion est une sous-classe de la `BroadcastReceiver` type et il doivent remplacer le [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) (méthode). Android exécutera `OnReceive` sur le thread principal, cette méthode doit-elle être conçue de s’exécutent rapidement. Soyez vigilant lors de la génération dynamique de threads dans `OnReceive` car Android peut terminer le processus lorsque la méthode se termine. Si un récepteur de diffusion doit effectuer un travail en cours d’exécution longue, il est recommandé de planifier un _travail_ à l’aide de la `JobScheduler` ou _répartiteur de travail Firebase_. Planification de travail associés à un travail est abordée dans un guide distinct.

Un _filtre intent_ est utilisé pour inscrire un récepteur de diffusion afin qu’Android peut acheminer correctement les messages. Le filtre d’intention peut être spécifié lors de l’exécution (cela est parfois appelé un _contexte enregistré le récepteur_ ou en tant que _l’enregistrement dynamique_) ou il peut être défini de manière statique dans le manifeste Android (un _manifeste inscrit un récepteur_). Xamarin.Android fournit une C# attribut, `IntentFilterAttribute`, qui s’inscriront statiquement au filtre d’intention (cela est abordée plus en détail plus loin dans ce guide). À partir de Android 8.0, il n’est pas possible pour une application d’enregistrer statiquement pour une diffusion implicite.

La principale différence entre le récepteur inscrit de manifeste et le récepteur du contexte enregistré est qu’un récepteur de contexte enregistré répond uniquement aux diffusions pendant l’exécution d’une application, tandis qu’un récepteur inscrit de manifeste peut répondre à diffuse même si l’application ne peut pas être exécuté.  

Il existe deux ensembles d’API pour la gestion d’un récepteur de diffusion et envoyer des diffusions :

1. **`Context`** &ndash; Le `Android.Content.Context` classe peut être utilisée pour inscrire un récepteur de diffusion qui va répondre aux événements de l’échelle du système. Le `Context` est également utilisé pour publier les diffusions de l’échelle du système.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Il s’agit d’une API qui est disponible via le [le package NuGet bibliothèque de prise en charge de Xamarin v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Cette classe est utilisée pour conserver les diffusions et les récepteurs de diffusion isolés dans le contexte de l’application qui les utilise. Cette classe peut être utile pour empêcher les autres applications de répondre aux diffusions uniquement par l’application d’envoyer des messages aux destinataires privées.

Un récepteur de diffusion peut ne pas affiche les boîtes de dialogue, et il est fortement déconseillé pour démarrer une activité à partir d’un récepteur de diffusion. Si un récepteur de diffusion doit notifier l’utilisateur, il doit publier une notification.

Il n’est pas possible de lier à ou démarrer d’un service dans un récepteur de diffusion. 

Ce guide couvre la création d’un récepteur de diffusion et comment inscrire afin qu’elle puisse recevoir les diffusions.

## <a name="creating-a-broadcast-receiver"></a>Création d’un récepteur de diffusion

Pour créer un récepteur de diffusion dans Xamarin.Android, une application doit sous-classe la `BroadcastReceiver` class, orner avec le `BroadcastReceiverAttribute`et remplacer le `OnReceive` méthode :

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

Lorsque Xamarin.Android compile la classe, il met également à jour le AndroidManifest avec les métadonnées nécessaires pour inscrire le récepteur. Pour un récepteur de diffusion inscrits de manière statique, le `Enabled` correctement doit être définie sur `true`, sinon Android ne sera pas en mesure de créer une instance du récepteur.
 
Le `Exported` propriété contrôle si le récepteur de diffusion peut recevoir des messages en dehors de l’application. Si la propriété n’est pas définie explicitement, la valeur par défaut de la propriété est déterminée par Android selon s’il existe des filtres d’intention associées du récepteur de diffusion. S’il existe au moins un filtre d’intention pour le récepteur de diffusion puis Android supposera que le `Exported` propriété est `true`. Si aucun filtre d’intention associée du récepteur de diffusion, Android partirons du principe que la valeur est `false`. 

Le `OnReceive` méthode reçoit une référence à la `Intent` qui a été distribué au récepteur de diffusion. Il est possible que l’expéditeur de l’intention de passer des valeurs pour le récepteur de diffusion.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statiquement l’inscription d’un récepteur de diffusion avec un filtre d’intention

Quand un `BroadcastReceiver` est décorée avec le [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android ajoutera nécessaires `<intent-filter>` élément l’Android manifeste au moment de la compilation. L’extrait de code suivant est un exemple d’un récepteur de diffusion qui s’exécute quand un appareil a terminé le démarrage (si les autorisations Android appropriées ont été accordées par l’utilisateur) :

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

Il est également possible de créer un filtre d’intention qui va répondre aux objectifs personnalisés. Prenons l'exemple suivant : 

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

Les applications qui ciblent Android 8.0 (niveau d’API 26) ou une version ultérieure ne peut pas inscrire statiquement pour une diffusion implicite. Les applications peuvent toujours statiquement s’inscrire pour une diffusion explicite. Il existe une petite liste des diffusions implicites qui sont exempts de cette restriction. Ces exceptions sont décrites dans le [implicite des Exceptions de diffusion](https://developer.android.com/guide/components/broadcast-exceptions.html) guide dans la documentation d’Android. Applications qui sont intéressées par des diffusions implicites doivent faire tout de façon dynamique à l’aide de la `RegisterReceiver` (méthode). Cela est décrit ci-après.

### <a name="context-registering-a-broadcast-receiver"></a>Contexte à l’inscription d’un récepteur de diffusion

Contexte de l’inscription (également appelée inscription dynamique) d’un récepteur est effectuée en appelant le `RegisterReceiver` (méthode) et le récepteur de diffusion doivent être annulée par un appel à la `UnregisterReceiver` (méthode). Pour éviter la fuite des ressources, il est important annuler l’inscription du récepteur lorsqu’il n’est plus pertinente pour le contexte (activité ou service). Par exemple, un service peut diffuser une intention pour informer une activité mises à jour sont disponibles pour être affichées à l’utilisateur. Lorsque l’activité démarre, il s’inscrit pour ces intentions. Lorsque l’activité est déplacée dans l’arrière-plan et ne soit plus visible à l’utilisateur, elle doit annuler l’inscription du récepteur, car l’interface utilisateur pour afficher les mises à jour n’est plus visible. L’extrait de code suivant est un exemple montrant comment inscrire et annuler l’inscription d’un récepteur de diffusion dans le contexte d’une activité :

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

Dans l’exemple précédent, lorsque l’activité est fournie au premier plan, il va inscrire un récepteur de diffusion qui écoutera les une intention personnalisée à l’aide de la `OnResume` méthode de cycle de vie. Lorsque l’activité se trouve dans l’arrière-plan, le `OnPause()` méthode doit annuler l’enregistrement du récepteur.

## <a name="publishing-a-broadcast"></a>Publication d’une diffusion

Une diffusion peut-être être publiée dans toutes les applications installées sur l’appareil de création d’un objet d’intention et sa distribution avec le `SendBroadcast` ou `SendOrderedBroadcast` (méthode).  

1. **Les méthodes Context.SendBroadcast** &ndash; il existe plusieurs implémentations de cette méthode.
   Ces méthodes va diffuser l’intention de l’ensemble du système. Récepteurs de diffusion qui recevront l’intention dans un ordre indéterminé. Cela fournit une très grande souplesse, mais signifie qu’il est possible pour d’autres applications pour vous inscrire et recevoir l’intention. Cela peut poser un risque de sécurité potentiel. Les applications peut-être implémenter la sécurité d’addition pour empêcher tout accès non autorisé. Une solution possible consiste à utiliser le `LocalBroadcastManager` qui distribue uniquement les messages au sein de l’espace privé de l’application. Cet extrait de code est un exemple montrant comment distribuer une intention en utilisant l’une de le `SendBroadcast` méthodes :

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    Cet extrait de code est un autre exemple d’envoi d’une diffusion à l’aide de la `Intent.SetAction` méthode pour identifier l’action :

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; cette méthode est très semblable à `Context.SendBroadcast`, la différence étant que l’objectif sera publié au moment de destinataires, dans l’ordre que les destinataires ont été inscrits.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

Le [v4 de la bibliothèque de prise en charge de Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) fournit une classe d’assistance appelée [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). Le `LocalBroadcastManager` est destiné aux applications que vous ne souhaitez pas envoyer ou recevoir les diffusions à partir d’autres applications sur l’appareil. Le `LocalBroadcastManager` publient des messages dans le contexte de l’application et uniquement à ces récepteurs de diffusion qui sont inscrits avec uniquement le `LocalBroadcastManager`. Cet extrait de code est un exemple d’inscription d’un récepteur de diffusion avec `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Autres applications sur l’appareil ne peut pas recevoir les messages qui sont publiés avec les `LocalBroadcastManager`. Cet extrait de code montre comment distribuer une intention à l’aide du `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>Liens associés

- [BroadcastReceiver API](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast API](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [API Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter API](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager (Android docs)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Notifications locales dans Android](~/android/app-fundamentals/notifications/local-notifications.md) guide
- [Android prend en charge v4 de bibliothèque (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
