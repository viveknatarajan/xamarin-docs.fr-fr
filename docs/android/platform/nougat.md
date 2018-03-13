---
title: "Fonctionnalités de commande"
description: "Comment commencer à développer des applications pour Android de commande à l’aide de Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: c666b7d5b680eab3c990950569868eacdb6f30af
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="nougat-features"></a>Fonctionnalités de commande

_Comment commencer à développer des applications pour Android de commande à l’aide de Xamarin.Android._

Cet article fournit une description des fonctionnalités introduites dans une commande Android, explique comment préparer Xamarin.Android pour le développement de la commande Android et fournit des liens vers des exemples d’applications qui illustrent l’utilisation des fonctionnalités de commande Android dans Applications de Xamarin.Android.


## <a name="overview"></a>Vue d'ensemble

[Commande Android](https://developer.android.com/about/versions/nougat/android-7.0.html) est suivi de Google pour Android Marshmallow 6.0. Prend en charge pour la Xamarin.Android **7.x Android liaisons** dans Xamarin Android 7.0 et versions ultérieures. Commande Android ajoute plusieurs nouvelles API pour les fonctionnalités de commande décrites ci-dessous ; Ces API sont disponibles pour les applications de Xamarin.Android lorsque vous utilisez Xamarin.Android 7.0.

[![Images héros de tablettes et téléphones Android de commande en cours d’exécution Android](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Pour plus d’informations sur les API de 7.x Android, consultez [Android 7.1 pour les développeurs](http://developer.android.com/preview/api-overview.html).
Pour obtenir la liste des problèmes connus de Xamarin.Android 7.0, consultez le [notes de publication](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Commande Android offre de nombreuses nouvelles fonctionnalités d’intérêt aux développeurs de Xamarin.Android. Ces fonctions incluent :

-   **Prise en charge plusieurs fenêtre** &ndash; cette amélioration permet aux utilisateurs d’ouvrir deux applications sur l’écran à la fois.

-   **Améliorations de la notification** &ndash; le système de notifications repensé dans commande Android inclut un *réponse directe* fonctionnalité permettant aux utilisateurs de répondre rapidement aux messages texte directement à partir de la notification INTERFACE UTILISATEUR. En outre, si votre application crée des notifications pour reçu messages, la nouvelle *regroupé notifications* fonctionnalité peut regrouper des notifications en un seul groupe lorsque plus d’un message est reçu.

-   **Données veille** &ndash; cette fonctionnalité est un nouveau service système qui permet de réduire l’utilisation de données cellulaires par les applications ; elle permet aux utilisateurs de contrôler la façon dont les applications utilisent des données cellulaires.

En outre, la commande Android met de nombreuses autres améliorations d’intérêt pour les développeurs d’applications comme une nouvelle fonctionnalité de configuration de sécurité réseau, Doze en déplacement, améliorations de l’attestation, nouveau rapide paramètres API, prise en charge de plusieurs paramètres régionaux, les API ICU4J, WebView, de clé accès aux fonctionnalités de langage Java 8, accès à l’annuaire avec étendue, une API de pointeur personnalisé, prise en charge de plates-formes VR, fichiers virtuels et les optimisations de traitement en arrière-plan.

Cet article explique comment commencer à créer des applications avec la commande Android pour essayer de nouvelles fonctionnalités et de planifier le travail de migration ou une fonctionnalité pour cibler la plateforme Android commande Nouveau.


## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités de commande Android dans les applications Xamarin :

-   **Visual Studio ou Visual Studio pour Mac** &ndash; si vous utilisez Visual Studio, version 4.2.0.628 ou version ultérieure de Xamarin pour Visual Studio. Si vous utilisez Visual Studio pour Mac, version 6.1.0 ou ultérieure de Visual Studio pour Mac n’est requis.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac.

-   **Kit de développement logiciel Android** -7.0 SDK Android (API 24) ou version ultérieure doit être installé via le Gestionnaire de kit de développement logiciel Android.

-   **Kit de développement Java** &ndash; le développement Xamarin Android 7.0 nécessite [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou une version ultérieure si vous développez pour le niveau de l’API 24 ou supérieure (JDK 8 prend également en charge les niveaux d’API antérieures à 24). La version 64 bits de JDK 8 est requise si vous utilisez des contrôles personnalisés ou l’aperçu des formulaires.

> [!IMPORTANT]
> Xamarin.Android ne prend pas en charge JDK 9.

Notez que les applications doivent être reconstruites avec Xamarin C6SR4 ou version ultérieure pour fonctionnent de façon fiable avec la commande Android. Étant donné que la commande Android peut lier qu’à [NDK autant de bibliothèques natives](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), les applications existantes à l’aide des bibliothèques telles que **Mono.Data.Sqlite.dll** peut se bloquer lors de l’exécution de la commande Android si elles ne sont pas correctement reconstruit.



## <a name="getting-started"></a>Prise en main

Pour commencer à l’aide de la commande Android avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages SDK avant de pouvoir créer un projet de commande Android :

1.  Installez les dernières mises à jour de Xamarin.Android à partir de la Xamarin.

2.  Installer le **Android 7.0 (API 24)** packages et des outils ou version ultérieure.

3.  Créer un nouveau projet Xamarin.Android qui cible une commande Android.

4.  Configurer un émulateur ou un périphérique pour la commande Android.

Chacune de ces étapes est expliquée dans les sections suivantes :


### <a name="install-xamarin-updates"></a>Installer les mises à jour de Xamarin

Pour ajouter la prise en charge de Xamarin pour commande Android, modifier le canal mises à jour dans Visual Studio ou Visual Studio pour Mac pour le canal Stable et appliquer les dernières mises à jour. Si vous devez également des fonctionnalités qui sont actuellement disponibles uniquement dans le canal Alpha ou bêta, vous pouvez basculer vers le canal Alpha ou bêta (les canaux Alpha et bêta prennent également en charge Android 7.x). Pour plus d’informations sur la façon de modifier le canal mises à jour (versions), consultez [modifier le canal mises à jour](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).



### <a name="install-the-android-sdk"></a>Installez le SDK Android

Pour créer un projet avec Xamarin Android 7.0, vous devez d’abord utiliser le Gestionnaire de SDK Android pour installer **Kit de développement logiciel Platform N Android (API 24)** ou version ultérieure. Vous devez également installer la dernière version **Outils kit de développement logiciel Android**:

1.  Démarrez le Gestionnaire de SDK Android (dans Visual Studio pour Mac, utilisez **Outils > ouvrez le Gestionnaire de SDK Android&hellip;**; dans Visual Studio, utilisez **Outils > Android > Gestionnaire de SDK Android**).

2.  Installer **Android 7.0 (API 24)** ou version ultérieure :

    [![Sélectionner les packages Android 7.0 dans le Gestionnaire de kit de développement logiciel Android.](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Installez les derniers outils du Kit de développement logiciel Android :

    [![En sélectionnant les tout derniers outils du Kit de développement logiciel Android dans le Gestionnaire de kit de développement logiciel Android.](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Vous devez installer la version d’Android SDK Tools 25.2.2 ou ultérieure, Android plateforme du Kit de développement logiciel outils 24.0.3 ou ultérieure et Android Build du Kit de développement logiciel 24.0.2 ou version ultérieure.

4.  Vérifiez que le **l’emplacement du Kit de développement Java** est configuré pour le JDK 1.8 :

    [![Configurer le chemin d’accès de JDK 8 sous Outils, options](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Pour afficher ce paramètre dans Visual Studio, cliquez sur **Outils > Options > Xamarin > Paramètres Android**. Dans Visual Studio pour Mac, cliquez sur **Préférences > projets > emplacements du SDK > Android**.



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous ne connaissez pas le développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets de Xamarin.Android.

Lorsque vous créez un projet Android, vous devez configurer les paramètres de la version à la cible Android 7.0 ou version ultérieure. Par exemple, pour cibler votre projet pour Android 7.0, vous devez configurer le niveau d’API Android de cible de votre projet pour **Android 7.0 (API 24 - commande)**. Il est recommandé de définir votre niveau de framework cible vers API 24 ou version ultérieure. Pour plus d’informations sur la configuration des niveaux de niveau d’API Android, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Actuellement, vous devez définir le **version minimale Android** à **Android 7.0 (API 24 - commande)** pour déployer votre application sur les appareils Android commande ou les émulateurs.



### <a name="configure-an-emulator-or-device"></a>Configurer un émulateur ou un périphérique

Si vous utilisez un émulateur, démarrez le gestionnaire AVD Android et créer un nouveau périphérique en utilisant les paramètres suivants :

-   Périphérique : Nexus 5 X Nexus 6, Nexus 6P, le lecteur Nexus, Nexus 9 ou Pixel C.
-   Cible : Android 7.0 - niveau de l’API 24
-   ABI : x86 ou x86\_64

Par exemple, cet appareil virtuel est configuré pour émuler un 6 Nexus :

[![Configuration d’un AVD à l’aide de Nexus 6 appareil Android 7.0 cible et Intel Atom x86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Si vous utilisez un périphérique physique comme un Nexus 5 X, 6 ou 9, vous pouvez mettre à jour votre appareil via automatique sur les mises à jour de l’air (OTA) ou télécharger une image système et flash directement de votre appareil. Pour plus d’informations sur la mise à jour manuelle de votre appareil pour la commande Android, consultez [OTA des Images pour les appareils Nexus](https://developers.google.com/android/nexus/ota).

Notez que Nexus 5 appareils ne sont pas pris en charge par la commande Android.



## <a name="new-features"></a>Nouvelles fonctionnalités

Commande Android introduit une variété de nouvelles fonctionnalités et fonctionnalités, telles que la prise en charge plusieurs fenêtres, des améliorations de Notifications et des données de veille. Les sections suivantes illustrent ces fonctionnalités et fournissent des liens pour vous aider à commencer à les utiliser dans votre application.



### <a name="multi-window-mode"></a>Mode fenêtre multiples

Mode fenêtre multi permet aux utilisateurs d’ouvrir simultanément les deux applications avec prise en charge complète multitâche. Ces applications peuvent s’exécuter côte à côte (paysage) ou un-ci-dessus-du-autres (portrait) dans le mode fractionné.
Les utilisateurs peuvent faire glisser un séparateur entre les applications pour les redimensionner et ils peuvent couper et coller le contenu entre les applications. Lorsque deux applications sont présentées en mode fenêtre multiples, l’activité sélectionnée continue de s’exécuter pendant que l’activité non sélectionnée est suspendu mais toujours visible. Mode fenêtre multiples ne modifie pas le cycle de vie d’une activité Android.

[![Exemples d’applications s’exécutant en mode fenêtre multiples dans le mode portrait et paysage](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Vous pouvez configurer comment les activités de votre application Xamarin.Android prennent en charge le mode fenêtre multiples. Par exemple, vous pouvez configurer les attributs que vous définissez la taille minimale et la hauteur par défaut et la largeur de votre application en mode fenêtre multi. Vous pouvez utiliser la nouvelle `Activity.IsInMultiWindowMode` propriété pour déterminer si votre activité est en mode fenêtre multiples. Exemple :

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Le [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) exemple d’application inclut le code c# qui montre comment tirer parti de la fenêtre de plusieurs interfaces utilisateur avec votre application.

Pour plus d’informations sur le mode de fenêtre multiples, consultez la [prise en charge plusieurs fenêtres](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Notifications améliorées

Commande Android introduit un système de notification de révision. Elle propose une nouvelle fonctionnalité de réponse directe qui permet aux utilisateurs de rapidement une réponse à des notifications pour les messages entrants du texte directement dans l’interface utilisateur de notification. À compter de Android 7.0, les messages peuvent être regroupés en un seul groupe lors de la réception de plusieurs messages de notification. En outre, les développeurs peuvent personnaliser les affichages, de tirer parti des décorations système dans les notifications et de tirer parti des nouveaux modèles de notification lors de la génération des notifications de notification.


#### <a name="direct-reply"></a>Réponse directe

Lorsqu’un utilisateur reçoit une notification pour un message entrant, commande Android rend possible répondre au message dans la notification (au lieu d’ouvrir de l’application de messagerie pour envoyer une réponse).
Cette fonctionnalité de réponse inline permet aux utilisateurs de répondre rapidement à un message SMS ou du texte directement dans l’interface de notification :

[![Capture d’écran de la notification avec un champ de réponse directe inline](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Pour prendre en charge cette fonctionnalité dans votre application, vous devez ajouter *actions de réponse inline* à votre application via un [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) afin que les utilisateurs peuvent répondre par texte directement à partir de la notification de l’interface utilisateur de l’objet.
Par exemple, le code suivant builds un `RemoteInput` pour la réception d’entrée de texte, génère une intention en attente pour l’action de réponse et crée une action activée d’entrée à distance :

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Cette action est ajoutée à la notification :

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

Le [Service de messagerie](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) exemple d’application inclut le code c# qui montre comment étendre des notifications avec une `RemoteInput` objet. Pour plus d’informations sur l’ajout de réponse d’inline actions à votre application pour Android 7.0 ou version ultérieure, reportez-vous à la Android [répond aux Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) rubrique.


#### <a name="bundled-notifications"></a>Notifications groupées

Commande Android peut regrouper les messages de notification (par exemple, par rubrique de message) et afficher le groupe plutôt que chaque message séparément.
Cela *regroupé notifications* fonctionnalité permet aux utilisateurs de faire disparaître ou archiver un groupe de notifications en une seule action. L’utilisateur peut glisser vers le bas pour développer le groupe de notifications à afficher chaque notification en détail :

[![Capture d’écran exemple de notifications groupées](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Pour prendre en charge les notifications groupées, votre application peut utiliser le [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) méthode pour regrouper les notifications semblables. Pour plus d’informations sur les groupes de notification fourni dans N Android, consultez le Android [Notifications de regroupement](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) rubrique.


#### <a name="custom-views"></a>Vues personnalisées

Commande Android permet de créer des vues de notification personnalisée avec les en-têtes systèmes de notification, les actions et les mises en page peut être développés. Pour plus d’informations sur les vues de notification personnalisée dans une commande Android, consultez le Android [améliorations de la Notification](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) rubrique.



### <a name="data-saver"></a>Économiseur de données

À partir de commande Android, les utilisateurs peuvent activer une nouvelle *données veille* définir que les blocs en arrière-plan à l’utilisation des données. Ce paramètre indique également votre application d’utiliser moins de données au premier plan dans la mesure du possible. Le [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) a été étendu dans la commande Android afin que votre application peut vérifier si l’utilisateur a activé les données de veille afin que votre application peut faire un effort pour limiter l’utilisation de données lors de la veille de données est activée.

Pour plus d’informations sur la nouvelle fonctionnalité de veille des données dans une commande Android, consultez le Android [optimisation de l’utilisation des données réseau](https://developer.android.com/training/basics/network-ops/data-saver.html) rubrique.



### <a name="app-shortcuts"></a>Raccourcis de l’application

Android 7.1 a introduit une *raccourcis de l’application* fonctionnalité qui permet aux utilisateurs de rapidement démarrer recommandée les tâches courantes ou avec votre application.
Pour activer le menu de raccourcis, l’utilisateur long-appuie sur l’icône d’application pour une seconde ou plus &ndash; le menu s’affiche avec une vibration rapide.
Libérer l’appui sur provoque le menu de rester :

[![Écran d’exemple d’un menu de raccourci d’application pour une application de messagerie](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Cette fonctionnalité est disponible uniquement API au niveau supérieur ou égal à 25.
Pour plus d’informations sur la nouvelle fonctionnalité de raccourcis de l’application Android 7.1, consultez le Android [raccourcis de l’application](https://developer.android.com/guide/topics/ui/shortcuts.html) rubrique.


### <a name="sample-code"></a>Exemple de code

Plusieurs exemples de Xamarin.Android sont disponibles pour vous montrer comment tirer parti des fonctionnalités de la commande Android :

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) illustre l’utilisation de l’API de fenêtre multi disponible dans la commande Android. Vous pouvez basculer de l’exemple d’application en mode windows multi pour voir comment il affecte le comportement et le cycle de vie de l’application.

-   [Service de messagerie](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) utilise un service simple qui envoie des notifications du `NotificationCompatManager`. Il permet également d’étendre la notification avec un `RemoteInput` objet à autoriser les appareils Android commande réponse via texte directement à partir de la notification sans avoir à ouvrir une application.

-   [Notifications actives](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) montre comment utiliser le `NotificationManager` API pour vous indiquer le nombre de notifications votre application est actuellement affichée.

-   [Une étendue d’accès à l’annuaire](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) montre comment utiliser les API d’accès de répertoire étendue pour accéder facilement aux répertoires spécifiques. Cela constitue une alternative à avoir à définir `READ_EXTERNAL_STORAGE` ou `WRITE_EXTERNAL_STORAGE` autorisations dans votre manifeste.

-   [Démarrage de diriger](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) illustre comment stocker des données dans un stockage d’appareil chiffré qui est toujours disponible, alors que le périphérique est démarré à la fois avant et après n’importe quel credentials(PIN/Pattern/Password) utilisateur sont entrés.


## <a name="summary"></a>Récapitulatif

Cet article a introduit la commande Android et explique comment installer et configurer les tout derniers outils et des packages pour le développement de Xamarin.Android sur commande Android. Il également une vue d’ensemble des fonctionnalités clés disponibles dans la commande Android, avec des liens vers des exemples de code source pour vous aider à prise en main de la création d’applications pour Android de commande.


## <a name="related-links"></a>Liens associés

- [Android 7.1 pour les développeurs](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Notes de mise à jour de Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
