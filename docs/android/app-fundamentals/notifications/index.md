---
title: Notifications de Xamarin.Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d321c3eb3fe1c882ef9cea6ed1aace3aa1dd953e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="notifications-in-xamarinandroid"></a>Notifications de Xamarin.Android


## <a name="overview"></a>Vue d'ensemble

Cette section explique comment implémenter des notifications dans Xamarin.Android. Il décrit les différents éléments d’interface utilisateur d’une notification d’Android et traite des API d’impliquée dans la création et affichage d’une notification.


## <a name="sections"></a>Sections

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Notifications locales dans Android](local-notifications.md)

Cette section explique comment implémenter des notifications locales dans Xamarin.Android. Il décrit les différents éléments d’interface utilisateur d’une notification d’Android et traitent de l’API d’impliquée dans la création et affichage d’une notification. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Procédure pas à pas - à l’aide de Notifications Local dans Xamarin.Android](local-notifications-walkthrough.md)  
 
Cette procédure pas à pas explique comment utiliser des notifications locales dans une application de Xamarin.Android. Il illustre les principes fondamentaux de la création et la publication d’une notification. Lorsque l’utilisateur clique sur la notification dans le tiroir de notifications qu’il démarre une deuxième activité. 


## <a name="for-further-reading"></a>Pour obtenir des informations supplémentaires

[Messagerie de Cloud firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) est un service qui simplifie la messagerie entre les applications mobiles et des applications serveur. Firebase de messagerie Cloud peut être utilisée pour implémenter des notifications à distance (également appelées des notifications push) dans les applications de Xamarin.Android.

[Notifications](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; rubrique de ce développeur Android est le guide de référence pour les notifications Android. Il inclut une conception section considérations qui vous permet de concevoir vos notifications afin qu’elles sont conformes aux instructions de l’interface utilisateur Android. Il fournit davantage d’informations sur la navigation de preserviing lors du démarrage d’une activité, et elle explique comment afficher la progression dans une lecture du média notification et le contrôle sur l’écran de verrouillage. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; Android ce service permet à votre application écouter (et interagir avec) toutes les notifications de validation sur l’appareil Android, et pas seulement les notifications inscrite auprès de votre application de réception. Notez que l’utilisateur doit explicitement accorder des autorisations à votre application pour qu’elle soit en mesure d’écouter les notifications sur l’appareil.





## <a name="related-links"></a>Liens associés

- [Notifications locales (exemple)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Notifications à distance (exemple)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
