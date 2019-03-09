---
title: Notifications à distance avec Google Cloud Messaging
description: Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser Google Cloud Messaging pour implémenter des notifications à distance (également appelées notifications push) dans une application Xamarin.Android. Il décrit les différentes classes que vous devez implémenter pour communiquer avec Google Cloud Messaging (GCM), elle explique comment définir des autorisations dans le manifeste Android pour l’accès à GCM, et illustre la messagerie de bout en bout avec un exemple de programme de test.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: e5a5e44a61d352b5de05564ebb7192d21ed83dfa
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668892"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Notifications à distance avec Google Cloud Messaging

_Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser Google Cloud Messaging pour implémenter des notifications à distance (également appelées notifications push) dans une application Xamarin.Android. Il décrit les différentes classes que vous devez implémenter pour communiquer avec Google Cloud Messaging (GCM), elle explique comment définir des autorisations dans le manifeste Android pour l’accès à GCM, et illustre la messagerie de bout en bout avec un exemple de programme de test._

> [!NOTE]
> GCM a été remplacé par [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serveur et client API [ont été déconseillées](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) et ne sera plus disponible dès que le 11 avril 2019.

## <a name="gcm-notifications-overview"></a>Vue d’ensemble des Notifications GCM

Dans cette procédure pas à pas, nous allons créer une application Xamarin.Android qui utilise Google Cloud Messaging (GCM) pour implémenter des notifications à distance (également appelé *notifications push*). Nous allons implémenter les différents services intention et l’écouteur qui utilisent GCM pour la messagerie à distance, et nous allons tester notre implémentation avec un programme de ligne de commande qui simule un serveur d’applications. 

Notez que Firebase Cloud Messaging (FCM) est la nouvelle version de GCM &ndash; Google recommande vivement à l’aide de FCM plutôt que GCM. Si vous utilisez actuellement GCM, mise à niveau vers FCM est recommandé. Pour plus d’informations sur FCM, consultez [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Avant de poursuivre cette procédure pas à pas, vous devez obtenir les informations d’identification nécessaires pour utiliser les serveurs GCM de Google Ce processus est expliqué dans [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). En particulier, vous devez un *clé API* et un *Sender ID* à insérer dans l’exemple de code présenté dans cette procédure pas à pas. 

Nous allons utiliser les étapes suivantes pour créer une application de client Xamarin.Android prenant en charge GCM :

1.  Installer les packages supplémentaires requis pour les communications avec les serveurs GCM.
2.  Configurer les autorisations d’application pour l’accès aux serveurs GCM.
3.  Implémentez le code pour vérifier la présence des Services Google Play. 
4.  Implémenter un service d’intention d’inscription qui négocie avec GCM pour un jeton d’inscription.
5.  Implémenter un service d’écoute ID instance qui écoute les mises à jour de jeton d’inscription de GCM.
6.  Implémenter un service d’écoute GCM qui reçoit des messages à distance à partir du serveur d’application via GCM.

Cette application utilise une nouvelle fonctionnalité GCM appelée *rubrique messagerie*. Dans la rubrique messagerie, le serveur d’application envoie un message à une rubrique, plutôt qu’à une liste des appareils individuels. Les appareils qui s’abonnent à cette rubrique peuvent recevoir des messages de la rubrique en tant que notifications push. Pour plus d’informations sur la messagerie de rubrique GCM, consultez de Google [mise en œuvre la messagerie de rubrique](https://developers.google.com/cloud-messaging/topic-messaging). 

Lorsque l’application cliente est prête, nous allons implémenter une ligne de commande C# application qui envoie une notification push à notre application client par le biais de GCM. 

## <a name="walkthrough"></a>Procédure pas à pas

Pour commencer, nous allons créer une nouvelle Solution vide appelée **RemoteNotifications**. Ensuite, nous allons ajouter un nouveau projet Android à cette Solution est basée sur le **application Android** modèle. Ce projet, nous allons appeler **ClientApp**. (Si vous n’êtes pas familiarisé avec la création de projets de Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) Le **ClientApp** projet contiendra le code pour l’application de client Xamarin.Android qui reçoit des notifications à distance par le biais de GCM. 

### <a name="add-required-packages"></a>Ajouter des Packages requis

Avant de nous pouvons implémenter notre code d’application cliente, nous devons installer plusieurs packages que nous allons utiliser pour la communication avec GCM. En outre, nous devons ajouter l’application Google Play Store sur votre appareil s’il n’est pas déjà installé.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Ajoutez le Package GCM Xamarin Google Play Services

Pour recevoir des messages à partir de Google Cloud Messaging, le [Google Play Services](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) framework doit être présent sur l’appareil. Sans cette infrastructure, une application Android ne peut pas recevoir des messages à partir de serveurs GCM. Google Play Services s’exécute en arrière-plan pendant que l’appareil Android est sous tension en mode silencieux à écouter les messages de GCM. Lorsque ces messages arrivent, Google Play Services convertit les messages en les intentions et diffuse ensuite ces intentions pour les applications qui ont inscrit pour eux. 

Dans Visual Studio, cliquez sur **références > Gérer les Packages NuGet...** ; dans Visual Studio pour Mac, avec le bouton droit **Packages > ajouter des Packages...** . Recherchez **Xamarin Google Play Services - GCM** et installer ce package dans le **ClientApp** projet : 

[![Installation des Services Google Play](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Lorsque vous installez **Xamarin Google Play Services - GCM**, **Xamarin Google Play Services - Base** est automatiquement installé. Si vous obtenez une erreur, modifiez le projet *minimale d’Android à cibler* affectant une valeur autre que **compiler à l’aide de la version SDK** et recommencez l’installation de NuGet. 

Ensuite, modifiez **MainActivity.cs** et ajoutez le code suivant `using` instructions :

```csharp
using Android.Gms.Common;
using Android.Util;
```

Alors types dans le package de Google Play Services GMS disponible à notre code, et il ajoute des fonctionnalités de journalisation que nous utiliserons pour effectuer le suivi de notre transactions avec GMS. 

#### <a name="google-play-store"></a>Google Play Store

Pour recevoir des messages de GCM, l’application Google Play Store doit être installée sur l’appareil. (Chaque fois qu’une application Google Play est installée sur un appareil, Google Play Store est également installé, il est probable qu’il est déjà installé sur votre appareil de test.) Sans Google Play, une application Android ne peut pas recevoir des messages de GCM. Si vous n’avez pas encore installé sur votre appareil à l’application Google Play Store, visitez le [Google Play](https://support.google.com/googleplay) site web pour télécharger et installer Google Play. 

Vous pouvez également utiliser un émulateur Android exécutant Android 2.2 ou version ultérieure au lieu d’un appareil de test (il est inutile d’installer Google Play Store sur un émulateur Android). Toutefois, si vous utilisez un émulateur, vous devez utiliser Wi-Fi pour se connecter à GCM et vous devez ouvrir plusieurs ports dans votre pare-feu Wi-Fi, comme expliqué plus loin dans cette procédure pas à pas. 

### <a name="set-the-package-name"></a>Définir le nom du Package

Dans [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), nous avons spécifié un nom de package pour notre application prenant en charge GCM (sert également de ce nom de package la *ID d’application* qui est associé à notre clé d’API et l’ID de l’expéditeur). Nous allons ouvrir les propriétés de la **ClientApp** de projet et définissez le nom du package à cette chaîne. Dans cet exemple, nous définissons le nom du package `com.xamarin.gcmexample`:

[![Définition du nom de package](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Notez que l’application cliente ne pourra pas recevoir un jeton d’inscription de GCM si ce nom de package n’est pas *exactement* correspond au nom de package que nous avions saisie dans la console de développeur de Google. 

### <a name="add-permissions-to-the-android-manifest"></a>Ajouter des autorisations pour le manifeste Android

Une application Android doit disposer des autorisations suivantes configurées avant de pouvoir recevoir des notifications à partir de Google Cloud Messaging : 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Accorde l’autorisation de notre application pour vous inscrire et recevoir des messages à partir de Google Cloud Messaging. (Ce que fait `c2dm` signifie ? Cela signifie _messagerie Cloud vers appareil_, qui est le prédécesseur désormais dépréciée à GCM. 
    GCM utilise toujours `c2dm` dans la plupart des chaînes d’autorisation.) 

-   `android.permission.WAKE_LOCK` &ndash; (Facultatif) Empêche le processeur de l’appareil à partir de la mise en veille en écoutant pour un message. 

-   `android.permission.INTERNET` &ndash; Accorde l’accès à internet pour que l’application cliente puisse communiquer avec GCM. 

-   *package_name* `.permission.C2D_MESSAGE` &ndash; enregistre l’application avec Android et demande l’autorisation de recevoir exclusivement C2D tous les messages (cloud à appareil). Le *package_name* préfixe est le même que votre ID d’application. 

Nous allons définir ces autorisations dans le manifeste Android. Nous allons modifier **AndroidManifest.xml** et remplacez le contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

Dans le fichier XML ci-dessus, remplacez *nom_package* pour le nom du package pour votre projet d’application cliente. Par exemple, `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Recherchez les Services Google Play

Pour cette procédure pas à pas, nous créons une application simple avec un seul `TextView` dans l’interface utilisateur. Cette application n’indique pas directement l’interaction avec GCM. Au lieu de cela, nous allons regarder la fenêtre Sortie pour voir comment notre établissements de liaisons d’application auprès de GCM, et nous cochons la barre d’état de notification pour les nouvelles notifications dès leur arrivée. 

Tout d’abord, nous allons créer une disposition pour la zone de message. Modifier **Resources.layout.Main.axml** et remplacez le contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

Enregistrer **Main.axml** et fermez-le.

Au démarrage de l’application cliente, nous voulons pouvoir vérifier que les Services Google Play est disponible avant que nous tenter de contacter GCM. Modifier **MainActivity.cs** et remplacez le ``count`` instance déclaration de variable avec la déclaration de variable d’instance suivant : 

```csharp
TextView msgText;
```

Ensuite, ajoutez la méthode suivante à la **MainActivity** classe : 

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "Sorry, this device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Ce code vérifie le périphérique pour voir si le fichier APK Google Play Services est installé. S’il n’est pas installé, un message s’affiche dans la zone de message qui demande à l’utilisateur à télécharger un fichier APK depuis le Store Play Google (ou l’activer dans les paramètres du périphérique système). Étant donné que nous souhaitons exécuter cette vérification au démarrage de l’application cliente, nous ajouterons un appel à cette méthode à la fin de `OnCreate`. 


Ensuite, remplacez le `OnCreate` méthode avec le code suivant :

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Ce code vérifie la présence de l’APK de Services Google Play et écrit le résultat dans la zone de message. 

Nous allons complètement régénérer et exécuter l’application. Vous devez voir un écran qui ressemble à la capture d’écran suivante : 

[![Google Play Services est disponible](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Si vous n’obtenez pas ce résultat, vérifiez que le fichier APK Google Play Services est installé sur votre appareil et que le **Xamarin Google Play Services - GCM** package est ajouté à votre **ClientApp** comme expliqué de projet précédemment. Si vous obtenez une erreur de build, essayez la Solution de nettoyage et de génération du projet à nouveau. 

Ensuite, nous allons écrire du code pour contacter GCM et d’obtenir un jeton d’inscription.

### <a name="register-with-gcm"></a>Inscrire auprès de GCM

Avant de l’application peut recevoir des notifications à distance à partir du serveur d’application, il doit s’inscrire auprès de GCM, récupérer un jeton d’inscription. Le travail de l’inscription de notre application auprès de GCM est géré par un `IntentService` que nous créons. Notre `IntentService` effectue les étapes suivantes : 

1.  Utilise le [InstanceID](https://developers.google.com/instance-id/) API pour générer des jetons de sécurité qui autorisent les notre application cliente pour accéder au serveur d’application. En retour, nous obtenons l’inscription d’une jeton de GCM.

2.  Transmet le jeton d’inscription sur le serveur d’application (si le serveur de l’application l’exige).

3.  S’abonne à un ou plusieurs canaux de rubrique de notification.

Une fois que nous implémentons cela `IntentService`, nous allons le tester pour voir si nous obtenons l’inscription d’une jeton de GCM.

Ajoutez un nouveau fichier appelé **RegistrationIntentService.cs** et remplacez le code du modèle par le code suivant :


```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

Dans l’exemple de code ci-dessus, remplacez *YOUR_SENDER_ID* pour le numéro d’ID de l’expéditeur pour votre projet d’application cliente. Pour obtenir l’ID d’expéditeur pour votre projet : 

1.  Connectez-vous à la [Google Cloud Console](https://console.cloud.google.com/) et sélectionnez le nom de votre projet dans le menu déroulant. Dans le **informations sur le projet** volet qui s’affiche pour votre projet, cliquez sur **accédez aux paramètres de projet**:

    [![En sélectionnant XamarinGCM projet](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  Sur le **paramètres** , recherchez le **numéro de projet** &ndash; c’est l’ID d’expéditeur pour votre projet :

    [![Numéro de projet affiché](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Nous souhaitons commencer notre `RegistrationIntentService` au démarrage de notre application en cours d’exécution. Modifier **MainActivity.cs** et modifier le `OnCreate` méthode afin que notre `RegistrationIntentService` est démarrée après avoir vérifié la présence des Services Google Play : 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

Maintenant examinons chaque section de `RegistrationIntentService` pour comprendre son fonctionnement. 

Tout d’abord, nous annoter notre `RegistrationIntentService` avec l’attribut suivant pour indiquer que notre service doit ne pas être instanciée par le système : 

```csharp
[Service (Exported = false)]
```

Le `RegistrationIntentService` constructeur nomme le thread de travail *RegistrationIntentService* pour faciliter le débogage. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

La fonctionnalité principale du `RegistrationIntentService` réside dans le `OnHandleIntent` (méthode). Nous allons étudier ce code pour savoir comment s’inscrit à notre application auprès de GCM.


##### <a name="request-a-registration-token"></a>Demande un jeton d’inscription

`OnHandleIntent` appelle d’abord de Google [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) méthode pour demander un jeton d’inscription de GCM. Nous encapsuler ce code dans un `lock` pour vous prémunir contre le risque de plusieurs modes d’inscription qui se produisent simultanément &ndash; le `lock` garantit que ces intentions sont traitées de manière séquentielle. Si nous Impossible d’obtenir un jeton d’inscription, une exception est levée, et nous consigner une erreur. Si l’inscription réussit, `token` est définie sur le jeton d’inscription, nous avons obtenu à partir de GCM : 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

##### <a name="forward-the-registration-token-to-the-app-server"></a>Transférer le jeton d’inscription sur le serveur d’application

Si nous obtenons un jeton d’inscription (autrement dit, aucune exception n’a été levée), nous appelons `SendRegistrationToAppServer` pour associer l’inscription de l’utilisateur jeton avec le compte du côté serveur (le cas échéant) qui est géré par notre application. Étant donné que cette implémentation dépend de la conception du serveur d’application, une méthode vide est fournie ici : 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Dans certains cas, le serveur d’application n’a pas besoin de jeton d’inscription de l’utilisateur ; Dans ce cas, cette méthode peut être omise. Lorsqu’un jeton d’inscription est envoyé au serveur d’applications, `SendRegistrationToAppServer` doit conserver une valeur booléenne indiquant si le jeton a été envoyé au serveur. Si cette valeur est false, `SendRegistrationToAppServer` envoie le jeton pour le serveur d’applications &ndash; dans le cas contraire, le jeton a déjà été envoyé au serveur d’application dans un appel précédent. 


##### <a name="subscribe-to-the-notification-topic"></a>S’abonner à la rubrique de Notification

Ensuite, nous appelons notre `Subscribe` méthode pour indiquer à GCM que nous souhaitons vous abonner à une rubrique de notification. Dans `Subscribe`, nous appelons le [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) API pour vous abonner à notre application client à tous les messages sous `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Le serveur de l’application doit envoyer des messages de notification à `/topics/global` si nous sommes à les recevoir. Notez que le nom de la rubrique sous `/topics` peut être tout ce que vous le souhaitez, tant que le serveur d’applications et l’application cliente d’accord sur ces noms. (Ici, nous avons choisi le nom `global` pour indiquer que nous souhaitons recevoir des messages sur toutes les rubriques prises en charge par le serveur d’application.) 

Pour plus d’informations sur la rubrique GCM messagerie côté serveur, consultez de Google [envoyer messagerie vers des rubriques](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Implémenter un Service d’écoute de ID Instance

Jetons d’inscription sont uniques et sécurisée ; Toutefois, l’application cliente (ou GCM) devrez peut-être actualiser le jeton d’inscription en cas de réinstallation de l’application ou un problème de sécurité. Pour cette raison, nous devons implémenter un `InstanceIdListenerService` qui répond aux demandes d’actualisation des jetons de GCM. 

Ajoutez un nouveau fichier appelé **InstanceIdListenerService.cs** et remplacez le code du modèle par le code suivant : 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

Annoter `InstanceIdListenerService` avec l’attribut suivant pour indiquer que le service doit ne pas être instanciée par le système et qu’il peut recevoir le jeton d’inscription GCM (également appelé *ID d’instance*) demandes d’actualisation : 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

Le `OnTokenRefresh` méthode dans notre service commence au `RegistrationIntentService` afin qu’il peut intercepter le nouveau jeton d’inscription.


#### <a name="test-registration-with-gcm"></a>Inscription de test auprès de GCM

Nous allons complètement régénérer et exécuter l’application. Si vous recevez un jeton d’inscription de GCM, le jeton d’inscription doit être affiché dans la fenêtre Sortie. Exemple : 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Gérer les Messages en aval 

Le code que nous avons implémenté jusqu'à présent est uniquement du code « configuration » ; Il vérifie si Google Play Services est installé et qu’il négocie avec GCM et le serveur d’applications en vue de notre application client pour recevoir des notifications à distance. Toutefois, nous devons encore implémenter le code qui reçoit et traite les messages de notification en aval. Pour ce faire, nous devons implémenter un *Service d’écoute GCM*. Ce service reçoit des messages de rubrique à partir du serveur d’application et les diffusions localement sous forme de notifications. Une fois que nous implémentons ce service, nous allons créer un programme de test pour envoyer des messages vers GCM afin que nous pouvons voir si notre implémentation fonctionne correctement. 


#### <a name="add-a-notification-icon"></a>Ajouter une icône de Notification

Nous allons tout d’abord ajouter une petite icône qui s’affiche dans la zone de notification lors du lancement de notre notification. Vous pouvez copier [cette icône](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) à votre projet ou créer votre propre icône personnalisée. Nous allons nommer le fichier d’icône **ic_stat_button_click.png** et copiez-le dans le **ressources/drawable** dossier. N’oubliez pas d’utiliser **Ajouter > élément existant...**  à inclure ce fichier d’icône dans votre projet.


#### <a name="implement-a-gcm-listener-service"></a>Implémenter un Service d’écoute GCM

Ajoutez un nouveau fichier appelé **GcmListenerService.cs** et remplacez le code du modèle par le code suivant :

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

Examinons chaque section de notre `GcmListenerService` pour comprendre son fonctionnement. 

Tout d’abord, nous annoter `GcmListenerService` avec un attribut pour indiquer que ce service ne doit ne pas être instanciée par le système, et nous incluons un filtre d’intention pour indiquer qu’il reçoit les messages GCM : 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Lorsque `GcmListenerService` reçoit un message de GCM, le `OnMessageReceived` méthode est appelée. Cette méthode extrait le contenu du message à partir du passé `Bundle`, enregistre le contenu du message (de sorte que nous pouvons l’afficher dans la fenêtre Sortie) et appelle `SendNotification` pour lancer une notification locale avec le contenu du message reçu : 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

Le `SendNotification` méthode utilise `Notification.Builder` pour créer la notification, puis utilise le `NotificationManager` pour lancer la notification. En effet, il convertit le message de notification à distance en une notification locale qui sera présenté à l’utilisateur.
Pour plus d’informations sur l’utilisation de `Notification.Builder` et `NotificationManager`, consultez [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Déclarez le récepteur dans le manifeste

Avant de nous pouvons recevoir des messages de GCM, nous devons déclarer l’écouteur GCM dans le manifeste Android. Nous allons modifier **AndroidManifest.xml** et remplacez le `<application>` section avec le code XML suivant : 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

Dans le fichier XML ci-dessus, remplacez *nom_package* pour le nom du package pour votre projet d’application cliente. Dans notre exemple de procédure pas à pas, le nom du package est `com.xamarin.gcmexample`. 

Examinons ce que fait chaque paramètre dans ce document XML :

|Paramètre|Description|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Déclare que notre application implémente un récepteur GCM qui capture et traite les messages entrants de notification push.|
|`com.google.android.c2dm.permission.SEND`|Déclare que seuls les serveurs GCM peuvent envoyer des messages directement à l’application.|
|`com.google.android.c2dm.intent.RECEIVE`|Filtre d’intention publicitaires que notre application gère les messages de diffusion de GCM.|
|`com.google.android.c2dm.intent.REGISTRATION`|Publicité que notre application gère les intentions d’inscription nouveau filtre d’intention (autrement dit, nous avons implémenté un Service d’écoute ID d’Instance).|

Vous pouvez également décorer `GcmListenerService` avec ces attributs, plutôt que de les spécifier dans XML ; ici nous spécifiez-les dans **AndroidManifest.xml** afin que les exemples de code sont plus faciles à suivre. 


### <a name="create-a-message-sender-to-test-the-app"></a>Création d’un expéditeur de Message pour tester l’application

Nous allons ajouter un C# application de console de bureau de projet à la Solution et appelez-le **MessageSender**. Nous allons utiliser cette application de console pour simuler un serveur d’applications &ndash; il enverra les messages de notification **ClientApp** via GCM. 


#### <a name="add-the-jsonnet-package"></a>Ajoutez le Package Json.NET

Dans cette application de console, nous créons une charge utile JSON qui contient le message de notification que vous souhaitez envoyer à l’application cliente. Nous allons utiliser le **Json.NET** empaqueter dans **MessageSender** pour le rendre plus facile de créer l’objet JSON requis par GCM. Dans Visual Studio, cliquez sur **références > Gérer les Packages NuGet...** ; dans Visual Studio pour Mac, avec le bouton droit **Packages > ajouter des Packages...** . 

Nous allons rechercher le **Json.NET** du package et l’installer dans le projet : 

[![Installation du package Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>Ajoutez une référence à System.Net.Http

Nous allons devoir également ajouter une référence à `System.Net.Http` afin que nous pouvons instancier un `HttpClient` pour l’envoi de notre message test à GCM. Dans le **MessageSender** de projet, avec le bouton droit **références > Ajouter une référence** et faites défiler jusqu'à ce que vous voyiez **System.Net.Http**. Placez une coche à côté **System.Net.Http** et cliquez sur **OK**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Implémenter le Code qui envoie un Message de Test

Dans **MessageSender**, modifier **Program.cs** et remplacez le contenu par le code suivant :

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

Dans le code ci-dessus, remplacez *YOUR_API_KEY* à la clé d’API pour votre projet d’application cliente. 

Ce serveur d’application de test envoie le message au format JSON suivant à GCM :

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM, transfère à son tour, ce message à votre application cliente. Commençons par créer **MessageSender** et ouvrez une fenêtre de console où nous pouvons l’exécuter à partir de la ligne de commande.



### <a name="try-it"></a>Essayez !

Nous sommes maintenant prêts à tester notre application client. Si vous utilisez un émulateur ou si votre périphérique communique avec GCM par Wi-Fi, vous devez ouvrir les ports TCP suivants sur votre pare-feu pour les messages GCM obtenir à l’aide : 5228, 5229 et 5230.

Démarrez votre application cliente et observez la fenêtre Sortie. Après le `RegistrationIntentService` reçoit correctement l’inscription d’une jeton de GCM, la fenêtre de sortie doit afficher le jeton avec la sortie du journal qui ressemble à ce qui suit :

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

À ce stade, l’application cliente est prête à recevoir un message de notification à distance. À partir de la ligne de commande, exécutez le **MessageSender.exe** programme pour envoyer un message de notification « Hello, Xamarin » dans l’application cliente.
Si vous n’avez pas encore créé le **MessageSender** project, faites-le maintenant.

Pour exécuter **MessageSender.exe** sous Visual Studio, ouvrez une invite de commandes, accédez au **MessageSender/bin/Debug** répertoire, puis exécutez la commande directement :

```cmd
MessageSender.exe
```

Pour exécuter **MessageSender.exe** sous Visual Studio pour Mac, ouvrez une session de Terminal, accédez au **MessageSender/bin/Debug** le répertoire et mono d’utilisation pour exécuter **MessageSender.exe** 

```bash
mono MessageSender.exe
```

Il peut prendre jusqu'à une minute pour le message se propager via GCM et revenir à votre application cliente. Si le message est reçu avec succès, nous devrions voir la sortie qui ressemble à ce qui suit dans la fenêtre Sortie : 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

En outre, vous devriez remarquer qu’une nouvelle icône de notification s’affiche dans la barre d’état de notification : 

[![Icône de notification s’affiche sur l’appareil](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Lorsque vous ouvrez la barre d’état de notification pour afficher les notifications, vous devriez voir notre notification à distance :

[![Message de notification s’affiche.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Félicitations, votre application a reçu sa première notification à distance.

Notez que GCM les messages seront ne sont plus reçus si l’application est arrêtée de force. Pour reprendre les notifications après un arrêt forcé, l’application doit être redémarrée manuellement. Pour plus d’informations sur cette stratégie Android, consultez [lancer des contrôles sur les applications arrêtées](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) et cela [post de dépassement de capacité de pile](https://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas détaillé les étapes permettant d’implémenter des notifications à distance dans une application Xamarin.Android. Il décrit comment installer des packages supplémentaires nécessaires pour les communications de GCM, et que vous avez appris comment configurer les autorisations d’application pour l’accès aux serveurs GCM. Il a fourni des exemples de code qui illustre comment vérifier la présence des Services Google Play, comment implémenter un service intent d’inscription et le service d’écoute d’ID instance qui négocie avec GCM pour un jeton d’inscription et comment implémenter un écouteur GCM service qui reçoit et traite les messages de notification à distance. Enfin, nous avons implémenté un programme de test de ligne de commande pour envoyer des notifications de test à notre application client via GCM. 


## <a name="related-links"></a>Liens associés

- [GCM RemoteNotifications (exemple)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Messagerie cloud Google](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
