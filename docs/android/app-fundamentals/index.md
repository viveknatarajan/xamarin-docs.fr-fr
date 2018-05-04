---
title: Notions de base Xamarin.Android Application
description: Principaux Concepts d’applications
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: caa51fa0a70da1b799d56c706e6de974f61a14d9
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xamarinandroid-application-fundamentals"></a>Notions de base Xamarin.Android Application

Cette section fournit un guide sur certaines des tâches de choses ou concepts que les développeurs doivent connaître lors du développement d’applications Android plus courant.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Accessibilité](~/android/app-fundamentals/accessibility.md)

Cette page décrit comment utiliser les API d’accessibilité Android pour générer des applications en fonction de la [liste de vérification d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md)

Ce guide décrit comment Android utilise les niveaux d’API pour gérer la compatibilité des applications sur les différentes versions d’Android, et elle explique comment configurer les paramètres de projet Xamarin.Android pour déployer ces niveaux d’API dans votre application. En outre, ce guide explique comment écrire du code de runtime qui négocie avec différents niveaux d’API, et il fournit une liste de référence de tous les niveaux d’API Android, les numéros de version (par exemple, Android 8.0), les noms de code Android (par exemple, Oreo) et codes de version de build.



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Ressources dans Android](~/android/app-fundamentals/resources-in-android/index.md)

Cet article présente le concept de ressources Android dans les documents et de Xamarin.Android comment les utiliser. Elle couvre l’utilisation des ressources dans votre application Android pour prendre en charge la localisation des applications et plusieurs appareils, y compris les densités et tailles d’écran.




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md)

Les activités sont un bloc de construction fondamental des Applications Android, et ils peuvent exister dans un nombre d’états différents. Le cycle de vie d’activité commence par instanciation et se termine par la destruction et inclut de nombreux états entre les deux. Lorsqu’une activité change d’état, la méthode d’événements du cycle de vie approprié est appelée pour informer de l’activité du changement d’état imminente et lui permettant d’exécuter du code pour s’adapter à cette modification. Cet article examine le cycle de vie des activités et explique la responsabilité qu’une activité au cours de ces changements d’état pour faire partie d’une application fiable valide.

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Localisation](~/android/app-fundamentals/localization.md)

Cet article explique comment localiser un Xamarin.Android dans d’autres langues en traduisant les chaînes et en fournissant des images de remplacement.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Services](~/android/app-fundamentals/services/index.md)

Cet article traite des services Android, qui sont des composants Android qui autorisent le travail à effectuer en arrière-plan. Il explique les différents scénarios de services sont adaptés à et montre comment les implémenter à la fois pour les tâches en arrière-plan de longs ainsi que pour fournir une interface pour les appels de procédure distante.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Récepteurs de diffusion](~/android/app-fundamentals/broadcast-receivers.md)

Ce guide explique comment créer et utiliser des récepteurs de diffusion, un composant Android qui répond aux diffusions à l’échelle du système, dans Xamarin.Android.



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Autorisations](~/android/app-fundamentals/permissions.md)

Vous pouvez utiliser la prise en charge des outils intégré à Visual Studio pour Mac ou Visual Studio pour créer et ajouter des autorisations pour le manifeste Android. Ce document décrit comment ajouter des autorisations dans Visual Studio et Xamarin Studio.



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Graphismes et animation](~/android/app-fundamentals/graphics-and-animation.md)

Android fournit une infrastructure de très riche et diverses pour prendre en charge les animations et les graphiques 2D. Ce document présente ces infrastructures et explique comment créer des animations et des graphiques personnalisés et les utiliser dans une application Xamarin.Android.


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[Architectures de processeur](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android prend en charge plusieurs architectures d’UC, y compris les appareils 32 bits et 64 bits. Cet article explique comment cibler une application sur un ou plusieurs des architectures pris en charge par Android de l’UC.




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Rotation de la gestion](~/android/app-fundamentals/handling-rotation.md)

Cet article décrit comment gérer les modifications de l’orientation de périphérique dans Xamarin.Android. Elle couvre l’utilisation avec le système de ressource Android pour charger automatiquement les ressources pour une orientation de périphérique particulier, ainsi que la manière de gérer par programme l’orientation change. Ensuite, il décrit les techniques de maintenir l’état lorsque vous faites pivoter un appareil.



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Audio Android](~/android/app-fundamentals/android-audio.md)

Le système d’exploitation Android prend en charge étendue multimédia, qui comprennent des données audio et vidéo. Ce guide se concentre sur audio dans Android et couvre la lecture et l’enregistrement audio à l’aide du lecteur audio intégré et classes de l’enregistreur, ainsi que l’API de bas niveau audio. Elle traite également travailler avec les événements Audio diffusion par d’autres applications, afin que les développeurs peuvent créer des applications conformes.




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Notifications](~/android/app-fundamentals/notifications/index.md)

Cette section explique comment implémenter des notifications locale et distantes dans Xamarin.Android. Il décrit les différents éléments d’interface utilisateur d’une notification d’Android et traite des API d’impliquée dans la création et affichage d’une notification. Pour les notifications à distance, la messagerie Cloud Google et messagerie de Cloud Firebase sont expliqués. Exemples de code et des procédures pas à pas détaillées sont incluses.



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Entrées tactiles](~/android/app-fundamentals/touch/index.md)

Cette section explique les concepts et les détails de l’implémentation tactiles sur Android. Tactile API est introduites et expliqué suivi d’une exploration des modules de reconnaissance de mouvement.



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[Pile HttpClient et SSL/TLS](~/android/app-fundamentals/http-stack.md)

Cette section explique les sélecteurs HttpClient pile et l’implémentation de SSL/TLS pour Android. Ces paramètres déterminent l’implémentation du client HTTP et SSL/TLS qui sera utilisée par vos applications Xamarin.Android.


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[L’écriture d’Applications réactives](writing-responsive-apps.md)

Cet article explique comment utiliser des threads pour maintenir une application Xamarin.Android la réactivité en déplaçant des tâches longues une session sur un thread d’arrière-plan.