---
title: Alertes critiques dans Xamarin.iOS
description: Ce document décrit comment utiliser les alertes critiques avec Xamarin.iOS. Alertes critiques, introduites avec iOS 12, sont des notifications sans interruption qui un signal sonore quel que soit le si ne pas déranger sur ou le commutateur de sonnerie est désactivée.
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 4f847a86f3f92bcf7168c2e104471e1ca052969c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131102"
---
# <a name="critical-alerts-in-xamarinios"></a>Alertes critiques dans Xamarin.iOS

Avec iOS 12, les applications peuvent envoyer des alertes critiques. Alertes critiques un signal sonore, quel que soit ou non ne pas déranger est activé ou le commutateur de sonnerie est désactivée. Ces notifications des perturbations et doivent être utilisées uniquement lorsque les utilisateurs doivent effectuer une action immédiate.

## <a name="custom-critical-alert-entitlement"></a>Droits personnalisés d’alerte critique

Pour afficher les alertes critiques dans votre application, tout d’abord [demander un droit de notifications d’alerte critique personnalisées](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/) auprès d’Apple.

Après réception de ce droit d’Apple et en suivant les instructions associées sur la configuration de votre application pour l’utiliser, ajouter personnalisé [droits](~/ios/deploy-test/provisioning/entitlements.md) à votre application **Entitlements.plist** fichier (s). Ensuite, configurez votre **signature du Bundle iOS** options à utiliser **Entitlements.plist** lors de l’inscription de l’application sur le simulateur et appareil.

## <a name="request-authorization"></a>Demander l’autorisation

Demande d’autorisation de notification d’une application invite l’utilisateur à autoriser ou interdire les notifications d’une application. Si la demande d’autorisation de notification demande l’autorisation d’envoyer des alertes critiques, l’application est également l’utilisateur une occasion pour s’abonner à des alertes critiques.

Le code suivant demande l’autorisation d’envoyer des alertes critiques et notifications standards et sons en passant approprié [`UNAuthorizationOptions`](https://developer.xamarin.com/api/type/UserNotifications.UNAuthorizationOptions/)
valeurs [ `RequestAuthorization` ](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenter.RequestAuthorization/):

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.CriticalAlert;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

## <a name="local-critical-alerts"></a>Alertes critiques locales

Pour envoyer une alerte critique locale, créez un [`UNMutableNotificationContent`](https://developer.xamarin.com/api/type/UserNotifications.UNMutableNotificationContent/)
et définissez son `Sound` propriété :

- `UNNotificationSound.DefaultCriticalSound`, qui utilise le son de notification critique par défaut.
- `UNNotificationSound.GetCriticalSound`, qui permet de spécifier un personnalisé audio qui est fourni avec votre application et un volume.

Ensuite, créez un `UNNotificationRequest` à partir de la notification de contenu et l’ajouter au centre de notification :

```csharp
var content = new UNMutableNotificationContent()
{
    Title = "Critical alert title",
    Body = "Text of the critical alert",
    CategoryIdentifier = "my-critical-alert-category",
    // Sound = UNNotificationSound.DefaultCriticalSound
    Sound = UNNotificationSound.GetCriticalSound("my_critical_sound.m4a", 1.0f)
};

var request = UNNotificationRequest.FromIdentifier(
    Guid.NewGuid().ToString(),
    content,
    UNTimeIntervalNotificationTrigger.CreateTrigger(3, false)
);

var center = UNUserNotificationCenter.Current;
center.AddNotificationRequest(request, null);
```

> [!IMPORTANT]
> Alertes critiques n’aboutissent pas si elles ne sont pas activées pour votre application. En même temps que l’invite qui s’affiche la première fois une application demande l’autorisation pour envoyer des alertes critiques, un utilisateur peut également activer ou désactiver les alertes critiques de votre application **Notifications** section d’iOS **paramètres**application.

## <a name="remote-critical-alerts"></a>Alertes critiques à distance

Pour plus d’informations sur les alertes critiques à distance, consultez le [ce qui est nouveau dans Notifications à l’utilisateur](https://developer.apple.com/videos/play/wwdc2018/710/) session à partir de WWDC 2018 et le [générer une Notification à distance](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) document.

## <a name="related-links"></a>Liens connexes

- [Infrastructure de Notifications d’utilisateur dans Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Meilleures pratiques et quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Générer une Notification à distance (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
