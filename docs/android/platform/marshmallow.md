---
title: Fonctionnalités de marshmallow
description: Cet article vous aide à commencer à utiliser dans l’utilisation de Xamarin.Android pour développer des applications pour Android Marshmallow 6.0.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 4f0bcd25662d3def719a89ccf833e845eb1728f2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242236"
---
# <a name="marshmallow-features"></a>Fonctionnalités de marshmallow

_Cet article vous aide à commencer à utiliser dans l’utilisation de Xamarin.Android pour développer des applications pour Android Marshmallow 6.0._

Cet article fournit une description des nouvelles fonctionnalités dans Android Marshmallow 6.0, explique comment préparer Xamarin.Android pour le développement d’Android Marshmallow et fournit des liens vers des exemples d’applications qui illustrent comment utiliser de nouveau Android Marshmallow fonctionnalités dans les applications Xamarin.Android. 


## <a name="overview"></a>Vue d'ensemble

[Android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html), est la prochaine Android majeure version après Android Lollipop.
Xamarin.Android prend en charge d’Android Marshmallow, qui inclut :

-   **Liaisons d’API 23/Android 6.0** &ndash; Android 6.0 ajoute plusieurs nouvelles API pour les nouvelles fonctionnalités décrites ci-dessous ; ces API sont disponibles pour les applications Xamarin.Android lorsque vous ciblez le niveau d’API 23. Pour plus d’informations sur les API 6.0 Android, consultez [Android 6.0 API](http://developer.android.com/preview/api-overview.html). 

[![Images de héros de tablettes et téléphones Marshmallow en cours d’exécution](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Bien que la version Marshmallow est principalement sur « polonais et qualité », il fournit également des nombreuses nouvelles fonctionnalités d’intérêt pour les développeurs Xamarin.Android. Ces fonctions incluent : 

-   **Autorisations d’exécution** &ndash; cette amélioration rend possible pour les utilisateurs à approuver les autorisations de sécurité sur un cas par cas en cours d’exécution. 

-   **Améliorations de l’authentification** &ndash; à compter d’Android Marshmallow, applications peuvent désormais utiliser capteurs d’empreinte digitale pour authentifier les utilisateurs et un nouveau *confirmer les informations d’identification* fonctionnalité réduit le besoin pour l’entrée mots de passe. 

-   **Liaison de l’application** &ndash; cette fonctionnalité permet d’éliminer la nécessité de disposer la **application sélecteur** contextuelle en associant automatiquement des applications avec des domaines web. 

-   **Diriger le partage** &ndash; vous pouvez définir *directe des cibles de partage* qui rendent partage rapide et intuitive pour les utilisateurs ; cette fonctionnalité permet de partager du contenu avec d’autres applications de la solution. 

-   **Voix Interactions** &ndash; cette nouvelle API permet de générer des fonctionnalités vocales conversationnel dans votre application. 

-   **Mode d’affichage de 4 K** &ndash; dans Android Marshmallow, votre application peut demander un 4 K résolution d’affichage sur du matériel qui prend en charge. 

-   **Nouvelles fonctionnalités Audio** &ndash; Marshmallow à compter, Android prend désormais en charge le protocole MIDI. Il fournit également les nouvelles classes pour créer des objets de la lecture et capture audio numérique, et il offre de nouvelles raccordements API permettant d’associer des périphériques audio et d’entrée. 

-   **Nouvelles fonctionnalités vidéo** &ndash; Marshmallow fournit une nouvelle classe qui permet aux applications d’effectuer le rendu des flux audio et vidéo synchronisés ; cette classe fournit également la prise en charge pour les taux de lecture dynamique. 

-   **Android for Work** &ndash; Marshmallow inclut des contrôles améliorés pour les appareils d’entreprise, le mode mono-utilisateur. Il prend en charge l’installation en mode silencieux et désinstaller des applications par le propriétaire de l’appareil, l’acceptation automatique des mises à jour système, la gestion de certificat améliorée, suivi de l’utilisation de données, la gestion des autorisations et notifications d’état de travail. 

-   **Bibliothèque de prise en charge de conception matériaux** &ndash; le nouveau *bibliothèque de prise en charge de conception* fournit des composants de conception et de modèles qui facilite la générer Material Design apparence dans votre application. 

En outre, les nombreuses mises à jour de bibliothèque Android core ont été publiés avec Android M, et ces mises à jour fournissent de nouvelles fonctionnalités pour Android M et versions antérieures d’Android.

En outre, les nombreuses mises à jour de bibliothèque Android core ont été publiés avec Android Marshmallow, et ces mises à jour fournissent de nouvelles fonctionnalités pour Android Marshmallow et versions antérieures d’Android. Cet article explique comment créer des applications avec Android Marshmallow, et il fournit qu'une vue d’ensemble de la nouvelle fonctionnalité met en évidence dans Android 6.0. 

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont nécessaire pour utiliser les nouvelles fonctionnalités d’Android Marshmallow dans les applications basées sur Xamarin : 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 ou version ultérieure doit être installé et configuré avec Visual Studio ou Xamarin Studio.

-   **Visual Studio pour Mac** ou **Visual Studio** &ndash; si vous utilisez Visual Studio pour Mac version 5.9.7.22 ou ultérieure est requise. Si vous utilisez Visual Studio, version 3.11.1537 ou ultérieure des outils Xamarin pour Visual Studio est nécessaire. 

-   **Kit de développement logiciel Android** &ndash; Android 6.0 SDK (API 23) ou version ultérieure doit être installé via le gestionnaire Android SDK.

-   **Kit de développement Java** &ndash; Xamarin.Android nécessite [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure si vous développez pour le niveau d’API 24 ou supérieure (JDK 1.8 prend également en charge les niveaux d’API antérieures à 24, y compris Marshmallow). La version 64 bits du JDK 1.8 est requise si vous utilisez des contrôles personnalisés ou le Générateur d’aperçu de formulaires.

Vous pouvez continuer à utiliser [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) si vous êtes développement spécifiquement pour le niveau d’API 23 ou une version antérieure. 


## <a name="getting-started"></a>Prise en main

Pour commencer à l’aide d’Android Marshmallow avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages de kit de développement logiciel avant de pouvoir créer un projet Android Marshmallow : 

1.  Installer les dernières mises à jour de Xamarin à partir de la **Stable** canal. 

2.  Installez les outils et les packages du Kit Android SDK Marshmallow 6.0.

3.  Créer un nouveau projet Xamarin.Android qui cible Android 6.0 Marshmallow (API niveau 23). 

4.  Configurer un appareil ou un émulateur pour Android Marshmallow.

Chacune de ces étapes est expliquée dans les sections suivantes :


### <a name="install-xamarin-updates"></a>Installer les mises à jour de Xamarin

Pour mettre à jour Xamarin afin qu’il inclue la prise en charge d’Android Marshmallow 6.0, modifiez le canal de mise à jour en **Stable** et installer toutes les mises à jour. Pour plus d’informations sur l’installation des mises à jour à partir du canal de mises à jour, consultez [modifier le canal mises à jour](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel). 


### <a name="install-the-android-60-sdk"></a>Installez le SDK Android 6.0

Pour créer un projet Xamarin.Android pour Android Marshmallow, vous devez tout d’abord utiliser le gestionnaire Android SDK pour installer le Kit de développement logiciel Android 6.0 :

-   Démarrer le gestionnaire Android SDK (dans Visual Studio pour Mac, utilisez **Outils > Gestionnaire du kit SDK**; dans Visual Studio, utilisez **Outils > Android > Gestionnaire Android SDK**) et installez la plus récente d’Android SDK Tools :

    [![En sélectionnant Outils Android SDK dans le Gestionnaire de kit de développement logiciel Android.](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   En outre, installez la dernière version **Android 6.0** packages SDK :

    [![Sélectionner les packages de kit de développement logiciel Android 6.0 dans le Gestionnaire de kit de développement logiciel Android.](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Vous devez installer la version d’Android SDK Tools 24.3.4 ou version ultérieure.
Pour plus d’informations sur l’utilisation du gestionnaire Android SDK pour installer le Kit de développement logiciel Android 6.0, consultez [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous êtes novice en développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Android. 

Lorsque vous créez un projet Android, vous devez configurer les paramètres de version à la cible Android MarshMallow 6.0. Pour cibler votre projet pour Marshmallow, vous devez configurer votre projet pour **niveau d’API 23 (v6.0 Xamarin.Android prise en charge)**. Pour plus d’informations sur la configuration des niveaux de niveau d’API Android, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Configurer un émulateur ou un périphérique

Si vous utilisez un émulateur, démarrez le Gestionnaire d’AVD Android et créer un nouvel appareil en utilisant les paramètres suivants :

-   APPAREIL : Nexus 5, 6 ou 9.
-   Cible : Android 6.0 - niveau d’API 23
-   ABI : x86

Par exemple, cet appareil virtuel est configuré pour émuler un Nexus 5 :

[![Configuration d’un AVD à l’aide de périphérique Nexus 5, Android 6.0 cible et Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Si vous utilisez un appareil physique comme un Nexus 5, 6 ou 9, vous pouvez installer une image d’aperçu d’Android Marshmallow. Pour plus d’informations sur la mise à jour de votre appareil vers Android Marshmallow, consultez [Images du système matériel](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Nouvelles fonctionnalités

La plupart des modifications introduites dans Android Marshmallow visent à améliorer l’expérience utilisateur Android, l’augmentation des performances et correction des bogues. Toutefois, Marshmallow a également introduit des modifications considérables aux notions de base de la plateforme Android. Les sections suivantes, mettez en surbrillance ces améliorations et fournissent des liens pour vous aider à commencer en utilisant les nouvelles fonctionnalités d’Android Marshmallow dans votre application. 



### <a name="runtime-permissions"></a>Autorisations d’exécution

Le système d’autorisations Android a été considérablement optimisé et simplifié depuis Android Lollipop. Dans Android Marshmallow, les utilisateurs accordent des autorisations sur une base de cas par cas lors de l’exécution plutôt qu’au moment de l’installation. Pour prendre en charge cette fonctionnalité sur Android Marshmallow et versions ultérieures, vous concevez votre application pour inviter l’utilisateur pour les autorisations lors de l’exécution (dans le contexte d’où les autorisations sont nécessaires). Cette modification facilite la démarrer à l’aide de votre application immédiatement, car elle simplifie le processus d’installation et la mise à niveau de votre application. 

Consultez [demande d’autorisations Runtime dans Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) pour plus d’informations (y compris les exemples de code) sur l’implémentation des autorisations d’exécution dans les applications Xamarin.Android.
Xamarin fournit également un exemple d’application qui illustre le fonctionnement des autorisations d’exécution dans Android Marshmallow (et versions ultérieures) : [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Cet exemple d’application montre les points suivants :

-   Comment vérifier et demander des autorisations en cours d’exécution.
-   Guide pratique pour déclarer des autorisations pour les appareils Android M.

Pour utiliser cet exemple d’application :

1.  Appuyez sur la **caméra** ou **Contacts** demande de boutons à afficher les autorisations d’un dialogue.
2.  Accorder l’autorisation de visualiser des fragments de Contacts ou une caméra.

Pour plus d’informations sur les nouvelles fonctionnalités d’autorisations de runtime dans Android Marshmallow, consultez [fonctionne avec les autorisations système](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Améliorations de l’authentification

Android Marshmallow inclut deux améliorations d’authentification qui aident à éliminer la nécessité pour les mots de passe :

-   **L’authentification de l’empreinte digitale** &ndash; utilise une analyse de l’empreinte digitale pour authentifier les utilisateurs.

-   **Confirmer les informations d’identification** &ndash; authentifie les utilisateurs en fonction de la durée pendant laquelle l’appareil a été déverrouillée.

Les liens et les exemples d’applications décrits ci-après peuvent vous aider à vous familiariser avec ces nouvelles fonctionnalités.


#### <a name="fingerprint-authentication"></a>Authentification par empreinte digitale

Sur les appareils qui prennent en charge d’analyse matérielle des empreintes digitales, vous pouvez utiliser la nouvelle `FingerPrintManager` classe pour authentifier un utilisateur.
Pour plus d’informations sur la fonctionnalité d’authentification par empreinte digitale dans Android Marshmallow, consultez [authentification par empreinte digitale](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin fournit un exemple d’application qui illustre l’utilisation d’empreintes digitales inscrits pour authentifier un utilisateur dans votre application : [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Pour utiliser cet exemple d’application :

1.  Touchez la **achat** bouton pour ouvrir une boîte de dialogue de l’authentification par empreinte digitale.
2.  Analyser dans votre empreinte digitale inscrit pour s’authentifier.

Notez que cet exemple d’application nécessite un appareil avec un lecteur d’empreintes digitales.
Cette application ne stocke pas vos empreintes digitales (ou votre mot de passe).



#### <a name="voice-interactions"></a>Interactions vocales

La nouvelle fonctionnalité d’Interactions vocales introduite dans Android Marshmallow permet aux utilisateurs de votre application à utiliser la voix pour confirmer actions et sélectionner parmi une liste d’options. Pour plus d’informations sur les Interactions vocales, consultez [vue d’ensemble de l’API d’Interaction de voix](https://developers.google.com/voice-actions/interaction/). 

Consultez [ajouter une Conversation à votre application Android avec des Interactions vocales](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) pour plus d’informations (y compris les exemples de code) sur l’implémentation des Interactions vocales dans les applications Xamarin.Android.
Vous trouverez un exemple d’application qui illustre comment utiliser l’API d’Interaction vocale dans une application Xamarin.Android : [Interactions vocales](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Confirmer les informations d’identification

À l’aide de la nouvelle *confirmer les informations d’identification* fonctionnalité d’Android Marshmallow, vous pouvez libérer les utilisateurs d’avoir à mémoriser et entrez les mots de passe spécifiques de l’application en authentifiant les selon la durée pendant laquelle leur appareil a été déverrouillée.
Pour ce faire, vous utilisez la nouvelle `SetUserAuthenticationValidityDurationSeconds` méthode de la `KeyGenerator`. Utilisez le `KeyGuardManager`de `CreateConfirmDeviceCredentialIntent` (méthode) à se réauthentifier l’utilisateur à partir de votre application. Pour plus d’informations sur cette nouvelle fonctionnalité dans Android Marshmallow, consultez [confirmer les informations d’identification](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin fournit un exemple d’application qui illustre l’utilisation des informations d’identification de l’appareil (par exemple, le code confidentiel, le modèle ou le mot de passe) dans votre application : [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Pour utiliser cet exemple d’application :

1.  Le programme d’installation d’un écran de verrouillage sécurisé sur votre appareil (**Secure > sécurité > Screenlock**).
2.  Appuyez sur la **achat** bouton et confirmer les informations d’identification écran de verrouillage sécurisé.



### <a name="chrome-custom-tabs"></a>Chrome des onglets personnalisés

Les développeurs d’applications doivent faire face à un choix lorsqu’un utilisateur appuie sur une URL : l’application peut lancer un navigateur ou utiliser un navigateur dans l’application basé sur un `WebView`. Les deux options présentent des défis &ndash; lancer le navigateur est un changement de contexte importante qui n’est pas personnalisable, tout en `WebView`s ne partagent pas d’état avec le navigateur. En outre, utilisez de `WebView`s peut ajouter des frais de maintenance supplémentaire. 

*Chrome des onglets personnalisés* rend possible pour vous permettre de manière simple et fiable d’affichage des sites Web grâce à la puissance de Chrome sans avoir à vos utilisateurs de laisser votre application. Cette fonctionnalité offre à votre application davantage de contrôle sur l’expérience web de l’utilisateur ; Il apporte les transitions entre natif et le contenu plus transparente de web sans avoir recours à un `WebView`. Votre application peut également affecter l’apparence et le semble en personnalisant les éléments suivants de Chrome : 

-   Couleur de la barre d’outils

-   Entrer et sortir des animations

-   Actions personnalisées dans le menu de barre d’outils et de dépassement de capacité de Chrome

-   Chrome pré-récupération préalable de début et de contenu (pour accélérer le chargement)

Pour tirer parti de cette fonctionnalité dans votre application Xamarin.Android, téléchargez et installez le [Android prise en charge personnalisée onglets bibliothèque](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Pour plus d’informations sur cette fonctionnalité, consultez [onglets personnalisés de Chrome](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Bibliothèque de prise en charge de conception matérielle

Android Lollipop introduit [Material Design](http://www.google.com/design/spec/material-design/introduction.html) comme un nouveau langage de conception pour actualiser l’expérience Android (consultez [matériau thème](~/android/user-interface/material-theme.md) pour plus d’informations sur l’utilisation de la conception matérielle dans les applications Xamarin.Android). Avec Android Marshmallow, Google a introduit le *bibliothèque de prise en charge de conception Android* pour faciliter le travail d’application les développeurs à adopter le matériau conçoivent apparence. Cette bibliothèque inclut les composants suivants :

-   **CoordinatorLayout** &ndash; le nouveau `CoordinatorLayout` widget est similaire à mais plus puissant qu’un `FrameLayout`. Vous pouvez utiliser `CoordinatorLayout` sous la forme d’un conteneur pour les vues enfants ou une disposition de niveau supérieur et il fournit un `layout_anchor` attribut qui peut être utilisé pour les vues d’ancrage par rapport à d’autres vues.

-   **Réduction des barres d’outils** &ndash; le nouveau `CollapsingToolbarLayout` est une barre d’application qui est un wrapper de réduction `Toolbar`. (Notez que le *barre de l’application* est ce qui a été précédemment appelé le *barre d’action*.)

-   **Flottante bouton d’Action** &ndash; un bouton rond qui désigne l’action principale sur l’interface de votre application.

-   **Flottant des étiquettes pour l’édition de texte** &ndash; utilise un nouveau `TextInputLayout` widget (qui enveloppe `EditText`) pour afficher une étiquette flottante lorsqu’un indicateur est masqué lorsqu’un utilisateur entre le texte.

-   **Mode navigation** &ndash; le nouveau `NavigationView` widget vous permet d’utiliser le tiroir de navigation d’une manière qui est plus facile aux utilisateurs de naviguer.

-   **Snackbar** &ndash; le nouveau `SnackBar` widget est un mécanisme de commentaires léger (semblable à un toast) affiche un bref message en bas de l’écran, qui apparaissent au-dessus de toutes les autres éléments à l’écran.

-   **Onglets Material** &ndash; le nouveau `TabLayout` widget fournit une disposition horizontale pour afficher les onglets comme moyen d’implémenter la navigation de niveau supérieur dans votre application.

Pour tirer parti de la [bibliothèque de prise en charge de conception](http://developer.android.com/tools/support-library/features.html#design) dans votre application Xamarin.Android, téléchargez et installez Xamarin [Design de bibliothèque de prise en charge de Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) package NuGet.

Consultez [magnifiques Material Design avec la bibliothèque de conception de prise en charge Android](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) pour plus d’informations (y compris les exemples de code) à l’aide de la bibliothèque de prise en charge de conception de documents dans les applications Xamarin.Android.
Xamarin fournit un exemple d’application qui la démonstration de la nouvelle bibliothèque de conception Android sur Xamarin.Android &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Cet exemple illustre les fonctionnalités suivantes de la bibliothèque de conception :


-   Réduction de barre d’outils
-   Bouton d’action flottant
-   Vue d’ancrage
-   NavigationView
-   Snackbar

Pour plus d’informations sur la bibliothèque de conception, consultez [bibliothèque de prise en charge de conception Android](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) dans le blog du développeur Android.


### <a name="additional-library-updates"></a>Mises à jour des bibliothèques supplémentaires

En plus d’Android Marshmallow, Google a annoncé des mises à jour à plusieurs bibliothèques Android core. Xamarin prend en charge de Xamarin.Android pour ces mises à jour via plusieurs packages de NuGet-version préliminaire : 

-   [Services Google Play](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; la dernière version des Services Google Play inclut la nouvelle *application invite* fonctionnalité, ce qui permet aux utilisateurs de partager leur application avec vos amis. Pour plus d’informations sur cette fonctionnalité, consultez [Reach de développer votre application avec App invite Google](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Les bibliothèques de prise en charge Android](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; ces packages NuGet offrent des fonctionnalités qui sont uniquement disponibles pour la bibliothèque API tout en assurant la compatibilité descendante des versions du framework Android API. 

-   [Bibliothèque portable Android](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; ce package NuGet inclut des liaisons de Services Google Play. La dernière version de la bibliothèque portable apporte de nouvelles fonctionnalités (notamment une navigation facilitée pour les applications personnalisées) pour la plateforme Android Wear. 


## <a name="summary"></a>Récapitulatif

Cet article a introduit Android Marshmallow et expliqué comment installer et configurer les derniers outils et packages pour le développement de Xamarin.Android sur Marshmallow. Elle également fourni une vue d’ensemble des nouvelles fonctionnalités Android Marshmallow plus intéressantes pour le développement de Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html)
- [Obtenir le kit SDK Android](https://developer.android.com/sdk/index.html#Other)
- [Vue d’ensemble de la fonctionnalité](https://developer.android.com/preview/api-overview.html)
- [Notes de publication](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (exemple)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (exemple)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (exemple)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
