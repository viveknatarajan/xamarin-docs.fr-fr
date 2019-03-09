---
title: Notifications dans Xamarin.Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
---

# <a name="notifications-in-xamarinandroid"></a>Notifications dans Xamarin.Android


## <a name="overview"></a>Vue d'ensemble

Cette section explique comment implémenter les notifications dans Xamarin.Android. Il décrit les différents éléments d’interface utilisateur d’une notification d’Android et traite des API d’impliquée dans la création et affichage d’une notification.


## <a name="sections"></a>Sections

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Notifications locales dans Android](local-notifications.md)

Cette section explique comment implémenter des notifications locales dans Xamarin.Android. Il décrit les différents éléments d’interface utilisateur d’une notification d’Android et traitent de l’API d’impliquée dans la création et affichage d’une notification. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Procédure pas à pas : utilisation des Notifications locales dans Xamarin.Android](local-notifications-walkthrough.md)  
 
Cette procédure pas à pas explique comment utiliser des notifications locales dans une application Xamarin.Android. Il illustre les principes fondamentaux de création et publication d’une notification. Lorsque l’utilisateur clique sur la notification dans le tiroir de notification qu’il démarre une deuxième activité. 


## <a name="for-further-reading"></a>Pour obtenir des informations supplémentaires

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) est un service qui simplifie la messagerie entre les applications mobiles et les applications serveur. Firebase Cloud Messaging peut être utilisé pour implémenter des notifications à distance (également appelées notifications push) dans les applications Xamarin.Android.

[Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; rubrique de ce développeur Android est le guide de référence pour les notifications Android. Il inclut une conception section considérations qui vous permet de concevoir vos notifications afin qu’elles sont conformes aux instructions de l’interface utilisateur Android. Il fournit davantage d’informations sur la navigation de preserviing lors du démarrage d’une activité, et elle explique comment afficher la progression dans une notification et contrôle la lecture du média sur l’écran de verrouillage. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; Android ce service permet à votre application écouter (et interagir avec) publié par toutes les notifications sur l’appareil Android, pas seulement les notifications que votre application est inscrite à recevoir. Notez que l’utilisateur doit explicitement accorder des autorisations à votre application pour qu’elle soit en mesure d’écouter les notifications sur l’appareil.





## <a name="related-links"></a>Liens associés

- [Notifications locales (exemple)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Notifications à distance (exemple)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
