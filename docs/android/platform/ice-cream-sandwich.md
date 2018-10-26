---
title: ICE Cream Sandwich fonctionnalités
description: Cet article décrit plusieurs des nouvelles fonctionnalités disponibles pour les développeurs d’applications avec l’API Android de 4 - Ice Cream Sandwich. Il couvre plusieurs nouvelles technologies d’interface utilisateur et examine une variété de nouvelles fonctionnalités Android 4 offre pour partager des données entre les applications et entre les appareils.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: adb53af9d2e6707cb1fca3c59af63db76e5346d5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102618"
---
# <a name="ice-cream-sandwich-features"></a>ICE Cream Sandwich fonctionnalités

_Cet article décrit plusieurs des nouvelles fonctionnalités disponibles pour les développeurs d’applications avec l’API Android de 4 - Ice Cream Sandwich. Il couvre plusieurs nouvelles technologies d’interface utilisateur et examine une variété de nouvelles fonctionnalités Android 4 offre pour partager des données entre les applications et entre les appareils._

## <a name="overview"></a>Vue d'ensemble

Système d’exploitation Android version 4.0 (API niveau 14) représente un retravailler majeure du système d’exploitation Android et inclut un certain nombre de modifications importantes et des mises à niveau, notamment :

-   **Mise à jour de l’Interface utilisateur** : plusieurs nouvelles fonctionnalités de l’interface utilisateur offrent plus de puissance et flexibilité lorsqu’ils créent l’utilisateur de l’application des interfaces. Ces nouvelles fonctionnalités incluent : `GridLayout` , `PopupMenu` , `Switch` widget, et `TextureView` . 
-   **L’accélération matérielle de meilleures** – 2D rendering maintenant a lieu sur le GPU pour Android tous les contrôles. En outre, l’accélération matérielle est activée, par défaut, dans toutes les applications développées pour Android 4.0. 
-   **Nouvelles API de données** – il est nouveau accès aux données qui n’était pas précédemment officiellement accessibles, telles que les données de calendrier et le profil utilisateur du propriétaire de l’appareil. 
-   **Partage des données d’application** – partage de données entre applications et périphériques est désormais plus facile que jamais grâce à des technologies telles que la `ShareActionProvider` , ce qui facilite la création d’une action de partage à partir d’une barre d’Action, et *Android faisceau* pour *Communications NFC (Near Field)* , ce qui rend très facile de partager des données entre les périphériques à proximité les uns des autres. 


Dans cet article, nous allons explorer ces fonctionnalités et autres modifications qui ont été apportées à l’API Android 4.0 et nous expliquerons comment utiliser chaque fonctionnalité avec Xamarin.Android.

## <a name="user-interface-features"></a>Fonctionnalités d’Interface utilisateur

Une variété de nouvelles technologies d’interface utilisateur sont disponibles avec 4 Android, notamment :

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – mise en page de grille 2D prend en charge des contrôles. 
-   **[Basculer le widget](~/android/user-interface/controls/switch.md)**  – permet de basculer entre ON ou OFF. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – permet de vidéo et le contenu OpenGL dans une vue. 
-   **[Barre de navigation](~/android/user-interface/controls/navigation-bar.md)**  – contient les boutons virtuels pour la sauvegarde, à domicile et multitâche. 


En outre, autres éléments d’interface utilisateur ont été améliorées, telles que le `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, qui est désormais plus facile à manipuler et des onglets, qui ont une apparence plus sophistiquée.

## <a name="sharing-features"></a>Fonctions de partage

Android 4 inclut plusieurs nouvelles technologies qui nous permettent de partager des données sur les appareils et les applications. Il donne également accès à différents types de données qui n’étaient pas auparavant disponibles, telles que les informations de calendrier et un profil d’utilisateur du propriétaire du périphérique. Dans cette section, nous allons examiner une variété de fonctionnalités offertes par Android 4 cette adresse ces domaines, y compris :

-  **[FAISCEAU Android](~/android/platform/android-beam.md)**  : partage via NFC de permet aux données.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – crée un fournisseur qui permet aux développeurs de spécifier des actions de partage à partir de la barre d’Action. 
-   **[Profil utilisateur](~/android/user-interface/user-profile.md)**  – fournit l’accès aux données de profil de propriétaire de l’appareil. 
-   **[API de calendrier](~/android/user-interface/controls/calendar.md)**  – fournit l’accès aux données de calendrier à partir du fournisseur de calendrier. 

## <a name="x86-emulators"></a>x86 émulateurs

ICS ne prend pas encore en charge le développement avec un x86 émulateur. x86 émulateurs sont uniquement pris en charge avec Android 2.3.3, niveau d’API 10. Consultez [configuration le x86 émulateur](~/android/get-started/installation/android-emulator/index.md) pour plus d’informations.

## <a name="summary"></a>Récapitulatif

Cet article a abordé une variété de nouvelles technologies qui sont désormais disponibles avec Android 4. Nous avons examiné les nouvelles fonctionnalités d’interface utilisateur tels que le *GridLayout*, *menu contextuel*, et *commutateur* widget. Nous avons également examiné certaines la nouvelle prise en charge pour le contrôle de l’interface utilisateur, système et la manière pour travailler avec le *TextureView*. Ensuite, nous avons abordé une variété de nouvelles technologies de partage. Nous couverts comment *faisceau Android* nous allons partager des informations sur les appareils qui utilisent *NFC*, décrit le nouveau *API Calendar*et a également montré comment utiliser intégrée dans  *ShareActionProvider*.
Enfin, nous avons examiné comment utiliser le *ContactsContract* fournisseur pour accéder aux données de profil utilisateur.



## <a name="related-links"></a>Liens associés

- [Exemples d’ice Cream Sandwich](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (exemple)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (exemple)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Didacticiel de mise en page d’onglet](~/android/user-interface/layouts/tab-layout/index.md)
- [ICE Cream Sandwich](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Plateforme 4.0 Android](http://developer.android.com/about/versions/android-4.0.html)
