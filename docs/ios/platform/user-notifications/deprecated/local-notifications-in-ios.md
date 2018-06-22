---
title: Notifications de Xamarin.iOS
description: Cette section montre comment implémenter des notifications locales dans Xamarin.iOS. Elle explique les différents éléments d’interface utilisateur d’une notification d’e/s et traitent de l’API d’impliquée dans la création et affichage d’une notification.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d4dd759f52e52f417441f69e6417fab536daf6d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777910"
---
# <a name="notifications-in-xamarinios"></a>Notifications de Xamarin.iOS

_Cette section montre comment implémenter des notifications locales dans Xamarin.iOS. Elle explique les différents éléments d’interface utilisateur d’une notification d’e/s et traitent de l’API d’impliquée dans la création et affichage d’une notification._

> [!IMPORTANT]
> Les informations contenues dans cette section relative à iOS 9 et antérieure, il est resté ici pour prendre en charge les anciennes versions d’iOS. Pour iOS 10 et versions ultérieures, consultez le [guide utilisateur Notification Framework](~/ios/platform/user-notifications/index.md) pour prendre en charge à la fois Local et distant Notification sur un appareil iOS.

iOS a trois méthodes pour indiquer à l’utilisateur qu’une notification a été reçue :

-  **Son ou Vibration** -iOS peuvent jouer un son pour avertir les utilisateurs. Si le son est désactivé, l’appareil peut être configuré pour faire vibrer.
-  **Alertes** -il est possible d’afficher une boîte de dialogue sur l’écran avec des informations sur la notification.
-  **Badges** - lorsqu’une notification est publiée, un nombre peut être affiché sur l’icône d’application (accès identifié).


iOS fournit également un *centre de notifications* qui affiche toutes les notifications, locales et distantes, à l’utilisateur. Les utilisateurs peuvent y accéder en faisant défiler vers le bas en haut de l’écran :

 ![](local-notifications-in-ios-images/image13.png "Le centre de notifications")

## <a name="creating-local-notifications-in-ios"></a>Création de Notifications Local dans iOS

iOS rend relativement simple créer et gérer les notifications locales.
Tout d’abord, iOS 8 exige des applications pour demander l’autorisation de l’utilisateur afficher les notifications. Ajoutez le code suivant à votre application avant de tenter d’envoyer une notification de locale - dans l’exemple joint place le **AppDelegate**de **FinishedLaunching** (méthode).

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "Confirmer la possibilité d’envoyer une notification locale")](local-notifications-in-ios-images/image0.png#lightbox)

Pour planifier une notification locale que vous créez un `UILocalNotification` de l’objet, définissez la `FireDate`et la programmer via la `ScheduleLocalNotification` méthode sur le `UIApplication.SharedApplication` objet. L’extrait de code suivant affiche comment planifier une notification qui déclenche une minute dans le futur, afficher une alerte avec un message :

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

La capture d’écran suivante affiche l’aspect de cette alerte :

  [![](local-notifications-in-ios-images/image2-sml.png "Une alerte de l’exemple")](local-notifications-in-ios-images/image2.png#lightbox)

Notez que si l’utilisateur a choisi de *n'autorise pas* notifications rien ne s’affiche.

Si vous souhaitez appliquer un badge à l’icône de l’application avec un nombre, vous pouvez définir comme indiqué dans la ligne de code suivant :

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

Lecture de la commande un signal sonore par une icône, définir la propriété SoundName sur la notification comme indiqué dans l’extrait de code suivant :

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Par les instructions d’Interface humaine Apple, si une notification joue un son, il doit également être accompagné d’un badge ou d’une alerte pour identifier l’application qui a déclenché l’alerte de l’utilisateur. En outre, si le son est plu de 30 secondes, iOS sera lu la valeur par défaut son à la place.

> [!IMPORTANT]
> Il existe un bogue dans le simulateur iOS qui déclenchera la notification du délégué à deux reprises. Ce problème ne doit pas se produire lors de l’exécution de l’application sur un appareil.

## <a name="handling-notifications"></a>Gestion des Notifications

des applications iOS de gérer les notifications locales et distantes dans presque exactement de la même manière. Lorsqu’une application s’exécute, le `ReceivedLocalNotification` méthode ou la `ReceivedRemoteNotification` méthode sur la classe AppDelegate est appelée, et les informations de notification seront passées en tant que paramètre.

Une application peut gérer une notification de différentes façons. Par exemple, l’application peut afficher uniquement une alerte pour rappeler aux utilisateurs sur un événement. Ou la notification peut être utilisée pour afficher une alerte à l’utilisateur un processus est terminé, tels que les fichiers de synchronisation en cours vers un serveur.

Le code suivant montre comment gérer une notification locale et afficher une alerte et rétablir le numéro de badge à zéro :

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

Si l’application n’est pas en cours d’exécution, iOS sera émettre le signal sonore et/ou mettre à jour le badge icône, le cas échéant. Lorsque l’utilisateur démarre l’application associée à l’alerte, l’application est lancée et la méthode FinishedLaunching sur le délégué de l’application sera appelée et les informations de notification seront transmises via le paramètre options. Si le dictionnaire d’options contient la clé `UIApplication.LaunchOptionsLocalNotificationKey`, le AppDelegate sait que l’application a été lancée à partir d’une notification locale. L’extrait de code suivant illustre ce processus :

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

S’il s’agit d’une notification à distance à la place le dictionnaire d’options contient un `LaunchOptionsRemoteNotificationKey`, et la valeur obtenue pour cette clé serait un `NSDictionary` objet avec la charge utile de notification à distance. Vous pouvez extraire la charge utile de notification via l’alerte, badge et des clés audio. L’extrait de code suivant montre comment obtenir des notifications à distance :

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Récapitulatif

Cette section vous a montré comment créer et publier une notification dans Xamarin.iOS. Il affiche la manière dont une application peut réagir aux notifications en substituant le `ReceivedLocalNotification` méthode ou la `ReceivedRemoteNotification` méthode dans le `AppDelegate`.


## <a name="related-links"></a>Liens associés

- [Notifications locales (exemple)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Local et de Notifications Push pour les développeurs](https://developer.apple.com/notifications/)
- [Local et envoyez le Guide de programmation de Notification](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
