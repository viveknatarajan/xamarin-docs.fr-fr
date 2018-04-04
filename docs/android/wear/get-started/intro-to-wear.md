---
title: Introduction à usure Android
description: Avec l’introduction de porter Android de Google, vous n’êtes plus limité aux simplement les téléphones portables et tablettes en matière de développement de superbes applications Android. Prise en charge de Xamarin.Android pour porter Android rend possible d’exécuter du code c# sur votre poignet ! Cette présentation fournit une vue d’ensemble d’usure Android, décrit ses principales fonctionnalités et offre une vue d’ensemble des fonctionnalités disponibles dans Android 2.0 porter. Il répertorie certaines des appareils Android d’usure plus populaires, et il fournit des liens vers la documentation de Google Android usure essentielle pour plus d’informations.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 0ab166bb71c23d456cb70d35a2794717110642fd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-android-wear"></a>Introduction à usure Android

_Avec l’introduction de porter Android de Google, vous n’êtes plus limité aux simplement les téléphones portables et tablettes en matière de développement de superbes applications Android. Prise en charge de Xamarin.Android pour porter Android rend possible d’exécuter du code c# sur votre poignet ! Cette présentation fournit une vue d’ensemble d’usure Android, décrit ses principales fonctionnalités et offre une vue d’ensemble des fonctionnalités disponibles dans Android 2.0 porter. Il répertorie certaines des appareils Android d’usure plus populaires, et il fournit des liens vers la documentation de Google Android usure essentielle pour plus d’informations._


## <a name="overview"></a>Vue d'ensemble

Usure Android s’exécute sur divers appareils, y compris la première génération 360 Motorola, G espion de LG et l’engrenage Samsung Live. Une deuxième génération, y compris SmartWatch 3 de Sony, a également été publiée avec des fonctionnalités supplémentaires, y compris GPS intégrés et lecture hors connexion. Pour Android 2.0 usure, Google a collaboré avec LG pour deux observations nouvelle : le Sport espion LG et le Style d’espion LG.

![Les appareils Android 2.0 d’usure](intro-to-wear-images/hero-image.png "exemple usure 2.0 les appareils Android")

Prend en charge de Xamarin.Android 5.0 et versions ultérieures prend en charge l’usure Android via notre 4.4W Android (API 20) et contrôles d’interface utilisateur spécifique à l’usure un package NuGet ajoute supplémentaires. Xamarin.Android 5.0 et versions ultérieures inclut également les fonctionnalités pour empaqueter vos applications d’usure. Les packages NuGet sont également disponibles pour Android usure 2.0 comme décrit plus loin dans ce guide.


## <a name="android-wear-basics"></a>Principes de base usure Android

Android usure a un modèle d’interface utilisateur qui diffère de celui des applications de poche Android. La première vague d’usure applications ont été conçues pour étendre un Assistant Application poche dans certains façon, mais l’usure Android 2.0 à compter, usure applications peut être autonome utilisé. Lorsque vous déployez une application d’usure, il est empaqueté avec une application de poche d’accompagnement. Étant donné que la plupart d’usure applications dépendent d’une application auxiliaire de poche, ils ont besoin d’un moyen de communiquer avec les applications de poche. Les sections suivantes décrivent ces scénarios d’utilisation et présentent les fonctionnalités essentielles d’usure Android. 



### <a name="usage-scenarios"></a>Scénarios d'utilisation

La première version de porter Android a été principalement axée sur extension d’applications de poche actuelles avec les notifications améliorées et la synchronisation des données entre l’application de poche et de l’application portable. Par conséquent, ces scénarios sont relativement faciles à implémenter.


#### <a name="wearable-notifications"></a>Notifications de portable

Pour prendre en charge l’usure Android, le plus simple consiste à tirer parti de la nature partagée des notifications entre l’ordinateur de poche et l’appareil portable. À l’aide de l’API de notification prise en charge v4 et `WearableExtender` classe (disponible dans le [bibliothèque de prise en charge Android Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), vous pouvez exploiter les fonctionnalités natives de la plateforme, tels que des cartes de style de la boîte de réception ou d’entrée de la voix. Le [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) exemple fournit des exemples de code qui montre comment envoyer une liste des notifications sur un appareil Android d’usure. 



#### <a name="companion-applications"></a>Applications d’accompagnement

Une autre stratégie consiste à créer une application complète qui s’exécute en mode natif sur le périphérique portable et paires avec une application de poche d’accompagnement. Un bon exemple de cette approche est la [questionnaire](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) exemple d’application, qui montre comment créer un questionnaire qui s’exécute sur un ordinateur de poche et pose des questions de questionnaire sur l’appareil portable. 



### <a name="user-interface"></a>Interface utilisateur

Le modèle de navigation principale d’usure est une série de cartes verticalement. Chacune de ces cartes permettre avoir des actions associées sont en couche sur la même ligne. Le `GridViewPager` classe fournit cette fonctionnalité ; il respecte le même concept de carte en tant que `ListView`. En général, vous associez la `GridViewPager` avec un `FragmentGridPagerAdaptor` (ou `GridPagerAdaptor`) qui vous permet de représenter les cellules de chaque ligne et de colonne en tant qu’un `Fragment`: 

[![Navigation d’usure](intro-to-wear-images/2d-picker-sml.png "usure Navigation")](intro-to-wear-images/2d-picker.png#lightbox)

Porter également facilite l’utilisation de boutons d’action qui se composent d’un grand cercle avec le texte de description de petits situés en dessous (comme illustré ci-dessus) de couleur.  Le [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) exemple montre comment utiliser `GridViewPager` et `GridPagerAdapter` dans une application d’usure.

Android usure 2.0 ajoute un tiroir de navigation, un tiroir de l’action et les boutons d’action inline à l’interface utilisateur de l’usure. Pour plus d’informations sur les éléments d’interface utilisateur d’usure Android version 2.0, consultez le Android [Anatomie](https://www.google.com/design/spec-wear/system-overview/anatomy.html) rubrique. 



### <a name="communications"></a>Communications

Usure Android fournit la communication différentes deux API afin de faciliter les communications entre les applications portable et le Guide poche : 

**API de données** &ndash; cette API méthode est similaire à une banque de données synchronisée entre l’appareil portable et l’ordinateur de poche. Android prend en charge de propagation des modifications entre portable et portable lors de la solution optimale consiste à le faire. Lorsque le portable est hors limites, il files d’attente de synchronisation pour une date ultérieure. Le point d’entrée principal pour cette API est `WearableClass.DataApi`. Pour plus d’informations sur cette API, consultez le Android [la synchronisation des éléments de données](https://developer.android.com/training/wearables/data-layer/data-items.html) rubrique. 

**API de message** &ndash; cette API permet d’utiliser un chemin d’accès des communications au niveau inférieur : une petite surcharge est envoyée à sens unique sans une synchronisation entre les applications de l’ordinateur de poche et portable.
Le point d’entrée principal pour cette API est `WearableClass.MessageApi`.
Pour plus d’informations sur cette API, consultez le Android [envoi et réception de Messages](https://developer.android.com/training/wearables/data-layer/messages.html) rubrique.

Vous pouvez choisir d’enregistrer des rappels pour recevoir ces messages par chacune des interfaces API écouteur, vous pouvez également implémenter un service dans votre application qui dérive de `WearableListenerService`.
Ce service sera automatiquement instancié par l’usure Android.
Le [FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) exemple illustre comment implémenter un `WearableListenerService`.



### <a name="deployment"></a>Déploiement

Chaque application portable est déployée avec son propre fichier APK incorporé à l’intérieur de l’application principale APK. Ce package est géré automatiquement dans Xamarin.Android 5.0 et versions ultérieures, mais doit être effectuée manuellement pour les versions antérieures à la version 5.0 de Xamarin.Android. 
[Utilisation avec l’empaquetage](~/android/wear/deploy-test/packaging.md) explique le déploiement en détail. 



## <a name="going-further"></a>Aller plus loin 

La meilleure façon de vous familiariser avec l’usure Android consiste à générer et tester votre première application. La liste suivante fournit un ordre de lecture recommandée pour vous aider à rapidement opérationnel :

1.  [Configuration et Installation](~/android/wear/get-started/installation.md) fournit des instructions détaillées pour installer et configurer votre environnement de développement pour la création d’applications Xamarin.Android usure. 

2.  Après avoir installé les packages nécessaires et configuré un émulateur ou un périphérique, consultez [Bonjour, usure](~/android/wear/get-started/hello-wear.md) pour obtenir des instructions qui expliquent comment créer un petit projet Android d’usure ce bouton handles clique sur et affiche un Cliquez sur le compteur sur le périphérique d’usure. 

3.  [Déploiement et test](~/android/wear/deploy-test/index.md) fournit plus d’informations sur la configuration et de déploiement pour les émulateurs et appareils, y compris des instructions sur la façon de déployer votre application sur un périphérique d’usure Bluetooth.

4.  [Utilisation de tailles d’écran](~/android/wear/screen-sizes.md) explique comment afficher un aperçu et optimiser votre interface utilisateur pour les différentes tailles d’écran disponibles sur les appareils d’usure. 

5.  [Utilisation avec l’empaquetage](~/android/wear/deploy-test/packaging.md) décrit les étapes de l’empaquetage des applications d’usure pour la distribution sur Google Play manuellement.

Après avoir créé votre première application usure, vous voudrez réessayer de générer un visage espion personnalisé pour porter Android. 
[Création d’un visage espion](~/android/wear/platform/creating-a-watchface.md) fournit des instructions pas à pas, l’exemple de code pour le développement d’un élément supprimé service de face espion numérique, suivi par le code plus améliore à un type de style analogique Espion avec des fonctionnalités supplémentaires. 



## <a name="android-wear-20"></a>Android usure 2.0

Android 2.0 usure présente diverses nouvelles fonctionnalités et fonctions, telles que *complications*, courbes de disposition, les tiroirs de navigation et l’action et les notifications étendues. Usure version 2.0 permet également de générer des applications autonomes qui fonctionnent indépendamment des applications de poche. La nouvelle *les mouvements poignet* permet une interactions avec votre application. Les sections suivantes illustrent ces fonctionnalités et fournissent des liens pour vous aider à démarrer les utiliser dans votre application.



### <a name="install-wear-20-packages"></a>Installation d’usure 2.0 Packages

Pour générer une application de la version 2.0 d’usure avec Xamarin.Android, vous devez ajouter le **Xamarin.Android.Wear v2.0** package pour votre projet (cliquez sur le **onglet Parcourir**) :

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "installer NuGet Xamarin.Android.Wear v2.0")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Ce package NuGet contient des liaisons pour à la fois la prise en charge de Android portable et Compat d’usure les bibliothèques.

En plus de **Xamarin.Android.Wear**, nous vous recommandons d’installer le **Xamarin.GooglePlayServices.Wearable** NuGet : 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "installer Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Principales fonctionnalités d’usure 2.0

Android 2.0 usure est la mise à jour plus grand porter Android depuis son lancement initial en 2014. Les sections suivantes illustrent les fonctionnalités clés de la version 2.0 d’usure Android, et des liens sont fournis pour aider à commencer à utiliser ces nouvelles fonctionnalités dans votre application. 


#### <a name="complications"></a>Complications

*Complications* est espion petits widgets face que vous pouvez voir en un coup de œil sans avoir à présenter le cadran de la montre. Complications sont semblables aux widgets de tableau de bord de style de bureau ; elles affichent des informations telles que la météo, autonomie, événements de calendrier et statistiques à l’adéquation de l’application : 

![Exemple de complications](intro-to-wear-images/complications.png "exemple de Complications")

Pour plus d’informations sur les problèmes, consultez le Android [espion Face Complications](https://developer.android.com/wear/preview/features/complications.html) rubrique. 



#### <a name="navigation-and-action-drawers"></a>Navigation et tiroirs de l’Action 

Deux bacs nouvelle sont inclus dans l’usure version 2.0. Le *tiroir de navigation*, qui apparaît en haut de l’écran, permet aux utilisateurs de naviguer entre les vues de l’application (comme indiqué sur la gauche ci-dessous). Le *tiroir d’action*, qui apparaît en bas de l’écran (comme indiqué sur la droite), permet aux utilisateurs de choisir à partir d’une liste d’actions. 

![Navigation et Action tiroirs](intro-to-wear-images/drawers.png "tiroirs de l’Action et de Navigation")

Pour plus d’informations sur ces deux bacs interactifs de nouveau, consultez le Android [porter la Navigation et les Actions](https://developer.android.com/wear/preview/features/ui-nav-actions.html) rubrique. 



#### <a name="curved-layouts"></a>Dispositions en arc 

Usure 2.0 introduit de nouvelles fonctionnalités pour l’affichage des dispositions sur les appareils usure round. Plus précisément, la nouvelle `WearableRecyclerView` classe est optimisée pour afficher une liste d’éléments verticales sur les écrans d’arrondi : 

![Exemple de disposition de courbe](intro-to-wear-images/curved-layout.png "exemple de disposition de courbes")

`WearableRecyclerView` étend la `RecyclerView` classe pour prendre en charge des dispositions et des mouvements de défilement circulaires. Pour plus d’informations, consultez le Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) documentation de l’API. 



#### <a name="standalone-apps"></a>Applications autonomes 

2.0 de porter des applications Android peuvent travailler indépendamment des applications de poche. Cela signifie que, par exemple, un espion actif pouvez continuer à offrir des fonctionnalités complètes même si l’ordinateur de poche Compagnon est désactivé ou éloigné de l’appareil portable. Pour plus d’informations sur cette fonctionnalité, consultez le Android [autonome applications](https://developer.android.com/wear/preview/features/standalone-apps.html) rubrique.



#### <a name="wrist-gestures"></a>Mouvements de poignet 

Les mouvements de poignet permettent aux utilisateurs d’interagir avec votre application sans l’aide de l’écran tactile &ndash; les utilisateurs puissent répondre à l’application avec une seule main. Les mouvements poignet deux sont pris en charge : 

-   Poignet Flick out
-   Poignet Flick dans

Pour plus d’informations, consultez le Android [poignet mouvements](https://developer.android.com/wear/preview/features/gestures.html) rubrique. 


Il existe beaucoup d’autres fonctionnalités 2.0 d’usure telles que les actions inline, smart entrée à distance, notifications étendues, réponse et un nouveau mode de pontage pour les notifications. Pour plus d’informations sur les nouvelles fonctionnalités d’usure version 2.0, consultez le Android [présentation de l’API](https://developer.android.com/wear/preview/api-overview.html). 



## <a name="devices"></a>Appareils

Voici quelques exemples des appareils qui peuvent s’exécuter usure Android :

* [Motorola 360](https://moto360.motorola.com/)
* [LG G espion](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G espion R](http://www.lg.com/us/smartwatch/g-watch-r)
* [ENGRENAGE Samsung dynamique](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASU ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>informations supplémentaires

Passez en revue de la documentation d’usure Android de Google :

* [À propos d’usure Android](http://www.android.com/wear/)
* [Conception d’application Android usure](https://developer.android.com/design/wear/index.html)
* [bibliothèque de Android.support.wearable ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android usure 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>Récapitulatif

Cette introduction fourni une vue d’ensemble d’usure Android. Il décrit les fonctionnalités de base d’usure Android et inclus une vue d’ensemble des fonctionnalités introduites dans Android 2.0 porter. Il fourni des liens vers la lecture essentiels pour aider les développeurs à prendre en main Xamarin.Android usure développement, et il répertorié quelques exemples de certaines des appareils Android d’usure actuellement sur le marché.


## <a name="related-links"></a>Liens associés

- [Installation et configuration](~/android/wear/get-started/installation.md)
- [Prise en main](~/android/wear/get-started/index.md)
