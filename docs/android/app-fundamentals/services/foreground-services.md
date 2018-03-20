---
title: Services de premier plan
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: d1267bc4a530deb6dfb6eb2e30bee2facabd8fed
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2018
---
# <a name="foreground-services"></a>Services de premier plan

Un service de premier plan est un type spécial d’un service lié ou d’un service démarré. Occasionnellement services effectue les tâches que les utilisateurs doivent connaître activement, ces services sont appelés _des services de premier plan_. Un exemple d’un service de premier plan est une application qui fournit l’utilisateur avec des instructions lors de la commande ou à pied. Même si l’application est en arrière-plan, il est important que le service dispose de suffisamment de ressources pour fonctionner correctement et que l’utilisateur dispose d’un moyen rapide et pratique pour accéder à l’application. Pour une application Android, cela signifie qu’un service de premier plan doit recevoir la priorité supérieure à celle d’un service « standard » et un service de premier plan doit fournir un `Notification` qui Android s’affichera en tant que le service est en cours d’exécution.
 
Pour démarrer un service de premier plan, l’application doit envoyer une intention indiquant Android pour démarrer le service. Puis le service doit s’enregistrer comme un service de premier plan avec Android. Applications qui s’exécutent sur Android 8.0 (ou version ultérieure) doivent utiliser le `Context.StartForegroundService` pour démarrer le service, tandis que les applications qui sont exécutent sur des appareils avec une ancienne version d’Android doivent utiliser (méthode) `Context.StartService`

Cette méthode d’extension c# est un exemple montrant comment démarrer un service de premier plan. Sur Android 8.0 et versions ultérieures, il utilise le `StartForegroundService` (méthode), sinon l’ancien `StartService` méthode sera utilisée.  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>L’inscription d’un Service de premier plan

Une fois qu’un service de premier plan a démarré, il doit s’inscrire avec Android en appelant le [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Si le service est démarré avec le `Service.StartForegroundService` méthode mais n’inscrit pas lui-même, puis Android arrête le service et marquer l’application comme non réactif.

`StartForeground` accepte deux paramètres, qui sont tous deux obligatoires :
 
* Valeur entière qui est unique au sein de l’application pour identifier le service.
* A `Notification` objet qui affiche dans la barre d’état pour Android tant que le service est en cours d’exécution.

Android la notification s’affiche dans la barre d’état pour que le service est en cours d’exécution. La notification, au minimum, fournit une aide visuelle à l’utilisateur qui exécute le service. Dans l’idéal, la notification doit fournir l’utilisateur avec un raccourci vers l’application ou éventuellement des boutons d’action pour contrôler l’application. Ceci est un lecteur de musique &ndash; la notification s’affiche peut-être comporter des boutons à suspendre/lire la musique, de rembobiner la chanson précédente, ou ignorer le morceau suivant. 

Cet extrait de code est un exemple d’enregistrement d’un service en tant qu’un service de premier plan :   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

La notification précédente affiche une notification de barre d’état qui ressemble à ce qui suit :

![Image montrant la notification dans la barre d’état](foreground-services-images/foreground-services-01.png "Image illustrant la notification dans la barre d’état")

Cette capture d’écran montre la notification de développé dans la barre d’état de notification avec deux actions permettant aux utilisateurs de contrôler le service :

![Image montrant la notification de développé](foreground-services-images/foreground-services-02.png "Image illustrant la notification de développé.")

Plus d’informations sur les notifications sont disponibles dans le [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md) section de la [aux Notifications Android](~/android/app-fundamentals/notifications/index.md) guide.

## <a name="unregistering-as-a-foreground-service"></a>Annulation de l’inscription en tant qu’un Service de premier plan

Un service peut annuler répertorié comme un service de premier plan en appelant la méthode `StopForeground`. `StopForeground` ne sera pas arrêter le service, mais elle supprime l’icône de notification et des signaux Android ce service peut être arrêtée si nécessaire.

La notification de barre d’état qui s’affiche peut également être supprimée en passant `true` à la méthode : 

```csharp
StopForeground(true);
```

Si le service est interrompu par un appel à `StopSelf` ou `StopService`, la notification de barre d’état est supprimée.

## <a name="related-links"></a>Liens associés

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
