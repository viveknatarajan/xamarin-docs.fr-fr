---
title: Notifications provisoires dans Xamarin.iOS
description: Ce document décrit l’utilisation de Xamarin.iOS pour fonctionner avec les notifications provisoires. Notifications provisoires, introduites dans iOS 12, permettent aux applications envoyer des notifications silencieuses sans l’autorisation utilisateur explicite.
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 31b4c6e98945cd7b5dd4cea8be6f5e3857444f78
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131676"
---
# <a name="provisional-notifications-in-xamarinios"></a>Notifications provisoires dans Xamarin.iOS

Notifications provisoires permettent aux applications de remettre des notifications sans consentement préalable explicite d’un utilisateur. Ces notifications arrivent en mode silencieux et affichent uniquement dans le centre de Notification, qui permet aux utilisateurs d’afficher un aperçu avant de vous inscrire dans ou hors de leur livraison continue.

Dans le centre de Notification, les utilisateurs peuvent spécifier qu’une application doit arrêter de recevoir les notifications provisoires, continuer leur remise provisoirement ou démarrer leur remise plus en détail.

## <a name="sample-app-redgreennotifications"></a>Exemple d’application : RedGreenNotifications

Jetez un coup de œil à la [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) exemple d’application, qui envoie des notifications provisoires.

## <a name="sending-provisional-notifications"></a>Envoi de notifications provisoire

Pour envoyer des notifications provisoires, fournissez `UNAuthorizationOptions.Provisional` en tant qu’option à la [`RequestAuthorization`](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenter.RequestAuthorization/)
méthode de `UNUserNotificationCenter`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

Si l’utilisateur promeut les notifications provisoires pour une remise importante, le `UNAuthorizationOptions` valeurs passées à `RequestAuthorization` déterminera les nouveaux paramètres de remise de notification (dans le code ci-dessus, `UNAuthorizationOptions.Alert` et `UNAuthorizationOptions.Sound`).

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Infrastructure de Notifications d’utilisateur dans Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Meilleures pratiques et quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Générer une Notification à distance (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
