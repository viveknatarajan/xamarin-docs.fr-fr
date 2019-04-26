---
title: Envoi de Notifications Push à partir d’Azure Mobile Apps
description: Cet article explique comment utiliser Azure Notification Hubs pour envoyer des notifications push à partir d’une instance d’Azure Mobile Apps à une application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 92c068ceb3d382ed4612318dc987d950ec7e7ef2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327448"
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Envoi de Notifications Push à partir d’Azure Mobile Apps

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)

_Azure Notification Hubs fournit une infrastructure push évolutif pour l’envoi de notifications push mobiles à partir de n’importe quel backend à n’importe quelle plateforme mobile, tout en éliminant la complexité d’un serveur principal d’avoir à communiquer avec les systèmes de notification de plateforme différente. Cet article explique comment utiliser Azure Notification Hubs pour envoyer des notifications push à partir d’une instance d’Azure Mobile Apps à une application Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure envoie le Hub de Notification et Xamarin.Forms, par [Xamarin University](https://university.xamarin.com/)**

Une notification push est utilisée pour fournir des informations, par exemple, un message, à partir d’un système principal à une application sur un appareil mobile pour augmenter l’utilisation et l’engagement de l’application. La notification peut être envoyée à tout moment, même lorsque l’utilisateur n’est pas activement à l’aide de l’application cible.

Systèmes principaux envoient des notifications push aux appareils mobiles via les systèmes de Notification de plateforme (PNS), comme indiqué dans le diagramme suivant :

[![](azure-images/pns.png "Platform Notification Systems")](azure-images/pns-large.png#lightbox "Platform Notification Systems")

Pour envoyer une notification push, le système principal contacte le PNS propres à la plateforme pour envoyer une notification à une instance d’application client. Cela augmente considérablement la complexité du serveur principal lorsque les notifications push multiplateforme sont requises, car le serveur principal doit utiliser chaque PNS API spécifiques à la plateforme et protocole.

Azure Notification Hubs éliminer cette complexité en faisant abstraction les détails des systèmes de notification de plateforme différents, ce qui permet une notification inter-plateformes à envoyer avec un seul appel d’API, comme illustré dans le diagramme suivant :

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Pour envoyer une notification push, les contacts d’uniquement Azure Notification Hub, qui à son tour, communique avec les systèmes de notification de plateforme différente, par conséquent de réduire la complexité du serveur principal système back-end de code qui envoie des notifications push.

Azure Mobile Apps ont une prise en charge intégrée pour les notifications push à l’aide des hubs de notification. Le processus pour envoyer une notification push à partir d’une instance d’Azure Mobile Apps à une application Xamarin.Forms est comme suit :

1. L’application Xamarin.Forms inscrit avec le PNS, qui retourne un handle.
1. L’instance d’Azure Mobile Apps envoie une notification à son Hub de Notification Azure, en spécifiant le descripteur de l’appareil à cibler.
1. Azure Notification Hub envoie la notification au PNS approprié pour l’appareil.
1. Le PNS envoie la notification à l’appareil spécifié.
1. L’application Xamarin.Forms traite la notification et l’affiche.

L’exemple d’application illustre une application de liste de tâches dont les données sont stockées dans une instance d’Azure Mobile Apps. Chaque fois qu’un nouvel élément est ajouté à l’instance d’Azure Mobile Apps, une notification push est envoyée à l’application Xamarin.Forms. Les captures d’écran suivantes montrent chaque plateforme affichant la notification push reçue :

[![](azure-images/screenshots.png "Exemple d’Application reçoit une Notification Push")](azure-images/screenshots-large.png#lightbox "exemple d’Application reçoit une Notification Push")

Pour plus d’informations sur Azure Notification Hubs, consultez [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) et [ajouter des notifications push à votre application Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure et les paramètres de système de Notification de plateforme

Le processus permettant d’intégrer un Hub de Notification Azure à une instance d’Azure Mobile Apps est la suivante :

1. Créez une instance d’Azure Mobile Apps. Pour plus d’informations, consultez [consommer une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Configurer un concentrateur de notification. Pour plus d’informations, consultez [configurer un concentrateur de notification](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-hub).
1. Mettre à jour l’instance d’Azure Mobile Apps pour envoyer des notifications push. Pour plus d’informations, consultez [mettre à jour le projet de serveur pour envoyer des notifications push](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Inscrire avec chaque PNS.
1. Configurer le concentrateur de notification pour communiquer avec chaque PNS.

Les sections suivantes fournissent des instructions d’installation supplémentaires pour chaque plateforme.

### <a name="ios"></a>iOS

Les étapes supplémentaires suivantes doivent être effectués à utiliser Apple Push Notification Service (APNS) à partir d’un Hub de Notification Azure :

1. Générer un certificat de signature de demande pour le certificat push avec l’outil trousseau d’accès. Pour plus d’informations, consultez [générer le fichier de demande de signature de certificat pour le certificat push](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) sur le centre de Documentation Azure.
1. Inscrire l’application Xamarin.Forms pour la prise en charge de la notification push sur le Developer Center d’Apple. Pour plus d’informations, consultez [inscrire votre application pour les notifications push](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) sur le centre de Documentation Azure.
1. Créer un push notifications activé profil de provisionnement pour l’application Xamarin.Forms sur le Developer Center d’Apple. Pour plus d’informations, consultez [créer un profil de provisionnement pour l’application](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) sur le centre de Documentation Azure.
1. Configurer le concentrateur de notification pour communiquer avec APNS. Pour plus d’informations, consultez [configurer le notification hub pour APNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Configurez l’application Xamarin.Forms à utiliser le nouvel ID d’application et un profil de configuration. Pour plus d’informations, consultez [configuration du projet iOS dans Xamarin Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) ou [configuration du projet iOS dans Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) sur le centre de Documentation Azure.

### <a name="android"></a>Android

Les étapes supplémentaires suivantes doivent être effectués pour utiliser Firebase Cloud Messaging (FCM) à partir d’un Hub de Notification Azure :

1. S’inscrire pour FCM. Une clé d’API de serveur et un ID de Client sont automatiquement générés et regroupées dans un `google-services.json` fichier est téléchargé. Pour plus d’informations, consultez [activer Firebase Cloud Messaging (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Configurer le concentrateur de notification pour communiquer avec FCM. Pour plus d’informations, consultez [configurer Mobile Apps précédent se terminer pour envoyer des demandes de notifications push à l’aide de FCM](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Les étapes supplémentaires suivantes doivent être effectués à utiliser le Service de Notification Windows (WNS) à partir d’un Hub de Notification Azure :

1. Inscrivez-vous pour le Service de Notification Windows (WNS). Pour plus d’informations, consultez [inscrire votre application Windows pour les notifications push avec WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) sur le centre de Documentation Azure.
1. Configurer le concentrateur de notification pour communiquer avec WNS. Pour plus d’informations, consultez [configurer le notification hub pour WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) sur le centre de Documentation Azure.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Ajout de prise en charge de la Notification Push à l’Application Xamarin.Forms

Les sections suivantes décrivent l’implémentation requise dans chaque projet spécifique à la plateforme pour prendre en charge des notifications push.

### <a name="ios"></a>iOS

Le processus d’implémentation de prise en charge de la notification push dans une application iOS est comme suit :

1. Inscrire avec le Apple Push Notification Service (APNS) dans le `AppDelegate.FinishedLaunching` (méthode). Pour plus d’informations, consultez [enregistrement avec le système de Notification Push Apple](#ios_register).
1. Implémentez la `AppDelegate.RegisteredForRemoteNotifications` méthode pour gérer la réponse d’inscription. Pour plus d’informations, consultez [gestion de la réponse d’inscription](#ios_registration_response).
1. Implémentez la `AppDelegate.DidReceiveRemoteNotification` méthode pour traiter les notifications push entrantes. Pour plus d’informations, consultez [de traitement des Notifications Push entrantes](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Inscription auprès du Service de Notification Push Apple

Qu’une application iOS puisse recevoir des notifications push, il doit s’inscrire avec le Apple Push Notification Service (APNS), ce qui génère un jeton d’appareil unique et renvoyez-le à l’application. L’inscription est appelée dans le `FinishedLaunching` remplacer dans la `AppDelegate` classe :

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

Lorsqu’une application iOS s’inscrit auprès d’APNS, il doit spécifier les types de notifications push qu’il souhaite recevoir. Le `RegisterUserNotificationSettings` méthode inscrit les types de notifications de l’application peut recevoir, avec la `RegisterForRemoteNotifications` méthode inscrire pour recevoir des notifications push à partir de APNS.

> [!NOTE]
> Ne pas appeler le `RegisterUserNotificationSettings` méthode entraîne des notifications push en mode silencieux reçues par l’application.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Gestion de la réponse d’inscription

La demande d’inscription APNS se produit en arrière-plan. Lorsque la réponse est reçue, iOS appellera le `RegisteredForRemoteNotifications` remplacer dans la `AppDelegate` classe :

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

Cette méthode crée un modèle de message de notification simple au format JSON et inscrit l’appareil pour recevoir des notifications de modèle à partir du hub de notification.

> [!NOTE]
> Le `FailedToRegisterForRemoteNotifications` remplacement doit être implémenté pour gérer les situations, par exemple, aucune connexion réseau. Ceci est important car les utilisateurs peuvent démarrer l’application tout en mode hors connexion.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Traitement des Notifications Push entrantes

Le `DidReceiveRemoteNotification` remplacer dans la `AppDelegate` classe est utilisée pour traiter les notifications push entrantes lors de l’application est en cours d’exécution et est appelée lorsqu’une notification est reçue :

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

Le `userInfo` dictionnaire contient la `aps` clé, dont la valeur est le `alert` dictionnaire avec les données restantes de la notification. Ce dictionnaire est récupéré, avec le `string` message de notification affiché dans une boîte de dialogue.

> [!NOTE]
> Si une application n’est pas en cours d’exécution lorsqu’une notification push arrive, l’application doit être lancée mais la `DidReceiveRemoteNotification` méthode ne peut pas traiter la notification. Au lieu de cela, obtenez la charge utile de notification et réagir de façon appropriée à partir de la `WillFinishLaunching` ou `FinishedLaunching` remplace.

Pour plus d’informations sur APNS, consultez [Notifications Push dans iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Le processus d’implémentation de prise en charge de la notification push dans une application Android est comme suit :

1. Ajouter le [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet package sur le projet Android, puis définir la version l’application cible à Android 7.0 ou version ultérieure.
1. Ajouter le `google-services.json` fichier, téléchargé à partir de la console Firebase, à la racine du projet Android et définissez son action de génération sur **GoogleServicesJson**. Pour plus d’informations, consultez [ajouter le fichier JSON des Services Google](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Inscrivez-vous avec Firebase Cloud Messaging (FCM) en déclarant un destinataire dans le manifeste Android de fichiers et en implémentant la `FirebaseRegistrationService.OnTokenRefresh` (méthode). Pour plus d’informations, consultez [l’inscription auprès de Firebase Cloud Messaging](#android_register_fcm).
1. Inscrire auprès d’Azure Notification Hub dans le `AzureNotificationHubService.RegisterAsync` (méthode). Pour plus d’informations, consultez [l’inscription auprès d’Azure Notification Hub](#android_register_azure).
1. Implémentez la `FirebaseNotificationService.OnMessageReceived` méthode pour traiter les notifications push entrantes. Pour plus d’informations, consultez [affichage du contenu d’une Notification Push](#android_displaying_notification).

Pour plus d’informations sur Firebase Cloud Messaging, consultez [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) et [des Notifications à distance avec Firebase Cloud Messaging](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Inscription auprès de Firebase Cloud Messaging

Avant d’une application Android peut recevoir des notifications push, elle doit inscrire auprès de FCM, ce qui génère un jeton d’inscription et renvoyez-le à l’application. Pour plus d’informations sur les jetons d’inscription, consultez [l’inscription auprès de FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Pour cela :

- Déclaration d’un destinataire dans le manifeste Android. Pour plus d’informations, consultez [déclarer le récepteur dans le manifeste Android](#declaring_a_receiver).
- Implémentation du Service Firebase Instance ID. Pour plus d’informations, consultez [implémentant le Service Firebase Instance ID](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Déclaration du récepteur dans le manifeste Android

Modifier **AndroidManifest.xml** et insérez le code suivant `<receiver>` éléments dans le `<application>` élément :

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
- Déclare un `FirebaseInstanceIdReceiver` implémentation qui fournit un identificateur unique pour chaque instance d’application. Ce récepteur également authentifie et autorise des actions.

Le `FirebaseInstanceIdReceiver` reçoit `FirebaseInstanceId` et `FirebaseMessaging` événements et les remet à la classe dérivée de `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Implémentation du Service Firebase Instance ID

L’inscription de l’application auprès de FCM est effectuée en dérivant une classe de la `FirebaseInstanceIdService` classe. Cette classe est chargée de générer des jetons de sécurité qui autorisent l’application cliente à accéder à FCM. Dans l’exemple d’application le `FirebaseRegistrationService` classe dérive de la `FirebaseInstanceIdService` classe et est illustré dans l’exemple de code suivant :

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

Le `OnTokenRefresh` méthode est appelée lorsque l’application reçoit un jeton d’inscription de FCM. La méthode récupère le jeton à partir de la `FirebaseInstanceId.Instance.Token` propriété, qui est mise à jour de façon asynchrone par FCM. Le `OnTokenRefresh` méthode est rarement appelée, car le jeton est à jour uniquement lorsque l’application est installée ou désinstallée, lorsque l’utilisateur supprime les données d’application, lorsque l’application efface l’ID d’Instance, ou lorsque la sécurité du jeton a été compromis. En outre, le service FCM Instance ID demande que l’application actualise son jeton périodiquement, généralement tous les 6 mois.

Le `OnTokenRefresh` méthode appelle également la `SendRegistrationTokenToAzureNotificationHub` (méthode), qui est utilisé pour associer un jeton d’inscription de l’utilisateur avec Azure Notification Hub.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>L’inscription auprès du Hub de Notification Azure

Le `AzureNotificationHubService` classe fournit le `RegisterAsync` (méthode), qui associe le jeton d’utilisateur d’inscription à Azure Notification Hub. Le code suivant montre l’exemple le `RegisterAsync` (méthode), qui est appelé par le `FirebaseRegistrationService` classe lorsque le jeton d’inscription utilisateur est modifié :

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

Cette méthode crée un modèle de message de notification simple en tant que JSON et l’inscrit pour recevoir des notifications de modèle à partir du hub de notification, à l’aide du jeton d’inscription Firebase. Cela garantit que les notifications envoyées depuis Azure Notification Hub ciblent l’appareil représenté par le jeton d’inscription.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Affichage du contenu d’une Notification Push

Affichage du contenu d’une notification push est obtenue en dérivant une classe à partir de la `FirebaseMessagingService` classe. Cette classe inclut le remplaçables `OnMessageReceived` fourni de méthode, qui est appelé lorsque l’application reçoit une notification de FCM, que l’application est en cours d’exécution au premier plan. Dans l’exemple d’application le `FirebaseNotificationService` classe dérive de la `FirebaseMessagingService` classe et est illustré dans l’exemple de code suivant :

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

Lorsque l’application reçoit une notification de FCM, le `OnMessageReceived` méthode extrait le contenu du message et appelle le `SendNotification` (méthode). Cette méthode convertit le contenu du message en une notification locale qui est lancée alors que l’application est en cours d’exécution, cette notification apparaissant dans la zone de notification.

##### <a name="handling-notification-intents"></a>Gestion des intentions de Notification

Lorsqu’un utilisateur appuie sur une notification, toutes les données qui accompagne le message de notification sont rendues disponibles dans le `Intent` extras. Ces données peuvent être extraites par le code suivant :

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

Lanceur de l’application `Intent` est déclenché quand l’utilisateur appuie sur son message de notification, donc ce code enregistrera les données associées dans le `Intent` dans la fenêtre Sortie.

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Avant d’une plateforme Windows universelle (UWP) l’application peut recevoir des notifications push qu'avec Windows Notification Services (WNS), qui retournera un canal de notification doit s’inscrire. L’inscription est appelée par le `InitNotificationsAsync` méthode dans la `App` classe :

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

Cette méthode récupère le canal de notification push, crée un modèle de message de notification au format JSON et inscrit l’appareil pour recevoir des notifications de modèle à partir du hub de notification.

Le `InitNotificationsAsync` méthode est appelée à partir de la `OnLaunched` remplacer dans la `App` classe :

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Cela garantit que l’inscription de notification push est créée ou actualisée chaque fois que l’application est lancée, donc s’assurer que le canal push WNS est toujours actif.

Lors de la réception d’une notification push qu’il s’affiche automatiquement comme un *toast* : une fenêtre non modale qui contient le message.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser Azure Notification Hubs pour envoyer des notifications push à partir d’une instance d’Azure Mobile Apps à une application Xamarin.Forms. Azure Notification Hubs fournit une infrastructure push évolutif pour l’envoi de notifications push mobiles à partir de n’importe quel backend à n’importe quelle plateforme mobile, tout en éliminant la complexité d’un serveur principal d’avoir à communiquer avec les systèmes de notification de plateforme différente.


## <a name="related-links"></a>Liens associés

- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Ajout de notifications push à votre application Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Notifications push dans iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Messagerie cloud Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Client Mobile Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
