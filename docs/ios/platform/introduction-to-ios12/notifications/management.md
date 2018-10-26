---
title: Gestion de notification dans Xamarin.iOS
description: Ce document décrit comment utiliser Xamarin.iOS pour tirer parti des nouvelles fonctionnalités de gestion de notification introduit dans iOS 12.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: a7a3bb8f720f1c6a2370a2510659693bb28ea09b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111515"
---
# <a name="notification-management-in-xamarinios"></a>Gestion de notification dans Xamarin.iOS

Dans iOS 12, le système d’exploitation peuvent être un lien profond à partir du centre de Notification et l’application de paramètres à l’écran de gestion de notification d’une application. Cet écran doit permettre aux utilisateurs de participer et les différents types de notifications hors de l’application envoie.

## <a name="sample-app-redgreennotifications"></a>Exemple d’application : RedGreenNotifications

Pour voir un exemple du fonctionne de la gestion de notification, examinons le [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) exemple d’application.

Cet exemple d’application envoie deux types de notifications – rouges et vertes – et fournit un écran qui permet aux utilisateurs d’accepter ou refuser un de ces types.

Extraits de code dans ce guide proviennent de cet exemple d’application.

## <a name="notification-management-screen"></a>Écran de gestion de notification

Dans l’exemple d’application, `ManageNotificationsViewController` définit une interface utilisateur qui permet aux utilisateurs indépendamment activer et désactiver les notifications rouge et les notifications vert. Il s’agit une norme [`UIViewController`](https://developer.xamarin.com/api/type/UIKit.UIViewController/)
contenant un [ `UISwitch` ](https://developer.xamarin.com/api/type/UIKit.UISwitch/) pour chaque type de notification. Le commutateur pour les deux types de notification d’activation/la désactivation enregistre, dans les valeurs par défaut de l’utilisateur, la préférence utilisateur pour ce type de notification :

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> L’écran de gestion de notification vérifie également si l’utilisateur a désactivé complètement les notifications pour l’application. Dans ce cas, elle masque les bascules pour les types de notification individuelle. Pour ce faire, l’écran de gestion de notification :
>
> - Appels [ `UNUserNotificationCenter.Current.GetNotificationSettingsAsync` ](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync()/) et examine les [ `AuthorizationStatus` ](https://developer.xamarin.com/api/property/UserNotifications.UNNotificationSettings.AuthorizationStatus/) propriété.
> - Masque les bascules pour les types de notification individuelle si les notifications ont été complètement désactivées pour l’application.
> - Vérifie à nouveau si les notifications ont été désactivées chaque fois que l’application passe au premier plan, dans la mesure où l’utilisateur peut activer/désactiver les notifications dans les réglages iOS à tout moment.

L’exemple d’application `ViewController` (classe), qui envoie les notifications, vérification la préférence utilisateur avant d’envoyer une notification locale pour vous assurer que la notification est d’un type de l’utilisateur souhaite recevoir :

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>Lien ciblé

iOS à des liens ciblés vers l’écran de gestion de notification d’une application de centre de Notification et les paramètres de notification de l’application dans l’application paramètres. Pour faciliter cela, une application doit :

- Indiquer qu’un écran de gestion de notification est disponible en passant `UNAuthorizationOptions.ProvidesAppNotificationSettings` à la demande d’autorisation de notification de l’application.
- Implémentez le `OpenSettings` méthode à partir de [ `IUNUserNotificationCenterDelegate` ](https://developer.xamarin.com/api/type/UserNotifications.IUNUserNotificationCenterDelegate/).

### <a name="authorization-request"></a>Demande d’autorisation

Pour indiquer au système d’exploitation qu’un écran de gestion de notification n’est disponible, une application doit passer la `UNAuthorizationOptions.ProvidesAppNotificationSettings` option (ainsi que toute autres notification options de remise dont il a besoin) pour le `RequestAuthorization` méthode sur le `UNUserNotificationCenter`.

Par exemple, dans l’exemple d’application `AppDelegate`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.ProvidesAppNotificationSettings | UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
```

### <a name="opensettings-method"></a>OpenSettings (méthode)

Le `OpenSettings` méthode, appelée par le système au lien ciblé de l’écran de gestion de notification d’une application, doit diriger l’utilisateur directement vers cet écran.

Dans l’exemple d’application, cette méthode exécute le segue à la `ManageNotificationsViewController` si nécessaire :

```csharp
[Export("userNotificationCenter:openSettingsForNotification:")]
public void OpenSettings(UNUserNotificationCenter center, UNNotification notification)
{
    var navigationController = Window.RootViewController as UINavigationController;
    if (navigationController != null)
    {
        var currentViewController = navigationController.VisibleViewController;
        if (currentViewController is ViewController)
        {
            currentViewController.PerformSegue(ManageNotificationsViewController.ShowManageNotificationsSegue, this);
        }

    }
}
```

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Infrastructure de Notifications d’utilisateur dans Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Meilleures pratiques et quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Générer une Notification à distance (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
