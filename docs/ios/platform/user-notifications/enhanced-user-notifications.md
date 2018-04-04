---
title: Notifications utilisateur améliorée
description: Cet article couvre toutes les méthodes que la Notification des utilisateurs ont été améliorée par iOS 10 et comment les utiliser dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9fd3ff17dc9af3fd30a7d5b31e8cea7ff8669a51
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="enhanced-user-notifications"></a>Notifications utilisateur améliorée

_Cet article couvre toutes les méthodes que la Notification des utilisateurs ont été améliorée par iOS 10 et comment les utiliser dans une application Xamarin.iOS._

Nouveau pour iOS 10, la Notification utilisateur framework permet la remise et le traitement des notifications locale et distantes. À l’aide de cette infrastructure, une application ou une Extension d’application permettre planifier la remise de notifications locales en spécifiant un ensemble de conditions comme emplacement ou l’heure du jour.

## <a name="about-user-notifications"></a>À propos des Notifications à l’utilisateur

Comme indiqué ci-dessus, la nouvelle infrastructure de Notification de l’utilisateur permet la remise et le traitement des notifications locale et distantes. À l’aide de cette infrastructure, une application ou une Extension d’application permettre planifier la remise de notifications locales en spécifiant un ensemble de conditions comme emplacement ou l’heure du jour.

En outre, l’application ou une extension peut recevoir (et éventuellement modifier) les notifications à la fois locale et distantes qu’ils sont remis à l’appareil iOS de l’utilisateur.

La nouvelle infrastructure d’interface utilisateur de Notification utilisateur permet à une application ou une Extension d’application pour personnaliser l’apparence des notifications à la fois locale et distantes lorsqu’ils sont présentés à l’utilisateur.

Cette infrastructure offre une application peut fournir les notifications à un utilisateur les domaines suivants :

- **Alertes Visual** - où la notification reporte à partir du haut de l’écran sous forme de bannière.
- **Son et aux Vibrations** -peut être associé à une notification.
- **Marquage d’icône application** - où icône de l’application affiche un badge indique que le nouveau contenu est disponible, comme le nombre de messages électroniques non lus.

En outre, selon le contexte actuel de l’utilisateur, il existe différentes façons d’une notification s’affiche :

- Si l’appareil est déverrouillé, la notification déploie vers le bas à partir du haut de l’écran sous forme de bannière.
- Si l’appareil est verrouillé, la notification s’affichera sur l’écran de verrouillage de l’utilisateur.
- Si l’utilisateur a manqué une notification, ils peuvent ouvrir le centre de notifications et afficher les notifications en attente disponibles, il.

Une application Xamarin.iOS a deux types de Notifications à l’utilisateur qu’il est en mesure d’envoyer :

- **Notifications locales** -ceux-ci sont envoyés par les applications installées localement sur l’appareil de l’utilisateur.
- **Notifications à distance** -sont envoyés à partir d’un ordinateur serveur et soit présentées à l’utilisateur, ou ils déclenchent une mise à jour de l’arrière-plan du contenu de l’application.

### <a name="about-local-notifications"></a>À propos des Notifications Local

Une application iOS peut envoyer les Notifications Local ont les fonctionnalités et les attributs suivants :

- Elles sont envoyées par des applications locales sur l’appareil de l’utilisateur. 
- Ils sont peut être configuré pour utiliser l’emplacement ou temps en fonction déclencheurs. 
- L’application planifie la notification avec l’appareil de l’utilisateur, et il est affiché lorsque la condition de déclenchement est remplie.
- Lorsque l’utilisateur interagit avec une notification, l’application reçoit un rappel.

Voici quelques exemples de Notifications Local :

- Alertes de calendrier
- Alertes de rappel
- Emplacement des déclencheurs prenant en charge

Pour plus d’informations, consultez le site d’Apple [Local et distant Guide de programmation de Notification](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) documentation.

### <a name="about-remote-notifications"></a>À propos des Notifications à distance

Une application iOS peut envoyer les Notifications à distance ont les fonctionnalités et les attributs suivants :

- L’application a un composant côté serveur avec lequel il communique.
- Apple Push Notification Service (APNs) est utilisé pour transmettre une remise meilleur effort des Notifications à distance à l’appareil de l’utilisateur à partir de serveurs de cloud en fonction du développeur.
- Lorsque l’application reçoit la Notification à distance, il sera affiché à l’utilisateur.
- Lorsque l’utilisateur interagit avec la notification, l’application reçoit un rappel.

Voici quelques exemples de Notifications à distance :

- Nouvelles alertes
- Mises à jour de sports
- Messages de messagerie instantanées

Il existe deux types de Notifications à distance disponibles pour une application iOS :

- **Direction de l’utilisateur** -elles sont affichées à l’utilisateur sur l’appareil.
- **Les mises à jour en mode silencieux** -ils fournissent un mécanisme pour mettre à jour le contenu d’une application iOS en arrière-plan. Lorsqu’une mise à jour sans assistance est reçue, l’application peut atteindre les serveurs supprimer par extraction vers le bas le contenu le plus récent.

Pour plus d’informations, consultez le site d’Apple [Local et distant Guide de programmation de Notification](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) documentation.

### <a name="about-the-existing-notifications-api"></a>À propos de l’API existant de Notifications

Avant d’iOS 10, utilisez une application iOS `UIApplication` pour inscrire une notification avec le système et pour planifier comment cette notification doit être déclenchée (par heure ou l’emplacement).

Il existe plusieurs problème dans lequel un développeur peut rencontrer lorsque vous travaillez avec l’API de notification existant :

- A été différents rappels requis pour Local ou distant des Notifications qui peut entraîner la duplication de code.
- L’application avait un contrôle limité de la notification une fois qu’il a été planifiée avec le système.
- Différents niveaux de prise en charge se sont produites sur l’ensemble des plateformes d’existantes d’Apple.

### <a name="about-the-new-user-notification-framework"></a>Sur la nouvelle infrastructure de Notification utilisateur

Avec iOS 10, Apple a introduit la nouvelle infrastructure de Notification de l’utilisateur, qui remplace l’existante `UIApplication` méthode indiqués ci-dessus.

L’infrastructure de Notification utilisateur offre les avantages suivants :

- Une API familière qui inclut la parité de fonctionnalités avec les méthodes précédentes pour simplifier la porter du code à partir de l’infrastructure existante.
- Inclut un ensemble étendu d’options de contenu qui autorise les notifications soient envoyées à l’utilisateur.
- Local et distant Notifications peuvent être gérées par le même code et les rappels.
- Simplifie le processus de gestion des rappels qui sont envoyées à une application lorsque l’utilisateur interagit avec une notification.
- Gestion améliorée des notifications en attente et remises, y compris la possibilité de supprimer ou de mettre à jour des notifications.
- Ajoute la possibilité d’effectuer la présentation dans l’application de notifications.
- Permet de planifier et gérer les notifications à partir d’Extensions d’application.
- Ajoute un nouveau point d’extension pour les notifications d’eux-mêmes. 

La nouvelle infrastructure de Notification de l’utilisateur fournit une notification unifiée API sur des plateformes multiple que Apple prend en charge, notamment : 

- **iOS** -prise en charge pour gérer et planifier des notifications complète.
- **tvOS** -ajoute la possibilité pour les icônes d’application badge pour les notifications locale et distantes.
- **watchOS** - ajoute la capacité de transférer des notifications à partir de son appareil iOS appariée à leur Apple Watch et permet aux applications de surveillance pour faire des notifications locales directement sur la montre.

Pour plus d’informations, consultez le site d’Apple [UserNotifications Framework référence](https://developer.apple.com/reference/usernotifications) et [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) documentation.

## <a name="preparing-for-notification-delivery"></a>Préparation pour la remise de Notification

Avant un iOS application peut envoyer des notifications à l’utilisateur de que l’application doit être enregistrée avec le système et, une notification étant une interruption à l’utilisateur, une application doit demander explicitement l’autorisation avant de les envoyer.

Il existe trois niveaux différents de demandes de notification que l’utilisateur peut approuver pour une application :

- Bannière s’affiche.
- Alertes sonores.
- Marquage de l’icône d’application.

En outre, ces niveaux d’approbation doit être demandé et définir des notifications à la fois locale et distantes.

Autorisation de notification doit être demandée dès que l’application démarre en ajoutant le code suivant à la `FinishedLaunching` méthode de la `AppDelegate` et en définissant le type de notification souhaitée (`UNAuthorizationOptions`) :

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

En outre, un utilisateur peut modifier toujours les privilèges de notification pour une application à tout moment à l’aide de la **paramètres** application sur l’appareil. L’application doit vérifier les privilèges de notification demandée de l’utilisateur avant de présenter une notification à l’aide du code suivant :

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Configuration de l’environnement de Notifications à distance

Nouveau pour iOS 10, le développeur doit informer le système d’exploitation, le notifications Push de l’environnement sont en cours d’exécution en tant que le développement ou la Production. Ne pas fournir ces informations peut entraîner l’application rejetée lorsque soumise à l’App Store iTune avec une notification similaire au suivant :

> Droits de Notification Push manquant - votre application inclut une API pour les services de notifications Push d’Apple, mais la `aps-environment` est manquant dans la signature de l’application.

Pour fournir les droits requis, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur le `Entitlements.plist` de fichiers dans le **Solution remplissage** à ouvrir pour le modifier.
2. Basculez vers le **Source** vue : 

    [![](enhanced-user-notifications-images/setup01.png "La vue de Source")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Cliquez sur le **+** pour ajouter une nouvelle clé.
4. Entrez `aps-environment` pour le **propriété**, laissez le **Type** en tant que `String` et entrez `development` ou `production` pour le **valeur**: 

    [![](enhanced-user-notifications-images/setup02.png "La propriété de l’environnement de points d’accès")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Enregistrez les modifications dans le fichier.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur le `Entitlements.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
3. Cliquez sur le **+** pour ajouter une nouvelle clé.
4. Entrez `aps-environment` pour le **propriété**, laissez le **Type** en tant que `String` et entrez `development` ou `production` pour le **valeur**: 

    [![](enhanced-user-notifications-images/setup02w.png "La propriété de l’environnement de points d’accès")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Enregistrez les modifications dans le fichier.

-----

### <a name="registering-for-remote-notifications"></a>Inscription aux Notifications à distance

Si l’application sera envoyer et recevoir des Notifications à distance, elle devra toujours faire _d’inscription de jeton_ à l’aide des `UIApplication` API. L’inscription nécessite l’appareil pour avoir un accès à la connexion réseau actif APNs, ce qui génère le jeton nécessaire qui sera envoyé à l’application. L’application doit ensuite transférer ce jeton pour l’application côté serveur du développeur pour s’inscrire aux notifications à distance :

[![](enhanced-user-notifications-images/token01.png "Vue d’ensemble de jeton d’inscription")](enhanced-user-notifications-images/token01.png#lightbox)

Utilisez le code suivant pour initialiser l’inscription requise :

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Le jeton qui est envoyé à l’application côté serveur du développeur doit être inclus comme partie de la charge utile de Notification que get envoyée à partir du serveur pour APNs lors de l’envoi d’une Notification à distance :

[![](enhanced-user-notifications-images/token02.png "Le jeton fait partie de la charge utile de Notification")](enhanced-user-notifications-images/token02.png#lightbox)

Le jeton fait Office de la clé qui associe la notification et l’application utilisé pour ouvrir ou de répondre à la notification.

Pour plus d’informations, consultez le site d’Apple [Local et distant Guide de programmation de Notification](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) documentation.

## <a name="notification-delivery"></a>Remise de notification

Avec l’application entièrement inscrit et demandé par les autorisations requises à partir d’et accordées par l’utilisateur, l’application est maintenant prête à envoyer et recevoir des notifications. 

### <a name="providing-notification-content"></a>Fournir le contenu de la Notification

Nouveau iOS 10, toutes les notifications contiennent les deux un **titre** et **sous-titre** qui sera toujours affiché avec la **corps** du contenu de la notification. Nouvelle, est également la possibilité d’ajouter **des pièces jointes de support** pour le contenu de la notification.

Pour créer le contenu d’une Notification en Local, utilisez le code suivant :

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Pour les Notifications à distance, le processus est similaire :

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Planification lors d’une Notification est envoyée.

Avec le contenu de la Notification créée, l’application a besoin de planifier le moment auquel la notification est présentée à l’utilisateur en définissant un *déclencheur*. iOS 10 fournit quatre types différents de déclencheur :

- **Notification d’émission** - est utilisé exclusivement avec des Notifications à distance et est déclenchée lorsque l’APNs les envoie une notification de package à l’application en cours d’exécution sur l’appareil.
- **Intervalle de temps** -permet une Notification Local planifié à partir d’une heure intervalle commencer maintenant et un un futur point de fin. Par exemple, `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`.
- **Date du calendrier** -autorise les Notifications Local à être planifié pour une date et heure spécifiques.
- **Emplacement basé** -permet de Notifications Local pour être planifiée quand l’appareil iOS est entrant ou en laissant un emplacement géographique spécifique ou à une proximité donnée de toutes les balises Bluetooth.

Lorsqu’une Notification Local est prêt, l’application doit appeler la `Add` méthode de la `UNUserNotificationCenter` objet planifier son affichage à l’utilisateur. Pour les Notifications à distance, l’application côté serveur envoie une charge utile de Notification pour le certificat APNs, qui envoie ensuite le paquet sur l’appareil de l’utilisateur.

Regrouper tous les éléments, un exemple de Notification locale peut ressembler à :

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>La gestion des Notifications de l’application au premier plan

Nouveau pour iOS 10, une application peut gérer les Notifications différemment lorsqu’il est au premier plan et une Notification est déclenchée. En fournissant un `UNUserNotificationCenterDelegate` et l’implémentation de la `UserNotificationCenter` (méthode), l’application puisse prendre la responsabilité de l’affichage de la Notification. Par exemple :

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Ce code est simplement écrire le contenu de la `UNNotification` à la sortie de l’Application et demande au système d’afficher l’alerte standard pour la notification. 

Si l’application voulu afficher la notification elle-même lorsqu’il était au premier plan et pas utiliser les valeurs par défaut du système, transmettez `None` au gestionnaire d’achèvement. Exemple :

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Avec ce code en place, ouvrez le `AppDelegate.cs` pour la modification de fichiers et de modifier le `FinishedLaunching` méthode à rechercher comme suit :

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Ce code est attachement personnalisé `UNUserNotificationCenterDelegate` ci-dessus actuel `UNUserNotificationCenter` afin de l’application peut gérer la notification lorsqu’elle est active et au premier plan.

## <a name="notification-management"></a>Gestion de la notification

Nouveau pour iOS 10, gestion de Notification permet d’accéder à des Notifications en attente et remises et ajoute la possibilité de supprimer, mettre à jour ou promouvoir ces Notifications.

Une partie importante de la gestion de la Notification est le _identificateur de demande_ qui a été assigné à la Notification quand il a été créé et planifié avec le système. Pour les Notifications à distance, il est attribué via la nouvelle `apps-collapse-id` champ dans l’en-tête de demande HTTP.

L’identificateur de demande permet de sélectionner l’application souhaite effectuer la gestion des notifications sur la Notification.

### <a name="removing-notifications"></a>Suppression des Notifications

Pour supprimer une Notification en attente à partir du système, utilisez le code suivant :

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Pour supprimer une Notification déjà remises, utilisez le code suivant :

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Mise à jour d’une Notification existante

Pour mettre à jour d’une Notification existante, simplement de créer une nouvelle notification avec les paramètres souhaités modifiées (par exemple, une nouvelle heure de déclenchement) et de l’ajouter au système avec le même identificateur de demande en tant que la Notification doit être modifiée. Exemple :


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Pour les Notifications déjà remises, la Notification existante sera obtenir mis à jour et promue vers le haut de la liste sur les écrans d’accueil et de verrou et dans le centre de Notification Si elle a déjà été lu par l’utilisateur.

## <a name="working-with-notification-actions"></a>Utilisation des Actions de Notification

Dans iOS 10, les Notifications sont remises à l’utilisateur ne sont pas statiques et fournissent plusieurs méthodes que l’utilisateur peut interagir avec eux (à partir d’intégrées à des Actions personnalisées).

Il existe trois types d’Actions capable de répondre à une application iOS :

- **Action par défaut** -c’est lorsque l’utilisateur appuie sur une Notification pour ouvrir l’application et afficher les détails de la Notification donnée.
- **Actions personnalisées** -ces ont été ajoutées dans iOS 8 et fournissent un moyen rapide de l’utilisateur d’effectuer une tâche personnalisée directement à partir de la Notification sans avoir à lancer l’application. Ils peuvent être présentées comme une liste des boutons avec un titre personnalisable ou un texte champ d’entrée qui peut s’exécuter en arrière-plan (où l’application est indiquée une petite quantité de temps pour répondre à la demande) ou de premier plan (sur lequel l’application est lancée au premier plan fu lfill la demande). Actions personnalisées sont disponibles sur iOS et watchOS.
- **Action de rejeter** -cette Action est envoyée à l’application lorsque l’utilisateur ferme une Notification donnée.

### <a name="creating-custom-actions"></a>Création d’Actions personnalisées

Pour créer et enregistrer une Action personnalisée avec le système, utilisez le code suivant :

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

Lorsque vous créez un nouveau `UNNotificationAction`, est associé à un ID unique et le titre qui apparaîtra sur le bouton. Par défaut, l’Action sera créée comme une Action en arrière-plan, mais les options peuvent être fournies pour ajuster le comportement de l’Action (par exemple la valeur soit une Action de premier plan).

Chacune des Actions créées doivent être associées à une catégorie. Lorsque vous créez un nouveau `UNNotificationCategory`, il est attribué à un ID unique, une liste d’Actions, il peut effectuer, une liste d’ID d’intention de fournir plus d’informations sur l’intention des Actions dans la catégorie et certaines options pour contrôler le comportement de la catégorie.

Enfin, toutes les catégories sont enregistrés avec le système à l’aide de la `SetNotificationCategories` (méthode).

### <a name="presenting-custom-actions"></a>Présentation des Actions personnalisées

Une fois qu’un ensemble d’Actions personnalisées et les catégories qui ont été créés et enregistrés avec le système, ils peuvent être présentées de Local ou distant des Notifications.

Pour recevoir une Notification à distance, vous devez définir un `category` dans la charge utile Notification à distance qui correspond à l’une des catégories créés ci-dessus. Par exemple :

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Pour les Notifications Local, définissez le `CategoryIdentifier` propriété de la `UNMutableNotificationContent` objet. Par exemple :

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Là encore, cet ID doit correspondre à une des catégories qui a été créé précédemment.

### <a name="handling-dismiss-actions"></a>Gestion des Actions de rejeter

Comme indiqué ci-dessus, une Action de faire disparaître peuvent être envoyée à l’application lorsque l’utilisateur ferme une Notification. Comme il ne s’agit pas d’une Action standard, une option devrez être définie lors de la catégorie est créée. Par exemple :

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Gestion des réponses de l’Action

Quand l’utilisateur interagit avec les Actions personnalisées et les catégories qui ont été créés ci-dessus, l’application a besoin pour répondre à la tâche demandée. Pour ce faire, vous devez fournir un `UNUserNotificationCenterDelegate` et l’implémentation de la `UserNotificationCenter` (méthode). Par exemple :

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Passé dans `UNNotificationResponse` classe a un `ActionIdentifier` propriété qui peut être l’Action par défaut ou l’Action de faire disparaître. Utilisez `response.Notification.Request.Identifier` pour tester des actions personnalisées.

Le `UserText` propriété conserve la valeur d’entrée de texte n’importe quel utilisateur. Le `Notification` propriété contient la Notification d’origine qui inclut la demande avec le déclencheur et Notification de contenu. L’application peut décider si elle a été Local ou distant de Notification en fonction du type de déclencheur.

## <a name="working-with-service-extensions"></a>Utilisation des Extensions de Service

Lorsque vous travaillez avec des Notifications à distance, _les Extensions de Service_ fournissent un moyen pour activer le chiffrement de bout en bout à l’intérieur de la charge utile de Notification. Les Extensions de service sont une extension sans Interface utilisateur (disponible dans iOS 10) qui s’exécutent en arrière-plan avec l’objectif principal de l’augmentation ou de remplacer le contenu visible d’une Notification avant qu’il est présenté à l’utilisateur. 

[![](enhanced-user-notifications-images/extension01.png "Vue d’ensemble de l’Extension de service")](enhanced-user-notifications-images/extension01.png#lightbox)

Les Extensions de service sont conçues pour s’exécuter rapidement de reçoivent uniquement un court laps de temps à exécuter par le système. Dans le cas où l’Extension de Service ne parvient pas à effectuer sa tâche dans le délai imparti, une méthode de secours sera appelée. Si le secours échoue, le contenu d’origine Notification s’affichera à l’utilisateur.

Certaines utilisations possibles des Extensions de Service sont les suivantes :

- Fourniture de chiffrement de bout en bout pour le contenu de la Notification à distance.
- Ajout de pièces jointes aux Notifications à distance pour enrichir les.

### <a name="implementing-a-service-extension"></a>Implémentation d’une Extension de Service

Pour implémenter une Extension de Service dans une application Xamarin.iOS, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Ouvrez la solution de l’application dans Visual Studio pour Mac.
2. Avec le bouton droit sur le nom de la Solution dans le **Solution remplissage** et sélectionnez **ajouter** > **ajouter un nouveau projet**.
3. Sélectionnez **iOS** > **Extensions** > **Extensions de Service de Notification** et cliquez sur le **suivant** bouton : 

    [![](enhanced-user-notifications-images/extension02.png "Sélectionnez les Extensions de Service de Notification")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Entrez un **nom** pour l’extension et cliquez sur le **suivant** bouton : 

    [![](enhanced-user-notifications-images/extension03.png "Entrez un nom pour l’extension")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Ajuster la **nom du projet** et/ou **nom de la Solution** si nécessaire et cliquez sur le **créer** bouton : 

    [![](enhanced-user-notifications-images/extension04.png "Ajuster le nom du projet et/ou le nom de la Solution")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ouvrez la solution de l’application dans Visual Studio.
2. Avec le bouton droit sur le nom de la Solution dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **ajouter un nouveau projet**.
3. Sélectionnez **iOS** > **Extensions** > **Extensions de Service de Notification**: 

    [![](enhanced-user-notifications-images/extension01w.png "Sélectionnez les Extensions de Service de Notification")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. Entrez un **nom** pour l’extension et cliquez sur le **OK** bouton.

-----

> [!IMPORTANT]
> L’identificateur de lot pour l’extension du service doit correspondre à l’identificateur de lot de l’application principale avec `.appnameserviceextension` ajouté à la fin. Par exemple, si l’application principale a un identificateur de lot de `com.xamarin.monkeynotify`, l’extension de service doit avoir un identificateur de lot de `com.xamarin.monkeynotify.monkeynotifyserviceextension`. Elle doit être définie automatiquement lorsque l’extension est ajoutée à la solution. 

Il existe une classe principale dans l’Extension du Service de Notification qui devront être modifiées pour fournir les fonctionnalités requises. Par exemple :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

La première méthode, `DidReceiveNotificationRequest`, est passée de l’identificateur de Notification ainsi que le contenu de Notification via le `request` objet. Passé dans `contentHandler` devront être appelés pour présenter la Notification à l’utilisateur.

La deuxième méthode, `TimeWillExpire`, sera appelée juste avant l’heure est épuisée pour l’Extension de Service traiter la demande. Si l’Extension de Service ne parvient pas à appeler le `contentHandler` dans le délai imparti, le contenu d’origine s’affichera à l’utilisateur.

### <a name="triggering-a-service-extension"></a>Déclenchement d’une Extension de Service

Avec une Extension de Service créé et remis à l’application, il peut être déclenché en modifiant la charge de Notification à distance envoyée à l’appareil. Par exemple :

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

La nouvelle `mutable-content` clé spécifie que l’Extension du Service devra être lancée pour mettre à jour le contenu de la Notification à distance. Le `encrypted-content` clé contient les données chiffrées qui peut le déchiffrer l’Extension du Service avant de les présenter à l’utilisateur.

Examinons l’exemple suivant, Extension de Service :

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Ce code déchiffre le contenu chiffré à partir de la `encrypted-content` de clé, crée un nouveau `UNMutableNotificationContent`, définit le `Body` propriété au contenu déchiffré et utilise le `contentHandler` pour présenter la notification à l’utilisateur.

## <a name="summary"></a>Récapitulatif

Cet article a couvert toutes les méthodes que la Notification des utilisateurs ont été améliorée par iOS 10. Elle présente la nouvelle infrastructure de Notification des utilisateurs et comment l’utiliser dans une application Xamarin.iOS ou d’une Extension d’application.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Référence UserNotifications Framework](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Guide de programmation Notification locaux et distants](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
