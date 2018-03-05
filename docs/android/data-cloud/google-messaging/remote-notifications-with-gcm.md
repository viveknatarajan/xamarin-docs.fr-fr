---
title: "Notifications à distance avec la messagerie Cloud Google"
description: "Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser la messagerie Cloud Google pour implémenter des notifications à distance (également appelées des notifications push) dans une application Xamarin.Android. Il décrit les différentes classes que vous devez implémenter pour communiquer avec Google Cloud Messaging (GCM), elle explique comment définir des autorisations dans le manifeste Android pour l’accès aux GCM et il montre un exemple de programme de test de messagerie de bout en bout."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: dda7597456421aa4ae401f56ed6cfc7983df29c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Notifications à distance avec la messagerie Cloud Google

_Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser la messagerie Cloud Google pour implémenter des notifications à distance (également appelées des notifications push) dans une application Xamarin.Android. Il décrit les différentes classes que vous devez implémenter pour communiquer avec Google Cloud Messaging (GCM), elle explique comment définir des autorisations dans le manifeste Android pour l’accès aux GCM et il montre un exemple de programme de test de messagerie de bout en bout._

## <a name="gcm-notifications-overview"></a>Vue d’ensemble des Notifications GCM

Dans cette procédure pas à pas, nous allons créer une application de Xamarin.Android qui utilise de Google Cloud Messaging (GCM) pour implémenter des notifications à distance (également appelé *des notifications push*). Nous allons implémenter différents services intention et d’écouteur qui utilisent GCM pour la messagerie à distance, et nous allons le tester notre implémentation avec un programme de ligne de commande qui simule un serveur d’applications. 

Notez que la messagerie de Cloud Firebase (FCM) est la nouvelle version de GCM &ndash; Google recommande d’utiliser FCM plutôt que GCM. Si vous utilisez actuellement GCM, mise à niveau vers FCM est recommandé. Pour plus d’informations sur FCM, consultez [Firebase de messagerie Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Avant de poursuivre cette procédure pas à pas, vous devez obtenir les informations d’identification nécessaires pour utiliser les serveurs GCM de Soogle ; Ce processus est expliqué dans [messagerie Cloud Google](~/android/data-cloud/google-messaging/google-cloud-messaging.md). En particulier, vous devez une *clé API* et un *ID de l’expéditeur* à insérer dans l’exemple de code présenté dans cette procédure pas à pas. 

Nous allons utiliser les étapes suivantes pour créer une application cliente de Xamarin.Android de GCM compatible :

1.  Installer les packages supplémentaires requis pour les communications avec les serveurs GCM.
2.  Configurer les autorisations d’application pour l’accès aux serveurs GCM.
3.  Implémentez le code pour vérifier la présence de Services Google Play. 
4.  Implémenter un service d’intention d’enregistrement qui négocie avec GCM pour un jeton d’inscription.
5.  Implémenter un service d’écoute ID instance qui écoute les mises à jour de GCM de jeton d’inscription.
6.  Implémenter un service d’écoute GCM qui reçoit des messages à distance à partir du serveur d’application via GCM.

Cette application utilise une nouvelle fonctionnalité GCM appelée *rubrique messagerie*. Dans la rubrique messagerie, le serveur d’applications envoie un message à une rubrique, plutôt qu’à une liste de périphériques individuels. Les appareils qui s’abonnent à cette rubrique peuvent recevoir des messages de la rubrique en tant que des notifications push. Pour plus d’informations sur la messagerie de rubrique GCM, consultez Google [implémentation de la messagerie de rubrique](https://developers.google.com/cloud-messaging/topic-messaging). 

Lorsque l’application cliente est prête, nous allons implémenter une application c# en ligne de commande qui envoie une notification push à notre application cliente via GCM. 

## <a name="walkthrough"></a>Procédure pas à pas

Pour commencer, nous allons créer une Solution vide appelée **RemoteNotifications**. Ensuite, vous allez ajouter un projet Android à cette Solution est basée sur le **application Android** modèle. Ce projet, nous allons appeler **ClientApp**. (Si vous n’êtes pas familiarisé avec la création de projets de Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) Le **ClientApp** projet contiendra le code de l’application cliente Xamarin.Android qui reçoit des notifications à distance par GCM. 

### <a name="add-required-packages"></a>Ajouter des Packages requis

Avant que nous pouvons implémenter le code d’application de nos clients, nous devons installer plusieurs packages que nous allons utiliser pour la communication avec GCM. En outre, nous devons ajouter l’application de Google Play Store sur votre appareil s’il n’est pas déjà installé.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Ajoutez le Package GCM Services Xamarin Google Play

Pour recevoir des messages à partir de la messagerie Cloud Google, le [Services Google Play](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) framework doit être présent sur l’appareil. Sans cette infrastructure, une application Android ne peut pas recevoir des messages à partir de serveurs GCM. Services Google Play s’exécute en arrière-plan pendant que l’appareil Android est sous tension, en mode silencieux à l’écoute des messages de GCM. Lorsque ces messages arrivent, Services Google Play convertit les messages en modes et diffuse ces objectifs pour les applications qui ont inscrit leur. 

Dans Visual Studio, cliquez sur **références > Gérer les Packages NuGet...** ; dans Visual Studio pour Mac, avec le bouton droit **Packages > ajouter des Packages en cours...** . Recherchez **Xamarin Services Google Play - GCM** et installer ce package dans le **ClientApp** projet : 

[ ![L’installation des Services Google Play](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png)

Lorsque vous installez **Xamarin Services Google Play - GCM**, **Xamarin Services Google Play - Base** est installé automatiquement. Si vous obtenez une erreur, modifiez le projet *Minimum Android à cibler* affecter une valeur autre que **compiler avec la version du Kit de développement logiciel** et recommencez l’installation de NuGet. 

Modifiez ensuite **MainActivity.cs** et ajoutez le code suivant `using` instructions :

```csharp
using Android.Gms.Common;
using Android.Util;
```

Cela rend les types dans le package de Google Play Services GMS disponibles pour notre code, et il ajoute des fonctionnalités de journalisation que nous allons utiliser pour effectuer le suivi de ses transactions avec GMS. 

#### <a name="google-play-store"></a>Google Play Store

Pour recevoir des messages à partir de GCM, l’application de Google Play Store doit être installée sur l’appareil. (Chaque fois qu’une application Google Play est installée sur un appareil, Google Play Store est également installé, il est moins probable qu’il est déjà installé sur votre appareil de test.) Sans Google Play, une application Android ne peut pas recevoir des messages à partir de GCM. Si vous n’avez pas encore de l’application de Google Play Store installée sur votre appareil, visitez le [Google Play](https://support.google.com/googleplay) site web pour télécharger et installer Google Play. 

Vous pouvez également utiliser un émulateur Android exécutant Android 2.2 ou version ultérieure à la place d’un appareil de test (il est inutile d’installer Google Play Store sur un émulateur Android). Toutefois, si vous utilisez un émulateur, vous devez utiliser Wi-Fi pour se connecter à GCM et vous devez ouvrir plusieurs ports dans votre pare-feu Wi-Fi, comme expliqué plus loin dans cette procédure pas à pas. 

### <a name="set-the-package-name"></a>Définir le nom du Package

Dans [messagerie Cloud Google](~/android/data-cloud/google-messaging/google-cloud-messaging.md), que nous avons spécifié le nom de package pour notre application GCM (ce nom de package sert également le *ID d’application* associé à notre clé d’API et l’ID de l’expéditeur). Ouvrez les propriétés de la **ClientApp** de projet et définissez le nom du package à cette chaîne. Dans cet exemple, nous devons définir le nom du package sur `com.xamarin.gcmexample`:

[ ![Définition du nom de package](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png)

Notez que l’application cliente ne pourra pas recevoir un jeton d’inscription GCM n’est pas le cas de ce nom de package *exactement* correspond au nom de package qui nous entrés dans la console de développement Google. 

### <a name="add-permissions-to-the-android-manifest"></a>Ajouter des autorisations pour le manifeste Android

Une application Android doit disposer des autorisations suivantes configurées avant de pouvoir recevoir des notifications à partir de la messagerie Cloud Google : 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Accorde l’autorisation de notre application pour inscrire et recevoir des messages de messagerie Cloud Google. (Ce que fait `c2dm` signifie ? Cela signifie _Cloud à la messagerie de l’appareil_, qui est le prédécesseur maintenant déconseillé de GCM. 
    GCM utilise toujours `c2dm` dans de nombreuses chaînes d’autorisation.) 

-   `android.permission.WAKE_LOCK` &ndash; (Facultatif) Empêche l’appareil de l’UC de passer en mode veille lors de l’écoute d’un message. 

-   `android.permission.INTERNET` &ndash; Autorise l’accès à internet pour l’application cliente puisse communiquer avec GCM. 

-   *package_name* `.permission.C2D_MESSAGE` &ndash; enregistre l’application avec Android et demande l’autorisation de réception exclusivement C2D tous les messages (cloud à l’appareil). Le *package_name* préfixe est le même que votre ID d’application. 

Nous allons définir ces autorisations dans le manifeste Android. Permet de modifier **AndroidManifest.xml** et remplacez le contenu par le code XML suivant : 

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

Dans le document XML ci-dessus, modifiez *nom_package* pour le nom du package pour votre projet d’application cliente. Par exemple, `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Recherchez les Services Google Play

Pour cette procédure pas à pas, nous créons une application simple avec un seul `TextView` dans l’interface utilisateur. Cette application n’indique pas directement l’interaction avec GCM. Au lieu de cela, nous allons observer la fenêtre Sortie pour voir comment notre établissement de liaisons d’application avec GCM, et nous vérifierons la barre d’état de notification pour les nouvelles notifications qu’elles arrivent. 

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

Démarrage de l’application cliente, nous souhaitons pour vérifier que les Services Google Play est disponible avant d’essayer de nous contacter GCM. Modifier **MainActivity.cs** et remplacez le ``count`` déclaration de variable avec la déclaration de variable d’instance suivante de l’instance : 

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

Ce code vérifie le périphérique pour voir si APK Google Play Services est installé. S’il n’est pas installé, un message s’affiche dans la zone de message qui demande à l’utilisateur à télécharger un fichier APK à partir de Google Play Store (ou l’activer dans les paramètres du périphérique système). Étant donné que nous souhaitons effectuer cette vérification au démarrage de l’application cliente, nous allons ajouter un appel à cette méthode à la fin de `OnCreate`. 


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

Ce code vérifie la présence de Google Play Services APK et écrit le résultat dans la zone de message. 

Nous allons reconstruire entièrement et exécuter l’application. Vous devez voir un écran qui ressemble à la capture d’écran suivante : 

[ ![Services Google Play n’est disponible](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png)

Si vous n’obtenez pas ce résultat, vérifiez que l’APK de Services Google Play est installé sur votre appareil et que le **Xamarin Services Google Play - GCM** package est ajouté à votre **ClientApp** comme expliqué de projet plus haut. Si vous obtenez une erreur de build, essayez de réexécuter la génération du projet et de la Solution de nettoyage. 

Ensuite, nous allons écrire du code pour contacter GCM et obtenir un jeton d’inscription.

### <a name="register-with-gcm"></a>Inscrire auprès de GCM

Avant de l’application peut recevoir des notifications à distance à partir du serveur d’application, il doit inscrire auprès de GCM et revenir à un jeton d’inscription. Le travail de l’inscription de votre application avec GCM est géré par un `IntentService` que vous créez. Notre `IntentService` effectue les étapes suivantes : 

1.  Utilise le [InstanceID](https://developers.google.com/instance-id/) API pour générer des jetons de sécurité qui autorisent les notre application cliente pour accéder au serveur d’application. En retour, nous obtenons l’inscription d’une jeton de GCM.

2.  Transmet le jeton d’inscription pour le serveur d’applications (si le serveur d’applications est nécessaire).

3.  S’abonne à un ou plusieurs canaux de rubrique de notification.

Une fois que nous implémentons ce `IntentService`, nous allons le tester pour voir si nous obtenons l’inscription d’une jeton de GCM.

Ajouter un nouveau fichier appelé **RegistrationIntentService.cs** et remplacez le code du modèle avec les éléments suivants :


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

Dans l’exemple de code ci-dessus, remplacez *YOUR_SENDER_ID* au numéro d’identification de l’expéditeur pour votre projet d’application cliente. Pour obtenir l’ID d’expéditeur pour votre projet : 

1.  Connectez-vous à la [Google Cloud Console](https://console.cloud.google.com/) et sélectionnez votre nom de projet dans le menu déroulant. Dans le **informations sur le projet** volet s’affiche pour votre projet, cliquez sur **accédez aux paramètres de projet**:

    [![Sélection XamarinGCM projet](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png)

2.  Sur le **paramètres** , recherchez le **numéro de projet** &ndash; c’est l’ID de l’expéditeur pour votre projet :

    [![Numéro de projet affiché](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png)

Nous voulons démarrer notre `RegistrationIntentService` au démarrage de notre application en cours d’exécution. Modifier **MainActivity.cs** et modifier le `OnCreate` méthode afin que notre `RegistrationIntentService` est démarré après avoir vérifié la présence de Services Google Play : 

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

Maintenant examinons chaque section de `RegistrationIntentService` pour comprendre comment il fonctionne. 

Tout d’abord, nous annotez notre `RegistrationIntentService` avec l’attribut suivant pour indiquer que notre service n’est ne pas à être instancié par le système : 

```csharp
[Service (Exported = false)]
```

Le `RegistrationIntentService` constructeur nomme le thread de travail *RegistrationIntentService* pour faciliter le débogage. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

La fonctionnalité principale de `RegistrationIntentService` réside dans le `OnHandleIntent` (méthode). Examinons ce code pour voir comment il inscrit notre application avec GCM.


##### <a name="request-a-registration-token"></a>Demande un jeton d’inscription

`OnHandleIntent` appelle tout d’abord de Google [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) méthode pour demander un jeton d’inscription par GCM. Nous placer ce code dans un `lock` pour vous prémunir contre la possibilité de plusieurs modes d’inscription qui se produisent simultanément &ndash; le `lock` garantit que ces modes sont traités de façon séquentielle. Si nous ne parviennent pas à obtenir un jeton d’inscription, une exception est levée, et nous allons nous connecter à une erreur. Si l’inscription réussit, `token` est définie sur le jeton d’inscription, nous avons obtenu à partir de GCM : 

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

##### <a name="forward-the-registration-token-to-the-app-server"></a>Transférer le jeton d’inscription pour le serveur d’applications

Si nous obtenons un jeton d’inscription (autrement dit, aucune exception n’a été levée), nous appelons `SendRegistrationToAppServer` pour associer l’inscription de l’utilisateur jeton avec le compte du côté serveur (le cas échéant) qui est géré par votre application. Étant donné que cette implémentation dépend de la conception du serveur d’application, une méthode vide est fournie ici : 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Dans certains cas, le serveur d’applications n’a pas besoin de jeton d’inscription de l’utilisateur ; Dans ce cas, cette méthode peut être omise. Lorsqu’un jeton d’inscription est envoyé vers le serveur d’applications, `SendRegistrationToAppServer` doivent conserver une valeur booléenne indiquant si le jeton a été envoyé au serveur. Si cette valeur est false, `SendRegistrationToAppServer` envoie le jeton vers le serveur d’applications &ndash; dans le cas contraire, le jeton a déjà été envoyé vers le serveur d’applications dans un appel précédent. 


##### <a name="subscribe-to-the-notification-topic"></a>S’abonner à la rubrique de Notification

Ensuite, nous appelons notre `Subscribe` méthode pour indiquer à GCM que nous souhaitons pour vous abonner à une rubrique de notification. Dans `Subscribe`, nous appelons le [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) API pour vous abonner à notre application client à tous les messages sous `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Le serveur d’applications doit envoyer des messages de notification à `/topics/global` si nous sommes à les recevoir. Notez que le nom de la rubrique sous `/topics` peut être comme vous le souhaitez, tant que le serveur d’applications et l’application cliente sont d’accord sur ces noms. (Ici, nous avons choisi le nom `global` pour indiquer que vous voulez recevoir des messages sur toutes les rubriques prises en charge par le serveur d’applications.) 

Pour plus d’informations sur la rubrique GCM côté serveur de messagerie, consultez Google [envoyer de messagerie vers les rubriques](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Implémenter un Service de récepteur de l’ID d’Instance

Les jetons d’inscription sont uniques et sécurisé ; Toutefois, l’application cliente (ou GCM) devrez peut-être actualiser le jeton d’inscription en cas de réinstallation de l’application ou un problème de sécurité. Pour cette raison, nous devons implémenter un `InstanceIdListenerService` qui répond aux demandes d’actualisation de jeton de GCM. 

Ajouter un nouveau fichier appelé **InstanceIdListenerService.cs** et remplacez le code du modèle avec les éléments suivants : 

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

Annoter `InstanceIdListenerService` avec l’attribut suivant pour indiquer que le service de doit ne pas être instancié par le système et qu’il peut recevoir le jeton d’inscription GCM (également appelé *ID d’instance*) demandes d’actualisation : 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

Le `OnTokenRefresh` méthode dans notre service démarre le `RegistrationIntentService` afin qu’elle peut intercepter le nouveau jeton d’inscription.


#### <a name="test-registration-with-gcm"></a>Tester l’inscription avec GCM

Nous allons reconstruire entièrement et exécuter l’application. Si vous recevez un jeton d’inscription à partir de GCM, le jeton d’inscription doit être affiché dans la fenêtre Sortie. Exemple : 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Gérer les Messages en aval 

Le code que nous avons implémenté jusqu’ici n'est que « installation » ; Il vérifie si les Services Google Play est installé et négocie avec GCM et le serveur d’applications pour préparer votre application cliente pour recevoir des notifications à distance. Toutefois, nous devons encore implémenter le code qui reçoit et traite les messages de notification en aval. Pour ce faire, nous devons implémenter un *Service d’écoute GCM*. Ce service reçoit des messages de la rubrique à partir du serveur d’application et les diffusions localement sous forme de notifications. Une fois que nous implémentons ce service, nous allons créer un programme de test pour envoyer des messages à GCM afin que nous pouvons voir si notre implémentation fonctionne correctement. 


#### <a name="add-a-notification-icon"></a>Ajouter une icône de Notification

Nous allons d’abord ajouter une petite icône qui apparaît dans la zone de notification lorsque la notification est lancée. Vous pouvez copier [cette icône](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) à votre projet ou créer votre propre icône personnalisée. Nous allons nommer le fichier d’icône **ic_stat_button_click.png** et copiez-le dans le **drawable/ressources** dossier. N’oubliez pas d’utiliser **Ajouter > élément existant...**  pour inclure ce fichier d’icône dans votre projet.


#### <a name="implement-a-gcm-listener-service"></a>Implémenter un Service d’écoute GCM

Ajouter un nouveau fichier appelé **GcmListenerService.cs** et remplacez le code du modèle avec les éléments suivants :

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

Examinons chaque section de notre `GcmListenerService` pour comprendre comment il fonctionne. 

Tout d’abord, nous annotez `GcmListenerService` avec un attribut pour indiquer que ce service ne doit ne pas être instancié par le système, et nous incluons un filtre intention pour indiquer qu’il reçoit les messages GCM : 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Lorsque `GcmListenerService` reçoit un message de GCM, le `OnMessageReceived` méthode est appelée. Cette méthode extrait le contenu du message à partir du passé `Bundle`, enregistre le contenu du message (de sorte que nous pouvons l’afficher dans la fenêtre de sortie) et appelle `SendNotification` pour lancer une notification locale avec le contenu du message reçu : 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

Le `SendNotification` utilise `Notification.Builder` pour créer la notification, puis utilise le `NotificationManager` pour lancer la notification. En réalité, il convertit le message de notification à distance dans une notification de locale à être présentés à l’utilisateur.
Pour plus d’informations sur l’utilisation de `Notification.Builder` et `NotificationManager`, consultez [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Déclarez le récepteur dans le manifeste

Avant que nous pouvons recevoir des messages de GCM, nous devons déclarer l’écouteur GCM dans le manifeste Android. Permet de modifier **AndroidManifest.xml** et remplacez le `<application>` section avec le code XML suivant : 

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

Dans le document XML ci-dessus, modifiez *nom_package* pour le nom du package pour votre projet d’application cliente. Dans notre exemple de procédure pas à pas, le nom du package est `com.xamarin.gcmexample`. 

Examinons ce que fait chaque paramètre dans ce document XML :

<table>
    <thead>
        <tr>
            <th>Paramètre</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>com.google.android.gms.gcm.GcmReceiver</code></td>
            <td>Déclare que notre application implémente un récepteur GCM qui capture et traite les messages entrants de notification push.</td>
        </tr>
        <tr>
            <td><code>com.google.android.c2dm.permission.SEND</code></td>
            <td>Déclare que seuls les serveurs GCM peuvent envoyer des messages directement à l’application.</td>
        </tr>
        <tr>
            <td><code>com.google.android.c2dm.intent.RECEIVE</code></td> 
            <td>Filtre intention publicitaires que notre application gère les messages de diffusion de GCM.</td>
        </tr>
        <tr>
            <td><code>com.google.android.c2dm.intent.REGISTRATION</code></td>
            <td>Filtre intention que notre application gère les intentions de nouveau l’inscription de la publication (autrement dit, nous avons implémenté un Service d’écoute ID d’Instance).</td>
        </tr>
    </tbody>
</table>

Ou bien, vous pouvez la décorer `GcmListenerService` avec ces attributs plutôt que de les spécifier dans le XML ; ici nous spécifions dans **AndroidManifest.xml** afin que les exemples de code sont plus faciles à suivre. 


### <a name="create-a-message-sender-to-test-the-app"></a>Création d’un expéditeur de Message pour tester l’application

Nous allons ajouter un projet d’application de bureau de console c# à la Solution et appelez-le **MessageSender**. Nous allons utiliser cette application de console pour simuler un serveur d’applications &ndash; il enverra les messages de notification à **ClientApp** via GCM. 


#### <a name="add-the-jsonnet-package"></a>Ajoutez le Package de Json.NET

Dans cette application de console, nous mettons en place une charge utile JSON qui contient le message de notification que vous souhaitez envoyer à l’application cliente. Nous allons utiliser la **Json.NET** du package dans **MessageSender** pour le rendre plus facile de générer l’objet JSON requis par GCM. Dans Visual Studio, cliquez sur **références > Gérer les Packages NuGet...** ; dans Visual Studio pour Mac, avec le bouton droit **Packages > ajouter des Packages en cours...** . 

Nous allons rechercher le **Json.NET** du package et l’installer dans le projet : 

[ ![Installation du package Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png)


#### <a name="add-a-reference-to-systemnethttp"></a>Ajoutez une référence à System.Net.Http

Nous devez également ajouter une référence à `System.Net.Http` afin que nous pouvons instancier un `HttpClient` pour l’envoi de notre message de test à GCM. Dans le **MessageSender** de projet, avec le bouton **références > Ajouter une référence** et faites défiler jusqu'à ce que vous voyiez **System.Net.Http**. Placez une coche à côté **System.Net.Http** et cliquez sur **OK**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Implémentez le Code qui envoie un Message de Test

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

Ce serveur d’application de test envoie le message au format JSON suivant pour GCM :

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM, transfère à son tour, ce message à votre application cliente. Commençons par créer **MessageSender** et ouvrez une fenêtre de console où nous pouvons l’exécuter à partir de la ligne de commande.


<a name="tryit" />

### <a name="try-it"></a>Essayez !

Nous sommes maintenant prêts à tester votre application cliente. Si vous utilisez un émulateur ou si votre périphérique communique avec GCM via le Wi-Fi, vous devez ouvrir les ports TCP suivants sur votre pare-feu pour les messages GCM traverser : 5228, 5229 et 5230.

Démarrez votre application cliente et regardez la fenêtre Sortie. Après le `RegistrationIntentService` reçoit correctement l’inscription d’une jeton de GCM, la fenêtre sortie doit afficher le jeton avec la sortie du journal qui ressemble à ce qui suit :

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

À ce stade, l’application cliente est prête à recevoir un message de notification à distance. À partir de la ligne de commande, exécutez le **MessageSender.exe** programme envoie un message de notification « Hello, Xamarin » à l’application cliente.
Si vous n’avez pas encore créé le **MessageSender** projet d’équipe, faites-le maintenant.

Pour exécuter **MessageSender.exe** sous Visual Studio, ouvrez une invite de commandes, accédez au **MessageSender/bin/Debug** active et exécutez la commande directement :

```cmd
MessageSender.exe
```

Pour exécuter **MessageSender.exe** sous Visual Studio pour Mac, ouvrez une session de Terminal, accédez au **MessageSender/bin/Debug** le répertoire et mono utilisé pour exécuter **MessageSender.exe** 

```bash
mono MessageSender.exe
```

Il peut prendre une minute pour le message se propager via GCM et revenir à votre application cliente. Si le message est reçu avec succès, nous devrions constater sortie semblable à celui-ci dans la fenêtre Sortie : 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

En outre, vous devez remarquer qu’une nouvelle icône de notification s’affiche dans la barre d’état de notification : 

[ ![Icône de Notiication s’affiche sur l’appareil](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png)

Lorsque vous ouvrez la barre d’état de notification pour afficher les notifications, vous devez voir notre notification à distance :

[ ![Message de notification s’affiche.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png)

Félicitations, votre application a reçu sa première notification à distance.

Notez que GCM les messages seront ne sont plus reçus si l’application est arrêtée de force. Pour reprendre des notifications après un arrêt forcé, l’application doit être manuellement redémarré. Pour plus d’informations sur cette stratégie Android, consultez [lancer des contrôles dans les applications arrêtées](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) et [post de dépassement de capacité de la pile](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas détaillée les étapes d’implémentation des notifications à distance dans une application Xamarin.Android. Il décrit comment installer des packages supplémentaires nécessaires pour les communications de GCM, et elle explique comment configurer les autorisations d’application pour l’accès aux serveurs GCM. Il a fourni l’exemple de code qui illustre comment vérifier la présence de Services Google Play, comment implémenter un service d’inscription de tentatives et le service d’écoute ID instance qui négocie avec GCM pour un jeton d’inscription et comment implémenter un écouteur GCM service qui reçoit et traite les messages de notification à distance. Enfin, nous avons implémenté un programme de test de ligne de commande pour envoyer des notifications de test pour notre application cliente via GCM. 


## <a name="related-links"></a>Liens associés

- [GCM RemoteNotifications (exemple)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Messagerie cloud Google](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
