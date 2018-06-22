---
title: Notifications de service
description: Ce guide explique comment un service Android peut utiliser des notifications locales de distribuer des informations à un utilisateur.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 65ccb94bd4fac1f3818b4f08eb34ecf73d6c52e4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762631"
---
# <a name="service-notifications"></a>Notifications de service

_Ce guide explique comment un service Android peut utiliser des notifications locales de distribuer des informations à un utilisateur._


## <a name="service-notifications-overview"></a>Vue d’ensemble des Notifications de service

Les notifications de service permettent une application afficher des informations à l’utilisateur, même si l’application Android n’est pas au premier plan. Il est possible d’actions de l’utilisateur, comme l’affichage d’une activité à partir d’une application de fournir une notification. L’exemple de code suivant montre comment un service peut envoyer une notification à un utilisateur :

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

Cette capture d’écran est un exemple de la notification s’affiche :

[![Icône de notification s’affichée dans la barre d’état](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Lorsque l’écran de notification à partir du haut vers le bas les diapositives de l’utilisateur, la notification complète s’affiche :

![Notification affichée dans la barre d’état de notification](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Une Notification de mise à jour

Pour mettre à jour une notification, le service republie la notification à l’aide de la même ID de notification. Android afficher ou mettre à jour de la notification dans la barre d’état si nécessaire.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Plus d’informations sur les notifications sont disponibles dans le [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md) section de la [aux Notifications Android](~/android/app-fundamentals/notifications/index.md) guide.


## <a name="related-links"></a>Liens associés

- [Notifications locales dans Android](~/android/app-fundamentals/notifications/local-notifications.md)
