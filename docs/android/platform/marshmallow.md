---
title: "Fonctionnalités de marshmallow"
description: "Cet article vous aide à commencer à utiliser dans l’utilisation de Xamarin.Android pour développer des applications pour Android Marshmallow 6.0."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: c9ff9eb0b8d013485f0437d470ec49f1c1795d7a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="marshmallow-features"></a>Fonctionnalités de marshmallow

_Cet article vous aide à commencer à utiliser dans l’utilisation de Xamarin.Android pour développer des applications pour Android Marshmallow 6.0._

Cet article fournit une description des nouvelles fonctionnalités dans Android Marshmallow 6.0, explique comment préparer Xamarin.Android pour le développement d’Android Marshmallow et fournit des liens vers des exemples d’applications qui illustrent la façon d’utiliser de nouveau Android Marshmallow fonctionnalités dans les applications Xamarin.Android. 


## <a name="overview"></a>Vue d'ensemble

[Android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html), est la prochaine Android principales version après Lollipop Android.
Xamarin.Android prend en charge d’Android Marshmallow et inclut :

-   **API 23/Android 6.0 liaisons** &ndash; Android 6.0 ajoute plusieurs nouvelles API pour les nouvelles fonctionnalités décrites ci-dessous ; ces API sont disponibles pour les applications de Xamarin.Android lorsque vous ciblez des API de niveau 23. Pour plus d’informations sur les API 6.0 Android, consultez [Android 6.0 API](http://developer.android.com/preview/api-overview.html). 

[![Images héros de tablettes et téléphones Marshmallow en cours d’exécution](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Bien que la version Marshmallow est se concentre essentiellement sur « polonais et qualité », il fournit également des nombreuses nouvelles fonctionnalités d’intérêt pour les développeurs de Xamarin.Android. Ces fonctions incluent : 

-   **Autorisations d’exécution** &ndash; cette amélioration permet aux utilisateurs d’approuver les autorisations de sécurité sur un cas par cas en cours d’exécution. 

-   **Amélioration de l’authentification** &ndash; à compter d’Android Marshmallow, les applications peuvent désormais utiliser les capteurs d’empreintes digitales pour authentifier les utilisateurs et un nouveau *confirmer les informations d’identification* fonctionnalité réduit la nécessité d’entrer mots de passe. 

-   **Liaison de l’application** &ndash; cette fonctionnalité permet d’éliminer la nécessité des **application sélecteur** contextuelle en associant automatiquement des applications à des domaines web. 

-   **Diriger le partage** &ndash; que vous pouvez définir *directe partage cibles* qui rendent le partage rapide et intuitive pour les utilisateurs ; cette fonctionnalité permet à la solution à partager du contenu avec d’autres applications. 

-   **Interactions de la voix** &ndash; cette nouvelle API permet de générer des fonctionnalités de conversation vocale dans votre application. 

-   **Mode d’affichage de 4 K** &ndash; dans Android Marshmallow, votre application peut demander un 4 K résolution d’affichage sur du matériel qui prend en charge. 

-   **Nouvelles fonctionnalités Audio** &ndash; à partir de Marshmallow, Android prend désormais en charge le protocole MIDI. Il fournit également de nouvelles classes pour créer des objets de la lecture et de capture audio numérique et il propose de nouvelles raccordements API permettant d’associer les périphériques audio et d’entrée. 

-   **Nouvelles fonctionnalités vidéo** &ndash; Marshmallow fournit une nouvelle classe qui vous aide à des applications restituer les flux audio et vidéo synchronisés ; cette classe prend également en charge les taux de lecture dynamique. 

-   **Android for Work** &ndash; Marshmallow comprend les contrôles améliorées pour les appareils d’entreprise, le mode mono-utilisateur. Il prend en charge l’installation sans assistance et la désinstallation des applications par le propriétaire de l’appareil, l’acceptation automatique de mises à jour du système, la gestion des certificats améliorée, suivi de l’utilisation des données, gestion des autorisations et notifications d’état de travail. 

-   **Bibliothèque de prise en charge de conception matériaux** &ndash; nouveau *bibliothèque de prise en charge de conception* fournit les composants de conception et les modèles qui rend plus facile de générer des documents conception apparence dans votre application. 

En outre, plusieurs mises à jour de bibliothèque Android principaux ont été publiées dans Android M, et ces mises à jour fournissent de nouvelles fonctionnalités pour Android M et les versions antérieures d’Android.

En outre, plusieurs mises à jour de bibliothèque Android principaux ont été publiées dans Android Marshmallow, et ces mises à jour fournissent de nouvelles fonctionnalités pour Android Marshmallow et les versions antérieures d’Android. Cet article explique comment commencer à créer des applications avec Android Marshmallow, et il fournit qu'une vue d’ensemble de la nouvelle fonctionnalité met en évidence dans Android 6.0. 

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités d’Android Marshmallow dans les applications Xamarin : 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 ou version ultérieure doit être installé et configuré avec Visual Studio ou Xamarin Studio.

-   **Visual Studio pour Mac** ou **Visual Studio** &ndash; si vous utilisez Visual Studio pour Mac, version 5.9.7.22 ou ultérieur est requis. Si vous utilisez Visual Studio, version 3.11.1537 ou version ultérieure des outils Xamarin pour Visual Studio. 

-   **Kit de développement logiciel Android** &ndash; 6.0 SDK Android (API 23) ou version ultérieure doit être installé via le Gestionnaire de kit de développement logiciel Android.

-   **Kit de développement Java** &ndash; Xamarin.Android requiert [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou une version ultérieure si vous développez pour le niveau de l’API 24 ou supérieure (JDK 1.8 prend également en charge les niveaux d’API antérieures à 24, y compris Marshmallow). La version 64 bits de JDK 1.8 est requise si vous utilisez des contrôles personnalisés ou l’aperçu des formulaires.

Vous pouvez continuer à utiliser [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) si vous êtes le développement en particulier pour les API de niveau 23 ou une version antérieure. 


## <a name="getting-started"></a>Prise en main

Pour commencer à l’aide d’Android Marshmallow avec Xamarin.Android, vous devez télécharger et installer les derniers outils et les packages SDK avant de pouvoir créer un projet Android Marshmallow : 

1.  Installer les dernières mises à jour de Xamarin à partir de la **Stable** canal. 

2.  Installez les outils et les packages Android 6.0 Marshmallow Kit de développement logiciel.

3.  Créer un nouveau projet Xamarin.Android ciblant Android 6.0 Marshmallow (API niveau 23). 

4.  Configurer un émulateur ou un périphérique pour Android Marshmallow.

Chacune de ces étapes est expliquée dans les sections suivantes :


### <a name="install-xamarin-updates"></a>Installer les mises à jour de Xamarin

Pour mettre à jour Xamarin afin qu’il prend en charge Android 6.0 Marshmallow, modifier le canal de mise à jour à **Stable** et installer les mises à jour. Pour plus d’informations sur l’installation des mises à jour à partir du canal de mises à jour, consultez [modifier le canal mises à jour](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/). 


### <a name="install-the-android-60-sdk"></a>Installer le Kit de développement Android 6.0

Pour créer un projet Xamarin.Android pour Android Marshmallow, vous devez d’abord utiliser le Gestionnaire de SDK Android pour installer le Kit de développement logiciel Android 6.0 :

-   Démarrez le Gestionnaire de SDK Android (dans Visual Studio pour Mac, utilisez **Outils > Gestionnaire de SDK**; dans Visual Studio, utilisez **Outils > Android > Android SDK Manager**) et installez les derniers outils de kit de développement logiciel Android :

    [![Sélection d’outils du SDK Android dans le Gestionnaire de kit de développement logiciel Android.](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   En outre, installez la dernière version **Android 6.0** packages SDK :

    [![Sélection des packages du Kit de développement logiciel Android 6.0 dans le Gestionnaire de kit de développement logiciel Android.](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Vous devez installer la version d’Android SDK Tools 24.3.4 ou version ultérieure.
Pour plus d’informations sur l’utilisation du Gestionnaire de SDK Android pour installer le Kit de développement logiciel Android 6.0, consultez [Gestionnaire du SDK](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Démarrer un projet Xamarin.Android

Créer un nouveau projet Xamarin.Android. Si vous ne connaissez pas le développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Android. 

Lorsque vous créez un projet Android, vous devez configurer les paramètres de la version à la cible Android MarshMallow 6.0. Pour cibler votre projet pour Marshmallow, vous devez configurer votre projet pour **API de niveau 23 (v6.0 Xamarin.Android prise en charge)**. Pour plus d’informations sur la configuration des niveaux de niveau d’API Android, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Configurer un émulateur ou un périphérique

Si vous utilisez un émulateur, démarrez le gestionnaire AVD Android et créer un nouveau périphérique en utilisant les paramètres suivants :

-   Périphérique : Connexion 5, 6 ou 9.
-   Cible : Android 6.0 - API de niveau 23
-   ABI : x86

Par exemple, cet appareil virtuel est configuré pour émuler un 5 Nexus :

[![Configuration d’un AVD à l’aide de Nexus 5 appareils Android 6.0 cible et Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Si vous utilisez un périphérique physique comme Nexus 5, 6 ou 9, vous pouvez installer une image d’aperçu d’Android Marshmallow. Pour plus d’informations sur la mise à jour votre appareil Android Marshmallow, consultez [Images du système matériel](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Nouvelles fonctionnalités

La plupart des modifications introduites dans Android Marshmallow visent à améliorer l’expérience utilisateur Android, améliorer les performances et la résolution des bogues. Toutefois, Marshmallow a également introduit des modifications considérables les notions de base de la plateforme Android. Les sections suivantes mettre en évidence ces améliorations et fournissent des liens pour vous aider à commencer en utilisant les nouvelles fonctionnalités d’Android Marshmallow dans votre application. 



### <a name="runtime-permissions"></a>Autorisations d’exécution

Le système d’autorisations Android a été considérablement optimisé et simplifié depuis l’interface Lollipop Android. Dans Android Marshmallow, les utilisateurs accordent des autorisations sur une base au cas par cas lors de l’exécution plutôt qu’au moment de l’installation. Pour prendre en charge cette fonctionnalité sur Android Marshmallow et versions ultérieures, vous concevez votre application pour inviter l’utilisateur pour les autorisations lors de l’exécution (dans le contexte d’où les autorisations sont nécessaires). Cette modification facilite la démarrer à l’aide de votre application immédiatement, car elle simplifie le processus d’installation et de la mise à niveau de votre application. 

Consultez [demande d’autorisations Runtime dans Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) pour plus d’informations (y compris les exemples de code) sur l’implémentation des autorisations d’exécution dans les applications Xamarin.Android.
Xamarin fournit également un exemple d’application qui illustre le fonctionnement des autorisations d’exécution dans Android Marshmallow (et versions ultérieures) : [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Cet exemple d’application illustre les éléments suivants :

-   Comment vérifier et demander des autorisations en cours d’exécution.
-   Comment déclarer des autorisations pour les appareils Android M.

Pour utiliser cet exemple d’application :

1.  Appuyez sur la **caméra** ou **Contacts** boutons à afficher les autorisations d’une demande de boîte de dialogue.
2.  Accordez l’autorisation de visualiser des fragments de Contacts ou une caméra.

Pour plus d’informations sur les nouvelles fonctionnalités de runtime des autorisations dans Android Marshmallow, consultez [fonctionne avec les autorisations système](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Améliorations de l’authentification

Android Marshmallow inclut deux améliorations d’authentification qui aident à éliminer la nécessité de mots de passe :

-   **Empreinte digitale authentification** &ndash; utilise une analyse d’empreintes digitales pour authentifier les utilisateurs.

-   **Confirmer les informations d’identification** &ndash; authentifie les utilisateurs en fonction de la durée pendant laquelle l’appareil a été déverrouillé.

Les liens et les exemples d’applications décrits ci-après peuvent aider à vous familiariser avec ces nouvelles fonctionnalités.


#### <a name="fingerprint-authentication"></a>Authentification par empreinte digitale

Sur les appareils qui prennent en charge les empreintes digitales analyse matérielle, vous pouvez utiliser la nouvelle `FingerPrintManager` classe pour authentifier un utilisateur.
Pour plus d’informations sur la fonctionnalité d’authentification par empreinte digitale dans Android Marshmallow, consultez [l’authentification par empreinte digitale](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin fournit un exemple d’application qui illustre l’utilisation des empreintes digitales pour authentifier un utilisateur dans votre application : [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Pour utiliser cet exemple d’application :

1.  Sélectionnez le **achat** bouton pour ouvrir une boîte de dialogue de l’authentification par empreinte digitale.
2.  Analyser dans vos empreintes inscrit pour s’authentifier.

Notez que cet exemple d’application nécessite un appareil doté d’un lecteur d’empreintes digitales.
Cette application ne stocke pas vos empreintes digitales (ou votre mot de passe).



#### <a name="voice-interactions"></a>Interactions de la voix

La nouvelle fonctionnalité d’Interactions vocales introduite dans Android Marshmallow permet aux utilisateurs de votre application utiliser la voix pour confirmer les actions et sélectionner à partir d’une liste d’options. Pour plus d’informations sur les Interactions des voix, consultez [vue d’ensemble de l’API d’Interaction de voix](https://developers.google.com/voice-actions/interaction/). 

Consultez [ajouter une Conversation à votre application Android avec les Interactions de voix](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) pour plus d’informations (y compris les exemples de code) sur l’implémentation des Interactions vocales dans les applications Xamarin.Android.
Vous trouverez un exemple d’application qui illustre l’utilisation de l’API d’Interaction de voix dans une application Xamarin.Android : [Interactions vocales](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Confirmer les informations d’identification

À l’aide de la nouvelle *confirmer les informations d’identification* fonctionnalité d’Android Marshmallow, vous pouvez libérer les utilisateurs d’avoir à mémoriser et à entrer les mots de passe spécifiques de l’application en authentifiant les en fonction de la durée pendant laquelle leur appareil a été déverrouillée.
Pour ce faire, vous utilisez la nouvelle `SetUserAuthenticationValidityDurationSeconds` méthode de la `KeyGenerator`. Utilisez le `KeyGuardManager`de `CreateConfirmDeviceCredentialIntent` méthode à se réauthentifier l’utilisateur à partir de votre application. Pour plus d’informations sur cette nouvelle fonctionnalité dans Android Marshmallow, consultez [confirmer les informations d’identification](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin fournit un exemple d’application qui illustre l’utilisation des informations d’identification de l’appareil (par exemple, le code confidentiel, le modèle ou le mot de passe) dans votre application : [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Pour utiliser cet exemple d’application :

1.  Le programme d’installation d’un écran de verrouillage sécurisé sur votre appareil (**Secure > sécurité > Screenlock**).
2.  Appuyez sur la **achat** bouton et confirmer les informations d’identification écran de verrou.



### <a name="chrome-custom-tabs"></a>Chrome des onglets personnalisés

Les développeurs d’applications sont confrontés à un choix quand un utilisateur appuie sur une URL : l’application peut lancer un navigateur ou utiliser un navigateur dans l’application basé sur un `WebView`. Les deux options posent des défis &ndash; lancer le navigateur est un changement de contexte importante qui n’est pas personnalisable, tout en `WebView`s ne partagent pas d’état avec le navigateur. En outre, utilisez des `WebView`s peut ajouter une surcharge de maintenance supplémentaire. 

*Chrome personnalisé onglets* permet à vous afficher les sites Web avec la puissance de Chrome de manière simple et fiable sans avoir à vos utilisateurs de laisser votre application. Cette fonctionnalité offre à votre application davantage de contrôle sur l’expérience web de l’utilisateur ; Il apporte les transitions entre natif et le contenu plus transparent de web sans avoir recours à un `WebView`. Votre application peut également affecter le Chrome aspect et estime en personnalisant les éléments suivants : 

-   Couleur de barre d’outils

-   Entrez et les animations de sortie

-   Actions personnalisées dans le menu de barre d’outils et de dépassement de capacité de Chrome

-   Chrome pré-récupération pré-démarrage et le contenu (pour un chargement plus rapide)

Pour tirer parti de cette fonctionnalité dans votre application de Xamarin.Android, téléchargez et installez le [Android prise en charge personnalisée onglets bibliothèque](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Pour plus d’informations sur cette fonctionnalité, consultez [Chrome personnalisé onglets](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Bibliothèque de prise en charge de matière de conception

Android Lollipop introduite [documents de conception](http://www.google.com/design/spec/material-design/introduction.html) en tant qu’un nouveau langage de conception pour actualiser l’expérience Android (consultez [matériel thème](~/android/user-interface/material-theme.md) pour plus d’informations sur l’utilisation de la conception de matériel dans les applications de Xamarin.Android). Avec Android Marshmallow, Google a introduit le *bibliothèque de prise en charge Android conception* pour le rendre plus facile pour l’application aux développeurs d’adopter les documents de conçoivent apparence. Cette bibliothèque inclut les composants suivants :

-   **CoordinatorLayout** &ndash; nouveau `CoordinatorLayout` widget est similaire à mais plus puissant qu’un `FrameLayout`. Vous pouvez utiliser `CoordinatorLayout` comme un conteneur pour les vues enfants ou une disposition de niveau supérieur et il fournit un `layout_anchor` attribut qui peut être utilisé pour les vues d’ancrage par rapport à d’autres vues.

-   **Réduction des barres d’outils** &ndash; nouveau `CollapsingToolbarLayout` est une barre en mode de réduction de l’application qui est un wrapper pour `Toolbar`. (Notez que la *barre de l’application* est ce qui a été précédemment appelé le *barre d’action*.)

-   **Flottante bouton d’Action** &ndash; un bouton rond qui indique l’action principale sur l’interface de votre application.

-   **Flottant des étiquettes pour l’édition de texte** &ndash; utilise un nouveau `TextInputLayout` widget (qui encapsule `EditText`) pour afficher une étiquette flottante lorsqu’un indicateur est masqué lorsqu’un utilisateur entre le texte.

-   **Mode de navigation** &ndash; nouveau `NavigationView` widget vous permet d’utiliser le tiroir de navigation d’une manière qui est plus facile pour les utilisateurs de naviguer.

-   **Snackbar** &ndash; nouveau `SnackBar` widget est un mécanisme de commentaires léger (semblable à une notification toast) affiche un bref message en bas de l’écran, qui apparaissent au-dessus de tous les autres éléments de l’écran.

-   **Onglets cas** &ndash; nouveau `TabLayout` widget fournit une disposition horizontale pour afficher des onglets en tant que moyen d’implémenter la navigation de niveau supérieur dans votre application.

Pour tirer parti de la [bibliothèque de prise en charge de conception](http://developer.android.com/tools/support-library/features.html#design) dans votre application de Xamarin.Android, téléchargez et installez le Xamarin [Design de bibliothèque de prise en charge de Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) package NuGet.

Consultez [belle conception matières avec la bibliothèque de conception de prise en charge Android](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) pour plus d’informations (y compris les exemples de code) sur l’utilisation de la bibliothèque de prise en charge de matériel de conception dans les applications Xamarin.Android.
Xamarin fournit un exemple d’application qui démonstrations de la nouvelle bibliothèque de conception Android sur Xamarin.Android &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Cet exemple illustre les fonctionnalités suivantes de la bibliothèque de conception :


-   Barre d’outils en mode de réduction
-   Bouton d’action flottante
-   Vue d’ancrage
-   NavigationView
-   Snackbar

Pour plus d’informations sur la bibliothèque de conception, consultez [bibliothèque de prise en charge Android conception](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) dans le blog de développeur Android.


### <a name="additional-library-updates"></a>Mises à jour de la bibliothèque supplémentaires

En plus d’Android Marshmallow, Google a annoncé des mises à jour de plusieurs bibliothèques Android core. Xamarin prend en charge de Xamarin.Android ces mises à jour au moyen de plusieurs packages de NuGet-version préliminaire : 

-   [Services Google Play](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; la dernière version des Services Google Play inclut la nouvelle *application invite* fonctionnalité, ce qui rend possible pour les utilisateurs de partager leur application avec vos amis. Pour plus d’informations sur cette fonctionnalité, consultez [couverture de développer votre application avec invite d’application de Google](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Bibliothèques de prise en charge Android](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; NuGets ces offrent des fonctionnalités qui sont uniquement disponibles pour l’API de bibliothèque tout en offrant une compatibilité descendante des versions du framework API Android. 

-   [Bibliothèque portable Android](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; cette NuGet comprend des liaisons de Services Google Play. La dernière version de la bibliothèque portable apporte de nouvelles fonctionnalités (y compris la navigation plus facile pour les applications personnalisées) à la plateforme d’usure Android. 


## <a name="summary"></a>Récapitulatif

Cet article introduit d’Android Marshmallow et explique comment installer et configurer les tout derniers outils et des packages pour le développement de Xamarin.Android sur Marshmallow. Il également une vue d’ensemble des nouvelles fonctionnalités Android Marshmallow plus intéressantes pour le développement de Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html)
- [Obtenir le Kit de développement logiciel Android](https://developer.android.com/sdk/index.html#Other)
- [Vue d’ensemble](https://developer.android.com/preview/api-overview.html)
- [Notes de publication](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (exemple)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (exemple)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (exemple)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
