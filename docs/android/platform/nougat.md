---
title: Fonctionnalités de nougat
description: La prise en main à l’aide de Xamarin.Android pour développer des applications pour Android Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: de2b92a4007f085a14c16f0c1e8ca9e568df1aff
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61209080"
---
# <a name="nougat-features"></a>Fonctionnalités de nougat

_La prise en main à l’aide de Xamarin.Android pour développer des applications pour Android Nougat._

Cet article fournit un aperçu des fonctionnalités introduites dans Android Nougat, explique comment préparer Xamarin.Android pour le développement Android Nougat et fournit des liens vers des exemples d’applications qui illustrent l’utilisation des fonctionnalités Android Nougat dans Applications Xamarin.Android.


## <a name="overview"></a>Vue d'ensemble

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) est suivi de Google pour Android Marshmallow 6.0. Xamarin.Android prend en charge **Android 7.x liaisons** dans Xamarin Android 7.0 et versions ultérieures. Android Nougat ajoute plusieurs nouvelles API pour les fonctionnalités de Nougat décrites ci-dessous ; Ces API sont disponibles pour les applications Xamarin.Android lorsque vous utilisez Xamarin.Android 7.0.

[![Images de héros de tablettes et téléphones Android Nougat en cours d’exécution Android](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Pour plus d’informations sur Android 7.x API, consultez [Android 7.1 pour les développeurs](https://developer.android.com/preview/api-overview.html).
Pour obtenir la liste des problèmes connus de Xamarin.Android 7.0, consultez le [notes de version](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android Nougat fournit de nombreuses fonctionnalités nouvelles d’intérêt pour les développeurs Xamarin.Android. Ces fonctions incluent :

-   **Prise en charge multi-window** &ndash; cette amélioration permet aux utilisateurs d’ouvrir les deux applications sur l’écran à la fois.

-   **Améliorations de la notification** &ndash; le système de notifications repensé dans Android Nougat inclut un *réponse directe* fonctionnalité qui permet aux utilisateurs de répondre rapidement aux messages texte directement à partir de la notification INTERFACE UTILISATEUR. En outre, si votre application crée des notifications pour reçu les messages, la nouvelle *regroupé les notifications* fonctionnalité peut regrouper notifications comme un seul groupe lors de la réception de plusieurs messages.

-   **Données veille** &ndash; cette fonctionnalité est un nouveau service de système qui permet de réduire l’utilisation de données cellulaires par les applications ; elle permet aux utilisateurs de contrôler la façon dont les applications utilisent les données cellulaires.

En outre, Android Nougat apporte de nombreuses autres améliorations d’intérêt pour les développeurs d’applications telles que d’une nouvelle fonctionnalité de configuration de sécurité réseau, Doze en déplacement, principales améliorations de l’attestation, nouveau rapide paramètres API, prise en charge de plusieurs paramètres régionaux, les API ICU4J, WebView, accès aux fonctionnalités de langage Java 8, accès à l’annuaire avec étendue, une API personnalisée de pointeur, prise en charge de plateforme VR, fichiers virtuels et optimisations de traitement en arrière-plan.

Cet article explique comment commencer à créer des applications avec Android Nougat tester les nouvelles fonctionnalités et de planifier le travail de migration ou une fonctionnalité pour cibler la nouvelle plateforme Android Nougat.


## <a name="requirements"></a>Spécifications

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités de Android Nougat dans les applications basées sur Xamarin :

-   **Visual Studio ou Visual Studio pour Mac** &ndash; si vous utilisez Visual Studio, version 4.2.0.628 ou une version ultérieure de Visual Studio Tools pour Xamarin est requise. Si vous utilisez Visual Studio pour Mac version 6.1.0 ou version ultérieure de Visual Studio pour Mac est nécessaire.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac.

-   **Kit de développement logiciel Android** -Android 7.0 SDK (API 24) ou version ultérieure doit être installé via le gestionnaire Android SDK.

-   **Kit de développement Java** &ndash; le développement Xamarin Android 7.0 nécessite [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure si vous développez pour le niveau d’API 24 ou supérieure (JDK 8 prend également en charge les niveaux d’API antérieures à 24). La version 64 bits du JDK 8 est requise si vous utilisez des contrôles personnalisés ou le Générateur d’aperçu de formulaires.

> [!IMPORTANT]
> Xamarin.Android ne prend pas en charge JDK 9.

Notez que les applications doivent être reconstruites avec Xamarin C6SR4 ou version ultérieure pour fonctionner de manière fiable avec Android Nougat. Étant donné que Android Nougat peut lier uniquement à [NDK fourni des bibliothèques natives](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), les applications existantes à l’aide de bibliothèques telles que **Mono.Data.Sqlite.dll** peut se bloquer lors de l’exécution sur Android Nougat si elles ne sont pas correctement reconstruit.



## <a name="getting-started"></a>Prise en main

Pour commencer à l’aide d’Android Nougat avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages de kit de développement logiciel avant de pouvoir créer un projet Android Nougat :

1.  Installer les dernières mises à jour Xamarin.Android à partir de Xamarin.

2.  Installer le **Android 7.0 (API 24)** packages et des outils ou version ultérieure.

3.  Créer un nouveau projet Xamarin.Android qui cible Android Nougat.

4.  Configurer un appareil ou un émulateur pour Android Nougat.

Chacune de ces étapes est expliquée dans les sections suivantes :


### <a name="install-xamarin-updates"></a>Installer les mises à jour de Xamarin

Pour ajouter la prise en charge de Xamarin pour Android Nougat, modifier le canal mises à jour dans Visual Studio ou Visual Studio pour Mac pour le canal Stable et appliquer les dernières mises à jour. Si vous avez également besoin des fonctionnalités qui sont actuellement disponibles uniquement dans le canal Alpha ou Beta, vous pouvez basculer vers le canal Alpha ou Beta (les canaux Alpha et bêta prennent également en charge Android 7.x). Pour plus d’informations sur la façon de modifier le canal mises à jour (versions), consultez [modifier le canal mises à jour](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).



### <a name="install-the-android-sdk"></a>Installez le SDK Android

Pour créer un projet avec Xamarin Android 7.0, vous devez tout d’abord utiliser le gestionnaire Android SDK pour installer **Platform SDK Android N (API 24)** ou version ultérieure. Vous devez également installer la dernière version **Android SDK Tools**:

1.  Démarrer le gestionnaire Android SDK (dans Visual Studio pour Mac, utilisez **Outils > Open Android SDK Manager&hellip;**; dans Visual Studio, utilisez **Outils > Android > Gestionnaire Android SDK**).

2.  Installer **Android 7.0 (API 24)** ou version ultérieure :

    [![Packages Android 7.0 en sélectionnant dans le Gestionnaire de kit de développement logiciel Android.](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Installez les derniers outils Android SDK :

    [![En sélectionnant les derniers outils Android SDK dans le Gestionnaire de kit de développement logiciel Android.](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Vous devez installer révision Android SDK Tools 25.2.2 ou version ultérieures, Android outils de plateforme du Kit de développement logiciel 24.0.3 ou versions ultérieures et Android SDK Build tools 24.0.2 ou version ultérieure.

4.  Vérifiez que le **emplacement du Java Development Kit** est configuré pour le JDK 1.8 :

    [![Configurer le chemin d’accès de JDK 8 sous outils/options](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Pour afficher ce paramètre dans Visual Studio, cliquez sur **Outils > Options > Xamarin > Paramètres Android**. Dans Visual Studio pour Mac, cliquez sur **Préférences > projets > emplacements SDK > Android**.



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous êtes novice en développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Xamarin.Android.

Lorsque vous créez un projet Android, vous devez configurer les paramètres de version à la cible Android 7.0 ou version ultérieure. Par exemple, pour cibler votre projet pour Android 7.0, vous devez configurer le niveau d’API Android de cible de votre projet pour **Android 7.0 (API 24 - Nougat)**. Il est recommandé de définir votre niveau de framework cible vers l’API 24 ou version ultérieure. Pour plus d’informations sur la configuration des niveaux de niveau d’API Android, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Actuellement, vous devez définir le **version minimale d’Android** à **Android 7.0 (API 24 - Nougat)** pour déployer votre application sur Android Nougat appareils ou émulateurs.



### <a name="configure-an-emulator-or-device"></a>Configurer un émulateur ou un périphérique

Si vous utilisez un émulateur, démarrez le Gestionnaire d’AVD Android et créer un nouvel appareil en utilisant les paramètres suivants :

-   Périphérique : Nexus 5 X, Nexus 6, Nexus 6P, Nexus Player, Nexus 9 ou Pixel C.
-   Cible : Android 7.0 - niveau d’API 24
-   ABI : x86 ou x86\_64

Par exemple, cet appareil virtuel est configuré pour émuler un Nexus 6 :

[![Configuration d’un AVD à l’aide de Nexus 6 appareil cible d’Android 7.0 et Intel Atom x86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Si vous utilisez un appareil physique comme un Nexus 5 X, 6 ou 9, vous pouvez mettre à jour de votre appareil via automatique sur les mises à jour de l’air (OTA) ou télécharger une image de système et flash directement de votre appareil. Pour plus d’informations sur la mise à jour manuelle de votre appareil vers Android Nougat, consultez [Images OTA pour les périphériques Nexus](https://developers.google.com/android/nexus/ota).

Notez que les appareils Nexus 5 ne sont pas pris en charge par Android Nougat.



## <a name="new-features"></a>Nouvelles fonctionnalités

Android Nougat propose de nouvelles fonctionnalités et fonctions, telles que la prise en charge plusieurs fenêtres, des améliorations de Notifications et des données de veille. Les sections suivantes mettent en évidence ces fonctionnalités et fournissent des liens pour vous aider à commencer à les utiliser dans votre application.



### <a name="multi-window-mode"></a>Mode de multi-Window

Multi-window mode permet aux utilisateurs d’ouvrir les deux applications à la fois avec prise en charge complète de multitâche. Ces applications peuvent s’exécuter côte à côte (paysage) ou un-ci-dessus-the-autres (portrait) dans le mode fractionné.
Les utilisateurs peuvent faire glisser un séparateur entre les applications pour les redimensionner, et elles peuvent couper et coller du contenu l’entre les applications. Lorsque deux applications sont présentées en mode de multi-window, l’activité sélectionnée continue de s’exécuter pendant que l’activité non sélectionnée est en pause, mais toujours visible. Mode de multi-window ne modifie pas le cycle de vie d’une activité Android.

[![Exemples d’applications en cours d’exécution en mode de multi-window en portrait et paysage](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Vous pouvez configurer comment les activités de votre application Xamarin.Android prennent en charge le mode de multi-window. Par exemple, vous pouvez configurer les attributs que définir la taille minimale et la hauteur par défaut et la largeur de votre application en mode de multi-window. Vous pouvez utiliser la nouvelle `Activity.IsInMultiWindowMode` propriété afin de déterminer si votre activité est en mode de multi-window. Exemple :

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

Le [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) exemple d’application inclut le code c# qui montre comment tirer parti de la fenêtre de plusieurs interfaces utilisateur avec votre application.

Pour plus d’informations sur le mode de multi-window, consultez le [prise en charge multi-Window](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Notifications améliorées

Android Nougat introduit un système de notification remaniée. Il comprend une nouvelle fonctionnalité de réponse directe qui permet aux utilisateurs de rapidement une réponse pour envoyer des notifications pour les messages entrants du texte directement dans la notification de l’interface utilisateur. À compter d’Android 7.0, les messages peuvent être regroupés en un seul groupe lors de la réception de plusieurs messages de notification. En outre, les développeurs peuvent personnaliser les affichages, tirer parti des ornements de système dans les notifications et tirer parti des nouveaux modèles de notification lors de la génération des notifications de notification.


#### <a name="direct-reply"></a>Réponse directe

Lorsqu’un utilisateur reçoit une notification de message entrant, Android Nougat rend possible répondre au message dans la notification (au lieu d’ouvrir l’application de messagerie pour envoyer une réponse).
Cette fonctionnalité de réponse inline permet aux utilisateurs de répondre rapidement à un SMS ou message texte directement au sein de l’interface de notification :

[![Capture d’écran d’une notification avec un champ de réponse directe inline](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Pour prendre en charge cette fonctionnalité dans votre application, vous devez ajouter *actions de réponse inline* à votre application via un [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) afin que les utilisateurs peuvent répondre par le biais de texte directement à partir de la notification de l’interface utilisateur de l’objet.
Par exemple, le code suivant builds un `RemoteInput` pour recevoir l’entrée de texte, génère une intention en attente pour l’action de réponse et crée une action activée d’entrée à distance :

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

Le [Service de messagerie](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) exemple d’application inclut le code c# qui montre comment étendre les notifications avec une `RemoteInput` objet. Pour plus d’informations sur l’ajout de réponse d’inline actions à votre application pour Android 7.0 ou version ultérieure, reportez-vous à la Android [répondant aux Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) rubrique.


#### <a name="bundled-notifications"></a>Notifications groupées

Android Nougat peut regrouper les messages de notification (par exemple, par rubrique de message) et afficher le groupe plutôt que chaque message séparé.
Cela *regroupé notifications* fonctionnalité permet aux utilisateurs d’ignorer ou archiver un groupe de notifications dans une seule action. L’utilisateur peut glisser vers le bas pour développer le groupe de notifications pour afficher chaque notification en détail :

[![Exemple de capture d’écran de notifications groupées](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Pour prendre en charge les notifications groupées, votre application peut utiliser le [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) méthode pour regrouper des notifications similaire. Pour plus d’informations sur les groupes de notification groupés dans Android N, consultez la Android [Notifications regroupement](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) rubrique.


#### <a name="custom-views"></a>Vues personnalisées

Android Nougat permet de créer des vues de notification personnalisée avec des en-têtes de notification système, les actions et les dispositions extensibles. Pour plus d’informations sur les vues de notification personnalisée dans Android Nougat, consultez le Android [améliorations de la Notification](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) rubrique.



### <a name="data-saver"></a>Économiseur de données

À compter d’Android Nougat, les utilisateurs peuvent activer une nouvelle *données veille* définissant que blocs d’arrière-plan de l’utilisation des données. Ce paramètre indique également votre application d’utiliser moins de données au premier plan dans la mesure du possible. Le [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) a été étendu dans Android Nougat afin que votre application puisse vérifier si l’utilisateur a activé les données de veille afin que votre application peut faire un effort pour limiter son utilisation des données lors de la veille de données est activée.

Pour plus d’informations sur la nouvelle fonctionnalité de données de veille en Android Nougat, consultez le Android [optimisation de l’utilisation des données réseau](https://developer.android.com/training/basics/network-ops/data-saver.html) rubrique.



### <a name="app-shortcuts"></a>Raccourcis de l’application

Android 7.1 a introduit une *raccourcis de l’application* fonctionnalité qui permet aux utilisateurs de rapidement démarrer recommandée les tâches courantes ou avec votre application.
Pour activer le menu de raccourcis, l’utilisateur long-appuie sur l’icône d’application pour une seconde ou plus &ndash; le menu s’affiche avec une vibration rapide.
Libération de la presse provoque le menu reste :

[![Écran d’exemple d’un menu contextuel de l’application pour une application de messagerie](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Cette fonctionnalité est disponible uniquement API au niveau supérieur ou égal à 25.
Pour plus d’informations sur la nouvelle fonctionnalité de raccourcis de l’application dans Android 7.1, consultez le Android [raccourcis de l’application](https://developer.android.com/guide/topics/ui/shortcuts.html) rubrique.


### <a name="sample-code"></a>Exemple de code

Plusieurs exemples de Xamarin.Android sont disponibles pour vous montrer comment tirer parti des fonctionnalités d’Android Nougat :

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) illustre l’utilisation de l’API multi-window disponible dans Android Nougat. Vous pouvez basculer de l’exemple d’application en mode windows multi pour voir comment il affecte le cycle de vie et le comportement de l’application.

-   [Service de messagerie](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) utilise un service simple qui envoie des notifications la `NotificationCompatManager`. Il étend également la notification avec un `RemoteInput` objet pour permettre aux appareils Android Nougat répondre par le biais de texte directement à partir de la notification sans avoir à ouvrir une application.

-   [Notifications actives](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) montre comment utiliser le `NotificationManager` API pour vous indiquer le nombre de notifications affiche actuellement votre application.

-   [Étendue d’accès à l’annuaire](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) montre comment utiliser l’accès de répertoire délimitée API pour accéder facilement à des répertoires spécifiques. Cela constitue une alternative de devoir définir `READ_EXTERNAL_STORAGE` ou `WRITE_EXTERNAL_STORAGE` autorisations dans votre manifeste.

-   [Démarrage de diriger](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) illustre comment stocker des données dans un stockage d’appareil chiffré qui est toujours disponible, alors que le périphérique est démarré à la fois avant et après n’importe quel credentials(PIN/Pattern/Password) utilisateur sont entrés.


## <a name="summary"></a>Récapitulatif

Cet article a introduit Android Nougat et expliqué comment installer et configurer les derniers outils et packages pour le développement de Xamarin.Android sur Android Nougat. Il montre également comment fourni une vue d’ensemble des principales fonctionnalités disponibles dans Android Nougat, avec des liens vers des exemples de code source pour vous aider à bien démarrer en créant des applications pour Android Nougat.


## <a name="related-links"></a>Liens associés

- [Android 7.1 pour les développeurs](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Notes de publication de Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
