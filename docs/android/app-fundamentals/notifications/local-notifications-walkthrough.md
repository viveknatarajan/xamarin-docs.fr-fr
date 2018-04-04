---
title: Procédure pas à pas - à l’aide de Notifications Local dans Xamarin.Android
description: Cette procédure pas à pas montre comment utiliser des notifications locales dans les applications de Xamarin.Android. Il illustre les principes fondamentaux de la création et la publication d’une notification locale. Lorsque l’utilisateur clique sur la notification dans la zone de notification, il démarre une deuxième activité.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Procédure pas à pas - à l’aide de Notifications Local dans Xamarin.Android

_Cette procédure pas à pas montre comment utiliser des notifications locales dans les applications de Xamarin.Android. Il illustre les principes fondamentaux de la création et la publication d’une notification locale. Lorsque l’utilisateur clique sur la notification dans la zone de notification, il démarre une deuxième activité._


## <a name="overview"></a>Vue d'ensemble

Dans cette procédure pas à pas, nous allons créer une application Android qui déclenche une notification lorsque l’utilisateur clique sur un bouton dans une activité. Lorsque l’utilisateur clique sur la notification, il lance une deuxième activité qui affiche le nombre de fois où l’utilisateur a cliqué sur le bouton de la première activité.

Les captures d’écran suivantes illustrent certains exemples de cette application :

[![Captures d’écran avec notification](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)



## <a name="walkthrough"></a>Procédure pas à pas

Pour commencer, nous allons créer un nouveau projet Android en utilisant le **application Android** modèle. Ce projet, nous allons appeler **LocalNotifications**. (Si vous n’êtes pas familiarisé avec la création de projets de Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)


### <a name="add-the-androidsupportv4app-component"></a>Ajouter le composant Android.Support.V4.App

Dans cette procédure pas à pas, nous utilisons `NotificationCompat.Builder` pour créer notre notification locale. Comme expliqué dans [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md), nous devons inclure le [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet dans notre projet à utiliser `NotificationCompat.Builder`.

Ensuite, nous allons modifier **MainActivity.cs** et ajoutez le code suivant `using` instruction afin que les types dans `Android.Support.V4.App` sont disponibles pour notre code :

```csharp
using Android.Support.V4.App;
```

En outre, nous devons faire clairement au compilateur que nous utilisons le `Android.Support.V4.App` version de `TaskStackBuilder` plutôt que `Android.App` version. Ajoutez le code suivant `using` instruction pour résoudre toute ambiguïté :

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```


### <a name="define-the-notification-id"></a>Définir l’ID de Notification

Nous devons un ID unique pour la notification. Permet de modifier **MainActivity.cs** et ajoutez la variable d’instance statique suivante à la `MainActivity` classe :

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```


### <a name="add-code-to-generate-the-notification"></a>Ajoutez du Code pour générer la Notification

Ensuite, nous avons besoin créer un gestionnaire d’événements pour le bouton `Click` événement. Ajoutez la méthode suivante à `MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

Dans le `OnCreate` méthode, assignez cet `ButtonOnClick` méthode à la `Click` événements du bouton (remplacez le Gestionnaire d’événements délégué fourni par le modèle) :

```csharp
button.Click += ButtonOnClick;
```


### <a name="create-a-second-activity"></a>Créer une deuxième activité

Nous devons maintenant créer une autre activité Android affichera lorsque l’utilisateur clique sur la notification. Ajouter une autre activité Android à votre projet appelé **SecondActivity**. Ouvrez **SecondActivity.cs** et remplacez son contenu par le code :

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
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
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
        android:id="@+id/textView" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Ajouter une icône de Notification

Enfin, nous allons ajouter une petite icône qui apparaît dans la zone de notification lorsque la notification est lancée. Vous pouvez copier [cette icône](local-notifications-walkthrough-images/ic-stat-button-click.png) à votre projet ou créer votre propre icône personnalisée. Nous allons nommer le fichier d’icône **ic\_stat\_bouton\_click.png** et copiez-le dans le **drawable/ressources** dossier. N’oubliez pas d’utiliser **Ajouter > élément existant...**  pour inclure ce fichier d’icône dans votre projet.


### <a name="run-the-application"></a>Exécution de l'application

Nous allons générer et exécuter l’application. S’affiche avec la première activité, semblable à la capture d’écran suivante :

[![Capture d’écran de première activité](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Lorsque vous cliquez sur le bouton, vous devez remarquer la petite icône de notification apparaît dans la zone de notification :

[![Icône de notification s’affiche.](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Si vous faites défiler vers le bas et exposez le tiroir de notification, vous devez voir la notification :

[![Message de notification](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Lorsque vous cliquez sur la notification, elle doit disparaître et autres notre activité doit être lancée &ndash; recherche similaire à la capture d’écran suivante :

[![Capture d’écran de deuxième activité](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Félicitations ! À ce stade, vous avez terminé la procédure pas à pas Android notification local et que vous disposez d’un exemple fonctionnel que vous pouvez faire référence à. Il y a beaucoup plus aux notifications que nous avons montré ici, par conséquent, si vous souhaitez plus d’informations, examinez [documentation de Google sur les notifications](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) et le Android [Notifications](http://developer.android.com/design/patterns/notifications.html) guide de conception.



## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons utilisé `NotificationCompat.Builder` pour créer et afficher les notifications. Nous avons vu comment démarrer une deuxième activité afin de répondre à une interaction utilisateur avec la notification d’un exemple de base, et nous l’avons démontré le transfert de données à partir de la première activité à la deuxième activité.


## <a name="related-links"></a>Liens associés

- [LocalNotifications (exemple)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Modèles de Guide de conception sur les Notifications](http://developer.android.com/design/patterns/notifications.html)
- [Notification](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
