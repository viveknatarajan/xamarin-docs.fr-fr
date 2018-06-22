---
title: Android Wear
description: Création d’applications pour les appareils Android portable.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: ec8ed32eba7d8341904f800d5c174235dc25388c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768673"
---
# <a name="android-wear"></a>Android Wear

Usure Android est une version d’Android est conçu pour des périphériques portable observe actives. Cette section inclut des instructions sur la façon d’installer et configurer les outils requis pour le développement d’usure, une procédure pas à pas détaillées pour la création de votre premier appareil usure et une liste d’exemples que vous pouvez consulter pour porter des applications pour créer votre propre.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Prise en main](~/android/wear/get-started/index.md)

Introduit l’usure Android, explique comment installer et configurer votre ordinateur pour le développement d’usure et fournit les étapes pour vous aider à créer et exécuter votre première application usure Android sur un émulateur ou un périphérique d’usure.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Interface utilisateur](~/android/wear/user-interface/index.md)

Explique Android usure contrôle et fournit des liens vers des exemples qui illustrent l’utilisation de ces contrôles.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Fonctionnalités de la plateforme](~/android/wear/platform/index.md)

Documents de cette section couvrent les fonctionnalités spécifiques à l’usure Android. Vous trouverez ici une rubrique qui explique comment créer un WatchFace.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Tailles d’écran](~/android/wear/screen-sizes.md)

Afficher un aperçu et optimiser votre interface utilisateur pour les tailles d’écran disponibles.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Déploiement et test](~/android/wear/deploy-test/index.md)

Explique comment déployer votre application d’usure Android sur un appareil Android d’usure ou émulateur Android configuré pour l’usure. Il inclut également le débogage des conseils et informations pour savoir comment configurer une connexion Bluetooth entre votre ordinateur de développement et un appareil Android.

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[API d’usure](https://developer.android.com/reference/android/support/wearable)

Le site de développement Android fournit des informations détaillées sur la clé API d’usure comme [activité portable](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [intentions](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [authentification](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ Complications](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [Complications rendu](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [Notifications](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [vues](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), et [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Exemples

Vous trouverez un certain nombre de [exemples](https://developer.xamarin.com/samples/android/Android%20Wear/) à l’aide de l’usure Android (ou accédez directement au [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

|Exemple|Description|Capture d'écran|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|Un exemple simple des notions de base des projets portable, y compris GridViewPager et notifications interactives.|![Capture d’écran de Skeletonwear](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|Une démonstration simple du contrôle WatchViewStub qui détecte la forme de l’écran et charge automatiquement la disposition correcte.  Voir comment WatchViewStub fonctionne dans le **Resources/layout/main_actvity.xml** mise en page.|![Capture d’écran de WatchViewStub](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|Démonstration de pages de notification d’usure, sous la forme d’étapes de la recette. Les notifications sont créées dans RecipeService.cs.|![Capture d’écran de RecipeAssistant](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|Exemple amusant de l’interaction avec un assistant « personnel » appelé Eliza, à l’aide de notifications d’usure interactives pour créer une conversation à l’aide de réponses toutes.|![Capture d’écran de ElizaChat](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager implémente le modèle de navigation 2D, où l’utilisateur fait glisser sa de verticalement puis horizontalement pour naviguer dans le contenu et les options.|![Capture d’écran de GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace est un type personnalisé Espion avec analogique-style heure, minute et seconde mains. Cet exemple montre comment créer un service de face espion qui dessine l’heure actuelle et de visibilité et le mode ambiante gère les événements de modification. Il inclut un récepteur de diffusion qui écoute les modifications de fuseau horaire et met automatiquement à jour l’heure en conséquence.|![Capture d’écran de WatchFace](images/gridviewpager.png)|


##  <a name="videos"></a>Vidéos

Consultez ces vidéo prend en charge les liens qui traitent de Xamarin.Android avec usure :

|Description|Capture d'écran|
|--- |--- |
|[Android L et bien plus](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android L Developer Preview introduit une multitude de nouvelles API permettant aux développeurs de tirer parti de, y compris les documents de conception, les notifications et les nouvelles animations citer que quelques-uns.|![Capture d’écran de vidéo de présentation](images/video-android-l.png)|
|[C# est enchante et mes yeux : Google Glass et usure Android](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; informatique portable peut sembler quelque chose de demain (ou un épisode Inspecteur Gadget), mais de nombreuses personnes déjà adoptent le futur dès aujourd'hui ! Les développeurs c# cette information et aient déjà les outils et les compétences nécessaires pour exploiter la puissance de périphériques portable (à partir de Evolve 2014).|![Capture d’écran de vidéo de présentation](images/video-eyes-ears.png)|
|[Nouveautés de Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; L Android, l’usure Android, TV Android, Android automatique, documents de conception et ART ; ce que fait cette moyenne pour vous en tant que développeur Xamarin ? de Evolve 2014.|![Capture d’écran de vidéo de présentation](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
