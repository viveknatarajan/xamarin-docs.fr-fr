---
title: Notifications de service
description: Ce guide décrit comment un service Android peut-être utiliser des notifications locales pour distribuer des informations à un utilisateur.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d56f67254a9eae334fa8ac3f08d3ef270800c309
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61012364"
---
# <a name="service-notifications"></a>Notifications de service

_Ce guide décrit comment un service Android peut-être utiliser des notifications locales pour distribuer des informations à un utilisateur._


## <a name="service-notifications-overview"></a>Vue d’ensemble des Notifications de service

Notifications de service permettent à une application afficher des informations à l’utilisateur, même si l’application Android n’est pas au premier plan. Il est possible pour une notification fournir des actions pour l’utilisateur, telles que l’affichage d’une activité à partir d’une application. L’exemple de code suivant montre comment un service peut envoyer une notification à un utilisateur :

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

[![Icône de notification s’affiché dans la barre d’état](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Lorsque l’écran de notification à partir du haut vers le bas les diapositives de l’utilisateur la notification complète s’affiche :

![Notification affichée dans la barre d’état de notification](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Une Notification de la mise à jour

Pour mettre à jour une notification, le service de republier la notification à l’aide de l’ID de notification même. Android afficher ou mettre à jour de la notification dans la barre d’état en fonction des besoins.

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

Plus d’informations sur les notifications sont disponibles dans le [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md) section de la [Notifications Android](~/android/app-fundamentals/notifications/index.md) guide.


## <a name="related-links"></a>Liens associés

- [Notifications locales dans Android](~/android/app-fundamentals/notifications/local-notifications.md)
