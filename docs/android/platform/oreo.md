---
title: Fonctionnalités de Oreo
description: La prise en main à l’aide de Xamarin.Android pour développer des applications pour la dernière version d’Android.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: 24a9fa0e954ddba1451ba8bf98216550d7d70b51
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61196784"
---
# <a name="oreo-features"></a>Fonctionnalités de Oreo

_La prise en main à l’aide de Xamarin.Android pour développer des applications pour la dernière version d’Android._

[Android 8.0 Oreo](https://developer.android.com/index.html) est la dernière version d’Android disponible à partir de Google. Android Oreo offre de nombreuses fonctionnalités nouvelles d’intérêt pour les développeurs Xamarin.Android. Ces fonctionnalités incluent des canaux de notification, des badges de notification, des polices personnalisées dans XML, les polices téléchargeables, le remplissage automatique et une image dans l’image (PIP). Android Oreo inclut de nouvelles API pour ces nouvelles fonctionnalités, et ces API est disponibles pour les applications Xamarin.Android lorsque vous utilisez Xamarin.Android 8.0 et versions ultérieures.

[![Image de héros Android Oreo](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Cet article est structuré pour vous aider à commencer le développement d’applications Xamarin.Android pour Android 8.0 Oreo. Il explique comment installer les mises à jour nécessaires, configurer le Kit de développement et créer un émulateur (ou périphérique) pour le test. Il fournit également un aperçu des nouvelles fonctionnalités de Android 8.0 Oreo, avec des liens vers des exemples d’applications qui illustrent l’utilisation des fonctionnalités d’Android Oreo dans les applications Xamarin.Android.


## <a name="requirements"></a>Configuration requise

Les éléments suivants sont nécessaire pour utiliser les fonctionnalités d’Android Oreo dans les applications basées sur Xamarin :

-   **Visual Studio** &ndash; si vous utilisez Windows, version 15.5 ou version ultérieure de Visual Studio est requise.  Si vous utilisez un Mac, Visual Studio pour Mac version 7.2.0 est requis.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 ou version ultérieure doit être installé et configuré avec Visual Studio.

-   **Kit de développement logiciel Android** &ndash; Android SDK 8.0 (API 26) ou version ultérieure doit être installé via le gestionnaire Android SDK.



## <a name="getting-started"></a>Prise en main

Pour commencer à l’aide d’Android Oreo avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages de kit de développement logiciel avant de pouvoir créer un projet Android Oreo :

1. Mettre à jour vers la dernière version de Visual Studio.

2. Installer le **8.0.0 Android (API 26)** ou version ultérieure des packages et outils via le Gestionnaire de kit de développement logiciel.

3. Créer un nouveau projet Xamarin.Android qui cible Android Oreo (API 26).

4. Configurer un émulateur ou un appareil pour tester des applications d’Android Oreo.

Chacune de ces étapes est expliquée dans les sections suivantes :

### <a name="update-visual-studio-and-xamarinandroid"></a>Mettre à jour Xamarin.Android et Visual Studio

Pour ajouter la prise en charge Android Oreo à Visual Studio, procédez comme suit :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Pour Visual Studio 2019, utilisez le [SDK Manager](~/android/get-started/installation/android-sdk.md) pour installer le niveau d’API 26.0 ou version ultérieur.

- Si vous utilisez Visual Studio 2017 :

    1. Mise à jour de Visual Studio 2017 version 15.7 ou version ultérieure (voir [mise à jour Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Utilisez le [SDK Manager](~/android/get-started/installation/android-sdk.md) pour installer le niveau d’API 26.0 ou version ultérieur.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

- Mettre à jour vers la dernière version stable de Visual Studio pour Mac comme expliqué dans [la mise à jour de Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/update).

-----

Pour plus d’informations sur la prise en charge de Xamarin pour Android Oreo, consultez le [notes de publication de Xamarin.Android 8.0](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/).



### <a name="install-the-android-sdk"></a>Installez le SDK Android

Pour créer un projet avec Xamarin.Android 8.0, vous devez d’abord utiliser le gestionnaire Xamarin Android SDK pour installer la plateforme du Kit de développement logiciel pour **Android 8.0 - Oreo** ou version ultérieure. Vous devez également installer d’Android SDK Tools 26.0 ou version ultérieure.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Démarrez le Gestionnaire du Kit de développement logiciel (dans Visual Studio, cliquez sur **Outils > Android > Gestionnaire Android SDK**).

2. Installer le **Android 8.0 - Oreo** packages. Si vous utilisez l’émulateur du Kit Android SDK, veillez à inclure le **x86** images du système dont vous aurez besoin :

    [![Packages Android 8.0 en sélectionnant dans le Gestionnaire de kit de développement logiciel Android.](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Installer **Android SDK Tools 26.0.2** ou version ultérieure, **Android SDK Platform-Tools 26.0.0** ou version ultérieure, et **Android SDK Build-Tools 26.0.0** (ou version ultérieure) :

    [![Sélection d’Android SDK Tools 26 dans le gestionnaire Android SDK](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Démarrez le Gestionnaire du Kit de développement logiciel (dans Visual Studio pour Mac, cliquez sur **Outils > Gestionnaire du kit SDK**).

2. Installer le **Android 8.0 - Oreo** packages du Kit de développement logiciel. Si vous utilisez l’émulateur du Kit Android SDK, veillez à inclure le **x86** images du système dont vous aurez besoin :

    [![Sélection des packages Android 8.0 dans le Gestionnaire de kit de développement logiciel](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Installer **Android SDK Tools 26.0.2** ou version ultérieure, **Android SDK Platform-Tools 26.0.0** ou version ultérieure, et **Android SDK Build-Tools 26.0.0** (ou version ultérieure) :

    [![Sélection d’Android SDK Tools 26 dans le Gestionnaire du Kit de développement logiciel](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous êtes novice en développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Xamarin.Android.

Lorsque vous créez un projet Android, vous devez configurer les paramètres de version à la cible Android 8.0 ou version ultérieure. Par exemple, pour cibler votre projet pour Android 8.0, vous devez configurer le niveau d’API Android de cible de votre projet pour **Android 8.0 (API 26)**. Il est recommandé de définir également votre niveau de framework cible vers l’API 26 ou version ultérieure. Pour plus d’informations sur la configuration des niveaux de niveau d’API Android, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Configurer un émulateur ou un périphérique

Si vous tentez de lancer la valeur par défaut en fonction de l’interface graphique utilisateur Google AVD Manager après l’installation d’Android SDK Tools 26.0 d’ou version ultérieure, vous pouvez obtenir la boîte de dialogue erreur suivante, qui vous indique d’utiliser le gestionnaire AVD ligne de commande **avdmanager** à la place :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Boîte de dialogue Avertissement Gestionnaire d’émulateur Android](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Boîte de dialogue Avertissement Gestionnaire d’émulateur Android](oreo-images/mac/03-avd-warning.png)

-----

Ce message s’affiche, car Google ne fournit plus autonome Gestionnaire AVD de l’interface graphique utilisateur qui prend en charge les API 26.0 et versions ultérieur. Pour Android 8.0 Oreo, vous devez utiliser le Gestionnaire d’émulateur Android Xamarin ou la ligne de commande `avdmanager` outil pour créer des appareils virtuels pour Android Oreo.

Pour utiliser le Gestionnaire d’appareils Android pour créer et gérer des appareils virtuels, consultez [la gestion des appareils virtuels avec le Gestionnaire d’appareils Android](~/android/get-started/installation/android-emulator/device-manager.md).
Pour créer des appareils virtuels sans Android Device Manager, suivez les étapes décrites dans la section suivante.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Création avdmanager virtuel à l’aide de périphériques

Pour utiliser **avdmanager** pour créer un nouvel appareil virtuel, procédez comme suit :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Ouvrez une fenêtre d’invite de commandes et définissez `JAVA_HOME` à l’emplacement du SDK Java sur votre ordinateur. Pour une installation classique de Xamarin, vous pouvez utiliser la commande suivante :

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Ajouter l’emplacement du SDK Android `bin` dossier pour votre `PATH`.
    Pour une installation classique de Xamarin, vous pouvez utiliser la commande suivante :

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Fermez la fenêtre d’invite de commandes et ouvrez une nouvelle fenêtre d’invite de commandes. Créer un nouvel appareil virtuel à l’aide de la [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) commande. Par exemple, pour créer un AVD nommé **AVD-Oreo-8.0** à l’aide de la x86 image du système pour le niveau d’API 26, utilisez la commande suivante :

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Lorsque vous êtes invité avec **vous souhaitez créer un profil matériel personnalisé [no]** vous pouvez entrer **aucun** et acceptez le profil matériel par défaut. Si vous dites **Oui**, **avdmanager** vous invitera à une liste de questions permettant de personnaliser le profil matériel.

Après avoir **avdmanager** pour créer votre appareil virtuel, elle sera incluse dans le menu déroulant des appareils :

[![Nouvel AVD ajouté au menu déroulant des appareils](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1.  Ouvrir un **Terminal** fenêtre et basculez vers l’emplacement du répertoire d’outils du Kit Android SDK sur votre Mac. Pour une installation classique de Xamarin, vous pouvez utiliser la commande suivante :

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Créer un nouvel appareil virtuel à l’aide de la [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) commande. Par exemple, pour créer un AVD nommé **AVD-Oreo-8.0** à l’aide de la x86 image du système pour le niveau d’API 26, utilisez la commande suivante :

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Lorsque vous êtes invité avec **vous souhaitez créer un profil matériel personnalisé [no]** vous pouvez entrer **aucun** et acceptez le profil matériel par défaut. Si vous dites **Oui**, **avdmanager** vous invitera à une liste de questions permettant de personnaliser le profil matériel.

Une fois que vous utilisez **avdmanager** pour créer votre appareil virtuel, elle sera incluse dans le menu déroulant des appareils :

[![Nouvel AVD ajouté au menu déroulant des appareils](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Pour plus d’informations sur la configuration d’un émulateur Android pour tester et déboguer, consultez [débogage sur l’émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md).

Si vous utilisez un périphérique physique tel qu’un Nexus ou d’un Pixel, vous pouvez mettre à jour de votre appareil via automatique sur les mises à jour de l’air (OTA) ou télécharger une image de système et flash directement de votre appareil. Pour plus d’informations sur la mise à jour manuelle de votre appareil pour Android Oreo, consultez [fabrique des Images pour les appareils de Pixel et de Nexus](https://developers.google.com/android/images).



## <a name="new-features"></a>Nouvelles fonctionnalités

Android Oreo propose de nouvelles fonctionnalités et fonctions, telles que les canaux de notification, badges de notification, des polices personnalisées au format XML, les polices téléchargeables, le remplissage automatique et d’image. Les sections suivantes mettent en évidence ces fonctionnalités et fournissent des liens pour vous aider à commencer à les utiliser dans votre application.



### <a name="notification-channels"></a>Canaux de notification

*Canaux de notification* sont des catégories définies par l’application pour les notifications.
Vous pouvez créer un canal de notification pour chaque type de notification que vous devez envoyer, et vous pouvez créer des canaux de notification pour refléter les choix effectués par les utilisateurs de votre application. La nouvelle fonctionnalité de canaux de notification rend possible pour vous permettre de fournir aux utilisateurs un contrôle affiné sur différents types de notifications. Par exemple, si vous implémentez une application de messagerie, vous pouvez créer des canaux de notification distinct pour chaque groupe de conversations est créé par un utilisateur.

[Canaux de notification](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) explique comment créer un canal de notification et l’utiliser pour la validation des notifications locales. Pour obtenir un exemple de code réel, consultez la [canaux de notification](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) exemple ; cet exemple d’application gère deux canaux et définit les options de notification supplémentaire.



### <a name="notification-badges"></a>Badges de notification

Badges de notification sont de petits points qui apparaissent sur les icônes d’application, comme indiqué dans cette capture d’écran :

[![Badges de notification d’exemple sur les icônes d’application](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Ces points indiquent qu’il existe de nouvelles notifications pour un ou plusieurs canaux de notification dans l’application associée à cette icône application &ndash; il s’agit de notifications que l’utilisateur n’a pas encore été fermée ou réactions. Les utilisateurs peuvent long-appuyez sur une icône pour coup les notifications associées à un badge de notification, ignorez ou agissant sur les notifications dans le menu de longue durée, appuyez sur ce appeaars.

Pour plus d’informations sur les badges de notification, consultez le développeur Android [Notification Badges](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) rubrique.



### <a name="custom-fonts-in-xml"></a>Polices personnalisées dans XML

Introduit Android Oreo *polices dans XML*, ce qui rend possible, vous pouvez incorporer des polices personnalisées en tant que ressources. OpenType (**.otf**) et TrueType (**.ttf**) de formats de police sont pris en charge. Pour ajouter des polices en tant que ressources, procédez comme suit :

1. Créer un **ressources/police** dossier.

2. Copiez vos fichiers de police (exemple, **.ttf** et **.otf** fichiers) à **ressources/police**. 

3. Si nécessaire, renommez chaque fichier de polices afin qu’il respecte le conventions de nom de fichier Android (par exemple, utilisez uniquement en minuscules *d’a à z*, *0-9*et des traits de soulignement dans les noms de fichiers). Par exemple, le fichier de police `Pacifico-Regular.ttf` a pu être renommé en quelque chose comme `pacifico.ttf`.

4. Appliquer la police personnalisée à l’aide de la nouvelle `android:fontFamily` attribut dans la disposition de XML. Par exemple, ce qui suit `TextView` déclaration utilise la **pacifico.ttf** ressource de police :

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Vous pouvez également créer un fichier XML family de la police qui décrit plusieurs polices, ainsi que les détails de poids et de style. Pour plus d’informations, consultez le développeur Android [polices dans XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) rubrique.


### <a name="downloadable-fonts"></a>Polices téléchargeables

À compter d’Android Oreo, les applications peuvent demander des polices à partir d’un fournisseur, plutôt que de leur regroupement dans le fichier APK. Les polices sont téléchargées à partir du réseau uniquement en fonction des besoins. Cette fonctionnalité réduit la taille des APK, la conservation d’un téléphone cellulaire mémoire et l’utilisation des données. Vous pouvez également utiliser cette fonctionnalité sur les versions d’API Android 14 et versions ultérieures en installant le package Android 26 de bibliothèque de prise en charge.

Lorsque votre application a besoin d’une police, vous créez un `FontsRequest` objet (en spécifiant la police à télécharger) et transmettez-le à un `FontsContract` méthode pour télécharger la police. Les étapes suivantes décrivent le processus de téléchargement de police plus en détail :

1.  Instancier un [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) objet. 

2.  Sous-classe et instancier [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implémentez le [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) (méthode), qui est utilisé pour gérer l’achèvement de la demande de police.

4.  Implémentez le [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) (méthode), qui est utilisé pour informer votre application de toutes les erreurs qui se produisent pendant le processus de demande de police.

5.  Appelez le [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) méthode pour récupérer la police à partir du fournisseur de police. 

Lorsque vous appelez le `RequestFonts` (méthode), il vérifie d’abord pour voir si la police est mis en cache localement (à partir d’un appel précédent à `RequestFont`). Si elle n’est pas mise en cache, il appelle le fournisseur de police, récupère la police de façon asynchrone, puis transmet les résultats à votre application en appelant votre `OnTypeFaceRetrieved` (méthode).

Le [polices téléchargeables](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) exemple montre comment utiliser la fonctionnalité de polices téléchargeables introduite dans Oreo Android. 

Pour plus d’informations sur le téléchargement des polices, consultez le développeur Android [polices téléchargeables](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) rubrique.



### <a name="autofill"></a>Remplissage automatique

La nouvelle _le remplissage automatique_ framework dans Oreo Android plus facilement aux utilisateurs de gérer les tâches répétitives telles que la connexion, la création du compte et les transactions de la carte de crédit. Les utilisateurs passent moins de temps à nouveau de taper des informations (ce qui peuvent entraîner des erreurs d’entrée). Avant que votre application peut fonctionner avec le Framework de remplissage automatique, un service de remplissage automatique doit être activé dans les paramètres système (les utilisateurs peuvent activer ou désactiver le remplissage automatique).

Le [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) exemple illustre l’utilisation de l’infrastructure de remplissage automatique. Il inclut les implémentations du client activités avec des vues qui doit être autofilled et un Service qui peut fournir des données de remplissage automatique pour les activités de client.

Pour plus d’informations sur la nouvelle fonctionnalité de remplissage automatique et comment optimiser votre application pour le remplissage automatique, consultez le développeur Android [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html) rubrique.



### <a name="picture-in-picture-pip"></a>Picture in Picture (PIP)

Android Oreo rend possible pour une activité lancer le mode d’image (PIP), recouvrant l’écran d’une autre activité. Cette fonctionnalité est conçue pour la lecture vidéo.

Pour spécifier que les activités de votre application peuvent utiliser le mode PIP, définissez l’indicateur sur true dans le manifeste Android :

```xml
android:supportsPictureInPicture
```

Pour spécifier le comportement de votre activité lorsqu’elle est en mode PIP, vous utilisez la nouvelle [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) objet. `PictureInPictureParams` représente un ensemble de paramètres que vous utilisez pour initialiser et mettre à jour d’une activité en mode PIP (par exemple, de l’activité préférée proportions). Les nouvelles méthodes PIP suivantes ont été ajoutées à `Activity` dans Oreo Android :

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; place l’activité en mode PIP. L’activité est placée dans le coin de l’écran, et le reste de l’écran est rempli avec l’activité précédente qui était à l’écran.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; met à jour les paramètres de configuration de l’activité PIP (par exemple, une modification dans le rapport hauteur / largeur).

Le [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) exemple illustre l’utilisation de base du mode d’image (PiP) pour les périphériques de poche introduite dans Oreo. L’exemple lit une vidéo qui se poursuit sans interruption lors du changement dans les deux sens entre les modes d’affichage ou d’autres activités.



### <a name="other-features"></a>Autres fonctionnalités

Android Oreo contient de nombreuses autres nouvelles fonctionnalités telles que les limites de Emoji prise en charge de bibliothèque, les API de l’emplacement, en arrière-plan, pour les applications, nouveaux codecs audio, des améliorations de WebView, prise en charge de la navigation au clavier améliorée et une nouvelle API AAudio (audio pro) pour une gamme de couleurs hautes performances à faible latence audio, pour plus d’informations sur ces fonctionnalités, consultez le développeur Android [Android Oreo fonctionnalités et API](https://developer.android.com/about/versions/oreo/android-8.0.html) rubrique.



## <a name="behavior-changes"></a>Changements de comportement

Android Oreo inclut une variété de système et les changements de comportement d’API qui peuvent avoir un impact sur les fonctionnalités des applications existantes. Ces modifications sont décrites comme suit.


### <a name="background-execution-limits"></a>Limites de l’exécution en arrière-plan

Pour améliorer l’expérience utilisateur, Android Oreo impose des limitations aux applications réalisables lors de l’exécution en arrière-plan. Par exemple, si l’utilisateur est regarder une vidéo ou un jeu, une application en cours d’exécution en arrière-plan peut gêner le fonctionnement de la performance d’une application vidéo gourmandes en cours d’exécution au premier plan. Par conséquent, Android Oreo impose les restrictions suivantes sur les applications qui ne sont pas interagir directement avec l’utilisateur :

1.  **Limitations du Service d’arrière-plan** &ndash; lorsqu’une application s’exécute en arrière-plan, il dispose d’une fenêtre de plusieurs minutes dans laquelle il est toujours autorisé à créer et utiliser des services. Arrête le service de l’application en arrière-plan et la traite comme étant à la fin de cette fenêtre, Android _inactif_.

2.  **Limitations de diffusion** &ndash; Android 7.0 (API 25) concernant les limitations de diffusions une application s’inscrit pour recevoir. Android Oreo effectue ces limitations plus strictes. Par exemple, les applications Android Oreo ne peuvent plus enregistrer des récepteurs de diffusion pour les diffusions implicites dans leur manifeste.

Pour plus d’informations sur les nouvelles limites de l’exécution en arrière-plan, consultez le développeur Android [limites de l’exécution en arrière-plan](https://developer.android.com/about/versions/oreo/background.html) rubrique.


### <a name="breaking-changes"></a>Modifications avec rupture

Applications qui ciblent Android Oreo ou une version ultérieure doivent modifier leurs applications pour prendre en charge les modifications suivantes, le cas échéant :

- Android Oreo déconseille la possibilité de définir la priorité de chaque notification. Au lieu de cela, vous définissez un niveau d’importance recommandée lors de la création d’un canal de notification. Le niveau d’importance que vous affectez à un canal de notification s’applique à tous les messages de notification que vous publiez à celui-ci.

- Pour les applications ciblant Android Oreo, `PendingIntent.GetService()` ne fonctionne pas en raison de nouvelles limites concernant les services démarrés en arrière-plan. Si vous ciblez Android Oreo, vous devez utiliser [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) à la place.  


## <a name="sample-code"></a>Exemple de code

Plusieurs exemples de Xamarin.Android sont disponibles pour vous montrer comment tirer parti des fonctionnalités d’Android Oreo :

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) montre comment utiliser le nouveau système de canaux de Notification introduit dans Android Oreo. Cet exemple gère deux canaux de notification : un avec l’importance de la valeur par défaut et l’autre avec une importance haute.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) illustre l’utilisation de base du mode d’image (PiP) pour les périphériques de poche introduite dans Oreo. L’exemple lit une vidéo qui se poursuit sans interruption lors du changement dans les deux sens entre les modes d’affichage ou d’autres activités.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) illustre l’utilisation de l’infrastructure de remplissage automatique. Il inclut les implémentations du client activités avec des vues qui doit être autofilled et un Service qui peut fournir des données de remplissage automatique pour les activités de client.

-   [Polices téléchargeables](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) fournit un exemple montrant comment utiliser la fonctionnalité de polices téléchargeables décrite précédemment.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) illustre l’utilisation de la bibliothèque de prise en charge EmojiCompat. Vous pouvez utiliser cette bibliothèque pour empêcher votre application de montrant les caractères emoji manquants comme des caractères de « tofu ».

-   [Emplacement des mises à jour en attente intention](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) illustre l’utilisation de l’API d’emplacement pour obtenir les mises à jour sur l’emplacement d’un appareil à l’aide un `PendingIntent`.

-   [Service de premier plan de mises à jour de localisation](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) montre comment utiliser l’API de l’emplacement pour obtenir les mises à jour sur l’emplacement d’un appareil à l’aide d’un service lié et prise en main de premier plan.


## <a name="video"></a>Vidéo

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Développement d’Android Oreo 8.0 avec c#**


## <a name="summary"></a>Récapitulatif

Cet article a introduit Android Oreo et expliqué comment installer et configurer les derniers outils et packages pour le développement de Xamarin.Android sur Android Oreo. Il a fourni une vue d’ensemble des principales fonctionnalités disponibles dans Oreo Android, des liens vers des exemples de code source pour plusieurs nouvelles fonctionnalités. Il inclus des liens vers la documentation de l’API et les rubriques du développeur Android pour vous aider à commencer en créant des applications pour Android Oreo. Elle également mis en surbrillance les modifications de comportement Android Oreo plus importantes qui peut affecter les applications existantes.


## <a name="related-links"></a>Liens associés

- [Android 8.0 Oreo](https://developer.android.com/index.html)
