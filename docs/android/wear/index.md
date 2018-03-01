---
title: Usure Android
description: "Création d’applications pour les appareils Android portable."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1dad5e859efdf69e7003b45724f718b16faffd62
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-wear"></a>Usure Android

## <a name="android-wear"></a>Usure Android

Usure Android est une version d’Android est conçu pour des périphériques portable observe actives. Cette section inclut des instructions sur la façon d’installer et configurer les outils requis pour le développement d’usure, une procédure pas à pas détaillées pour la création de votre premier appareil usure et une liste d’exemples que vous pouvez consulter pour porter des applications pour créer votre propre.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Prise en main](~/android/wear/get-started/index.md)

Introduit l’usure Android, explique comment installer et configurer votre ordinateur pour le développement d’usure et fournit les étapes pour vous aider à créer et exécuter votre première application usure Android sur un émulateur ou un périphérique d’usure.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Interface utilisateur](~/android/wear/user-interface/index.md)

Explique Android usure contrôle et fournit des liens vers des exemples qui illustrent l’utilisation de ces contrôles.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Fonctionnalités de plateforme](~/android/wear/platform/index.md)

Documents de cette section couvrent les fonctionnalités spécifiques à l’usure Android. Vous trouverez ici une rubrique qui explique comment créer un WatchFace.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Tailles d’écran](~/android/wear/screen-sizes.md)

Afficher un aperçu et optimiser votre interface utilisateur pour les tailles d’écran disponibles.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Déploiement et test](~/android/wear/deploy-test/index.md)

Explique comment déployer votre application d’usure Android sur un appareil Android d’usure ou émulateur Android configuré pour l’usure. Il inclut également le débogage des conseils et informations pour savoir comment configurer une connexion Bluetooth entre votre ordinateur de développement et un appareil Android.


<a name="Samples" />

## <a name="samples"></a>Exemples

Vous trouverez un certain nombre de [exemples](https://developer.xamarin.com/samples/android/Android%20Wear/) à l’aide de l’usure Android (ou accédez directement au [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>Sample</strong>
      </th>
      <th>
          <strong>Description</strong>
      </th>
      <th>
          <strong>Screenshot</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
Un exemple simple des notions de base des projets portable, y compris GridViewPager et notifications interactives.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
Une démonstration simple du contrôle WatchViewStub qui détecte la forme de l’écran et charge automatiquement la disposition correcte.
Voir comment WatchViewStub fonctionne dans le <b>Resources/layout/main_actvity.xml</b> mise en page.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
Démonstration de pages de notification d’usure, sous la forme d’étapes de la recette. Les notifications sont créées dans <b>RecipeService.cs</b>.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
Exemple amusant de l’interaction avec un assistant « personnel » appelé Eliza, à l’aide de notifications d’usure interactives pour créer une conversation à l’aide de réponses toutes.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager implémente le modèle de navigation 2D, où l’utilisateur fait glisser sa de verticalement puis horizontalement pour naviguer dans le contenu et les options.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> est un type personnalisé Espion avec analogique-style heure, minute et seconde mains. Cet exemple montre comment créer un service de face espion qui dessine l’heure actuelle et de visibilité et le mode ambiante gère les événements de modification. Il inclut un récepteur de diffusion qui écoute les modifications de fuseau horaire et met automatiquement à jour l’heure en conséquence.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>Vidéos

Consultez ces vidéo liens qui traitent de Xamarin.Android avec usure prennent en charge.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0"/ /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L et bien plus encore</a>
        <br />
Le Android Developer Preview L a introduit une multitude de nouvelles API permettant aux développeurs de tirer parti de, y compris les documents de conception, les notifications et les nouvelles animations pour citer que quelques.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# est enchante et mes yeux : Google Glass et usure Android</a>
        <br />
Informatique portable peut sembler quelque chose de demain (ou un épisode Inspecteur Gadget), mais de nombreuses personnes déjà adoptent le futur dès aujourd'hui ! Les développeurs c# cette information et aient déjà les outils et les compétences nécessaires pour exploiter la puissance de périphériques portable (à partir de Evolve 2014).</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Nouveautés de Xamarin.Android</a>
        <br />
        <i>Android L, usure Android, TV Android, Android automatique, conception de matériel et ART ; que cela signifie pour vous en tant que Xamarin développeur ? </i> de faire évoluer 2014.</td>
    </tr>
</table>


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
