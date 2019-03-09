---
title: 'Procédure pas à pas : utilisation des Notifications locales dans Xamarin.Android'
description: Cette procédure pas à pas montre comment utiliser des notifications locales dans les applications Xamarin.Android. Il illustre les principes fondamentaux de création et publication d’une notification locale. Lorsque l’utilisateur clique sur la notification dans la zone de notification, il démarre une deuxième activité.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: e60ed6cc49921fc7b6e8e2616a6b0bf6f8abb401
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670088"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Procédure pas à pas : utilisation des notifications locales dans Xamarin.Android

_Cette procédure pas à pas montre comment utiliser des notifications locales dans les applications Xamarin.Android. Il illustre les principes fondamentaux de création et publication d’une notification locale. Lorsque l’utilisateur clique sur la notification dans la zone de notification, il démarre une deuxième activité._


## <a name="overview"></a>Vue d'ensemble

Dans cette procédure pas à pas, nous allons créer une application Android qui déclenche une notification lorsque l’utilisateur clique sur un bouton dans une activité. Lorsque l’utilisateur clique sur la notification, il lance une deuxième activité qui affiche le nombre de fois où l’utilisateur avait cliqué sur le bouton dans la première activité.

Les captures d’écran suivantes illustrent quelques exemples de cette application :

[![Captures d’écran de l’exemple avec notification](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> Ce guide se concentre sur la [NotificationCompat APIs](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) à partir de la [bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Ces API est assurer la valeur maximale compatibilité descendante pour Android 4.0 (API niveau 14).

## <a name="creating-the-project"></a>Création du projet

Pour commencer, nous allons créer un nouveau projet Android en utilisant le **application Android** modèle. Ce projet, nous allons appeler **LocalNotifications**. (Si vous n’êtes pas familiarisé avec la création de projets de Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

Modifier le fichier de ressources **values/Strings.xml** pour qu’il contienne deux ressources de type chaîne supplémentaire qui seront utilisées lorsqu’il est temps de créer le canal de notification :


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Ajoutez le package NuGet de Android.Support.V4

Dans cette procédure pas à pas, nous utilisons `NotificationCompat.Builder` pour générer notre notification locale. Comme expliqué dans [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md), nous devons inclure le [v4 de la bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet dans notre projet à utiliser `NotificationCompat.Builder`.

Ensuite, nous allons modifier **MainActivity.cs** et ajoutez le code suivant `using` instruction afin que les types dans `Android.Support.V4.App` sont disponibles pour notre code :

```csharp
using Android.Support.V4.App;
```

En outre, nous devons faire claires pour le compilateur que nous utilisons le `Android.Support.V4.App` version de `TaskStackBuilder` plutôt que `Android.App` version. Ajoutez le code suivant `using` instruction pour résoudre toute ambiguïté :

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>Créer le canal de notification

Ensuite, ajoutez une méthode à `MainActivity` qui créera un canal de notification (si nécessaire) :

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Mise à jour le `OnCreate` méthode à appeler cette nouvelle méthode :

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>Définir l’ID de notification

Nous aurons besoin d’un ID unique pour notre notification et le canal de notification. Nous allons modifier **MainActivity.cs** et ajoutez la variable d’instance statique suivante à la `MainActivity` classe :

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>Ajoutez du code pour générer la notification

Ensuite, nous devons créer un nouveau gestionnaire d’événements pour le bouton `Click` événement. Ajoutez la méthode suivante à `MainActivity`:

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

Le `OnCreate` méthode de MainActivity doit effectuer l’appel pour créer le canal de notification et affecter le `ButtonOnClick` méthode à la `Click` événements du bouton (à remplacer le Gestionnaire d’événements délégué fourni par le modèle) :

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```


### <a name="create-a-second-activity"></a>Créer une deuxième activité

Nous devons maintenant créer une autre activité Android affichera lorsque l’utilisateur clique sur notre notification. Ajouter une autre activité Android à votre projet appelé **SecondActivity**. Ouvrez **SecondActivity.cs** et remplacez son contenu par ce code :

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

Nous devons également créer une disposition de ressource pour **SecondActivity**. Ajouter un nouveau **disposition Android** fichier à votre projet appelé **Second.axml**. Modifier **Second.axml** et collez le code de mise en page suivante :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Ajouter une icône de notification

Enfin, ajoutez une petite icône qui s’affiche dans la zone de notification lorsque la notification est lancée. Vous pouvez copier [cette icône](local-notifications-walkthrough-images/ic-stat-button-click.png) à votre projet ou créer votre propre icône personnalisée. Nommez le fichier icône **ic\_stat\_bouton\_click.png** et copiez-le dans le **ressources/drawable** dossier. N’oubliez pas d’utiliser **Ajouter > élément existant...**  à inclure ce fichier d’icône dans votre projet.


### <a name="run-the-application"></a>Exécuter l'application

Générez et exécutez l’application. Doit s’afficher avec la première activité, similaire à la capture d’écran suivante :

[![Première capture d’écran activité](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Lorsque vous cliquez sur le bouton, vous devriez remarquer que la petite icône de notification apparaît dans la zone de notification :

[![Icône de notification s’affiche](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Si vous faites défiler vers le bas et exposez le tiroir de notification, vous devez voir la notification :

[![Message de notification](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Lorsque vous cliquez sur la notification, il doit disparaître, et nos autres activités doivent être lancées &ndash; recherche un peu comme la capture d’écran suivante :

[![Capture d’écran de deuxième activité](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Félicitations ! À ce stade vous avez terminé la procédure pas à pas Android notification locale et que vous avez un exemple fonctionnel que vous pouvez faire référence à. Il y a beaucoup plus aux notifications que nous avons montré ici, par conséquent, si vous souhaitez plus d’informations, jeter un œil [documentation de Google sur les notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html).


## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas utilisé `NotificationCompat.Builder` pour créer et afficher les notifications. Il vous a montré un exemple montrant comment démarrer une deuxième activité qui permet de répondre à une interaction utilisateur avec la notification de base, et il vous a expliqué le transfert de données à partir de la première activité à la deuxième activité.


## <a name="related-links"></a>Liens associés

- [LocalNotifications (sample)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Canaux de Notification Android Oreo](https://blog.xamarin.com/android-oreo-notification-channels/)
- [Notification](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
