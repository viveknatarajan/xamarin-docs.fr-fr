---
title: watchOS des Notifications dans Xamarin
description: Ce document décrit comment utiliser des notifications watchOS dans Xamarin. Elle décrit les contrôleurs de notification créer, générer des notifications et test des notifications.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 42b0354f19a9e0c31b7a859d598526fddad726cd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791908"
---
# <a name="watchos-notifications-in-xamarin"></a>watchOS des Notifications dans Xamarin

Les applications de surveillance peuvent recevoir des notifications si l’application iOS contenant les prend en charge. Il est intégrée notification gestion afin de ne pas *doivent* pour ajouter la prise en charge de la notification supplémentaire décrite ci-dessous, cependant si vous souhaitez personnaliser le comportement de notification et d’apparence de lecture puis sur.

Reportez-vous à la [iOS Notifications](~/ios/platform/user-notifications/deprecated/index.md) sur la documentation pour plus d’informations sur l’ajout de la prise en charge de la notification à l’application iOS dans votre solution.

## <a name="creating-notification-controllers"></a>Création de contrôleurs de Notification

Sur le plan conceptuel des contrôleurs de notifications sont un type spécial de segue leur déclenchement. Lorsque vous faites glisser une nouvelle **contrôleur d’Interface Notification** sur un plan conceptuel, il aura automatiquement un segue attachée :

![](notifications-images/notification-storyboard1.png "Un nouveau contrôleur de Notification d’Interface avec un segue attaché")

Lorsque la notification segue est sélectionnée. vous pouvez modifier ses propriétés :

![](notifications-images/notification-storyboard2.png "La notification segue sélectionné")

Une fois que vous avez personnalisé le contrôleur, il peut ressembler à cet exemple à partir de la WatchKitCatalog :

![](notifications-images/notifications-segue.png "Les propriétés de Notification")


Il existe deux types de notification :

- **Présentation de courte** -non déroulante vue statique définie par le système.

- **Aspect de longue durée** - permettant le défilement, vue personnalisable définie par vous-même ! Une version plus simple, statique et une version dynamique plus complexe peuvent être spécifiés.

### <a name="short-look-notification-controller"></a>Contrôleur de Notification de l’apparence de courte

L’interface utilisateur de recherche courte se compose de l’icône d’application, nom de l’application et la chaîne de titre de notification.

Si l’utilisateur n’ignore pas la notification, le système passe automatiquement à une notification détaillée de longue durée qui fournit des informations supplémentaires.


### <a name="long-look-notification-controller"></a>Contrôleur de Notification de l’apparence de longue durée

Le système d’exploitation décide s’il faut afficher la vue statique ou dynamique basée sur un nombre de facteurs. Vous devez fournir une interface statique et peut éventuellement également inclure une interface dynamique pour les notifications.

#### <a name="static"></a>Statique

La vue statique doit être simple et rapide à afficher.

![](notifications-images/notification-static.png "L’affichage statique")

#### <a name="dynamic"></a>dynamique

La vue dynamique peut afficher plus de données et fournir plus d’interactivité.

![](notifications-images/notification-dynamic.png "L’affichage dynamique")


## <a name="generating-notifications"></a>Génération des Notifications

Les notifications peuvent provenir d’un serveur distant ([services de Notifications Push Apple](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), ou APNS) ou vous pouvez générer localement dans l’application iOS.

Reportez-vous à la [iOS procédure pas à pas de Notifications](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) pour obtenir un exemple montrant comment générer des notifications locales et le [WatchNotifications exemple](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) pour obtenir un exemple.

Notifications locales doivent avoir le `AlertTitle` définie pour être affichée sur l’Apple Watch - le `AlertTitle` chaîne s’affiche dans l’interface de l’apparence de courte. À la fois le `AlertTitle` et `AlertBody` sont affichés dans la liste des notifications ; et le `AlertBody` s’affiche dans l’interface de l’apparence de longue durée.

Cette capture d’écran montre le `AlertTitle` s’affiche dans la liste des notifications et le `AlertBody` affiché dans l’interface de l’apparence de longue durée (à l’aide de la [exemple de code](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)) :

![](notifications-images/watch-notificationslist-sml.png "Cette capture d’écran montre le AlertTitle s’affiche dans la liste des notifications") ![ ] (notifications-images/watch-notificationcontroller-sml.png "le AlertBody affiché dans l’interface de l’apparence de longue durée")

## <a name="testing-notifications"></a>Test des Notifications

Notifications (locales et distantes) ne peuvent être correctement testées sur un appareil, mais ils peuvent être simulés à l’aide un **.json** fichier dans le simulateur iOS.

### <a name="testing-on-apple-watch"></a>Test sur Apple Watch

Lorsque vous testez des notifications sur un Apple Watch, n’oubliez pas que [documentation d’Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) indique les éléments suivants :

> Lorsqu’une des notifications de local ou distant de votre application arrive sur iPhone de l’utilisateur, iOS décide s’il faut afficher la notification sur l’iPhone ou sur l’Apple Watch.

Cela est alluding au fait qu’iOS décide si une notification s’affichera sur l’iPhone ou sur la surveillance. Si l’iPhone apparié est active lorsqu’une notification est reçue, la notification est susceptible d’être affichées sur l’iPhone et *pas* acheminé vers la montre.

Pour garantir que la notification s’affiche sur la surveillance, désactivez l’écran iPhone (le bouton d’alimentation en appuyant sur une seule fois) ou laissez-le se mettre en veille. Si l’observation appariée est dans la plage, possède une puissance et est portée sur votre poignet, la notification sera routé il et s’affichent sur la surveillance (accompagnée par un subtiles).

### <a name="testing-on-the-ios-simulator"></a>Test sur le simulateur iOS

Vous *doit* fournissent une charge utile JSON de test lorsque vous testez le mode de notification dans le simulateur iOS. Définir le chemin d’accès dans le **Arguments d’exécution personnalisée** fenêtre dans Visual Studio pour Mac.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Visual Studio pour Mac affiche des options supplémentaires quand une extension de surveillance est définie comme le **projet de démarrage**.
Avec le bouton droit sur le projet d’extension espion et choisissez **exécuter avec > Personnaliser les paramètres...** :
    
[![](notifications-images/runwith-customparams-sml.png "En cours d’exécution avec les propriétés personnalisées")](notifications-images/runwith-customparams.png#lightbox)
    
Cette opération ouvre le **Arguments d’exécution** fenêtre qui contient un **WatchKit** onglet. Sélectionnez **Notification** et fournir une charge utile JSON, puis appuyez sur **Execute** pour démarrer l’application de surveillance dans le simulateur :
    
[![](notifications-images/runwith-execargs-sml.png "Sélectionnez la valeur par défaut de charge utile de Notification")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour définir la charge utile de notification test dans avec le bouton droit de Visual Studio sur l’extension de surveillance pour modifier la **propriétés de projet**. Accédez à la **déboguer** section et sélectionnez un fichier JSON de notifications à partir de la liste (il sera automatiquement tous les fichiers JSON inclus dans le projet).
    
[![](notifications-images/runwith-execargs-sml-vs.png "Sélectionnez un fichier JSON de notifications")](notifications-images/runwith-execargs-vs.png#lightbox)

Lorsque l’extension de surveillance est la **projet de démarrage**, Visual Studio affiche des options supplémentaires, comme indiqué ci-dessous. Choisissez parmi les **Notification** options pour démarrer l’application de surveillance dans **Notification** mode (à l’aide du fichier JSON sélectionné dans la fenêtre Propriétés) :
    
![](notifications-images/runwith-vs.png "Menu du périphérique")

-----

Lorsque vous testez sur le simulateur avec le fichier JSON de charge par défaut, le contrôleur de notification par défaut se présente ainsi :

![](notifications-images/notification-debug-sml.png "Une notification d’exemple")

Il est également possible d’utiliser le [ligne de commande](~/ios/watchos/troubleshooting.md#command_line) pour démarrer le simulateur iOS.

### <a name="example-notification-payload"></a>Exemple charge utile de Notification

Dans le [espion Kit catalogue](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) il s’agit d’un exemple de fichier JSON charge utile **NotificationPayload.json** (ci-dessous).

```csharp
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>Liens associés

- [WatchNotifications (notifications locales) (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Documents de Notifications de Kit espion d’Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
