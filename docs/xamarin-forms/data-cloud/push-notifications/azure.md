---
title: "Envoi de Notifications Push à partir des applications mobiles Azure"
description: "Azure Notification Hubs fournit une infrastructure évolutive push pour l’envoi de notifications push mobiles à partir de n’importe quel serveur principal pour toutes les plateformes mobiles, tout en éliminant la complexité d’un service principal ayant communiquer avec les systèmes de notification de plateforme différente. Cet article explique comment utiliser Azure Notification Hubs pour envoyer des notifications push à partir d’une instance Azure Mobile Apps à une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: f0f767179a9280d7a6c6d7ce8125696d5e664cba
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Envoi de Notifications Push à partir des applications mobiles Azure

_Azure Notification Hubs fournit une infrastructure évolutive push pour l’envoi de notifications push mobiles à partir de n’importe quel serveur principal pour toutes les plateformes mobiles, tout en éliminant la complexité d’un service principal ayant communiquer avec les systèmes de notification de plateforme différente. Cet article explique comment utiliser Azure Notification Hubs pour envoyer des notifications push à partir d’une instance Azure Mobile Apps à une application de Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure Push Hub de Notification et de Xamarin.Forms, par [Xamarin University](https://university.xamarin.com/)**

Une notification push est utilisée pour fournir des informations, telles qu’un message, à partir d’un système principal vers une application sur un appareil mobile pour augmenter l’utilisation et l’engagement de l’application. La notification peut être envoyée à tout moment, même lorsque l’utilisateur n’est pas activement à l’aide de l’application ciblée.

Systèmes principaux envoyer des notifications push aux appareils mobiles via les systèmes de Notification plateforme (PNS), comme indiqué dans le diagramme suivant :

[![](azure-images/pns.png "Les systèmes de Notification de plateforme")](azure-images/pns-large.png#lightbox "systèmes de Notification de plateforme")

Pour envoyer une notification push, le système principal contacte le PNS propres à la plate-forme pour envoyer une notification à une instance d’application cliente. Cela augmente considérablement la complexité du serveur principal lorsque des notifications push d’inter-plateformes sont requises, car le serveur principal doit utiliser chaque PNS API spécifiques à la plateforme et protocole.

Azure Notification Hubs éliminer cette complexité en masquant les détails des systèmes de notification de plateforme différente, ce qui permet une notification inter-plateformes avec un seul appel d’API, comme indiqué dans le diagramme suivant :

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Pour envoyer une notification de transmission, les contacts uniquement du système principal Azure Notification Hub, qui à son tour, communique avec les systèmes de notification de plateforme différente, par conséquent, réduire la complexité du serveur principal du code qui envoie les notifications push.

Les applications mobiles Azure ont une prise en charge intégrée pour les notifications push à l’aide de concentrateurs de notification. Le processus d’envoi d’une notification push à partir d’une instance Azure Mobile Apps à une application de Xamarin.Forms est la suivante :

1. L’application Xamarin.Forms inscrit avec le PNS, qui retourne un handle.
1. L’instance Azure Mobile Apps envoie une notification à son concentrateur de Notification Azure, en spécifiant le descripteur de l’appareil à cibler.
1. Le Hub de Notification Azure envoie la notification au PNS approprié pour l’appareil.
1. Le PNS envoie la notification au périphérique spécifié.
1. L’application Xamarin.Forms traite la notification et l’affiche.

L’exemple d’application illustre une application de liste de tâches dont les données sont stockées dans une instance Azure Mobile Apps. Chaque fois qu’un nouvel élément est ajouté à l’instance Azure Mobile Apps, une notification est envoyée à l’application de Xamarin.Forms. Les captures d’écran suivantes illustrent chaque plateforme d’afficher les notifications reçues :

[![](azure-images/screenshots.png "Exemple d’Application reçoit une Notification Push")](azure-images/screenshots-large.png#lightbox "exemple d’Application reçoit une Notification Push")

Pour plus d’informations sur Azure Notification Hubs, consultez [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) et [ajouter des notifications de push à votre application Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure et les paramètres de système de Notification de plateforme

Le processus d’intégration d’un concentrateur de Notification Azure dans une instance Azure Mobile Apps est comme suit :

1. Créer une instance Azure Mobile Apps. Pour plus d’informations, consultez [consommation d’une application Mobile de Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Configurer un concentrateur de notification. Pour plus d’informations, consultez [configurer un concentrateur de notification](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub).
1. Mettre à jour l’instance Azure Mobile Apps pour envoyer des notifications push. Pour plus d’informations, consultez [mettre à jour le projet de serveur pour envoyer des notifications push](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Inscrire auprès de chaque solution.
1. Configurer le concentrateur de notification pour communiquer avec chaque PNS.

Les sections suivantes fournissent des instructions d’installation supplémentaires pour chaque plateforme.

### <a name="ios"></a>iOS

Les étapes supplémentaires suivantes doivent être effectués à utiliser Apple Push Notification Service (APNS) à partir d’un concentrateur de Notification Azure :

1. Générer un certificat de signature de demande pour le certificat push avec l’outil de trousseau d’accès. Pour plus d’informations, consultez [générer le fichier de demande de signature de certificat pour le certificat push](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) dans le centre de Documentation Azure.
1. Inscrire l’application de Xamarin.Forms pour la prise en charge des notifications push sur le centre de développement Apple. Pour plus d’informations, consultez [inscrire votre application pour les notifications push](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) dans le centre de Documentation Azure.
1. Créer un push notifications activées profil de configuration pour l’application de Xamarin.Forms sur le centre de développement Apple. Pour plus d’informations, consultez [créer un profil de configuration de l’application](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) dans le centre de Documentation Azure.
1. Configurer le concentrateur de notification pour communiquer avec APNS. Pour plus d’informations, consultez [configurer le concentrateur de notification pour APNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Configurer l’application de Xamarin.Forms pour utiliser le nouvel ID d’application et un profil de configuration. Pour plus d’informations, consultez [configurer le projet iOS dans Xamarin Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) ou [configuration le projet iOS dans Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) dans le centre de Documentation Azure.

### <a name="android"></a>Android

Les étapes supplémentaires suivantes doivent être effectuées à utiliser la messagerie de Cloud Firebase (FCM) à partir d’un concentrateur de Notification Azure :

1. S’inscrire pour FCM. Une clé d’API de serveur et un ID de Client sont automatiquement générés et regroupées dans un `google-services.json` fichier est téléchargé. Pour plus d’informations, consultez [activer Firebase Cloud Messaging (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Configurer le concentrateur de notification pour communiquer avec FCM. Pour plus d’informations, consultez [configurer de fin dans les applications mobiles pour envoyer des demandes de transmission à l’aide de FCM](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Les étapes supplémentaires suivantes doivent être effectués à utiliser les services de Notification de Windows (WNS) à partir d’un concentrateur de Notification Azure :

1. S’inscrire pour le Service de Notification de Windows (WNS). Pour plus d’informations, consultez [inscrire votre application Windows pour les notifications push avec WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) dans le centre de Documentation Azure.
1. Configurez le hub de notification pour communiquer avec WNS. Pour plus d’informations, consultez [configurer le concentrateur de notification pour WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) dans le centre de Documentation Azure.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Ajout de prise en charge des notifications Push à l’Application de Xamarin.Forms

Les sections suivantes décrivent l’implémentation requise dans chaque projet spécifique à la plateforme pour prendre en charge des notifications push.

### <a name="ios"></a>iOS

Le processus d’implémentation de prise en charge des notifications push dans une application iOS est comme suit :

1. Inscrire avec l’Apple Push Notification Service (APNS) dans le `AppDelegate.FinishedLaunching` (méthode). Pour plus d’informations, consultez [enregistrement avec le système de Notification Push Apple](#ios_register).
1. Implémentez la `AppDelegate.RegisteredForRemoteNotifications` méthode pour gérer la réponse d’inscription. Pour plus d’informations, consultez [gestion de la réponse d’inscription](#ios_registration_response).
1. Implémentez la `AppDelegate.DidReceiveRemoteNotification` méthode pour traiter les notifications push entrant. Pour plus d’informations, consultez [le traitement des Notifications Push entrantes](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>L’inscription auprès du Service de Notification Push Apple

Avant qu’une application iOS peut recevoir des notifications push, il doit s’inscrire avec l’Apple Push Notification Service (APNS), ce qui génère un jeton d’appareil unique et retourner à l’application. L’inscription est appelée dans le `FinishedLaunching` remplacer dans la `AppDelegate` classe :

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

Lorsqu’une application iOS s’inscrit avec APNS, il doit spécifier les types de notifications push qu’il souhaite recevoir. Le `RegisterUserNotificationSettings` méthode inscrit les types de notifications de l’application peut recevoir, avec la `RegisterForRemoteNotifications` méthode inscrire pour recevoir des notifications push à partir de APNS.

> [!NOTE]
> Ne pas appeler le `RegisterUserNotificationSettings` méthode entraîne des notifications push reçues par l’application en mode silencieux.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Gestion de la réponse d’inscription

La demande d’inscription APNS se produit en arrière-plan. Lorsque la réponse est reçue, iOS appellera la `RegisteredForRemoteNotifications` remplacer dans la `AppDelegate` classe :

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

Cette méthode crée un modèle de message de notification simple au format JSON et inscrit l’appareil pour recevoir les notifications de modèle à partir du hub de notification.

> [!NOTE]
> Le `FailedToRegisterForRemoteNotifications` remplacement doit être implémenté pour gérer les situations comme aucune connexion réseau. Ceci est important car les utilisateurs peuvent démarrer l’application lors de hors connexion.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Traitement des Notifications Push entrant

Le `DidReceiveRemoteNotification` remplacer dans la `AppDelegate` classe est utilisée pour traiter les notifications push entrant lorsque l’application est en cours d’exécution et est appelée lorsqu’une notification est reçue :

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

Le `userInfo` dictionnaire contient la `aps` clé, dont la valeur est le `alert` dictionnaire avec les données restantes de la notification. Ce dictionnaire est récupéré, avec le `string` message de notification s’affiche dans une boîte de dialogue.

> [!NOTE]
> Si une application n’est pas en cours d’exécution lorsqu’une notification push arrive, l’application sera lancée mais la `DidReceiveRemoteNotification` méthode ne sera pas traiter la notification. Au lieu de cela, obtenez la charge utile de notification et réagir de façon appropriée à partir de la `WillFinishLaunching` ou `FinishedLaunching` substitue.

Pour plus d’informations sur APNS, consultez [des Notifications Push dans iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Le processus d’implémentation de prise en charge des notifications push dans une application Android est comme suit :

1. Ajouter le [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet package pour le projet Android et définir la version de cible de l’application pour Android 7.0 ou version ultérieure.
1. Ajouter le `google-services.json` fichier téléchargé à partir de la console Firebase, à la racine du projet Android et définissez son action de génération sur **GoogleServicesJson**. Pour plus d’informations, consultez [ajouter le fichier JSON de Services Google](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Registre de messagerie avec Cloud Firebase (FCM) en déclarant un récepteur dans le manifeste Android de fichier et l’implémentation par le `FirebaseRegistrationService.OnTokenRefresh` (méthode). Pour plus d’informations, consultez [l’inscription auprès de la messagerie de Cloud Firebase](#android_register_fcm).
1. Inscrire auprès du Hub de Notification Azure dans le `AzureNotificationHubService.RegisterAsync` (méthode). Pour plus d’informations, consultez [l’inscription auprès du Hub de Notification Azure](#android_register_azure).
1. Implémentez la `FirebaseNotificationService.OnMessageReceived` méthode pour traiter les notifications push entrant. Pour plus d’informations, consultez [affichage du contenu d’une Notification Push](#android_displaying_notification).

Pour plus d’informations sur la messagerie de Cloud Firebase, consultez [de messagerie Cloud Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) et [des Notifications à distance avec la messagerie de Cloud Firebase](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>L’inscription auprès de Firebase de messagerie de cloud computing

Avant qu’une application Android peut recevoir des notifications push, il doit inscrire à FCM, ce qui génère un jeton d’inscription et retourner à l’application. Pour plus d’informations sur les jetons de l’inscription, consultez [inscription FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Pour cela :

- Déclaration d’un récepteur dans le manifeste Android. Pour plus d’informations, consultez [déclarer le récepteur dans le manifeste Android](#declaring_a_receiver).
- Implémenter le Service d’ID d’Instance Firebase. Pour plus d’informations, consultez [implémentant le Service d’ID d’Instance Firebase](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Déclaration du récepteur dans le manifeste Android

Modifier **AndroidManifest.xml** et insérez le code suivant `<receiver>` des éléments dans le `<application>` élément :

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

Ce code XML effectue les opérations suivantes :

- Déclare un interne `FirebaseInstanceIdInternalReceiver` implémentation qui est utilisée pour démarrer les services en toute sécurité.
- Déclare un `FirebaseInstanceIdReceiver` implémentation qui fournit un identificateur unique pour chaque instance de l’application. Ce récepteur également authentifie et autorise des actions.

Le `FirebaseInstanceIdReceiver` reçoit `FirebaseInstanceId` et `FirebaseMessaging` les événements et les remet à la classe est dérivée de `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Implémentation du Service de ID Instance Firebase

Inscription de l’application avec FCM est obtenue en dérivant une classe de la `FirebaseInstanceIdService` classe. Cette classe est chargée pour la génération de jetons de sécurité qui autorisent l’application cliente pour accéder aux FCM. Dans l’exemple d’application le `FirebaseRegistrationService` classe dérive de la `FirebaseInstanceIdService` classe et est indiqué dans l’exemple de code suivant :

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

Le `OnTokenRefresh` méthode est appelée lorsque l’application reçoit un jeton d’inscription FCM. La méthode récupère le jeton à partir de la `FirebaseInstanceId.Instance.Token` propriété mis à jour de façon asynchrone par FCM. Le `OnTokenRefresh` méthode rarement appelée, car le jeton est mise à jour uniquement lorsque l’application est installée ou désinstallée, lorsque l’utilisateur supprime les données d’application, lorsque l’application efface l’ID d’Instance, ou lorsque la sécurité du jeton a été compromis. En outre, le service de l’ID d’Instance FCM demande que l’application actualise périodiquement, son jeton généralement tous les 6 mois.

Le `OnTokenRefresh` méthode appelle également la `SendRegistrationTokenToAzureNotificationHub` méthode, qui est utilisé pour associer un jeton d’inscription de l’utilisateur avec le Hub de Notification Azure.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>L’inscription auprès du Hub de Notification Azure

Le `AzureNotificationHubService` classe fournit le `RegisterAsync` (méthode), qui associe le jeton d’inscription de l’utilisateur avec le Hub de Notification Azure. Ce qui suit montre l’exemple de code la `RegisterAsync` (méthode), qui est appelé par le `FirebaseRegistrationService` lorsque le jeton d’inscription de l’utilisateur change de classe :

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

Cette méthode crée un modèle de message de notification simple en tant que JSON et l’inscrit pour recevoir des notifications de modèle à partir du hub de notification, à l’aide du jeton d’inscription Firebase. Cela garantit que les notifications envoyées depuis le concentrateur de Notification Azure cible le périphérique représenté par le jeton d’inscription.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Affichage du contenu d’une Notification Push

Affichage du contenu d’une notification push est effectuée en dérivant une classe de la `FirebaseMessagingService` classe. Cette classe inclut la substituable `OnMessageReceived` fourni de méthode, qui est appelé lorsque l’application reçoit une notification FCM, que l’application s’exécute au premier plan. Dans l’exemple d’application le `FirebaseNotificationService` classe dérive de la `FirebaseMessagingService` classe et est indiqué dans l’exemple de code suivant :

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

Lorsque l’application reçoit une notification FCM, le `OnMessageReceived` méthode extrait le contenu du message et appelle le `SendNotification` (méthode). Cette méthode convertit le contenu du message en une notification locale qui est lancée alors que l’application est en cours d’exécution, à la notification qui apparaissent dans la zone de notification.

##### <a name="handling-notification-intents"></a>La gestion des modes de Notification

Quand un utilisateur appuie sur une notification, toutes les données qui accompagne le message de notification sont mises à disposition dans le `Intent` extras. Ces données peuvent être extraites par le code suivant :

```csharp
if (Intent.Extras != null)
{
  foreach (var key in Intent.Extras.KeySet())
  {
    var value = Intent.Extras.GetString(key);
    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
  }
}
```

Lancement de l’application `Intent` est déclenché lorsque l’utilisateur appuie sur son message de notification, donc ce code enregistrera les données associées le `Intent` dans la fenêtre Sortie.

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Avant une plateforme Windows universelle (UWP) application peut recevoir des notifications push qu'avec Windows Notification Services (WNS), qui permet de renvoyer un canal de notification doit s’inscrire. L’inscription est appelée par le `InitNotificationsAsync` méthode dans la `App` classe :

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

Cette méthode obtient le canal de notification push, crée un modèle de message de notification au format JSON et inscrit l’appareil pour recevoir les notifications de modèle à partir du hub de notification.

Le `InitNotificationsAsync` méthode est appelée à partir de la `OnLaunched` remplacer dans la `App` classe :

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Cela garantit que l’enregistrement des notifications push est créé ou actualisé chaque fois que l’application est lancée, par conséquent s’assurer que le canal de transmission WNS est toujours actif.

Lorsqu’une notification est reçue il sera automatiquement affiché comme un *toast* : une fenêtre non modale qui contient le message.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser Azure Notification Hubs pour envoyer des notifications push à partir d’une instance Azure Mobile Apps à une application de Xamarin.Forms. Azure Notification Hubs fournit une infrastructure évolutive push pour l’envoi de notifications push mobiles à partir de n’importe quel serveur principal pour toutes les plateformes mobiles, tout en éliminant la complexité d’un service principal ayant communiquer avec les systèmes de notification de plateforme différente.


## <a name="related-links"></a>Liens associés

- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Concentrateurs de Notification Azure](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Ajout de notifications push à votre application de Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Notifications push dans iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Messagerie cloud Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Client Mobile Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
