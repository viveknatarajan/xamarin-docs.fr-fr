---
title: Boutons d’action de notification dynamique dans Xamarin.iOS
description: Avec iOS 12, une extension de contenu de notification peut ajouter, supprimer et mettre à jour les boutons d’action affichées le long d’une notification. Ce document décrit comment utiliser les boutons d’action de notification dynamique avec Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 5611d673ecc7af896fd3a9e566e184e408b6b367
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60876056"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Boutons d’action de notification dynamique dans Xamarin.iOS

Dans iOS 12, notifications pouvant dynamiquement ajouter, supprimer et mettre à jour leurs boutons d’action associée. Cette personnalisation rend possible fournir aux utilisateurs les actions qui concernent directement les contenu de la notification et l’interaction utilisateur avec lui.

## <a name="sample-app-redgreennotifications"></a>Exemple d’application : RedGreenNotifications

Les extraits de code dans ce guide proviennent de la [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) exemple d’application, qui montre comment utiliser Xamarin.iOS pour travailler avec des boutons d’action de notification dans iOS 12.

Cet exemple d’application envoie deux types de notifications locales : rouge et vert.
Une fois que l’application envoie une notification, utilisez 3D Touch pour afficher son interface utilisateur personnalisée. Ensuite, utilisez les boutons d’action de la notification pour faire pivoter l’image que s’affiche. Comme l’image fait pivoter, un **réinitialiser la rotation** bouton apparaît et disparaît si nécessaire.

Extraits de code dans ce guide proviennent de cet exemple d’application.

## <a name="default-action-buttons"></a>Boutons d’action par défaut

Catégorie d’une notification détermine ses boutons d’action par défaut.

Créez et inscrivez les catégories de notification pendant une application est lancée.
Par exemple, dans le [exemple d’application](#sample-app-redgreennotifications), le `FinishedLaunching` méthode `AppDelegate` effectue les opérations suivantes :

- Définit une catégorie pour les notifications rouge et l’autre pour les notifications vert
- Enregistre ces catégories en appelant le [`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
méthode de `UNUserNotificationCenter`
- Attache un seul [`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
à chaque catégorie

L’exemple de code suivant montre comment cela fonctionne :

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional | UNAuthorizationOptions.ProvidesAppNotificationSettings;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
        var rotateTwentyDegreesAction = UNNotificationAction.FromIdentifier("rotate-twenty-degrees-action", "Rotate 20°", UNNotificationActionOptions.None);

        var redCategory = UNNotificationCategory.FromIdentifier(
            "red-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var greenCategory = UNNotificationCategory.FromIdentifier(
            "green-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var set = new NSSet<UNNotificationCategory>(redCategory, greenCategory);
        center.SetNotificationCategories(set);
    });
    // ...
}
```

En fonction de ce code, toutes les notifications, dont la propriété [`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
« red-catégorie » ou « green-catégorie » sera, par défaut, afficher un **faire pivoter de 20°** bouton d’action.

## <a name="in-app-handling-of-notification-action-buttons"></a>Gestion dans l’application de boutons d’action de notification

`UNUserNotificationCenter` a un `Delegate` propriété de type [ `IUNUserNotificationCenterDelegate` ](xref:UserNotifications.IUNUserNotificationCenterDelegate).

Dans l’exemple d’application, `AppDelegate` définit lui-même en tant que délégué du centre de notification utilisateur dans `FinishedLaunching`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = // ...
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        center.Delegate = this;
        // ...
```

Ensuite, `AppDelegate` implémente [`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
pour gérer les clics de bouton d’action :

```csharp
[Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
{
    if (response.IsDefaultAction)
    {
        Console.WriteLine("ACTION: Default");
    }
    if (response.IsDismissAction)
    {
        Console.WriteLine("ACTION: Dismiss");
    }
    else
    {
        Console.WriteLine($"ACTION: {response.ActionIdentifier}");
    }

    completionHandler();
        }
```

Cette implémentation de `DidReceiveNotificationResponse` ne gère pas la notification **faire pivoter de 20°** bouton d’action. Au lieu de cela, extension de contenu de la notification gère les pressions sur ce bouton. Davantage la section suivante décrit la gestion de bouton action de notification.

## <a name="action-buttons-in-the-notification-content-extension"></a>Boutons d’action dans l’extension de contenu de notification

Une extension de contenu de notification contient un contrôleur d’affichage qui définit l’interface personnalisée pour une notification.

Ce contrôleur d’affichage peut utiliser le `GetNotificationActions` et `SetNotificationActions` méthodes sur ses [`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
propriété pour accéder et modifier des boutons d’action de la notification.

Dans l’exemple d’application, contrôleur d’affichage de l’extension contenu notification modifie les boutons d’action uniquement lorsqu’il répond à un clic sur un bouton d’action existante.

> [!NOTE]
> Une extension de contenu peut répondre à un appui sur un bouton action dans son contrôleur d’affichage de notification [ `DidReceiveNotificationResponse` ](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*) méthode, déclarée dans le cadre de [IUNNotificationContentExtension](xref:UserNotificationsUI.IUNNotificationContentExtension).
>
> Même si elle porte le même nom avec le `DidReceiveNotificationResponse` méthode [décrite ci-dessus](#in-app-handling-of-notification-action-buttons), il s’agit d’une autre méthode.
>
> Une fois une extension de contenu de notification a terminé le traitement d’un appui sur un bouton, il peut choisir d’indiquer à l’application principale pour gérer ce même appui sur un bouton ou non. Pour ce faire, il doit passer une valeur appropriée de [UNNotificationContentExtensionResponseOption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption) à son gestionnaire d’achèvement :
>
> - `Dismiss` Indique que l’interface de notification doit être fermée et que l’application principale n’a pas besoin gérer l’appui sur un bouton.
> - `DismissAndForwardAction` Indique que l’interface de notification doit être fermée et que l’application principale doit également gérer l’appui sur un bouton.
> - `DoNotDismiss` Indique que l’interface de notification ne doit pas être fermée et que l’application principale n’a pas besoin gérer l’appui sur un bouton.

L’extension de contenu `DidReceiveNotificationResponse` méthode détermine que l'on a cliqué sur le bouton d’action, fait pivoter l’image dans l’interface de la notification et affiche ou masque un **réinitialiser** bouton d’action :

```csharp
[Export("didReceiveNotificationResponse:completionHandler:")]
public void DidReceiveNotificationResponse(UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
{
    var rotationAction = ExtensionContext.GetNotificationActions()[0];

    if (response.ActionIdentifier == "rotate-twenty-degrees-action")
    {
        rotationButtonTaps += 1;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        // 9 rotations * 20 degrees = 180 degrees. No reason to
        // show the reset rotation button when the image is half
        // or fully rotated.
        if (rotationButtonTaps % 9 == 0)
        {
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
        }
        else if (rotationButtonTaps % 9 == 1)
        {
            var resetRotationAction = UNNotificationAction.FromIdentifier("reset-rotation-action", "Reset rotation", UNNotificationActionOptions.None);
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction, resetRotationAction });
        }
    }

    if (response.ActionIdentifier == "reset-rotation-action")
    {
        rotationButtonTaps = 0;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
    }

    completionHandler(UNNotificationContentExtensionResponseOption.DoNotDismiss);
}
```

Dans ce cas, la méthode passe `UNNotificationContentExtensionResponseOption.DoNotDismiss` à son gestionnaire d’achèvement. Ce qui signifie que les interface de la notification restera ouverte.

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Infrastructure de Notifications d’utilisateur dans Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Déclaration des Types de votre Notification actionnables](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Meilleures pratiques et quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Générer une Notification à distance (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
