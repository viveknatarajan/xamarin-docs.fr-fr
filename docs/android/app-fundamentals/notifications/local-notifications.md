---
title: Notifications locales
description: Cette section montre comment implémenter des notifications locales dans Xamarin.Android. Il explique les différents éléments d’interface utilisateur d’une notification d’Android et traite des API d’impliquée dans la création et affichage d’une notification.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 97c8372656f0cbfa5b8f7bb12d15b00feac4b5c3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="local-notifications"></a>Notifications locales

_Cette section montre comment implémenter des notifications locales dans Xamarin.Android. Il explique les différents éléments d’interface utilisateur d’une notification d’Android et traite des API d’impliquée dans la création et affichage d’une notification._

## <a name="local-notifications-overview"></a>Vue d’ensemble des Notifications local

Cette rubrique explique comment implémenter des notifications locales dans une application de Xamarin.Android. Il décrit les différentes parties d’une notification d’Android, il explique les styles de notification différents sont disponibles pour les développeurs d’applications et il présente certains des API qui sont utilisés pour créer et publier des notifications.

Android fournit deux zones contrôlés par le système pour afficher les icônes de notification et les informations de notification à l’utilisateur. Lorsqu’une notification est tout d’abord publiée, son icône s’affiche dans le *zone de notification*, comme illustré dans la capture d’écran suivante :

![Zone de notification d’exemple sur un appareil](local-notifications-images/01-notification-shade.png)

Pour obtenir des informations sur la notification, l’utilisateur peut ouvrir le tiroir de notification (qui se développe chaque icône de notification pour afficher le contenu de la notification) et effectuer toutes les actions associées aux notifications. L’écran suivant capture montre un *tiroir de notifications* qui correspond à la zone de notification affichée au-dessus :

[![Tiroir de notifications exemple afficher les trois notifications](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Les notifications Android utilisent deux types de disposition :

-   ***Disposition de base*** &ndash; un format de présentation compact, fixe.

-   ***Disposition de développé*** &ndash; un format de présentation qui peut se développer pour une plus grande taille pour obtenir plus d’informations.

Chacun de ces types de mise en page (et comment les créer) sont expliquées dans les sections suivantes.


### <a name="base-layout"></a>Disposition de base

Toutes les notifications Android reposent sur le format de mise en page de base, qui, au minimum, inclut les éléments suivants :

1.  A *icône de notification*, qui représente l’application d’origine, ou si le type de notification si l’application prend en charge différents types de notifications.

2.  La notification *titre*, ou le nom de l’expéditeur si la notification est un message personnel.

3.  Le message de notification.

4.  A *timestamp*.

Ces éléments sont affichent comme illustré dans le diagramme suivant :

[![Emplacement des éléments de notification](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Dispositions de base sont limitées à 64 indépendant de la densité de pixels (dp) en hauteur. Android crée ce style de notification de base par défaut.

Le cas échéant, les notifications peuvent s’affichent une grande icône qui représente l’application ou une photo de l’expéditeur. Lorsqu’une grande icône est utilisée dans une notification dans Android 5.0 et versions ultérieures, l’icône de notification de petits s’affiche comme un badge sur l’icône de grande taille :

![Photo de notification simple](local-notifications-images/04-simple-notification-photo.png)

À partir de Android 5.0, les notifications peuvent également apparaître sur l’écran de verrouillage :

[![Notification de l’écran de verrouillage d’exemple](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

L’utilisateur peut double-cliquer la notification de l’écran de verrouillage pour déverrouiller l’appareil et accéder à l’application qui a créé cette notification, ou effectuez un balayage à ignorer les notifications. Les applications peuvent affecter le niveau de visibilité d’une notification pour contrôler ce qui est affiché sur l’écran de verrouillage, et les utilisateurs peuvent choisir s’il faut autoriser le contenu à afficher dans l’écran de verrouillage notifications sensibles.

Android 5.0 introduit un format de présentation de notification de haute priorité appelé *frontal*. Notifications de tête haute glissement vers le bas à partir du haut de l’écran pendant quelques secondes et puis reformatage sauvegarder sur la zone de notification :

[![Notification profondes d’exemple](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Notifications de tête haute rendent possible pour le système de l’interface utilisateur pour placer des informations importantes devant l’utilisateur sans interrompre l’état de l’activité en cours d’exécution.

Android prend en charge des métadonnées des notifications afin que les notifications peuvent être triées et affichées intelligemment. Métadonnées des notifications de contrôle également comment les notifications s’affichent sur l’écran de verrouillage et au format de tête haute. Les applications peuvent définir les types suivants de métadonnées de la notification :

-   **Priorité** &ndash; le niveau de priorité détermine quand et comment les notifications s’affichent. Par exemple, dans Android 5.0, les notifications de haute priorité sont affichées sous forme de notifications de tête haute.

-   **Visibilité** &ndash; spécifie la quantité de contenu notification doit être affiché lorsque la notification s’affiche sur l’écran de verrouillage.

-   **Catégorie** &ndash; indique au système comment gérer la notification dans différentes circonstances, par exemple quand l’appareil est dans *ne pas perturber* mode.

**Remarque :** **visibilité** et **catégorie** ont été introduits dans Android 5.0 et ne sont pas disponibles dans les versions antérieures d’Android. Compter d’Android 8.0, [canaux de notification](#notif-chan) permettent de contrôler la façon dont les notifications sont présentées à l’utilisateur.


### <a name="expanded-layouts"></a>Dispositions de développé

À compter d’Android 4.1, les notifications peuvent être configurées avec des styles de disposition développé permettant à l’utilisateur développer la hauteur de la notification pour afficher davantage de contenu. Par exemple, l’exemple suivant illustre une notification de mise en page développée en mode conventionnel :

![Notification conventionnel](local-notifications-images/07-contracted-notification.png)

Cette notification est développée, elle révèle la totalité du message :

![Étendue de notification](local-notifications-images/08-expanded-notification.png)

Android prend en charge trois styles de disposition de développé pour les notifications d’événement unique :

-   ***Texte Big*** &ndash; en mode conventionnel, affiche un extrait de la première ligne du message suivi par deux points. En mode développé, affiche le message entier (comme indiqué dans l’exemple ci-dessus).

-   ***La boîte de réception*** &ndash; en mode conventionnel, affiche le nombre de nouveaux messages. En mode développé, affiche le premier message de courrier électronique ou une liste des messages dans la boîte de réception.

-   ***Image*** &ndash; en mode conventionnel, affiche uniquement le texte du message. En mode développé, affiche le texte et une image.

[Au-delà de la Notification de base](#beyond-the-basic-notification) (plus loin dans cet article) explique comment créer *texte Big*, *boîte de réception*, et *Image* des notifications.


## <a name="notification-creation"></a>Création de notification

Pour créer une notification dans Android, vous utilisez la [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) classe. `Notification.Builder` a été introduite dans Android 3.0 pour simplifier la création d’objets de notification. Pour créer des notifications qui sont compatibles avec les versions antérieures d’Android, vous pouvez utiliser la [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) classe au lieu de `Notification.Builder` (consultez [compatibilité](#compatibility) plus loin dans cette rubrique pour plus d’informations sur l’utilisation de `NotificationCompat.Builder`).
`Notification.Builder` Fournit des méthodes pour définir les différentes options dans une notification, telles que :

-   Le contenu, y compris le titre, le texte du message et l’icône de notification.

-   Le style de la notification, tel que *texte Big*, *boîte de réception*, ou *Image* style.

-   La priorité de la notification : au minimum, faible, par défaut, haute ou maximum.

-   La visibilité de la notification sur l’écran de verrouillage : public, privé ou secret.

-   Métadonnées de catégorie qui vous aide à Android classer et filtrer la notification.

-   Une intention facultatif qui indique une activité à lancer lorsque l’utilisateur clique sur la notification.

Après avoir défini ces options dans le générateur, vous générez un objet de notification qui contient les paramètres. Pour publier la notification, vous passez cet objet de notification pour le *Gestionnaire de Notification*. Android fournit le [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) (classe), qui est responsable de la publication des notifications et les afficher à l’utilisateur. Une référence à cette classe peut être obtenue à partir de n’importe quel contexte, tel qu’une activité ou un service.


### <a name="how-to-generate-a-notification"></a>Comment générer une Notification

Pour générer une notification dans Android, procédez comme suit :

1.  Instancier une `Notification.Builder` objet.

2.  Appeler des méthodes différentes sur le `Notification.Builder` objet pour définir les options de notification.

3.  Appelez le [générer](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) méthode de la `Notification.Builder` objet pour instancier un objet de notification.

4.  Appelez le [notifier](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) méthode du Gestionnaire de notification pour la notification de publication.

Vous devez fournir au moins les informations suivantes pour chaque notification :

-   Une petite icône (24 x 24 dp taille)

-   Un titre court

-   Le texte de la notification

L’exemple de code suivant illustre l’utilisation de `Notification.Builder` pour générer une notification de base. Notez que `Notification.Builder` prend en charge les méthodes [le chaînage de méthode](http://en.wikipedia.org/wiki/Method_chaining); autrement dit, chaque méthode retourne l’objet générateur afin de pouvoir utiliser le résultat de la dernière méthode à appeler l’appel de méthode suivant :

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Dans cet exemple, un nouveau `Notification.Builder` objet appelé `builder` est instancié, le titre et le texte de la notification sont définies, et l’icône de notification est chargé à partir de **Resources/drawable/ic_notification.png**. L’appel au Générateur de notification `Build` méthode crée un objet de notification avec ces paramètres. L’étape suivante consiste à appeler la `Notify` méthode du Gestionnaire de notification. Pour obtenir le Gestionnaire de notification, vous appelez `GetSystemService`, comme indiqué ci-dessus.

Le `Notify` méthode accepte deux paramètres : l’identificateur de notification et l’objet de notification. L’identificateur de la notification est un entier unique qui identifie la notification à votre application. Dans cet exemple, l’identificateur de notification a la valeur zéro (0) ; Toutefois, dans une application de production, vous pouvez donner à chaque notification un identificateur unique. Réutilisation de la précédente valeur d’identificateur dans un appel à `Notify` provoque la dernière notification doivent être remplacées.

Lorsque ce code s’exécute sur un appareil Android 5.0, il génère une notification qui ressemble à l’exemple suivant :

![Résultat de notification pour l’exemple de code](local-notifications-images/09-hello-world.png)

L’icône de notification s’affiche sur le côté gauche de la notification &ndash; cette image d’un CERCLÉE &ldquo;i&rdquo; comporte un canal alpha qui Android peut dessiner un fond gris circulaire derrière lui. Vous pouvez également fournir une icône sans un canal alpha. Pour afficher une photo sous forme d’icône, consultez [grand Format icône](#large-icon-format) plus loin dans cette rubrique.

L’horodatage est défini automatiquement, mais vous pouvez remplacer ce paramètre en appelant le [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) méthode du Générateur de notification. Par exemple, l’exemple de code suivant définit l’horodatage à l’heure actuelle :

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Vibration et son activation

Si vous souhaitez que votre notification également émettre un signal sonore, vous pouvez appeler le Générateur de notification [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) (méthode) et passez le `NotificationDefaults.Sound` indicateur :

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Cet appel à `SetDefaults` fait que l’appareil lire un son lorsque la notification est publiée. Si vous voulez que l’appareil faire vibrer au lieu d’émettre un signal sonore, vous pouvez passer `NotificationDefaults.Vibrate` à `SetDefaults.` si vous souhaitez que l’appareil pour émettre un signal sonore, faire vibrer l’appareil, vous pouvez passer les deux indicateurs à `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Si vous activez son sans spécifier un son à lire, Android utilise le son de notification du système par défaut. Toutefois, vous pouvez modifier le son est lu en appelant le Générateur de notification [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) (méthode). Par exemple, pour lire l’alarme audio avec votre notification (au lieu du son de notification par défaut), vous pouvez obtenir l’URI de l’alarme sonore à partir de la [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) et passez-la à `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Vous pouvez également utiliser la sonnerie par défaut système audio pour votre notification :

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Après avoir créé un objet de notification, il est possible de définir les propriétés de notification sur l’objet de notification (plutôt que de les configurer à l’avance via `Notification.Builder` méthodes). Par exemple, au lieu d’appeler le `SetDefaults` méthode permettant de vibration sur une notification, vous pouvez modifier directement l’indicateur de bit de la notification [par défaut est](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) propriété :

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Cet exemple montre comment l’appareil à faire vibrer lorsque la notification est publiée.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Une Notification de mise à jour

Si vous souhaitez mettre à jour le contenu d’une notification une fois qu’il a été publié, vous pouvez réutiliser existants `Notification.Builder` objet pour créer un nouvel objet de notification et de publier cette notification avec l’identificateur de la dernière notification. Par exemple :

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

Dans cet exemple, existants `Notification.Builder` objet est utilisé pour créer un nouvel objet de notification avec un autre titre et un message.
Le nouvel objet de notification est publié à l’aide de l’identificateur de la notification précédente, et cela met à jour le contenu de la notification précédemment publiée :

![Notification de mise à jour](local-notifications-images/12-updated-notification.png)

Le corps de la notification précédente est réutilisé &ndash; que le titre et le texte de la notification change lorsque la notification est affichée dans le tiroir de notification. Le texte du titre change à partir de « Exemple de Notification » à « Notification de mise à jour » et le texte du message remplace « Hello World ! Voici ma première notification ! » à « Changé à ce message. »

Une notification reste visible jusqu'à ce qu’un des trois choses se produit :

-   L’utilisateur ferme la notification (ou appuie sur *Effacer tout*).

-   L’application effectue un appel à `NotificationManager.Cancel`, en passant l’ID unique de notification qui a été affecté lors de la notification a été publiée.

-   L’application appelle `NotificationManager.CancelAll`.

Pour plus d’informations sur la mise à jour aux notifications Android, consultez [modifier une Notification](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Démarrage d’une activité à partir d’une Notification

Dans Android, il est courant pour une notification à associer à un *action* &ndash; une activité qui s’affiche lorsque l’utilisateur appuie sur la notification. Cette activité peut résider dans une autre application ou même dans une autre tâche. Pour ajouter une action à une notification, vous créez un [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) de l’objet et associer le `PendingIntent` avec la notification. A `PendingIntent` est un type spécial d’intention qui permet à l’application destinataire exécuter une partie prédéfinie de code avec les autorisations de l’application émettrice. Lorsque l’utilisateur appuie sur la notification, Android démarre l’activité spécifiée par le `PendingIntent`.

L’extrait de code suivant montre comment créer une notification avec un `PendingIntent` qui va lancer l’activité de l’application d’origine, `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Ce code est très similaire au code de notification dans la section précédente, à ceci près qu’un `PendingIntent` est ajouté à l’objet de notification. Dans cet exemple, le `PendingIntent` est associé à l’activité de l’application d’origine avant d’être passé au Générateur de notification [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) (méthode). Le `PendingIntentFlags.OneShot` indicateur est passé à la `PendingIntent.GetActivity` méthode afin que le `PendingIntent` est utilisé qu’une seule fois. Lorsque ce code s’exécute, la notification suivante s’affiche :

![Première notification d’action](local-notifications-images/10-first-action-notification.png)

En appuyant sur cette notification est l’utilisateur vers l’activité d’origine.

Dans une application de production, votre application doit gérer les *pile back* lorsque l’utilisateur appuie sur le **précédent** bouton dans l’activité de notification (si vous n’êtes pas familiarisé avec les tâches Android et la pile de retour, voir [ Tâches et la pile Back](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
Dans la plupart des cas, la navigation vers l’arrière en dehors de l’activité de notification doit retourner l’utilisateur à partir de l’application et revenir à l’écran d’accueil. Pour gérer la pile back, votre application utilise le [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) classe pour créer un `PendingIntent` avec une pile back.

Une autre considération réelle est que l’activité d’origine ont besoin d’envoyer des données à l’activité de notification. Par exemple, la notification peut indiquer qu’un message est arrivé, et que l’activité de notification (message affichage écran), requiert l’ID du message à afficher le message à l’utilisateur. L’activité qui crée le `PendingIntent` pouvez utiliser la [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) méthode pour ajouter des données (par exemple, une chaîne) à l’intention afin que ces données sont passées à l’activité de notification.

L’exemple de code suivant illustre l’utilisation de `TaskStackBuilder` pour gérer la pile de retour et il inclut un exemple d’envoi d’une chaîne de message unique à une activité de notification appelée `SecondActivity`:

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Dans cet exemple de code, l’application se compose de deux activités : `MainActivity` (qui contient le code de la notification ci-dessus), et `SecondActivity`, l’écran de l’utilisateur voit après avoir appuyé sur la notification. Lorsque ce code est exécuté, une notification simple (semblable à l’exemple précédent) est présentée. Appuyez sur la notification dirige l’utilisateur vers le `SecondActivity` écran :

![Capture d’écran de deuxième activité](local-notifications-images/11-second-activity.png)

Le message de type chaîne (passé dans le but `PutExtra` méthode) est récupérée dans `SecondActivity` via cette ligne de code :

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Ce message récupéré, « Greetings de MainActivity ! », s’affiche dans le `SecondActivity` écran, comme illustré dans la capture d’écran ci-dessus. Lorsque l’utilisateur appuie sur le **précédent** bouton lors de `SecondActivity`, navigation mène hors de l’application et revenir à l’écran précédant le lancement de l’application.

Pour plus d’informations sur la création en attente intentions, consultez [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan" />

## <a name="notification-channels"></a>Canaux de notification

À compter d’Android 8.0 (Oreo), vous pouvez utiliser la *canaux de notification* fonctionnalité pour créer un canal personnalisables par l’utilisateur pour chaque type de notification que vous souhaitez afficher. Canaux de notification permettent à vous pour les notifications de groupe afin que toutes les notifications publié dans une exposition de canal, le même comportement. Par exemple, vous pouvez avoir un canal de notification qui est destiné à des notifications qui requièrent une attention immédiate et un canal distinct et « silencieux » qui est utilisé pour les messages d’information.

Le **YouTube** application installée avec Android Oreo répertorie deux catégories de notification : **télécharger notifications** et **notifications générales**:

[![Écrans de notification pour YouTube dans Oreo Android](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Chacune de ces catégories correspond à un canal de notification. YouTube application implémente une **télécharger notifications** canal et un **Notifications général** canal. L’utilisateur peut appuyer **télécharger notifications**, qui affiche l’écran des paramètres pour les canaux de notifications de téléchargement de l’application :

[![Télécharger l’écran de notifications pour l’application YouTube](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

Dans cet écran, l’utilisateur peut modifier le comportement de la **télécharger** des notifications de canal en procédant comme suit :

-   Définir l’Importance au niveau **Urgent**, **haute**, **support**, ou **faible**, qui configure le niveau de l’interruption du son et visuel.

-   Activer le point de notification ou désactiver.

-   Activer la lumière clignotante ou désactiver.

-   Afficher ou masquer des notifications sur l’écran de verrouillage.

-   Remplacer la **faire pas perturber** paramètre.

Le **Notifications général** canal possède des paramètres similaires :

[![Écran de notifications générales pour l’application YouTube](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Notez que vous n’avez pas de contrôle absolu sur la façon dont vos canaux de notification interagissent avec l’utilisateur &ndash; l’utilisateur peut modifier les paramètres de n’importe quel canal de notification sur l’appareil comme indiqué dans les captures d’écran ci-dessus. Toutefois, vous pouvez configurer les valeurs par défaut (comme sera décrite ci-dessous). Comme l’illustrent les exemples suivants, la nouvelle fonctionnalité de canaux de notification permet aux utilisateurs un contrôle affiné sur les différents types de notifications.

Doit ajouter la prise en charge pour les canaux de notification à votre application ? Si vous ciblez Android 8.0, votre application *doit* implémenter des canaux de notification.
Une application destinée à Oreo qui tente d’envoyer une notification de locale à l’utilisateur sans utiliser un canal de notification ne pourront pas afficher la notification sur les appareils Oreo. Si vous ne ciblez Android 8.0, votre application s’exécutera sur Android 8.0, mais avec le même comportement de notification comme il exposerait lors de l’exécution sur Android 7.1 ou version antérieure.


### <a name="creating-a-notification-channel"></a>Création d’un canal de Notification

Pour créer un canal de notification, procédez comme suit :

1. Construire un [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) objet avec les éléments suivants :

    - Une chaîne d’ID qui est unique au sein de votre package. Dans l’exemple suivant, la chaîne `com.xamarin.myapp.urgent` est utilisé.

    - Le nom visible par l’utilisateur du canal (moins de 40 caractères). Dans l’exemple suivant, le nom **Urgent** est utilisé.

    - L’importance du canal, qui contrôle comment par notifications sont publiées dans le `NotificationChannel`. L’importance peut être `Default`, `High`, `Low`, `Max`, `Min`, `None`, ou `Unspecified`.

    Passer ces valeurs à la [constructeur](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (dans cet exemple, `Resource.String.noti_chan_urgent` a la valeur **Urgent**) :

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Configurer le `NotificationChannel` objet avec les paramètres initiaux.
    Par exemple, le code suivant configure le `NotificationChannel` afin que les notifications publiées sur ce canal seront faire vibrer l’appareil et s’affichent sur l’écran de verrouillage par défaut de l’objet :

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Envoyer l’objet de canal de notification pour le Gestionnaire de notification :

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>Validation à un canal de Notifications

Pour valider une notification à un canal de notification, procédez comme suit :

1.  Configurer la notification à l’aide de la `Notification.Builder`, en passant l’ID de canal à le `SetChannelId` (méthode). Par exemple :

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  Créer et émettre la notification à l’aide du Gestionnaire de Notification [notifier](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) méthode :

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

Vous pouvez répéter les étapes ci-dessus pour créer un autre canal de notification pour les messages d’information. Cet deuxième canal par défaut, désactiver les vibrations, peuvent définir la visibilité d’écran de verrouillage par défaut `Private`et l’importance de la notification `Default`.

Pour obtenir un exemple de code complet Android Oreo des canaux de Notification en action, consultez le [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) sample ; cet exemple d’application gère deux canaux et définit les options de notification supplémentaire.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Au-delà de la Notification de base

Notifications par défaut dans un format de mise en page de base efficace dans Android, mais vous pouvez améliorer le format de base en rendant plus `Notification.Builder` les appels de méthode. Dans cette section, vous allez apprendre à ajouter une icône de grande photo à votre notification, et vous verrez des exemples montrant comment créer des notifications de mise en page développé.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Format de grandes icônes

En règle générale, aux notifications Android afficheront l’icône de l’application d’origine (sur le côté gauche de la notification). Toutefois, les notifications peuvent afficher une image ou une photo (un *grandes icônes*) au lieu de la petite icône standard. Par exemple, une application de messagerie peut afficher une photo de l’expéditeur plutôt que sur l’icône d’application.

Voici un exemple d’une notification de base Android 5.0 &ndash; il affiche uniquement l’icône d’application petit :

![Notification normale d’exemple](local-notifications-images/13-sample-notification.png)

Et Voici une capture d’écran de la notification après l’avoir modifié pour afficher une grande icône &ndash; qu’il utilise une icône créée à partir d’une image d’un singe de code de Xamarin :

![Notification de grandes icônes exemple](local-notifications-images/14-large-icon-sample.png)

Notez que lorsqu’une notification est présentée dans le format grandes icônes, l’icône d’application petit est affichée comme un badge sur le coin inférieur droit d’une grande icône.

Pour utiliser une image comme une grande icône dans une notification, vous appelez le Générateur de notification [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) (méthode) et passez une image bitmap de l’image. Contrairement aux `SetSmallIcon`, `SetLargeIcon` accepte uniquement une image bitmap. Pour convertir un fichier image en bitmap, vous utilisez la [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) classe. Par exemple :

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Cet exemple de code ouvre le fichier image dans **Resources/drawable/monkey_icon.png**, le convertit en une image bitmap et transmet l’image bitmap obtenue à `Notification.Builder`. En règle générale, la résolution de l’image source est supérieure à la petite icône &ndash; mais pas beaucoup plus volumineux. Une image qui est trop volumineux peut entraîner des opérations de redimensionnement inutiles qui risque de retarder la validation de la notification.
Pour plus d’informations sur les tailles d’icônes de notification dans Android, consultez [icônes de Notification](http://developer.android.com/design/style/iconography.html#notification).


### <a name="big-text-style"></a>Style de texte volumineux

Le *texte Big* style est un modèle de disposition développée que vous utilisez pour afficher les messages longs dans les notifications. Comme toutes les notifications de mise en page développé, la notification de texte volumineux est initialement affichée dans un format compact de présentation :

![Notification de texte volumineux d’exemple](local-notifications-images/15-big-text-notification.png)

Dans ce format, seul un extrait du message est affiché, terminé par deux points. Lorsque l’utilisateur fait glisser vers le bas sur la notification, il se développe pour afficher le message de notification en entier :

![Notification de texte volumineux développée](local-notifications-images/16-big-text-expanded.png)

Ce format développé de mise en page inclut également un texte en bas de la notification de la synthèse. La hauteur maximale de la *texte Big* notification est 256 point de distribution.

Pour créer un *texte Big* notification, vous instanciez un `Notification.Builder` de l’objet, comme précédemment, puis instancier et d’ajouter un [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) de l’objet à le `Notification.Builder` objet. Par exemple :

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

Dans cet exemple, le texte du message et le texte de résumé sont stockés dans le `BigTextStyle` objet (`textStyle`) avant d’être transmis à `Notification.Builder.`


### <a name="image-style"></a>Style de l’image

Le *Image* style (également appelé le *présentation* style) est un format développé de notification que vous pouvez utiliser pour afficher une image dans le corps d’une notification. Par exemple, une application de la capture d’écran ou une application photo permettre utiliser le *Image* notification style pour l’utilisateur avec une notification de la dernière image qui a été capturée. Notez que la hauteur maximale de la *Image* notification est 256 dp &ndash; Android va se redimensionner l’image pour l’ajuster à cette restriction de hauteur maximale, dans les limites de mémoire disponible.

Comme toutes les développé des notifications de mise en page, *Image* tout d’abord, les notifications sont affichées dans un format compact qui affiche un extrait du texte de message qui accompagne :

![Image compact notification n’indique aucune image](local-notifications-images/17-image-compact.png)

Lorsque l’utilisateur fait glisser vers le bas la *Image* notification, il se développe pour afficher une image. Par exemple, voici la version développée de la notification précédente :

![Image développée notification révèle image](local-notifications-images/18-image-expanded.png)

Notez que lorsque la notification est affichée dans un format compact, il affiche le texte de notification (le texte qui est passé au Générateur de notification `SetContentText` méthode, comme indiqué plus haut). Toutefois, lorsque la notification est développée pour afficher l’image, il affiche le texte du résumé au-dessus de l’image.

Pour créer un *Image* notification, vous instanciez un `Notification.Builder` de l’objet qu’avant, puis créer et insérer un [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) de l’objet dans le `Notification.Builder` objet. Par exemple :

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

Comme le `SetLargeIcon` méthode de `Notification.Builder`, le [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) méthode de `BigPictureStyle` requiert une image bitmap de l’image que vous souhaitez afficher dans le corps de la notification. Dans cet exemple, le [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) méthode `BitmapFactory` lit le fichier image situé **Resources/drawable/x_bldg.png** et le convertit en une image bitmap.

Vous pouvez également afficher des images qui ne sont pas compressées en tant que ressource. Par exemple, l’exemple de code suivant charge une image à partir de la carte SD locale et l’affiche dans un *Image* notification :

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

Dans cet exemple, le fichier image situé **/sdcard/Pictures/my-tshirt.jpg** est chargé, redimensionné à la moitié de sa taille d’origine, puis convertie en une bitmap à utiliser dans la notification :

![Exemple d’image T-shirt de notification](local-notifications-images/19-tshirt-notification.png)

Si vous ne connaissez pas la taille du fichier image à l’avance, il est judicieux d’encapsuler l’appel à [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) dans un gestionnaire d’exceptions &ndash; un `OutOfMemoryError` exception peut être levée si l’image est trop grande pour Android à redimensionner.

Pour plus d’informations sur le chargement et le décodage des images bitmap volumineux, consultez [charge volumineux Bitmaps efficacement](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Style de la boîte de réception

Le *boîte de réception* format est un modèle de disposition développé prévu pour l’affichage des lignes distinctes de texte (par exemple, une boîte de réception résumé) dans le corps de la notification. Le *boîte de réception* format s’affiche tout d’abord dans un format compact :

![Notification de la boîte de réception compact exemple](local-notifications-images/20-inbox-compact.png)

Lorsque l’utilisateur fait glisser vers le bas sur la notification, il se développe pour afficher une synthèse de l’e-mail comme indiqué dans la capture d’écran ci-dessous :

![Notification de la boîte de réception exemple développée](local-notifications-images/21-inbox-expanded.png)

Pour créer un *boîte de réception* notification, vous instanciez un `Notification.Builder` de l’objet, comme précédemment et ajoutez une [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) de l’objet à le `Notification.Builder`. Par exemple :

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

Pour ajouter de nouvelles lignes de texte dans le corps de la notification, appelez le [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) méthode de la `InboxStyle` objet (la hauteur maximale de la *boîte de réception* notification est 256 dp). Notez que, contrairement à *texte Big* style, la *boîte de réception* style prend en charge des lignes de texte dans le corps de la notification.

Vous pouvez également utiliser le *boîte de réception* style pour les notifications qui a besoin d’afficher des lignes de texte dans un format développé. Par exemple, le *boîte de réception* style de notification peut être utilisé pour combiner plusieurs notifications en attente en une synthèse &ndash; vous pouvez mettre à jour un seul *boîte de réception* style notification avec new lignes de contenu de la notification (voir [mise à jour d’une Notification](#updating-a-notification) ci-dessus), plutôt que de générer un flux continu de notifications principalement similaire. Pour plus d’informations sur cette approche, consultez [résumer vos notifications](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).


## <a name="configuring-metadata"></a>Configuration des métadonnées

`Notification.Builder` inclut des méthodes que vous pouvez appeler pour définir les métadonnées sur votre notification, telles que la priorité, visibilité et la catégorie. Android utilise ces informations &mdash; , ainsi que les paramètres de préférence utilisateur &mdash; pour déterminer quand et comment pour afficher les notifications.


### <a name="priority-settings"></a>Paramètres de priorité

Le paramètre de priorité d’une notification détermine deux résultats lors de la notification de publication :

-   Où la notification s’affiche par rapport à d’autres notifications.
    Par exemple, les notifications de haute priorité présentées au-dessus des notifications de priorité plus faibles dans le tiroir de notifications, indépendamment du lors de chaque notification a été publiée.

-   Indique si la notification est affichée dans le format de notification profondes (Android 5.0 et versions ultérieur). Uniquement *haute* et *maximale* les notifications de priorité sont affichées sous forme de notifications de tête haute.

Xamarin.Android définit les énumérations suivantes pour définir la priorité de notification :

-   `NotificationPriority.Max` &ndash; Avertit l’utilisateur à une condition d’urgence ou critique (par exemple, un appel entrant, les instructions étape par étape ou une alerte en cas d’urgence). Sur Android 5.0 et versions ultérieures, les notifications de la priorité maximale sont affichées au format de tête haute.

-   `NotificationPriority.High` &ndash; Informe l’utilisateur des événements importants (tels que des messages électroniques importants ou l’arrivée des messages de conversation en temps réel). Sur Android 5.0 et versions ultérieures, les notifications de priorité élevée sont affichées au format de tête haute.

-   `NotificationPriority.Default` &ndash; Notifie l’utilisateur des conditions qui ont le niveau d’importance moyen.

-   `NotificationPriority.Low` &ndash; Pour plus d’informations non urgentes que l’utilisateur doit être informé de (par exemple, des rappels de mise à jour de logiciels ou mises à jour de réseau social).

-   `NotificationPriority.Min` &ndash; Pour plus d’informations que l’utilisateur Remarque uniquement lorsque affichage des notifications (par exemple, les informations emplacement ou la météo).

Pour définir la priorité d’une notification, appelez le [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) méthode de la `Notification.Builder` objet, en passant le niveau de priorité. Par exemple :

```csharp
builder.SetPriority (NotificationPriority.High);
```

Dans l’exemple suivant, la notification de priorité élevée, « message important ! » s’affiche en haut du tiroir de notification :

![Notification de haute priorité exemple](local-notifications-images/22-hi-priority-drawer.png)

Comme il s’agit d’une notification de haute priorité, il s’affiche également en tant que frontal notification au-dessus d’actuel activité écran l’utilisateur dans Android 5.0 :

![Notification de tête haute exemple](local-notifications-images/23-heads-up-example.png)

Dans l’exemple suivant, la notification de « Vu pour la journée » de priorité basse s’affiche sous une notification au niveau de priorité plus élevée batterie :

![Notification de faible priorité d’exemple](local-notifications-images/24-lo-priority-drawer.png)

Étant donné que la notification de « Pensée pour la journée » est une notification de faible priorité, Android affichera pas dans un format profondes.


### <a name="visibility-settings"></a>Paramètres de visibilité

À partir de Android 5.0, le *visibilité* paramètre n’est disponible pour contrôler la quantité de contenu notification s’affiche sur l’écran de verrouillage sécurisé.
Xamarin.Android définit les énumérations suivantes pour la définition de la visibilité de la notification :

-   `NotificationVisibility.Public` &ndash; La totalité du contenu de la notification s’affiche sur l’écran de verrouillage sécurisé.

-   `NotificationVisibility.Private` &ndash; Uniquement les informations essentielles s’affiche sur l’écran de verrouillage sécurisé (par exemple, l’icône de notification et le nom de l’application qui validée), mais le reste des détails de la notification sont masquées. Toutes les notifications par défaut `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Rien ne s’affiche sur la sécurité l’écran de verrouillage, ni même l’icône de notification. Le contenu de la notification est disponible uniquement après que l’utilisateur déverrouille l’appareil.

Pour définir la visibilité d’une notification, l’appel d’applications la `SetVisibility` méthode de la `Notification.Builder` objet, en passant le paramètre de visibilité. Par exemple, cet appel à `SetVisibility` rend la notification `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Lorsqu’un `Private` notification est envoyée, que le nom et l’icône de l’application s’affiche sur l’écran de verrouillage sécurisé. Au lieu du message de notification, l’utilisateur voit « Déverrouiller votre appareil pour voir cette notification » :

![Déverrouiller votre message de notification d’appareil](local-notifications-images/25-lockscreen-private.png)

Dans cet exemple, **NotificationsLab** est le nom de l’application d’origine. Cette version rédigée de la notification s’affiche uniquement lorsque l’écran de verrouillage est sécurisée (c'est-à-dire sécurisé via le code confidentiel, modèle ou un mot de passe) &ndash; si l’écran de verrouillage n’est pas sécurisée, la totalité du contenu de la notification est disponible sur l’écran de verrouillage.


### <a name="category-settings"></a>Paramètres de catégorie

Compter Android 5.0, catégories prédéfinies sont disponibles pour le classement et le filtrage de notifications. Xamarin.Android fournit les énumérations suivantes pour ces catégories :

-   `Notification.CategoryCall` &ndash; Appel entrant.

-   `Notification.CategoryMessage` &ndash; Message entrant de texte.

-   `Notification.CategoryAlarm` &ndash; Une alarme condition du minuteur d’expiration ou.

-   `Notification.CategoryEmail` &ndash; Message de courrier électronique entrant.

-   `Notification.CategoryEvent` &ndash; Un événement de calendrier.

-   `Notification.CategoryPromo` &ndash; Un message promotionnel ou la publication.

-   `Notification.CategoryProgress` &ndash; La progression d’une opération d’arrière-plan.

-   `Notification.CategorySocial` &ndash; Mise à jour de réseau social.

-   `Notification.CategoryError` &ndash; Échec d’un processus d’authentification ou d’opération en arrière-plan.

-   `Notification.CategoryTransport` &ndash; Mise à jour de lecture de médias.

-   `Notification.CategorySystem` &ndash; Réservé pour le système (état système ou périphérique).

-   `Notification.CategoryService` &ndash; Indique que le service d’arrière-plan est en cours d’exécution.

-   `Notification.CategoryRecommendation` &ndash; Un message de recommandation lié à l’application en cours d’exécution.

-   `Notification.CategoryStatus` &ndash; Informations sur l’appareil.

Lorsque les notifications sont triées, priorité de la notification est prioritaire sur son paramètre de catégorie. Par exemple, une notification de priorité élevée s’affichera en tant que frontal même s’il appartient à la `Promo` catégorie. Pour définir la catégorie d’une notification, vous appelez le `SetCategory` méthode de la `Notification.Builder` objet, en passant le paramètre de catégorie. Par exemple :

```csharp
builder.SetCategory (Notification.CategoryCall);
```

Le *ne pas perturber* fonctionnalité (nouveau dans Android 5.0) filtre les notifications en fonction de catégories. Par exemple, le *ne pas perturber* écran **paramètres** permet à l’utilisateur à des notifications d’exempts pour les appels téléphoniques et des messages :

![Pas perturber les commutateurs de l’écran](local-notifications-images/26-do-not-disturb.png)

Lorsque l’utilisateur configure *ne pas perturber* pour bloquer toutes les interruptions, à l’exception des appels téléphoniques (comme illustré dans la capture d’écran ci-dessus), Android permet des notifications avec un paramètre de catégorie de `Notification.CategoryCall` à présenter lors de l’appareil est de *ne pas perturber* mode. Notez que `Notification.CategoryAlarm` notifications ne sont jamais bloquées dans *ne pas perturber* mode.


<a name="compatibility" />

## <a name="compatibility"></a>Compatibilité

Si vous créez une application qui sera également dans les versions antérieures d’Android (dès que le niveau API 4), exécutez le [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) classe au lieu de `Notification.Builder`. Lorsque vous créez des notifications avec `NotificationCompat.Builder`, Android garantit que le contenu de la notification de base est correctement affiché sur des périphériques plus anciens. Toutefois, étant donné que certaines fonctionnalités avancées de notification ne sont pas disponibles sur les versions antérieures d’Android, votre code doit gérer explicitement les problèmes de compatibilité pour les styles de notification développé, les catégories et niveaux de visibilité comme expliqué ci-dessous.

Pour utiliser `NotificationCompat.Builder` dans votre application, vous devez inclure le [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet dans votre projet.

L’exemple de code suivant illustre comment créer une notification de base à l’aide de `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

Comme l’illustre cet exemple, les appels de méthode pour les options de notification essentielles sont identiques à celles de `Notification.Builder`. Toutefois, votre code doit gérer les problèmes de compatibilité descendante pour les notifications plus complexes (décrites dans la section suivante).

Le [LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) exemple montre comment utiliser `NotificationCompat.Builder` pour lancer une deuxième activité à partir d’une notification. Cet exemple de code est expliquée dans la [Notifications de Local à l’aide de Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) procédure pas à pas.


### <a name="notification-styles"></a>Styles de notification

Pour créer *texte Big*, *Image*, ou *boîte de réception* style des notifications avec `NotificationCompat.Builder`, votre application doit utiliser les versions de la compatibilité de ces styles. Par exemple, pour utiliser le *texte Big* de style, instancier `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

De même, votre application peut utiliser `NotificationCompat.InboxStyle` et `NotificationCompat.BigPictureStyle` pour *boîte de réception* et *Image* styles, respectivement.


### <a name="notification-priority-and-category"></a>Catégorie et priorité de notification

`NotificationCompat.Builder` prend en charge la `SetPriority` (méthode) (disponible à partir de Android 4.1). Toutefois, le `SetCategory` méthode est *pas* pris en charge par `NotificationCompat.Builder` , car les catégories font partie du nouveau système de métadonnées de notification qui a été introduit dans Android 5.0.

Pour prendre en charge des versions antérieures de where Android, `SetCategory` est non disponible, votre code peut vérifier le niveau de l’API à l’exécution pour appeler de manière conditionnelle `SetCategory` lorsque le niveau de l’API est égale ou supérieure à Android 5.0 (niveau 21 d’API) :

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

Dans de cet exemple, l’application **Framework cible** est défini sur Android 5.0 et **la Version minimale Android** a la valeur **Android 4.1 (API niveau 16)**. Étant donné que `SetCategory` est disponible dans le niveau de l’API 21 et versions ultérieur, cet exemple de code appelle `SetCategory` uniquement lorsqu’il est disponible &ndash; elle n’appelle pas `SetCategory` lorsque le niveau de l’API est inférieur à
21.


### <a name="lockscreen-visibility"></a>Visibilité de l’écran de verrouillage

Étant donné que Android ne prenait pas en charge les notifications de l’écran de verrouillage avant Android 5.0 (niveau 21 d’API), `NotificationCompat.Builder` ne prend pas en charge la `SetVisibility` (méthode). Comme expliqué ci-dessus pour `SetCategory`, votre code peut vérifier le niveau de l’API au moment et appel `SetVisiblity` uniquement lorsqu’il est disponible :

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment créer des notifications locales dans Android. Il décrit la structure d’une notification, elle explique comment utiliser `Notification.Builder` pour créer des notifications, les notifications de style dans les grandes icônes, *texte Big*, *Image* et *la boîte de réception*  formats, comment définir des paramètres de métadonnées telles que la priorité, visibilité et la catégorie de notification et comment lancer une activité à partir d’une notification. Cet article décrit également le fonctionnement de ces paramètres de notification avec le nouveau frontal, l’écran de verrouillage, et *ne pas perturber* fonctionnalités introduites dans Android 5.0. Enfin, vous avez appris à utiliser `NotificationCompat.Builder` pour assurer la compatibilité avec les versions antérieures d’Android notification.

Pour connaître les instructions sur la conception des notifications pour Android, consultez [Notifications](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>Liens associés

- [NotificationsLab (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (exemple)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Notifications locales dans la procédure pas à pas Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Notifications](http://developer.android.com/design/patterns/notifications.html)
- [Avertir l’utilisateur](http://developer.android.com/training/notify-user/index.html)
- [Notification](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
