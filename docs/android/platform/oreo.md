---
title: Fonctionnalités de Oreo
description: Comment démarrer à l’aide de Xamarin.Android à développer des applications pour la dernière version d’Android.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a23072427a74119bfa339fea8a695cd13b775685
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935101"
---
# <a name="oreo-features"></a>Fonctionnalités de Oreo

_Comment démarrer à l’aide de Xamarin.Android à développer des applications pour la dernière version d’Android._

[Android Oreo 8.0](https://developer.android.com/index.html) la dernière version d’Android n’est disponible à partir de Google. Android Oreo offre de nombreuses fonctionnalités nouvelles d’intérêt pour les développeurs de Xamarin.Android. Ces fonctionnalités sont notamment les canaux de notification, des badges de notification, des polices personnalisées dans XML, les polices téléchargeables, le remplissage automatique et une image dans l’image (PIP). Android Oreo inclut de nouvelles API pour ces nouvelles fonctions, et ces API sont disponibles pour les applications Xamarin.Android lorsque vous utilisez Xamarin.Android 8.0 et versions ultérieures.

[![Image de héros Oreo Android](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Cet article est conçu pour vous aider à démarrer dans le développement d’applications de Xamarin.Android pour Android 8.0 Oreo. Il explique comment installer les mises à jour nécessaires, configurer le Kit de développement et créer un émulateur (ou périphérique) pour le test. Il fournit également une description des nouvelles fonctionnalités dans Android 8.0 Oreo, avec des liens vers des exemples d’applications qui illustrent l’utilisation des fonctionnalités de Oreo Android dans les applications Xamarin.Android.


## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les fonctionnalités Oreo Android dans les applications Xamarin :

-   **Visual Studio** &ndash; si vous utilisez des fenêtres, 15,5 ou version ultérieure de Visual Studio est requise.  Si vous utilisez un Mac, Visual Studio pour Mac version 7.2.0 est requis.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 ou version ultérieure doit être installé et configuré avec Visual Studio.

-   **Kit de développement logiciel Android** &ndash; 8.0 SDK Android (API 26) ou version ultérieure doit être installé via le Gestionnaire de kit de développement logiciel Android.



## <a name="getting-started"></a>Prise en main

Pour commencer à l’aide d’Android Oreo avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages SDK avant de pouvoir créer un projet Android Oreo :

1. Mettre à jour vers la dernière version de Visual Studio.

2. Installer le **8.0.0 Android (API 26)** ou version ultérieure des packages et outils via le Gestionnaire de kit de développement logiciel.

3. Créer un nouveau projet Xamarin.Android qui cible Oreo Android (API 26).

4. Configurer un émulateur ou un appareil Android Oreo applications de test.

Chacune de ces étapes est expliquée dans les sections suivantes :



### <a name="update-visual-studio-and-xamarinandroid"></a>Xamarin.Android et mise à jour de Visual Studio

Pour ajouter la prise en charge Android Oreo à Visual Studio, procédez comme suit :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Si vous utilisez Visual Studio 2017 : 

    1. Mise à jour de Visual Studio 2017 15,5 ou version ultérieure (voir [mise à jour Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio)).

    2. Utilisez le Gestionnaire du Kit de développement logiciel [des instructions d’installation](~/android/get-started/installation/android-sdk.md#installation) pour installer le Gestionnaire du Kit de développement Xamarin.
       Le Gestionnaire de SDK Xamarin doit être installé car Google ne fournit pas une dotée du Gestionnaire de SDK de l’interface graphique utilisateur qui prend en charge les API 26.0 et versions ultérieur.

-   Si vous utilisez Visual Studio 2015, nous nous vous recommandons de rétrograder les outils du Kit de développement logiciel à 25 et à l’aide de l’ancien Gestionnaire d’émulateur Google l’interface graphique utilisateur. Outils du Kit de développement logiciel 25 peuvent toujours être utilisés en même temps que les API 26, 27 et les versions ultérieures et n’affecte pas le développement de nouvelles plateformes. Cela vous donnera une interface pour la gestion de votre Kit de développement logiciel Android pour les versions antérieures de Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

-   Mettre à jour vers la dernière version stable de Visual Studio 2017 pour Mac comme expliqué dans [mise à jour de Visual Studio pour Mac](https://docs.microsoft.com/en-us/visualstudio/mac/update).

-----

Pour plus d’informations sur la prise en charge de Xamarin pour Oreo Android, consultez le [Xamarin.Android 8.0 notes de publication](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Installez le SDK Android

Pour créer un projet avec Xamarin.Android 8.0, vous devez d’abord utiliser le Gestionnaire de kit de développement logiciel Android Xamarin pour installer la plateforme du Kit de développement logiciel pour **8.0 Android - Oreo** ou version ultérieure. Vous devez également installer les outils de kit de développement logiciel Android 26.0 ou version ultérieures.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Démarrez le Gestionnaire de kit de développement logiciel (dans Visual Studio, cliquez sur **Outils > Android > Gestionnaire de SDK Android**).

2. Installer le **8.0 Android - Oreo** packages. Si vous utilisez l’émulateur Android SDK, veillez à inclure le **x86** images du système que vous devez :

    [![Sélectionner les packages de 8.0 Android dans le Gestionnaire de kit de développement logiciel Android.](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Installer **Outils kit de développement logiciel Android 26.0.2** ou version ultérieure, **Android SDK outils de plateforme 26.0.0** ou version ultérieure, et **Android SDK outils de génération 26.0.0** (ou version ultérieure) :

    [![Sélection des outils de kit de développement logiciel Android 26 dans le Gestionnaire du SDK Android](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Démarrez le Gestionnaire de kit de développement logiciel (Visual Studio pour Mac, cliquez sur **Outils > Gestionnaire de SDK**).

2. Installer le **8.0 Android - Oreo** packages SDK. Si vous utilisez l’émulateur Android SDK, veillez à inclure le **x86** images du système que vous devez :

    [![Sélection des packages 8.0 Android dans le Gestionnaire de kit de développement logiciel](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Installer **Outils kit de développement logiciel Android 26.0.2** ou version ultérieure, **Android SDK outils de plateforme 26.0.0** ou version ultérieure, et **Android SDK outils de génération 26.0.0** (ou version ultérieure) :

    [![Sélection des outils de kit de développement logiciel Android 26 dans le Gestionnaire du SDK](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous ne connaissez pas le développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets de Xamarin.Android.

Lorsque vous créez un projet Android, vous devez configurer les paramètres de la version à la cible Android version 8.0 ou ultérieure. Par exemple, pour cibler votre projet pour Android 8.0, vous devez configurer le niveau d’API Android de cible de votre projet pour **8.0 Android (API 26)**. Il est recommandé de définir également votre niveau de framework cible à 26 d’API ou une version ultérieure. Pour plus d’informations sur la configuration des niveaux de niveau d’API Android, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Configurer un émulateur ou un périphérique

Si vous essayez de lancer la valeur par défaut du gestionnaire AVD basé sur une interface graphique utilisateur Google après l’installation de 26.0 d’outils Kit de développement logiciel Android ou une version ultérieure, vous risquez d’obtenir la boîte de dialogue erreur suivantes, qui vous indique d’utiliser l’outil de gestionnaire de ligne de commande AVD **avdmanager** à la place :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Boîte de dialogue Gestionnaire de l’émulateur Android avertissement](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Boîte de dialogue Gestionnaire de l’émulateur Android avertissement](oreo-images/mac/03-avd-warning.png)

-----

Ce message s’affiche car Google ne fournit plus d’une dotée du gestionnaire AVD de l’interface graphique utilisateur qui prend en charge les API 26.0 et versions ultérieur. Pour Android 8.0 Oreo, vous devez utiliser le Gestionnaire d’émulateur Android Xamarin ou la ligne de commande `avdmanager` outil pour créer des périphériques virtuels pour Oreo Android.

Pour utiliser le Gestionnaire de périphériques Android pour créer et gérer des appareils virtuels, consultez [la gestion de périphériques virtuels avec le Gestionnaire de périphériques Android](~/android/get-started/installation/android-emulator/device-manager.md).
Pour créer des périphériques virtuels sans le Gestionnaire de périphériques Android, suivez les étapes décrites dans la section suivante.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Création d’avdmanager à l’aide de périphériques virtuels

Pour utiliser **avdmanager** pour créer un nouveau périphérique virtuel, procédez comme suit :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Ouvrez une fenêtre d’invite de commandes et définissez `JAVA_HOME` à l’emplacement du Kit de développement Java sur votre ordinateur. Pour une installation classique de Xamarin, vous pouvez utiliser la commande suivante :

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Ajoutez l’emplacement du SDK Android `bin` dossier à votre `PATH`.
    Pour une installation classique de Xamarin, vous pouvez utiliser la commande suivante :

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Fermez la fenêtre d’invite de commandes et ouvrir une nouvelle fenêtre d’invite de commandes. Créer un nouveau périphérique virtuel à l’aide de la [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) commande. Par exemple, pour créer un AVD nommé **AVD-Oreo-8.0** à l’aide de la x86 image du système pour le niveau de l’API 26, utilisez la commande suivante :

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Lorsque vous êtes invité à **vous souhaitez créer un profil matériel personnalisé [no]** que vous pouvez entrer **aucun** et accepter le profil matériel par défaut. Si vous dites **Oui**, **avdmanager** vous invitera avec une liste de questions pour personnaliser le profil matériel.

Après avoir **avdmanager** pour créer votre appareil virtuel, elle sera incluse dans le menu déroulant de périphérique :

[![AVD de nouveau ajouté au menu déroulant de périphérique](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Ouvrir un **Terminal** fenêtre et indiquez l’emplacement du répertoire des outils de kit de développement logiciel Android sur votre Mac. Pour une installation classique de Xamarin, vous pouvez utiliser la commande suivante :

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Créer un nouveau périphérique virtuel à l’aide de la [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) commande. Par exemple, pour créer un AVD nommé **AVD-Oreo-8.0** à l’aide de la x86 image du système pour le niveau de l’API 26, utilisez la commande suivante :

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Lorsque vous êtes invité à **vous souhaitez créer un profil matériel personnalisé [no]** que vous pouvez entrer **aucun** et accepter le profil matériel par défaut. Si vous dites **Oui**, **avdmanager** vous invitera avec une liste de questions pour personnaliser le le profil matériel.

Une fois que vous utilisez **avdmanager** pour créer votre appareil virtuel, elle sera incluse dans le menu déroulant de périphérique :

[![AVD de nouveau ajouté au menu déroulant de périphérique](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Pour plus d’informations sur la configuration d’un émulateur Android pour tester et déboguer, consultez [débogage sur l’émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md).

Si vous utilisez un périphérique physique comme un canal ou un Pixel, vous pouvez mettre à jour votre appareil via automatique sur les mises à jour de l’air (OTA) ou télécharger une image système et flash directement de votre appareil. Pour plus d’informations sur la mise à jour manuelle de votre appareil à Oreo Android, consultez [Images fabrique pour les appareils de Pixel et de Nexus](https://developers.google.com/android/images).



## <a name="new-features"></a>Nouvelles fonctionnalités

Android Oreo introduit une variété de nouvelles fonctionnalités et des fonctionnalités, telles que les canaux de notification, badges de notification, des polices personnalisées dans XML, les polices téléchargeables, le remplissage automatique et d’image. Les sections suivantes illustrent ces fonctionnalités et fournissent des liens pour vous aider à commencer à les utiliser dans votre application.



### <a name="notification-channels"></a>Canaux de notification

*Canaux de notification* sont des catégories définies par l’application pour les notifications.
Vous pouvez créer un canal de notification pour chaque type de notification que vous devez envoyer, et vous pouvez créer des canaux de notification pour refléter les choix effectués par les utilisateurs de votre application. La nouvelle fonctionnalité de canaux de notification permet de donner aux utilisateurs un contrôle affiné sur différents types de notifications. Par exemple, si vous implémentez une application de messagerie, vous pouvez créer des canaux de notification distincte pour chaque groupe de conversations est créé par un utilisateur.

[Canaux de notification](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) explique comment créer un canal de notification et l’utiliser pour la validation des notifications locales. Pour obtenir un exemple de code réel, consultez la [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) sample ; cet exemple d’application gère deux canaux et définit les options de notification supplémentaire.



### <a name="notification-badges"></a>Badges de notification

Badges de notification sont de petits points qui apparaissent sur les icônes de l’application, comme indiqué dans cette capture d’écran :

[![Badges de notification d’exemple sur les icônes de l’application](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Ces points indiquent qu’il existe de nouvelles notifications pour un ou plusieurs canaux de notification dans l’application associée à cette icône application &ndash; il s’agit de notifications que l’utilisateur n’a pas encore été ignorés ou réactions. Les utilisateurs peuvent long-appuyez sur une icône pour œil les notifications associées à un badge de notification, ignorez ou agissant sur les notifications à partir du menu de longue durée, appuyez sur ce appeaars.

Pour plus d’informations sur les badges de notification, consultez le développeur Android [Notification Badges](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) rubrique.



### <a name="custom-fonts-in-xml"></a>Polices personnalisées dans XML

Android Oreo introduit *polices dans XML*, ce qui rend possible, vous pouvez incorporer des polices personnalisées en tant que ressources. OpenType (**.otf**) et TrueType (**.ttf**) de formats de police sont pris en charge. Pour ajouter des polices en tant que ressources, procédez comme suit :

1. Créer un **ressources/police** dossier.

2. Copiez vos fichiers de polices (exemple, **.ttf** et **.otf** fichiers) pour **ressources/police**. 

3. Si nécessaire, renommez chaque fichier de polices afin qu’il respecte le conventions d’affectation de noms de fichier Android (par exemple, utilisez uniquement en minuscules *d’a à z*, *0-9*et des traits de soulignement dans les noms de fichier). Par exemple, le fichier de polices `Pacifico-Regular.ttf` a pu être renommé comme `pacifico.ttf`.

4. Appliquer la police personnalisée à l’aide de la nouvelle `android:fontFamily` attribut dans votre mise en forme XML. Par exemple, `TextView` déclaration utilise ajouté **pacifico.ttf** des ressources de police :

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Vous pouvez également créer un fichier XML family de la police qui décrit plusieurs polices, ainsi que les détails de style et de poids. Pour plus d’informations, consultez le développeur Android [polices dans XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) rubrique.


### <a name="downloadable-fonts"></a>Polices téléchargeables

À partir de Oreo Android, les applications peuvent demander des polices à partir d’un fournisseur plutôt que regroupez-les dans APK. Les polices sont téléchargées à partir du réseau uniquement en fonction des besoins. Cette fonctionnalité réduit la taille APK, conserver l’utilisation des données de mémoire et un réseau cellulaire téléphone. Vous pouvez également utiliser cette fonctionnalité sur les versions d’API Android 14 et versions ultérieures en installant le package Android 26 bibliothèque de prise en charge.

Lorsque votre application a besoin d’une police, vous créez un `FontsRequest` objet (qui spécifie la police à télécharger), puis passer à un `FontsContract` méthode pour télécharger la police. Les étapes suivantes décrivent le processus de téléchargement de police plus en détail :

1.  Instancier une [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) objet. 

2.  Sous-classe et instancier [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implémentez la [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) (méthode), qui permet de gérer l’achèvement de la demande de police.

4.  Implémentez la [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) (méthode), qui est utilisé pour informer votre application de toutes les erreurs qui se produisent pendant le processus de demande de police.

5.  Appelez le [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) pour récupérer la police à partir du fournisseur de la police. 

Lorsque vous appelez le `RequestFonts` (méthode), il vérifie si la police est mis en cache localement (à partir d’un appel précédent à `RequestFont`). Si elle n’est pas mise en cache, il appelle le fournisseur de police récupère la police de façon asynchrone et transmet ensuite les résultats vers votre application en appelant votre `OnTypeFaceRetrieved` (méthode).

Le [polices téléchargeables](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) exemple montre comment utiliser la fonctionnalité de polices téléchargeables introduite dans Oreo Android. 

Pour plus d’informations sur le téléchargement de polices, consultez le développeur Android [polices téléchargeables](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) rubrique.



### <a name="autofill"></a>Remplissage automatique

La nouvelle _le remplissage automatique_ framework dans Android Oreo plus facilement aux utilisateurs de gérer les tâches répétitives, telles que la connexion, la création du compte et les transactions de carte de crédit. Les utilisateurs passent moins de temps Retapez les informations (ce qui risquent d’erreurs d’entrée). Avant que votre application puisse fonctionner avec le Framework le remplissage automatique, un service de remplissage automatique doit être activé dans les paramètres système (les utilisateurs peuvent activer ou désactiver le remplissage automatique).

Le [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) exemple illustre l’utilisation de l’infrastructure de recopie incrémentée. Elle comprend des implémentations du client activités avec des vues qui doivent être autofilled et un Service qui peut fournir des données de remplissage automatique au client d’activités.

Pour plus d’informations sur la nouvelle fonctionnalité de remplissage automatique et comment optimiser votre application pour le remplissage automatique, consultez le développeur Android [le remplissage automatique Framework](https://developer.android.com/guide/topics/text/autofill.html) rubrique.



### <a name="picture-in-picture-pip"></a>Image de l’image (PIP)

Android Oreo rend possible pour une activité lancer le mode d’image (PIP), recouvrir l’écran d’une autre activité. Cette fonctionnalité est conçue pour la lecture vidéo.

Pour spécifier que les activités de votre application peuvent utiliser le mode PIP, définissez l’indicateur suivant à la valeur true dans le manifeste Android :

```xml
android:supportsPictureInPicture
```

Pour spécifier le comportement de votre activité lorsqu’il est en mode PIP, vous utilisez la nouvelle [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) objet. `PictureInPictureParams` représente un ensemble de paramètres que vous utilisez pour initialiser et mettre à jour d’une activité dans le mode PIP (par exemple, l’activité préféré proportions). Les nouvelles méthodes PIP suivantes ont été ajoutées à `Activity` dans Oreo Android :

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; place l’activité en mode PIP. L’activité est placée dans l’angle de l’écran, et le reste de l’écran est renseigné avec l’activité précédente a été à l’écran.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; met à jour les paramètres de configuration de l’activité PIP (par exemple, une modification dans le rapport hauteur / largeur).

Le [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) exemple illustre l’utilisation de base du mode d’image (PiP) pour des périphériques de poche introduite dans Oreo. L’exemple lit une vidéo qui se poursuit sans interruption lors du changement dans les deux sens entre les modes d’affichage ou d’autres activités.



### <a name="other-features"></a>Autres fonctionnalités

Android Oreo contient de nombreuses autres nouvelles fonctionnalités telles que les limites d’arrière-plan Emoji prise en charge de bibliothèque, les API de l’emplacement, les applications, nouveaux codecs audio, WebView améliorations, prise en charge de navigation de clavier améliorés et une nouvelle API AAudio (audio pro) pour une gamme de couleurs hautes performances à faible latence audio, pour plus d’informations sur ces fonctionnalités, consultez le développeur Android [Android Oreo fonctionnalités et API](https://developer.android.com/about/versions/oreo/android-8.0.html) rubrique.



## <a name="behavior-changes"></a>Changements de comportement

Android Oreo inclut divers du système et des changements de comportement d’API qui peuvent avoir un impact sur les fonctionnalités des applications existantes. Ces modifications sont décrites comme suit.


### <a name="background-execution-limits"></a>Limites de l’exécution en arrière-plan

Pour améliorer l’expérience utilisateur, Oreo Android impose des limitations sur ce que les applications peuvent faire lors de l’exécution en arrière-plan. Par exemple, si l’utilisateur est regarder une vidéo ou un jeu, une application en cours d’exécution en arrière-plan peut détériorer les performances d’une application de ressources vidéo en cours d’exécution au premier plan. Par conséquent, Oreo Android impose les restrictions suivantes sur les applications qui ne sont pas interagir directement avec l’utilisateur :

1.  **Limitations du Service d’arrière-plan** &ndash; lorsqu’une application s’exécute en arrière-plan, il dispose d’une fenêtre de plusieurs minutes dans laquelle il est toujours autorisé à créer et utiliser des services. Arrête le service de l’application en arrière-plan et le traite comme étant à la fin de cette fenêtre, Android _inactif_.

2.  **Limitations de diffusion** &ndash; Android 7.0 (API 25) placé limitations sur des diffusions une application s’inscrit pour recevoir. Android Oreo rend ces limitations plus strictes. Par exemple, les applications Android Oreo ne peuvent plus enregistrer récepteurs de diffusion pour les diffusions implicites dans leur manifeste.

Pour plus d’informations sur les nouvelles limites de l’exécution en arrière-plan, consultez le développeur Android [limites de l’exécution en arrière-plan](https://developer.android.com/about/versions/oreo/background.html) rubrique.


### <a name="breaking-changes"></a>Modifications avec rupture

Applications qui ciblent Oreo Android ou une version ultérieure doivent y modifier leurs applications pour prendre en charge les modifications suivantes, le cas échéant :

- Android Oreo est déconseillée à la possibilité de définir la priorité de chaque notification. Au lieu de cela, vous définissez un niveau d’importance recommandé lors de la création d’un canal de notification. Le niveau d’importance que vous affectez à un canal de notification s’applique à tous les messages de notification que vous publiez à celui-ci.

- Pour les applications ciblant Android Oreo, `PendingIntent.GetService()` ne fonctionne pas en raison de nouvelles limites placés sur les services démarrés en arrière-plan. Si vous ciblez Android Oreo, vous devez utiliser [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) à la place.  


## <a name="sample-code"></a>Exemple de code

Plusieurs exemples de Xamarin.Android sont disponibles pour vous montrer comment tirer parti des fonctionnalités de Oreo Android :

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) montre comment utiliser le nouveau système de canaux de Notification introduit dans Oreo Android. Cet exemple gère deux canaux de notification : un avec l’importance de la valeur par défaut et l’autre avec une importance haute.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) illustre l’utilisation de base du mode d’image (PiP) pour des périphériques de poche introduite dans Oreo. L’exemple lit une vidéo qui se poursuit sans interruption lors du changement dans les deux sens entre les modes d’affichage ou d’autres activités.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) illustre l’utilisation de l’infrastructure de recopie incrémentée. Elle comprend des implémentations du client activités avec des vues qui doivent être autofilled et un Service qui peut fournir des données de remplissage automatique au client d’activités.

-   [Polices téléchargeables](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) fournit un exemple montrant comment utiliser la fonctionnalité de polices téléchargeables décrite précédemment.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) illustre l’utilisation de la bibliothèque de prise en charge EmojiCompat. Vous pouvez utiliser cette bibliothèque pour empêcher votre application à partir de l’affichage manquant emoji caractères « tofu ».

-   [Intention en attente de mises à jour l’emplacement](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) illustre l’utilisation de l’API de l’emplacement pour obtenir les mises à jour sur l’emplacement d’un périphérique à l’aide un `PendingIntent`.

-   [Service de premier plan de mises à jour emplacement](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) montre comment utiliser l’API de l’emplacement pour obtenir les mises à jour sur l’emplacement d’un périphérique à l’aide d’un service dépendant et de démarrage de premier plan.


## <a name="video"></a>Vidéo

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Développement de Oreo 8.0 Android avec c#**


## <a name="summary"></a>Récapitulatif

Cet article introduit Oreo Android et explique comment installer et configurer les tout derniers outils et des packages pour le développement de Xamarin.Android sur Oreo Android. Il fourni une vue d’ensemble des fonctionnalités clés disponibles dans Android Oreo, avec des liens vers des exemples de code source pour plusieurs nouvelles fonctionnalités. Il inclus des liens vers la documentation de l’API et les développeurs Android des rubriques pour vous aider à démarrer dans la création d’applications pour Android Oreo. Il également mis en surbrillance les modifications de comportement Oreo Android plus importantes qui peuvent avoir un impact sur les applications existantes.


## <a name="related-links"></a>Liens associés

- [Android Oreo 8.0](https://developer.android.com/index.html)
