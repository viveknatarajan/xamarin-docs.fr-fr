---
title: Fonctionnalités de la plateforme de Xamarin.Android
description: Documents de cette section couvrent les fonctionnalités spécifiques pour Android. Vous trouverez ici des sujets tels que l’utilisation de Fragments, utiliser des cartes d’et encapsulation des données avec les fournisseurs de contenu.
ms.prod: xamarin
ms.assetid: DDE54082-6E2B-9ED9-05FB-D9C1D1B1258E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8c26cc3af97cad26e8add248b5bf4aabcefc614b
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436529"
---
# <a name="platform-features"></a>Fonctionnalités de la plateforme

_Documents de cette section couvrent les fonctionnalités spécifiques pour Android. Vous trouverez ici des sujets tels que l’utilisation de Fragments, utiliser des cartes d’et encapsulation des données avec les fournisseurs de contenu._

## <a name="android-beamandroidplatformandroid-beammd"></a>[Android Beam](~/android/platform/android-beam.md)

Faisceaux Android est une nouvelle technologie de Communication NFC (Near Field) dans Android 4 qui permet aux applications de partager des informations sur NFC lorsqu’à proximité.

## <a name="fingerprint-authenticationandroidplatformfingerprint-authenticationindexmd"></a>[Authentification par empreinte digitale](~/android/platform/fingerprint-authentication/index.md)

Cette section explique comment utiliser l’authentification par empreinte digitale, introduite initialement dans Android 6.0, à une application Xamarin.Android.


## <a name="firebase-job-dispatcherandroidplatformfirebase-job-dispatchermd"></a>[Répartiteur de travail Firebase](~/android/platform/firebase-job-dispatcher.md)

Ce guide décrit le répartiteur de travail Firebase et comment l’utiliser pour simplifier les tâches en arrière-plan en cours d’exécution dans une application Xamarin.Android.



##  <a name="fragmentsandroidplatformfragmentsindexmd"></a>[Fragments](~/android/platform/fragments/index.md)

Android 3.0 introduit des Fragments, en montrant comment prendre en charge des modèles plus flexibles pour les nombreux différentes tailles d’écran trouvés sur les téléphones et tablettes. Cet article couvre l’utilisation de Fragments de développer des applications de Xamarin.Android et également la prise en charge des Fragments sur Android des périphériques (API niveau 11) 3.0. 



## <a name="app-linkingandroidplatformapp-linkingmd"></a>[Liaison d’application](~/android/platform/app-linking.md)

Ce guide explique la prise en charge Android 6.0 _la liaison application_, une technique qui permet de répondre aux URL sur les sites Web des applications mobiles. Il explique comment implémenter la liaison application dans une application Android 6.0 et comment configurer un site Web pour accorder des autorisations à l’application mobile pour gérer les liens pour le domaine d’application.



##  <a name="android-8-oreoandroidplatformoreomd"></a>[Android 8 Oreo](~/android/platform/oreo.md)

Cet article fournit une description des nouvelles fonctionnalités de Oreo Android, explique comment préparer Xamarin.Android pour le développement de Oreo Android et fournit des liens vers des exemples d’applications qui illustrent l’utilisation des fonctionnalités de Oreo Android dans les applications Xamarin.Android.



##  <a name="android-7-nougatandroidplatformnougatmd"></a>[Android 7 Nougat](~/android/platform/nougat.md)

Cet article fournit une vue d’ensemble générale des nouvelles fonctionnalités introduites dans Android 7.0 commande.




##  <a name="android-6-marshmallowandroidplatformmarshmallowmd"></a>[Android 6 Marshmallow](~/android/platform/marshmallow.md)

Cet article fournit une vue d’ensemble générale des nouvelles fonctionnalités introduites dans Android Marshmallow 6.0.




##  <a name="android-5-lollipopandroidplatformlollipopmd"></a>[Android 5 Lollipop](~/android/platform/lollipop.md)

Ce guide fournit une vue d’ensemble des nouvelles fonctionnalités Android 5.0 Lollipop comme thème de matériel, CardView, RecyclerView et les têtes de Notifications, et il lie détaillées pour les articles qui vous aident à utilisent ces nouvelles fonctionnalités dans votre application. 



##  <a name="android-44-kitkatandroidplatformkitkatmd"></a>[Android 4.4 KitKat](~/android/platform/kitkat.md)

Android 4.4 (KitKat) est fournie avec une multitude de fonctionnalités pour les utilisateurs et les développeurs. Ce guide met en surbrillance plusieurs de ces fonctionnalités et fournit des exemples de code et les détails d’implémentation pour vous aider à tirer le meilleur parti KitKat. 




##  <a name="android-41-jelly-beanandroidplatformjelly-beanmd"></a>[Android 4.1 Jelly Bean](~/android/platform/jelly-bean.md)

Ce document fournit une vue d’ensemble générale des nouvelles fonctionnalités pour les développeurs qui ont été introduits dans Android 4.1. Ces fonctionnalités sont notamment : améliorée des notifications, les mises à jour Android faisceaux de partager des fichiers volumineux, mises à jour de la découverte du réseau multimédia, égal à égal, les animations, les nouvelles autorisations. 



##  <a name="android-40-ice-cream-sandwichandroidplatformice-cream-sandwichmd"></a>[Android 4.0 Ice Cream Sandwich](~/android/platform/ice-cream-sandwich.md)

Cet article décrit plusieurs des nouvelles fonctionnalités disponibles aux développeurs d’applications avec le *API 4 Android - glace Sandwich*. Elle couvre plusieurs nouvelles technologies d’interface utilisateur et examine un éventail de fonctionnalités nouvelles qui Android 4 offre de partage des données entre les applications et entre les appareils. 


##  <a name="working-with-the-android-manifestandroid-manifestmd"></a>[Utilisation avec le manifeste Android](android-manifest.md)

Cet article introducts le fichier AndroidManifest.xml et comment il est peut-être être utilisé pour contrôler les fonctionnalités et décrire la configuration requise d’un Mono pour une application Android.


##  <a name="introduction-to-content-providersandroidplatformcontent-providersindexmd"></a>[Introduction aux fournisseurs de contenu](~/android/platform/content-providers/index.md)

Un ContentProvider encapsule un référentiel de données et fournit une API pour y accéder. Le fournisseur existe dans le cadre d’une application Android qui généralement fournit également une interface utilisateur pour l’affichage et la gestion des données. Le principal avantage de l’utilisation d’un fournisseur de contenu est l’activation d’autres applications d’accéder facilement aux données encapsulées à l’aide d’un objet de client de fournisseur (appelé un ContentResolver). Ensemble un fournisseur de contenu et le programme de résolution de contenu offrent une API cohérente entre les applications d’accès aux données est simple pour créer et consommer. Ce document montre comment accéder à et générer ContentProviders avec Xamarin.Android. 



##  <a name="maps-and-locationandroidplatformmaps-and-locationindexmd"></a>[Cartes et emplacement](~/android/platform/maps-and-location/index.md)

Cette section explique comment utiliser des cartes et un emplacement avec Xamarin.Android. Elle couvre tous les éléments de tirer parti de l’application de cartes intégrées à l’utilisation de la [API Android de Google mappe v2](https://developers.google.com/maps/documentation/android/) directement. En outre, elle explique comment utiliser une seule API pour travailler avec les services d’emplacement, qui utilisent la triangulation cellulaire pour permettre à une application obtenir les correctifs de l’emplacement, l’emplacement du Wi-Fi et GPS. 



## <a name="android-speechandroidplatformspeechmd"></a>[Reconnaissance vocale Android](~/android/platform/speech.md)

Cette section explique comment utiliser le texte Android pour la reconnaissance vocale et de reconnaissance vocale pour les installations de texte. Elle traite également des modules linguistiques lors de l’installation et l’interprétation du texte pour l’appareil. 


##  <a name="binding-a-java-librarybinding-java-libraryindexmd"></a>[Liaison d’une bibliothèque Java](binding-java-library/index.md)

Ce guide explique comment incorporer des bibliothèques Java dans les applications Xamarin.Android en créant une bibliothèque de liaisons.

##  <a name="java-integrationjava-integrationindexmd"></a>[Intégration Java](java-integration/index.md)

Cet article fournit une vue d’ensemble des méthodes que les développeurs peuvent réutiliser des composants existants dans les applications de Xamarin.Android Java.

##  <a name="renderscriptrenderscriptmd"></a>[Renderscript](renderscript.md)

Ce guide décrit Renderscript.