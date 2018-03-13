---
title: Services de premier plan
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 96e8d1a3658a515b6b1d37cf0fdd93157954c01d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="foreground-services"></a>Services de premier plan

Certains services effectuez certaines tâches que les utilisateurs connaissent activement, ces services sont connus en tant que _des services de premier plan_. Un exemple d’un service de premier plan est une application qui fournit l’utilisateur avec des instructions lors de la commande ou à pied. Même si l’application est en arrière-plan, il est important que le service dispose de suffisamment de ressources pour fonctionner correctement et que l’utilisateur dispose d’un moyen rapide et pratique pour accéder à l’application. Pour une application Android, cela signifie qu’un service de premier plan doit recevoir la priorité supérieure à celle d’un service « standard » et un service de premier plan doit fournir un `Notification` qui Android s’affichera en tant que le service est en cours d’exécution.
 
Un service de premier plan est créé et démarré juste comme tout autre service. Lorsque le service démarre, il doit s’inscrire auprès Android comme un service de premier plan.
 
Ce guide traite les étapes supplémentaires qui doivent être prises pour inscrire un service de premier plan et d’arrêter le service lorsqu’il est terminé.

## <a name="registering-as-a-foreground-service"></a>L’inscription d’un Service de premier plan

Un service de premier plan est un type spécial d’un service lié ou d’un service démarré. Le service, une fois démarrée, elle appelle la [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/) méthode pour s’inscrire auprès d’Android comme un service de premier plan.   

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

Si le service est interrompu par un appel à `StopSelf` ou `StopService`, puis la notification de barre d’état est également supprimée.


## <a name="related-links"></a>Liens associés

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
