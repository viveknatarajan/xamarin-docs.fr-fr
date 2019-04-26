---
title: Notifications groupées dans Xamarin.iOS
description: Avec iOS 12, il est possible pour les notifications de groupe dans le centre de Notification ou de l’écran de verrouillage par application ou par thread. Ce document décrit comment envoyer des fils et notifications non filetées avec Xamarin.iOS.
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 6798c4c5fa7502ba5e99cb8bc209468acaa4a9ec
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61402419"
---
# <a name="grouped-notifications-in-xamarinios"></a>Notifications groupées dans Xamarin.iOS

Par défaut, iOS 12 place toutes les notifications d’une application dans un groupe. L’écran de verrouillage et le centre de notifications affichent ce groupe comme une pile avec la dernière notification en haut. Les utilisateurs peuvent développer le groupe pour afficher toutes les notifications qu’il contient et ignorer l’ensemble du groupe.

Les applications peuvent également les notifications de groupe par thread, ce qui facilite pour les utilisateurs de rechercher et interagir avec les informations spécifiques qui que les intéresse.

## <a name="sample-app-groupednotifications"></a>Exemple d’application : GroupedNotifications

Pour savoir comment utiliser les notifications regroupées avec Xamarin.iOS, examinons le [GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications) exemple d’application.

Cet exemple d’application simule les conversations avec différents amis, envoyer une notification pour chaque message et en les regroupant par thread. Il montre également comment non filetés land de notifications dans un groupe de niveau application.

Extraits de code dans ce guide proviennent de cet exemple d’application.

## <a name="request-authorization-and-allow-foreground-notifications"></a>Demander l’autorisation et autoriser les notifications de premier plan

Qu’une application puisse envoyer des notifications locales, il doit demander l’autorisation pour ce faire. Dans l’exemple d’application [ `AppDelegate` ](xref:UIKit.UIApplicationDelegate), le [ `FinishedLaunching` ](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary)) méthode demande cette autorisation :

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    center.RequestAuthorization(UNAuthorizationOptions.Alert, (bool success, NSError error) =>
    {
        // Set the Delegate regardless of success; users can modify their notification
        // preferences at any time in the Settings app.
        center.Delegate = this;
    });
    return true;
}
```

Le [ `Delegate` ](xref:UserNotifications.UNUserNotificationCenter.Delegate) (défini ci-dessus) pour un [ `UNUserNotificationCenter` ](xref:UserNotifications.UNUserNotificationCenter) décide si une application de premier plan doit afficher une notification entrante en appelant le Gestionnaire d’achèvement passé à [`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions})):

```csharp
[Export("userNotificationCenter:willPresentotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

Le [ `UNNotificationPresentationOptions.Alert` ](xref:UserNotifications.UNNotificationPresentationOptions) paramètre indique que l’application doit afficher l’alerte, mais pas un signal sonore ou un badge de mise à jour.

## <a name="threaded-notifications"></a>Notifications de threads

Appuyez sur l’exemple d’application **Message avec Alice** à plusieurs reprises pour qu’il envoie des notifications pour une conversation avec un ami nommé Alice.
Étant donné que les notifications de cette conversation font partie du même thread, l’écran de verrouillage et le centre de Notification les regroupent.

Pour démarrer une conversation avec un ami différent, appuyez sur la **choisir un nouvel ami** bouton. Notifications pour cette conversation s’affichent dans un groupe distinct.

### <a name="threadidentifier"></a>ThreadIdentifier

N’importe quel moment de que l’exemple d’application démarre un nouveau thread, il crée un identificateur de thread unique :

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

Pour envoyer une notification de thread, l’exemple d’application :

- Vérifie si l’application dispose d’autorisations pour envoyer une notification.
- Crée un [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
objet pour la notification du contenu et définit son [`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier)
à l’identificateur de thread créé ci-dessus.
- Crée une demande et planifie la notification :

```csharp
async partial void ScheduleThreadedNotification(UIButton sender)
{
    var center = UNUserNotificationCenter.Current;

    UNNotificationSettings settings = await center.GetNotificationSettingsAsync();
    if (settings.AuthorizationStatus != UNAuthorizationStatus.Authorized)
    {
        return;
    }

    string author =  // ...
    string message = // ...

    var content = new UNMutableNotificationContent()
    {
        ThreadIdentifier = threadId,
        Title = author,
        Body = message,
        SummaryArgument = author
    };

    var request = UNNotificationRequest.FromIdentifier(
        Guid.NewGuid().ToString(),
        content,
        UNTimeIntervalNotificationTrigger.CreateTrigger(1, false)
    );

    center.AddNotificationRequest(request, null);

    // ...
}
```

Toutes les notifications à partir de la même application avec le même identificateur de thread seront affiche dans le même groupe de notification.

> [!NOTE]
> Pour définir un identificateur de thread sur une notification à distance, ajoutez le `thread-id` clé pour la charge utile JSON de la notification. Consultez d’Apple [générer une Notification à distance](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) document pour plus d’informations.

### <a name="summaryargument"></a>SummaryArgument

`SummaryArgument` Spécifie comment une notification aura un impact sur le texte du résumé qui s’affiche dans le coin inférieur gauche d’un groupe de notification à laquelle appartient la notification. texte du résumé agrégats iOS à partir des notifications dans le même groupe pour créer une description résumée de global.

L’exemple d’application utilise l’auteur du message en tant que l’argument de résumé. À l’aide de cette approche, le texte de résumé d’un groupe de six notifications avec Alice peut être **plus de 5 notifications à partir d’Alice et Me**.

## <a name="unthreaded-notifications"></a>Notifications non filetées

Chaque en appuyant sur l’exemple d’application **rappel de rendez-vous** bouton envoie une des différentes notifications de rappel de rendez-vous. Étant donné que ces rappels ne sont pas suivies, elles apparaissent dans le groupe de notification au niveau de l’application sur l’écran de verrouillage et dans le centre de Notification.

Pour envoyer une non filetés de la notification, l’exemple d’application `ScheduleUnthreadedNotification` méthode utilise un code similaire comme indiqué ci-dessus.
Toutefois, il ne définit pas le `ThreadIdentifier` sur la `UNMutableNotificationContent` objet.

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications)
- [Infrastructure de Notifications d’utilisateur dans Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [À l’aide de Notifications groupées (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [Meilleures pratiques et quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Générer une Notification à distance (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
