---
title: Interfaces utilisateur de notification interactive dans Xamarin.iOS
description: Avec iOS 12, il est possible de créer des interfaces utilisateur interactives pour les notifications locales et distantes. Ce guide décrit comment utiliser ces fonctionnalités avec Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 5a3a737360f898c2638bc5431cb8f8fc4882f3b0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131093"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Interfaces utilisateur de notification interactive dans Xamarin.iOS

[Extensions de contenu de notification](~/ios/platform/user-notifications/advanced-user-notifications.md), introduit dans iOS 10, permettent de créer des interfaces utilisateur personnalisées pour les notifications. Compter d’iOS 12, les interfaces utilisateur de notification peuvent contenir des éléments interactifs tels que des boutons et des curseurs.

## <a name="sample-app-redgreennotifications"></a>Exemple d’application : RedGreenNotifications

Le [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) exemple d’application contient une extension de contenu de notification avec une interface utilisateur interactive.

Extraits de code dans ce guide proviennent de cet exemple.

## <a name="notification-content-extension-infoplist-file"></a>Fichier Info.plist de notification d’extension de contenu

Dans l’exemple d’application, le **Info.plist** de fichiers dans le **RedGreenNotificationsContentExtension** projet contient la configuration suivante :

```xml
<!-- ... -->
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>UNNotificationExtensionCategory</key>
        <array>
            <string>red-category</string>
            <string>green-category</string>
        </array>
        <key>UNNotificationExtensionUserInteractionEnabled</key>
        <true/>
        <key>UNNotificationExtensionDefaultContentHidden</key>
        <true/>
        <key>UNNotificationExtensionInitialContentSizeRatio</key>
        <real>0.6</real>
    </dict>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.usernotifications.content-extension</string>
    <key></key>
    <true/>
</dict>
<!-- ... -->
```

Notez les fonctionnalités suivantes :

- Le `UNNotificationExtensionCategory` tableau spécifie le type de catégories de notification les handles de l’extension de contenu.
- Pour prendre en charge du contenu interactif, l’extension de contenu de notification définit les `UNNotificationExtensionUserInteractionEnabled` clé à `true`.
- Le `UNNotificationExtensionInitialContentSizeRatio` clé spécifie le rapport hauteur/largeur initiale pour l’interface de l’extension contenu.

## <a name="interactive-interface"></a>Interface interactive

**MainInterface.storyboard**, qui définit l’interface pour une extension de contenu de notification est une table de montage standard contenant un contrôleur d’affichage unique. Dans l’exemple d’application, le contrôleur d’affichage est de type `NotificationViewController`, et il contient un affichage de l’image, trois boutons et un curseur. La table de montage séquentiel associe les gestionnaires définis dans ces contrôles **NotificationViewController.cs**:

- Le **lancer une application** appels du Gestionnaire de bouton la `PerformNotificationDefaultAction` méthode d’action sur `ExtensionContext`, ce qui lance l’application :

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    Dans de l’application, le centre de notification utilisateur `Delegate` (dans l’exemple d’application, le `AppDelegate`) peut répondre à l’interaction dans le `DidReceiveNotificationResponse` méthode :

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- Le **faire disparaître une Notification** appels du Gestionnaire de bouton `DismissNotificationContentExtension` sur `ExtensionContext`, ce qui ferme la notification :

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- Le **une Notification de suppression** Gestionnaire du bouton ferme la notification et le supprime de centre de Notification :

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- La méthode qui gère les changements de valeurs sur le curseur met à jour la valeur alpha de l’image affichée dans l’interface de la notification :

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Infrastructure de Notifications d’utilisateur dans Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Meilleures pratiques et quelles sont les nouveautés dans les Notifications à l’utilisateur (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Notifications de riches (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [Générer une Notification à distance (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
