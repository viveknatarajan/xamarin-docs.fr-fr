---
title: Android 9 Pie
description: Comment commencer à développer des applications pour Android 9 à secteurs à l’aide de Xamarin.Android.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: cd1c374fa68420e1923ef4dee0bb37a4665f3535
ms.sourcegitcommit: 215cad17324ba3fbc23487ce66cd4e1cc74eb879
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710021"
---
# <a name="android-pie-features"></a>Fonctionnalités de graphique à secteurs Android

_Comment commencer à développer des applications pour Android 9 à secteurs à l’aide de Xamarin.Android._

[Graphique à secteurs 9 Android](https://developer.android.com/about/versions/pie/) est désormais disponible à partir de Google. Un nombre de nouvelles fonctionnalités et API seront disponible dans cette version, et bon nombre d'entre eux sont nécessaires pour tirer parti des nouvelles fonctionnalités du matériel dans les derniers appareils Android.

![Image de héros à secteurs Android](pie-images/01-android-p-logo.png)

Cet article est structuré pour vous aider à commencer le développement d’applications Xamarin.Android pour Android à secteurs. Il explique comment installer les mises à jour nécessaires, configurer le Kit de développement et préparer un émulateur ou un périphérique pour le test. Il fournit un aperçu des nouvelles fonctionnalités de graphique à secteurs Android et fournit des exemples de code source qui illustre comment utiliser certaines des principales fonctionnalités de graphique à secteurs Android.

Xamarin.Android 9.0 prend en charge pour Android à secteurs. Pour plus d’informations sur la prise en charge de Xamarin.Android pour Android à secteurs, consultez le [Android P Developer Preview 3](https://developer.xamarin.com/releases/android/xamarin.android_9/xamarin.android_9.0/#android-p-dp1) notes de publication.

## <a name="requirements"></a>Configuration requise

La liste suivante est nécessaire pour utiliser les fonctionnalités de graphique à secteurs Android dans les applications basées sur Xamarin :

-   **Visual Studio** &ndash; si vous utilisez Windows, mettre à jour vers Visual Studio 2017 version 15,8 ou version ultérieure. Si vous utilisez un Mac, mettez à jour Visual Studio 2017 pour Mac 7.6 ou version ultérieure.

-   **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 ou version ultérieure doit être installé avec Visual Studio (Xamarin.Android est automatiquement installé dans le cadre de la **développement Mobile en .NET** charge de travail).

-   **Kit de développement Java** &ndash; 9.0 Android Xamarin développement nécessite [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (ou vous pouvez essayer la version préliminaire de distribution Microsoft de la [OpenJDK](~/android/get-started/installation/openjdk.md)). JDK8 est automatiquement installé dans le cadre de la **développement Mobile en .NET** charge de travail.

-   **Kit de développement logiciel Android** &ndash; API SDK Android 28 ou version ultérieure doit être installé via le gestionnaire Android SDK.

## <a name="getting-started"></a>Bien démarrer

Pour commencer à développer des applications Android à secteurs avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages de kit de développement logiciel avant de pouvoir créer votre premier projet Android en secteurs :

1. Mettre à jour vers [Visual Studio 2017 version 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) ou version ultérieure. Si vous utilisez Visual Studio pour Mac, la mise à jour vers [Visual Studio 2017 pour Mac version 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) ou version ultérieure.

2. Installer **Android à secteurs (API 28)** packages et outils via le Gestionnaire de kit de développement logiciel.

3. Créer un nouveau projet Xamarin.Android qui cible **9.0 Android**.

4. Configurer un émulateur ou un périphérique pour le test des applications Android Pie.

Chacune de ces étapes est expliquée dans les sections suivantes :


### <a name="update-visual-studio"></a>Mettre à jour Visual Studio 2017

Pour ajouter la prise en charge Android à secteurs à Visual Studio, la mise à jour vers Visual Studio 2017 version 15,8 ou version ultérieure (pour obtenir des instructions, consultez [mettre à jour Visual Studio 2017 vers la version la plus récente](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

Pour ajouter la prise en charge Android à secteurs à Visual Studio pour Mac, la mise à jour vers Visual Studio 2017 pour Mac 7.6 ou version ultérieure (pour obtenir des instructions, consultez [le programme d’installation et installer Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/installation)).


### <a name="install-the-android-sdk"></a>Installez le SDK Android

Pour créer un projet avec Xamarin.Android 9.0, vous devez d’abord utiliser le gestionnaire Android SDK pour installer la plateforme du Kit de développement logiciel pour **à secteurs Android (niveau d’API 28)** ou version ultérieure.

1. Démarrez le Gestionnaire du Kit de développement logiciel. Dans Visual Studio, cliquez sur **Outils > Android > Gestionnaire Android SDK**. Dans Visual Studio pour Mac, cliquez sur **Outils > Gestionnaire du kit SDK**.

2. Dans le coin inférieur droit, cliquez sur l’icône d’engrenage et sélectionnez **référentiel > Google (non pris en charge)**:

    [![Définition du référentiel à Google](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. Installer le **à secteurs Android** packages de kit de développement logiciel, qui sont répertoriés en tant que **28 de plateforme Android SDK** dans le **plateformes** onglet (pour plus d’informations sur l’utilisation du Gestionnaire de SDK, consultez [ Le programme d’installation du Kit Android SDK](~/android/get-started/installation/android-sdk.md)) :

    [![Installation des packages à secteurs Android](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. Si vous utilisez un émulateur, créer un appareil virtuel qui prend en charge **API niveau 28**. Pour plus d’informations sur la création d’appareils virtuels, consultez [la gestion des appareils virtuels avec le Gestionnaire d’appareils Android](~/android/get-started/installation/android-emulator/device-manager.md).



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous êtes novice en développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Xamarin.Android.

Lorsque vous créez un projet Android, vous devez configurer les paramètres de version à la cible Android 9.0 ou version ultérieure. Par exemple, pour cibler votre projet pour Android à secteurs, vous devez configurer le niveau d’API Android de cible de votre projet pour **9.0 Android** (28 API). Il est recommandé que vous également définir votre Framework cible au niveau API 28 ou version ultérieure. Pour plus d’informations sur la configuration des niveaux d’API Android, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-a-device-or-emulator"></a>Configurer un appareil ou un émulateur

Si vous utilisez un périphérique physique tel qu’un Nexus ou d’un Pixel, vous pouvez mettre à jour votre appareil en secteur Android en suivant les instructions dans [fabrique des Images pour les appareils de Pixel et de Nexus](https://developers.google.com/android/images).

Si vous utilisez un émulateur, créer un appareil virtuel pour le niveau d’API 28 et sélectionnez une image x86. Pour plus d’informations sur l’utilisation du Gestionnaire d’appareils Android pour créer et gérer des appareils virtuels, consultez [la gestion des appareils virtuels avec le Gestionnaire d’appareils Android](~/android/get-started/installation/android-emulator/device-manager.md).
Pour plus d’informations sur l’utilisation de l’émulateur Android pour tester et déboguer, consultez [débogage sur l’émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md).



## <a name="new-features"></a>Nouvelles fonctionnalités

Graphique à secteurs Android propose de nouvelles fonctionnalités. Certaines de ces nouvelles fonctionnalités sont conçues pour tirer parti des nouvelles fonctionnalités de matériel offertes par les appareils Android plus récente, tandis que d’autres sont conçus pour améliorer l’expérience utilisateur Android :

-   **Prise en charge du découpage affichage** &ndash; fournit des API pour rechercher l’emplacement et la forme de la _découpage_ en haut de l’écran sur les appareils Android plus récente.

-   **Améliorations de la notification** &ndash; messages de Notification peuvent désormais afficher des images et un nouveau `Person` classe est utilisée pour simplifier les participants de la conversation.

-   **Positionnement intérieur** &ndash; plates-formes prises en charge le protocole Wi-Fi Round-Trip-temps, ce qui rend possible pour les applications d’utiliser des appareils de Wi-Fi pour la navigation dans les paramètres de l’intérieur.

-   **Prise en charge plusieurs caméra** &ndash; permet aux flux d’accès simultanément à partir de plusieurs appareils physiques (par exemple, des caméras de double-avant et arrière à la double).


Les sections suivantes mettent en évidence ces fonctionnalités et fournissent des exemples de code court pour vous aider à commencer à les utiliser dans votre application.

### <a name="display-cutout-support"></a>Afficher la prise en charge du découpage

Ont de nombreux appareils Android plus récents des écrans de bord à bord un *afficher un découpage* (ou « coupe-bande ») en haut de l’affichage pour les appareils photo et de l’orateur.
La capture d’écran suivante fournit un exemple de l’émulateur d’un découpage :

[![Émulateur Android simulant un découpage](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

Pour gérer la façon dont votre fenêtre de l’application affiche son contenu sur les appareils avec un découpage de l’affichage, à secteurs Android a ajouté une nouvelle [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) attribut de disposition de fenêtre. Cet attribut peut être défini à une des valeurs suivantes :

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; la fenêtre n’est jamais autorisée à se chevaucher avec la zone de découpage.

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; la fenêtre est autorisée à étendre dans la zone de découpage, mais uniquement sur les bords courts de l’écran. 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; la fenêtre est autorisée à étendre dans la zone de découpage si le découpage est contenu dans une barre système.

Par exemple, pour empêcher la fenêtre d’application de chevauchement avec la zone de découpage, la valeur est le mode de découpage de disposition *jamais*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

Les exemples suivants fournissent des exemples de ces modes de découpage. La première capture d’écran de gauche est de l’application en mode non-plein écran. Dans la capture d’écran du centre, l’application accède plein écran avec `LayoutInDisplayCutoutMode` défini sur `LayoutInDisplayCutoutModeShortEdges`. Notez qu’arrière-plan blanc de l’application s’étend dans la zone de découpage d’affichage :

[![Exemple afficher les modes de découpage dans l’émulateur](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

Dans la capture d’écran finale (ci-dessus à droite), `LayoutInDisplayCutoutMode` est définie sur `LayoutInDisplayCutoutModeShortNever` avant d’entrer en mode plein écran.
Notez que l’arrière-plan blanc de l’application n’est pas autorisé à étendre à la zone de découpage d’affichage.

Si vous avez besoin des informations plus détaillées sur la zone de découpage sur l’appareil, vous pouvez utiliser la nouvelle [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) classe. `DisplayCutout` représente la zone de l’affichage ne peut pas être utilisé pour afficher le contenu. Vous pouvez utiliser ces informations pour récupérer l’emplacement et la forme du découpage afin que votre application n’essaie pas d’afficher le contenu de cette zone non fonctionnelle.

Pour plus d’informations sur les nouvelles fonctionnalités de découpage de P Android, consultez [prise en charge du découpage affichage](https://developer.android.com/about/versions/pie/android-9.0#cutout).



### <a name="notifications-enhancements"></a>Améliorations des notifications

Android à secteurs présente les améliorations suivantes pour améliorer l’expérience de messagerie :

-   Canaux de notification (introduite dans [Android Oreo](~/android/platform/oreo.md)) prend désormais en charge le blocage des groupes de canal.

-   Le système de notification a trois nouvelles-ne pas déranger catégories (hiérarchisation des alarmes, des sons système et des sources multimédias). En outre, il existe sept nouveaux-ne pas déranger modes qui peuvent être utilisées pour supprimer des interruptions visual (par exemple, des badges, lumières de notification, apparences de barre d’état et le lancement des activités de plein écran).

-   Un nouveau [personne](https://developer.android.com/reference/android/app/Person.html) classe a été ajoutée pour représenter l’expéditeur d’un message. Utilisation de cette classe permet d’optimiser le rendu de chaque notification en identifiant les personnes impliquées dans une conversation (y compris leurs URI et avatars).

-   Notifications peuvent maintenant afficher des images. 

L’exemple suivant illustre comment utiliser les nouvelles API pour générer une notification qui contient une image. Dans les captures d’écran suivante, une notification de texte est validée et est suivie d’une notification avec une image incorporée. Lorsque les notifications sont développées (comme indiqué sur la droite), le texte de la première notification s’affiche et l’image incorporée dans la deuxième notification est agrandie :

[![Exemple de notification avec l’image](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

L’exemple suivant montre comment inclure une image dans une notification à secteurs Android, et illustre l’utilisation de la nouvelle `Person` classe :

1. Créer un `Person` objet qui représente l’expéditeur. Par exemple, le nom et l’icône de l’expéditeur sont inclus dans `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Créer un `Notification.MessagingStyle.Message` qui contient l’image à envoyer, en passant l’image vers le nouveau [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) (méthode).
   Exemple :

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Ajouter le message d’un `Notification.MessagingStyle` objet. Exemple :

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Branchez ce style dans le Générateur de notification. Exemple :

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. La notification de publication. Exemple :

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Pour plus d’informations sur la création de notifications, consultez [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md).


### <a name="indoor-positioning"></a>Positionnement intérieur

Graphique à secteurs Android prend en charge IEEE 802.11mc (également appelé _Wi-Fi Round-Trip-heure_ ou _WiFi RTT_), ce qui rend possible pour les applications détecter la distance à un ou les points d’accès Wi-Fi. À l’aide de ces informations, il est possible pour votre application tirer parti des *positionnement intérieur* avec une précision de deux compteurs. Sur les appareils Android qui prennent en charge le matériel pour IEEE 801.11mc, votre application peut offrir des fonctionnalités de navigation par exemple, en fonction du lieu de contrôle des appareils intelligents ou obtenir des instructions étape par étape dans un magasin :

[![Exemple de navigation intérieur à l’aide de WiFi RTT](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

La nouvelle [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) classe et plusieurs classes d’assistance fournit les moyens de mesure de distance sur des appareils Wi-Fi. Pour plus d’informations sur l’API de positionnement intérieur introduites dans Android P, consultez [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).


### <a name="multi-camera-support"></a>Prise en charge plusieurs caméra

Nombre d’appareils Android plus récent ont des caméras de double-front et/ou double différée qui sont utiles pour des fonctionnalités telles que la vision stéréo, effets visuels améliorés et la fonctionnalité de zoom améliorée. P Android introduit une nouvelle [plusieurs caméras](https://developer.android.com/about/versions/pie/android-9.0#camera) API qui permet à votre application d’utiliser un *caméra logique* (ou *logique plusieurs caméras*) qui repose sur deux ou plusieurs caméras de surveillance physiques.
Pour déterminer si l’appareil prend en charge une caméra multiples logique, vous pouvez examiner les fonctionnalités de chaque appareil photo sur l’appareil pour voir si elle prend en charge [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

Graphique à secteurs Android inclut également un nouveau [Configurationsession](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) classe qui peut être utilisée pour aider à réduire les délais durant la capture initiale et éliminent le besoin pour démarrer et lancer le flux de l’appareil photo.

Pour plus d’informations sur l’appareil photo multi prennent en charge dans Android P, consultez [plusieurs caméras photo et prise en charge les mises à jour](https://developer.android.com/about/versions/pie/android-9.0#camera).


### <a name="other-features"></a>Autres fonctionnalités

En outre, à secteurs Android prend en charge plusieurs autres nouvelles fonctionnalités :

-   La nouvelle [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) (classe), qui peut être utilisé pour le dessin et afficher des images animées.

-   Un nouveau [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) classe qui remplace `BitmapFactory`. `ImageDecoder` peut être utilisé pour décoder un `AnimatedImageDrawable`.

-   Prise en charge des images pour la vidéo HDR (TRES dynamique) et HEIF (haute efficacité Image File Format).

-   Le [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) a été amélioré pour gérer plus intelligemment les tâches liées au réseau. La nouvelle [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) méthode de la [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) classe retourne la meilleure réseau pour effectuer toutes les demandes réseau pour un travail donné.

Pour plus d’informations sur les dernières fonctionnalités de graphique à secteurs Android, consultez [9 Android fonctionnalités et API](https://developer.android.com/about/versions/pie/android-9.0).


## <a name="behavior-changes"></a>Changements de comportement

Lorsque la Version Android cible est définie au niveau de l’API 28, il existe plusieurs modifications de plateforme qui peuvent affecter le comportement de votre application même si vous n’implémentez pas les nouvelles fonctionnalités décrites ci-dessus. La liste suivante est un bref résumé de ces modifications :

-  Applications doivent désormais demander des autorisations de premier plan avant d’utiliser les services de premier plan.

-  Si votre application comporte plusieurs processus, il ne peut pas partager un même [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) répertoire de données entre plusieurs processus.

-  Accès direct à répertoire de données d’une autre application par chemin d’accès n’est plus autorisée.

Pour plus d’informations sur les modifications de comportement pour les applications qui ciblent Android P, consultez [changements de comportement](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps).


## <a name="sample-code"></a>Exemple de code

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) est un exemple d’application Xamarin.Android pour Android à secteurs qui montre comment définir les modes d’affichage découpage, comment utiliser la nouvelle `Person` classe et comment envoyer une notification qui inclut une image.


## <a name="summary"></a>Récapitulatif

Cet article a introduit Android à secteurs et expliqué comment installer et configurer les derniers outils et packages pour le développement Xamarin.Android avec Android à secteurs. Il a fourni une vue d’ensemble des principales fonctionnalités disponibles dans le graphique à secteurs Android, exemple de code source pour plusieurs de ces fonctionnalités.
Il inclus des liens vers la documentation de l’API et les rubriques du développeur Android pour vous aider à commencer en créant des applications pour Android à secteurs. Elle également mis en surbrillance les modifications de comportement à secteurs Android plus importantes qui peut affecter les applications existantes.


## <a name="related-links"></a>Liens connexes

- [Graphique à secteurs 9 Android](https://developer.android.com/about/versions/pie/)
