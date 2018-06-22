---
title: Notifications à distance avec Firebase de cloud computing de messagerie
description: Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser la messagerie de Cloud Firebase pour implémenter des notifications à distance (également appelées des notifications push) dans une application Xamarin.Android. Il montre comment implémenter les classes différentes qui sont nécessaires pour les communications avec la messagerie de Cloud Firebase (FCM) et fournit des exemples montrant comment configurer le manifeste Android pour l’accès à FCM montre en aval de la messagerie à l’aide de la Firebase Console.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: e2f25504b971a0332dc51dc9b017c9c83222ec57
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044936"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Notifications à distance avec Firebase de cloud computing de messagerie

_Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser la messagerie de Cloud Firebase pour implémenter des notifications à distance (également appelées des notifications push) dans une application Xamarin.Android. Il montre comment implémenter les classes différentes qui sont nécessaires pour les communications avec la messagerie de Cloud Firebase (FCM) et fournit des exemples montrant comment configurer le manifeste Android pour l’accès à FCM montre en aval de la messagerie à l’aide de la Firebase Console._

## <a name="fcm-notifications-overview"></a>Vue d’ensemble des Notifications FCM

Dans cette procédure pas à pas, une application de base appelée **FCMClient** sera créé pour illustrer l’essentiel de la messagerie de FCM. **FCMClient** vérifie la présence de Services Google Play, reçoit les jetons de l’inscription des FCM, affiche des notifications à distance que vous envoyez à partir de la Console Firebase et s’abonne aux messages de la rubrique :

[![Capture d’écran de l’exemple d’application](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Les sujets suivants seront présentées :

1.  Notifications d’arrière-plan

2.  Messages de la rubrique

3.  Notifications de premier plan

Au cours de cette procédure pas à pas, vous ajouterez progressivement des fonctionnalités à **FCMClient** et l’exécuter sur un appareil ou un émulateur à comprendre comment il interagit avec FCM. Vous allez utiliser la journalisation afin de surveiller les transactions d’application en temps réel avec les serveurs FCM, et vous pourrez observer comment les notifications sont générées à partir des messages FCM que vous entrez dans l’interface utilisateur graphique de Notifications de la Console Firebase. 

## <a name="requirements"></a>Spécifications

Avant de poursuivre cette procédure pas à pas, vous devez obtenir les informations d’identification nécessaires pour utiliser des serveurs de Google FCM ; Ce processus est expliqué dans [Firebase de messagerie Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). En particulier, vous devez télécharger le **services.json de google** fichier à utiliser avec l’exemple de code présenté dans cette procédure pas à pas. Si vous n’avez pas encore créé un projet dans la Console Firebase (ou si vous n’avez pas encore téléchargé le **google-services.json** fichier), consultez [Firebase de messagerie Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Pour exécuter l’exemple d’application, vous devez un émulateur compatible avec Firebase ou un périphérique Android de test. Messagerie de Cloud firebase prend en charge les clients qui exécutent sur Android 2.3 (personnage en pain) ou version ultérieure, et ces appareils doivent également disposer le l’application de Google Play Store installée (Google Play Services 9.2.1 ou ultérieur est requis). Si vous n’avez pas encore de l’application de Google Play Store installée sur votre appareil, visitez le [Google Play](https://support.google.com/googleplay) site web pour télécharger et l’installer. Ou bien, vous pouvez utiliser l’émulateur Kit de développement logiciel Android exécutant Android 2.3 ou version ultérieure à la place d’un appareil de test (il est inutile d’installer le Google Play Store si vous utilisez l’émulateur Android SDK). 

## <a name="start-an-app-project"></a>Démarrer un projet d’application

Pour commencer, créez un nouveau projet Xamarin.Android vide nommé **FCMClient**. Si vous n’êtes pas familiarisé avec la création de projets de Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md). Après la création de la nouvelle application, l’étape suivante consiste à définir le nom du package et installer plusieurs packages NuGet qui seront utilisés pour la communication avec FCM. 

### <a name="set-the-package-name"></a>Définir le nom du Package

Dans [Firebase de messagerie Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), vous avez spécifié un nom de package pour l’application FCM compatible. Ce nom de package sert également le *ID d’application* qui est associé à la clé d’API. Configurer l’application pour utiliser ce nom de package :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Ouvrez les propriétés de la **FCMClient** projet. 

2.  Dans le **manifeste Android** , définissez le nom du package. 

Dans l’exemple suivant, le nom du package a la valeur `com.xamarin.fcmexample`: 

[![Définition du nom de package](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Lorsque vous mettez à jour le **manifeste Android**, également une vérification pour vous assurer que le `Internet` autorisation est activée. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Ouvrez les propriétés de la **FCMClient** projet. 

2.  Dans le **Application Android** , définissez le nom du package. 

Dans l’exemple suivant, le nom du package a la valeur `com.xamarin.fcmexample`: 

[![Définition du nom de package](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Lorsque vous mettez à jour le **manifeste Android**, également une vérification pour vous assurer que le `INTERNET` autorisation est activée (sous **autorisations requises**). 

-----

Notez que l’application cliente ne pourra pas recevoir un jeton d’inscription FCM n’est pas le cas de ce nom de package *exactement* correspond au nom de package qui a été entré dans la Console Firebase. 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Ajoutez le Package de Base Services Xamarin Google Play

Étant donné que de messagerie Cloud Firebase dépend des Services Google Play, le [Xamarin Services Google Play - Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) package NuGet doit être ajouté au projet Xamarin.Android. Vous devez version 29.0.0.2 ou version ultérieure.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Dans Visual Studio, cliquez sur **références > Gérer les Packages NuGet...** . 

2.  Cliquez sur le **Parcourir** onglet et recherchez **Xamarin.GooglePlayServices.Base**. 

3.  Installer ce package dans le **FCMClient** projet : 

    [![L’installation de Base des Services Google Play](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Dans Visual Studio pour Mac, cliquez sur **Packages > ajouter des Packages en cours...** . 

2.  Recherchez **Xamarin.GooglePlayServices.Base**. 

3.  Installer ce package dans le **FCMClient** projet : 

    [![L’installation de Base des Services Google Play](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Si vous obtenez une erreur pendant l’installation de NuGet, fermez le **FCMClient** de projet, ouvrez de nouveau et recommencez l’installation de NuGet. 

Lorsque vous installez **Xamarin.GooglePlayServices.Base**, toutes les dépendances nécessaires sont également installés. Modifier **MainActivity.cs** et ajoutez le code suivant `using` instruction : 

```csharp
using Android.Gms.Common;
```

Cette instruction effectue la `GoogleApiAvailability` classe dans **Xamarin.GooglePlayServices.Base** disponibles pour **FCMClient** code. 
`GoogleApiAvailability` est utilisé pour vérifier la présence de Services Google Play. 

### <a name="add-the-xamarin-firebase-messaging-package"></a>Ajouter le Xamarin Firebase Package de messagerie

Pour recevoir les messages FCM, le [Xamarin Firebase - messagerie](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) package NuGet doit être ajouté au projet d’application. Sans ce package, une application Android ne peut pas recevoir des messages à partir de serveurs FCM.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Dans Visual Studio, cliquez sur **références > Gérer les Packages NuGet...** . 

2.  Vérifiez **inclure la version préliminaire** et recherchez **Xamarin.Firebase.Messaging**. 

3.  Installer ce package dans le **FCMClient** projet : 

    [![Lors de l’installation Xamarin Firebase de messagerie](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Dans Visual Studio pour Mac, cliquez sur **Packages > ajouter des Packages en cours...** . 

2.  Vérifiez **afficher les packages de la version préliminaire** et recherchez **Xamarin.Firebase.Messaging**. 

3.  Installer ce package dans le **FCMClient** projet : 

    [![Lors de l’installation Xamarin Firebase de messagerie](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----
 
Lorsque vous installez **Xamarin.Firebase.Messaging**, toutes les dépendances nécessaires sont également installés.

Modifiez ensuite **MainActivity.cs** et ajoutez le code suivant `using` instructions :

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Les deux premières instructions rendre les types dans les **Xamarin.Firebase.Messaging** package NuGet disponible pour **FCMClient** code. **Android.Util** ajoute des fonctionnalités de journalisation qui seront utilisée pour observer les transactions avec FMS. 


### <a name="add-the-google-services-json-file"></a>Ajoutez le fichier JSON de Services Google

L’étape suivante consiste à ajouter la **services.json de google** dans le répertoire racine de votre projet : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Copie **services.json de google** dans le dossier du projet.

2.  Ajouter **google-services.json** au projet d’application (cliquez sur **afficher tous les fichiers** dans les **l’Explorateur de solutions**, cliquez droit **google-services.json**, puis sélectionnez **inclure dans le projet**). 

3.  Sélectionnez **google-services.json** dans les **l’Explorateur de solutions** fenêtre.

4.  Dans le **propriétés** volet, définissez la **Action de génération** à **GoogleServicesJson** (si le **GoogleServicesJson** action de génération n’est pas affichée, Enregistrez et fermez la Solution, puis le rouvrir) :

    [![Définition de l’action de génération pour GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Copie **services.json de google** dans le dossier du projet.

2.  Ajouter **services.json de google** au projet d’application. 

3.  Avec le bouton droit **google-services.json**.

4.  Définir le **Action de génération** à **GoogleServicesJson**: 

    [![Définition de l’action de génération pour GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)
 
-----
 

Lorsque **google-services.json** est ajouté au projet (et la **GoogleServicesJson** action de génération est définie), le processus de génération extrait la clé d’API et ID de client et qu’il ajoute ensuite ces informations d’identification pour la fusion / généré **AndroidManifest.xml** qui réside sur **obj/Debug/android/AndroidManifest.xml**. Ce processus de fusion ajoute automatiquement toutes les autorisations et autres éléments FCM qui sont nécessaires pour la connexion aux serveurs FCM. 


## <a name="check-for-google-play-services"></a>Recherchez les Services Google Play

Tout d’abord une disposition initiale pour l’interface utilisateur de l’application est créée. Modifier **Resources/layout/Main.axml** et remplacez son contenu par le code XML suivant : 

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

Cela `TextView` servira à afficher les messages qui indiquent si les Services Google Play est installé. Enregistrer les modifications apportées à **Main.axml**. Modifier **MainActivity.cs** et ajoutez la déclaration de variable instance suivante à la `MainActivity` classe : 

```csharp
TextView msgText;
```

Google recommande que les applications Android vérifier la présence de Google Play Services APK avant d’accéder aux fonctionnalités des Services Google Play (pour plus d’informations, consultez [vérifier les services Google Play](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)). Dans l’exemple suivant, la `OnCreate` méthode doit vérifier que les Services Google Play est disponible avant que l’application tente d’utiliser les services FCM. Ajoutez la méthode suivante à la `MainActivity` classe : 

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
            msgText.Text = "This device is not supported";
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

Ce code vérifie le périphérique pour voir si APK Google Play Services est installé. S’il n’est pas installé, un message s’affiche dans le `TextBox` qui demande à l’utilisateur à télécharger un fichier APK à partir de Google Play Store (ou pour l’activer dans les paramètres du périphérique système). 

Remplacez la méthode `OnCreate` par le code suivant : 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` est appelée à la fin de `OnCreate` afin que les Services Google Play vérifier s’exécute à chaque démarrage de l’application. Si votre application a une `OnResume` (méthode), il doit appeler `IsPlayServicesAvailable` de `OnResume` également. Complètement régénérer et exécuter l’application. Si tout est correctement configuré, vous devez voir un écran qui ressemble à la capture d’écran suivante : 

[![Application indique que les Services Google Play est disponible](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Si vous n’obtenez pas ce résultat, vérifiez que l’APK de Services Google Play est installé sur votre appareil (pour plus d’informations, consultez [paramètre des Services Google Play](https://developers.google.com/android/guides/setup)). Vérifiez également que vous avez ajouté le **Xamarin.Google.Play.Services.Base** le package à votre **FCMClient** projet comme expliqué précédemment.


## <a name="add-the-instance-id-receiver"></a>Ajouter le destinataire d’ID d’Instance

L’étape suivante consiste à ajouter un service qui étend `FirebaseInstanceIdService` pour gérer la création, la rotation et la mise à jour de jetons de l’inscription de Firebase. (Pour plus d’informations sur les jetons de l’inscription, consultez [inscription FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).) Le `FirebaseInstanceIdService` service est requis pour FCM pouvoir envoyer des messages à l’appareil. Lorsque la `FirebaseInstanceIdService` service est ajouté à l’application cliente, l’application sera automatiquement recevoir des messages FCM et les afficher sous forme de notifications à chaque fois que l’application est backgrounded. 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Déclarez le récepteur dans le manifeste Android

Modifier **AndroidManifest.xml** et insérez le code suivant `<receiver>` des éléments dans la `<application>` section : 

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Ce code XML effectue les opérations suivantes :

-   Déclare un `FirebaseInstanceIdReceiver` implémentation qui fournit un [identificateur unique](https://developers.google.com/instance-id/) pour chaque instance de l’application. Ce récepteur également authentifie et autorise des actions. 

-   Déclare un interne `FirebaseInstanceIdInternalReceiver` implémentation qui est utilisée pour démarrer les services en toute sécurité.

Le `FirebaseInstanceIdReceiver` est un `WakefulBroadcastReceiver` qui reçoit `FirebaseInstanceId` et `FirebaseMessaging` les événements et les remet à la classe que vous dérivez de `FirebaseInstanceIdService`. 

### <a name="implement-the-firebase-instance-id-service"></a>Implémenter le Service d’ID d’Instance Firebase

Le travail de l’inscription de l’application avec FCM est géré par la version personnalisée `FirebaseInstanceIdService` service que vous fournissez. 
`FirebaseInstanceIdService` effectue les étapes suivantes : 

1.  Utilise le [API de l’ID d’Instance](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) pour générer des jetons de sécurité qui autorisent l’application cliente pour accéder aux FCM et le serveur d’applications. En retour, l’application récupère l’inscription d’une jeton de FCM. 

2.  Transmet le jeton d’inscription pour le serveur d’applications si le serveur de l’application l’exige.

Ajouter un nouveau fichier appelé **MyFirebaseIIDService.cs** et remplacez son code de modèle avec les éléments suivants : 

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Ce service implémente un `OnTokenRefresh` méthode qui est appelée lorsque le jeton d’inscription est initialement créé ou modifié. Lorsque `OnTokenRefresh` s’exécute, il récupère le dernier jeton à partir de la `FirebaseInstanceId.Instance.Token` propriété (qui est mise à jour de façon asynchrone par FCM). Dans cet exemple, le jeton d’actualisation est enregistré afin qu’elle peut être affichée dans la fenêtre Sortie : 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` est appelé rarement : il est utilisé pour mettre à jour le jeton dans les circonstances suivantes : 

-   Lorsque l’application soit installée ou désinstallée.

-   Lorsque l’utilisateur supprime les données d’application. 

-   Lorsque l’application efface l’ID d’Instance.

-   Lorsque la sécurité du jeton a été compromise.

En fonction de Google [ID d’Instance](https://developers.google.com/instance-id/guides/android-implementation) documentation, le service de l’ID d’Instance FCM demande que l’application actualiser régulièrement son jeton (en règle générale, toutes les 6 mois). 

`OnTokenRefresh` appelle également `SendRegistrationToAppServer` pour associer l’inscription de l’utilisateur jeton avec le compte du côté serveur (le cas échéant) qui est géré par l’application : 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Étant donné que cette implémentation dépend de la conception du serveur d’application, un corps de méthode vide est fourni dans cet exemple. Si le serveur de votre application requiert des informations d’inscription FCM, modifiez `SendRegistrationToAppServer` pour associer un jeton d’ID de l’utilisateur FCM instance avec n’importe quel compte côté serveur géré par votre application. (Notez que le jeton est opaque à l’application cliente). 

Lorsqu’un jeton est envoyé vers le serveur d’applications, `SendRegistrationToAppServer` doivent conserver une valeur booléenne indiquant si le jeton a été envoyé au serveur. Si cette valeur est false, `SendRegistrationToAppServer` envoie le jeton vers le serveur d’applications &ndash; dans le cas contraire, le jeton a déjà été envoyé vers le serveur d’applications dans un appel précédent. Dans certains cas (tel que `FCMClient` exemple), le serveur d’applications n’a pas besoin du jeton ; par conséquent, cette méthode n’est pas requise pour cet exemple. 

## <a name="implement-client-app-code"></a>Implémentez le Code application cliente

Maintenant que les services de récepteur sont en place, code d’application client peut être écrites pour tirer parti de ces services. Dans les sections suivantes, un bouton est ajouté à l’interface utilisateur pour se connecter le jeton d’inscription (également appelé le *le jeton d’ID d’Instance*), et plus de code est ajouté à `MainActivity` pour afficher `Intent` informations lorsque l’application est lancée à partir d’un notification : 

[![Bouton de jeton de journal ajouté à l’écran de l’application](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>Jetons de journal

Le code ajouté à cette étape est destiné uniquement à des fins de démonstration &ndash; une application cliente de production n’a pas besoin pour se connecter à des jetons d’inscription. Modifier **Resources/layout/Main.axml** et ajoutez le code suivant `Button` déclaration immédiatement après le `TextView` élément : 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Modifier **MainActivity.cs** et ajoutez la déclaration de membre suivante à la `MainActivity` classe :

```csharp
const string TAG = "MainActivity";
```

Ajoutez le code suivant à la fin de la `OnCreate` méthode : 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Ce code enregistre le jeton actuel dans la fenêtre de sortie lorsque le **journal Jeton** bouton. 

### <a name="handle-notification-intents"></a>Gérer les modes de Notification

Lorsque l’utilisateur appuie sur une notification émise à partir de **FCMClient**, toutes les données qui accompagne cette notification message est mis à disposition dans `Intent` extras. Modifier **MainActivity.cs** et ajoutez le code suivant en haut de la `OnCreate` (méthode) (avant l’appel à `IsPlayServicesAvailable`) : 

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

Lancement de l’application `Intent` est déclenché lorsque l’utilisateur appuie sur son message de notification, donc ce code enregistrera les données associées le `Intent` dans la fenêtre Sortie. Si une autre `Intent` doit être déclenché, le `click_action` champ du message de notification doit être définie sur qui `Intent` (le service de lancement `Intent` est utilisé lorsqu’aucun `click_action` est spécifié). 


## <a name="background-notifications"></a>Notifications d’arrière-plan

Générez et exécutez le **FCMClient** application. Le **journal Jeton** bouton s’affiche :

[![Bouton de jeton de journal est affiché.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Appuyez sur la **journal Jeton** bouton. Un message semblable à celui-ci doit être affiché dans la fenêtre de sortie IDE : 

[![Jeton d’ID instance affiché dans la fenêtre Sortie](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

La chaîne longue étiqueté avec **jeton** est le jeton d’ID d’instance que vous allez coller dans la Console Firebase &ndash; sélectionner et copier cette chaîne dans le Presse-papiers. Si vous ne voyez pas un jeton d’ID instance, ajoutez la ligne suivante en haut de la `OnCreate` méthode pour vérifier que **services.json de google** a été analysée correctement :

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

Le `google_app_id` valeur enregistrée dans la fenêtre sortie doit correspondre à la `mobilesdk_app_id` valeur enregistrée dans **google-services.json**. 

### <a name="send-a-message"></a>Envoyer un Message

Se connecter à la [Firebase Console](https://console.firebase.google.com), sélectionnez votre projet, cliquez sur **Notifications**, puis cliquez sur **envoyer votre premier MESSAGE**: 

[![Bouton de votre premier Message envoyer](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Sur le **message message** page, entrez le texte du message et sélectionnez **seul périphérique**. Copiez le jeton d’ID de l’instance de la fenêtre de sortie IDE et collez-la dans la **jeton d’inscription FCM** champ de la Console Firebase : 

[![Composer la boîte de dialogue de message](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Sur l’appareil Android (ou l’émulateur), l’application d’arrière-plan en touchant le Android **vue d’ensemble** bouton et toucher l’écran d’accueil. Quand l’appareil est prêt, cliquez sur **envoyer un MESSAGE** dans la Console Firebase : 

[![Envoyer des messages, bouton](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Lorsque le **message révision** boîte de dialogue s’affiche, cliquez sur **envoyer**.
L’icône de notification doit apparaître dans la zone de notification de l’appareil (ou l’émulateur) : 

[![Icône de notification s’affiche.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Ouvrez l’icône de notification pour afficher le message. Le message de notification doit être exactement ce qui a été tapé dans le **texte du Message** champ de la Console Firebase : 

[![Message de notification s’affiche sur l’appareil](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Appuyez sur l’icône de notification pour revenir à la **FCMClient** application. Le `Intent` extras envoyés à **FCMClient** sont répertoriés dans la fenêtre de sortie IDE : 

[![Listes d’intention de fonctionnalités supplémentaires à partir de la clé, ID de message et réduire la clé](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

Dans cet exemple, le **de** clé est définie pour le numéro de projet Firebase de l’application (dans cet exemple, `41590732`) et le **collapse_key** est définie sur son nom de package ( **com.xamarin.fcmexample**). Si vous ne recevez pas un message, essayez de supprimer le **FCMClient** application sur l’appareil (ou l’émulateur) et répétez les étapes ci-dessus. 


> [!NOTE]
> Si vous forcer à fermer l’application, FCM s’arrête de livraison des notifications. Android empêche les diffusions de service en arrière-plan de lancer les composants d’applications arrêtées par inadvertance ou inutilement. (Pour plus d’informations sur ce comportement, consultez [lancer des contrôles dans les applications arrêtées](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Pour cette raison, il est nécessaire de désinstaller manuellement l’application à chaque fois que vous l’exécuter et l’arrêter à partir d’une session de débogage &ndash; cela force FCM pour générer un nouveau jeton afin que les messages continuent à être reçus.

### <a name="add-a-custom-default-notification-icon"></a>Ajouter une icône de Notification personnalisée par défaut

Dans l’exemple précédent, l’icône de notification est défini sur l’icône de l’application. Le code XML suivant configure une icône personnalisée par défaut pour les notifications. Android affiche cette icône personnalisée par défaut pour tous les messages de notification lorsque l’icône de notification n’est pas définie explicitement. 

Pour ajouter une icône de notification personnalisée par défaut, ajoutez l’icône pour le **drawable/ressources** active, modifier **AndroidManifest.xml**et insérez le code suivant `<meta-data>` élément dans le `<application>`section : 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

Dans cet exemple, l’icône de notification qui réside sur **ressources/drawable/ic\_stat\_ic\_notification.png** sera utilisé en tant que l’icône de notification personnalisée par défaut. Si une icône par défaut personnalisée n’est pas configurée dans **AndroidManifest.xml** et aucune icône n’est définie dans la charge utile de notification, Android utilise l’icône de l’application en tant que l’icône de notification (comme indiqué dans la notification icône capture d’écran ci-dessus). 

## <a name="handle-topic-messages"></a>Gérer les Messages de la rubrique

Le code écrit jusqu’ici gère les jetons de l’inscription et ajoute la fonctionnalité de notification à distance à l’application. L’exemple suivant ajoute le code qui écoute les *messages de rubrique* et les transfère à l’utilisateur de notifications. Messages de la rubrique sont FCM qui sont envoyées à un ou plusieurs périphériques s’abonner à une rubrique particulière. Pour plus d’informations sur les messages de la rubrique, consultez [rubrique messagerie](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

### <a name="subscribe-to-a-topic"></a>S’abonner à une rubrique

Modifier **Resources/layout/Main.axml** et ajoutez le code suivant `Button` déclaration immédiatement après la précédente `Button` élément :

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Ce code XML ajoute un **s’abonner aux notifications** bouton à la disposition.
Modifier **MainActivity.cs** et ajoutez le code suivant à la fin de la `OnCreate` méthode : 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Ce code localise le **s’abonner aux notifications** bouton dans la disposition et l’affecte ses événements Click au code qui appelle `FirebaseMessaging.Instance.SubscribeToTopic`, en passant dans la rubrique abonnement _news_. Lorsque l’utilisateur appuie sur le **s’abonner** bouton, l’application s’abonne à la _news_ rubrique. Dans la section suivante, un _news_ rubrique message sera envoyé à partir de l’interface utilisateur graphique de Notifications de la Console Firebase. 

### <a name="send-a-topic-message"></a>Envoyer un Message de rubrique

Désinstallez l’application, régénérer, puis exécuter de nouveau. Cliquez sur le **s’abonner aux Notifications** bouton :

[![S’abonner à un bouton de Notifications](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Si l’application est abonnée avec succès, vous devez voir **a réussi la synchronisation rubrique** dans l’IDE de la fenêtre Sortie : 

[![Fenêtre Sortie affiche le message de synchronisation réussie de rubrique](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Utilisez les étapes suivantes pour envoyer un message de la rubrique :

1.  Dans la Console Firebase, cliquez sur **nouveau MESSAGE**. 

2.  Sur le **message message** page, entrez le texte du message et sélectionnez **rubrique**. 

3.  Dans le **rubrique** menu déroulant, sélectionnez la rubrique intégrée, **news**: 

    [![Sélection de la rubrique de news](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Sur l’appareil Android (ou l’émulateur), l’application d’arrière-plan en touchant le Android **vue d’ensemble** bouton et toucher l’écran d’accueil. 

5.  Quand l’appareil est prêt, cliquez sur **envoyer un MESSAGE** dans la Console Firebase. 

6.  Consultez la fenêtre de sortie IDE pour voir **/rubriques/news** dans la sortie du journal : 

    [![À partir de /topic/news est signalée](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Lorsque ce message est visible dans la fenêtre Sortie, l’icône de notification doit également apparaître dans la zone de notification sur l’appareil Android. Ouvrez l’icône de notification pour afficher le message de la rubrique : 

[![Le message de la rubrique s’affiche en tant que notification](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Si vous ne recevez pas un message, essayez de supprimer le **FCMClient** application sur l’appareil (ou l’émulateur) et répétez les étapes ci-dessus. 

## <a name="foreground-notifications"></a>Notifications de premier plan

Pour recevoir des notifications dans les applications foregrounded, vous devez implémenter `FirebaseMessagingService`. Ce service est également requis pour la réception des charges utiles de données et pour envoyer des messages en amont. Les exemples suivants illustrent comment implémenter un service qui étend `FirebaseMessagingService` &ndash; l’application résultante sera en mesure de gérer à distance notifications pendant son exécution au premier plan. 

### <a name="implement-firebasemessagingservice"></a>Implémenter FirebaseMessagingService

Le `FirebaseMessagingService` service inclut un `OnMessageReceived` méthode que vous écrivez pour gérer les messages entrants. Notez que `OnMessageReceived` est invoqué pour les messages de notification *uniquement* quand l’application s’exécute en premier plan. Lorsque l’application s’exécute en arrière-plan, une notification générée automatiquement s’affiche (comme illustré précédemment dans cette procédure pas à pas). 

Ajouter un nouveau fichier appelé **MyFirebaseMessagingService.cs** et remplacez son code de modèle avec les éléments suivants : 

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Notez que la `MESSAGING_EVENT` intention filtre doit être déclaré afin que les nouveaux messages FCM sont dirigées vers `MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Lorsque l’application cliente reçoit un message FCM, `OnMessageReceived` extrait le contenu du message à partir du passé `RemoteMessage` objet en appelant son `GetNotification` (méthode). Ensuite, il enregistre le contenu du message afin qu’elle peut être affichée dans la fenêtre de sortie IDE : 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> Si vous définissez des points d’arrêt dans `FirebaseMessagingService`, votre session de débogage peut ou peut ne pas toucher ces points d’arrêt en raison de la façon dont FCM remet les messages.
 

### <a name="send-another-message"></a>Envoyer un autre Message

Désinstallez l’application, reconstruire, exécutez-le à nouveau et suivez ces étapes pour envoyer un autre message :

1.  Dans la Console Firebase, cliquez sur **nouveau MESSAGE**. 

2.  Sur le **message message** page, entrez le texte du message et sélectionnez **seul périphérique**. 

3.  Copier la chaîne de jeton à partir de la fenêtre de sortie IDE et collez-la dans la **jeton d’inscription FCM** champ de la Console Firebase comme avant. 

4.  Assurez-vous que l’application s’exécute au premier plan, puis cliquez sur **envoyer un MESSAGE** dans la Console Firebase : 

    [![Envoi d’un autre message à partir de la Console](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Lorsque le **message révision** boîte de dialogue s’affiche, cliquez sur **envoyer**.

6.  Le message entrant est enregistré dans la fenêtre de sortie IDE :

    [![Corps du message est imprimé dans une fenêtre de sortie](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notifications-sender"></a>Ajouter un expéditeur Notifications Local

Dans cet exemple restant, le message entrant FCM sera converti en une notification locale qui est lancée alors que l’application s’exécute au premier plan. Modifier **MyFirebaseMessageService.cs** et ajoutez le code suivant `using` instructions : 

```csharp
using FCMClient;
using System.Collections.Generic;
```

Ajoutez la méthode suivante à `MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

Pour distinguer cette notification à partir des notifications de l’arrière-plan, ce code marque des notifications avec une icône qui diffère de l’icône applicatiion. Ajouter [ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) à **drawable/ressources** et l’inclure dans le **FCMClient** projet. 

Le `SendNotification` utilise `Notification.Builder` pour créer la notification, et `NotificationManager` est utilisé pour lancer la notification. La notification conserve un `PendingIntent` qui permet à l’utilisateur ouvrir l’application et afficher le contenu de la chaîne passée dans `messageBody`. Selon les versions d’Android que vous souhaitez cibler avec votre application, il pouvez que vous souhaitez utiliser `NotificationCompat.Builder` à la place. Pour plus d’informations sur `Notification.Builder`, consultez [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md). 

Appelez le `SendNotification` méthode à la fin de la `OnMessageReceived` méthode : 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

À la suite de ces modifications, `SendNotification` s’exécutera chaque fois qu’une notification est reçue pendant que l’application est au premier plan et la notification s’affiche dans la zone de notification. Si l’application est backgrounded, `SendNotification` ne s’exécute pas et qui, au lieu de cela, sera lancée une notification en arrière-plan (illustrée plus haut dans cette procédure pas à pas). 

### <a name="send-the-last-message"></a>Envoyer le dernier Message

Désinstaller l’application, régénérer, exécutez-le à nouveau, puis procédez comme suit pour envoyer le dernier message :

1.  Dans la Console Firebase, cliquez sur **nouveau MESSAGE**. 

2.  Sur le **message message** page, entrez le texte du message et sélectionnez **seul périphérique**. 

3.  Copier la chaîne de jeton à partir de la fenêtre de sortie IDE et collez-la dans la **jeton d’inscription FCM** champ de la Console Firebase comme avant. 

4.  Assurez-vous que l’application s’exécute au premier plan, puis cliquez sur **envoyer un MESSAGE** dans la Console Firebase : 

    [![Envoi du message de premier plan](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Cette fois, le message a été enregistré dans la fenêtre sortie est également empaqueté dans une nouvelle notification &ndash; l’icône de notification apparaît dans la barre d’état de notification pendant l’exécution de l’application au premier plan : 

[![Icône de notification pour le message de premier plan](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Lorsque vous ouvrez la notification, vous devez voir le dernier message qui a été envoyé à partir de l’interface utilisateur graphique de Notifications de la Console Firebase : 

[![Notification de premier plan indiquée avec une icône de premier plan](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>Déconnecter les FCM

Pour annuler l’abonnement à une rubrique, appelez le [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) méthode sur le [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) classe. Par exemple, pour annuler l’abonnement à la _news_ rubrique abonné précédemment, à une **Unsubscribe** bouton peut être ajouté à la mise en page avec le code de gestionnaire suivant :

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Pour annuler l’inscription de l’appareil à partir de purement et simplement FCM, supprimez l’ID d’instance en appelant le [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) méthode sur le [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) classe. Par exemple :

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Cet appel de méthode supprime l’ID d’instance et les données associées. Par conséquent, l’envoi périodiques de données FCM à l’appareil est interrompue.

 
## <a name="troubleshooting"></a>Résolution des problèmes

Les problèmes de décrire et solutions de contournement qui peuvent survenir lors de l’utilisation de la messagerie de Cloud Firebase avec Xamarin.Android suivants.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp n’est pas initialisé.

Dans certains cas, vous pouvez voir ce message d’erreur :

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Il s’agit d’un problème connu que vous pouvez contourner par la solution de nettoyage et de régénérer le projet (**Générer > Nettoyer la Solution**, **Générer > Régénérer la Solution**). Pour plus d’informations, consultez ce [forum de discussion](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas détaillée les étapes d’implémentation de messagerie Cloud Firebase des notifications à distance dans une application Xamarin.Android. Il décrit comment installer les packages requis nécessaires pour les communications FCM, et elle explique comment configurer le manifeste Android pour l’accès aux serveurs FCM. Il fourni d’exemple de code qui montre comment vérifier la présence de Services Google Play. Il vous a montré comment implémenter un service d’écoute ID instance qui négocie avec FCM pour un jeton d’inscription, et elle explique comment ce code crée les notifications en arrière-plan pendant que l’application est backgrounded. Il explique comment s’abonner aux messages de la rubrique, et il fourni un exemple d’implémentation d’un service de récepteur de message qui est utilisé pour recevoir et afficher des notifications à distance pendant l’exécution de l’application au premier plan. 


## <a name="related-links"></a>Liens associés

- [FCMNotifications (exemple)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Messagerie cloud Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
