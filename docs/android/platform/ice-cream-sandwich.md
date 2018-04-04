---
title: Fonctionnalités de glace Sandwich
description: Cet article décrit plusieurs des nouvelles fonctionnalités disponibles aux développeurs d’applications avec l’API Android de 4 - Sandwich de glace. Elle couvre plusieurs nouvelles technologies d’interface utilisateur et examine un éventail de fonctionnalités nouvelles qui Android 4 offre de partage des données entre les applications et entre les appareils.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 3c5412629f6dcd31fb0a82634ef530569eef459a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="ice-cream-sandwich-features"></a>Fonctionnalités de glace Sandwich

_Cet article décrit plusieurs des nouvelles fonctionnalités disponibles aux développeurs d’applications avec l’API Android de 4 - Sandwich de glace. Elle couvre plusieurs nouvelles technologies d’interface utilisateur et examine un éventail de fonctionnalités nouvelles qui Android 4 offre de partage des données entre les applications et entre les appareils._

## <a name="overview"></a>Vue d'ensemble

Système d’exploitation Android version 4.0 (API niveau 14) représente un retravailler principales du système d’exploitation Android et inclut un certain nombre de modifications importantes et les mises à niveau, y compris :

-   **Mise à jour de l’Interface utilisateur** : plusieurs nouvelles fonctionnalités de l’interface utilisateur offrent plus de puissance et flexibilité lors de la création d’utilisateur de l’application des interfaces. Ces nouvelles fonctionnalités incluent : `GridLayout` , `PopupMenu` , `Switch` widget, et `TextureView` . 
-   **Une meilleure l’accélération matérielle** – 2D rendu maintenant a lieu sur le GPU pour Android tous les contrôles. En outre, l’accélération matérielle est activée, par défaut, dans toutes les applications développées pour Android 4.0. 
-   **Nouvelles API de données** – il est nouveau accès aux données qui n’était pas précédemment officiellement accessibles, tels que les données de calendrier et le profil d’utilisateur du propriétaire du périphérique. 
-   **Partage des données d’application** – partage des données entre des applications et des périphériques est désormais plus facile que jamais grâce à des technologies telles que la `ShareActionProvider` , ce qui la rend facile de créer une action de partage à partir de la barre d’Action et *FAISCEAUX Android* pour *Communications NFC (Near Field)* , ce qui rend un composant logiciel enfichable pour partager des données entre les périphériques à proximité les uns des autres. 


Dans cet article, nous allons explorer ces fonctionnalités et les autres modifications ont été apportées à l’API Android 4.0 et nous allons expliquer comment utiliser chaque fonctionnalité avec Xamarin.Android.

## <a name="user-interface-features"></a>Fonctionnalités d’Interface utilisateur

Une variété de nouvelles technologies d’interface utilisateur sont disponibles pour Android 4, y compris :

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – mise en page de grille 2D prend en charge des contrôles. 
-   **[Basculer le widget](~/android/user-interface/controls/switch.md)**  – permet de basculer entre ON ou OFF. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – permet de vidéo et le contenu OpenGL dans une vue. 
-   **[Barre de navigation](~/android/user-interface/controls/navigation-bar.md)**  – contient des boutons virtuel pour la sauvegarde, domicile et multitâches. 


En outre, autres éléments d’interface utilisateur ont été améliorées, telles que le `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, qui est désormais plus facile à utiliser et les tabulations, qui ont une apparence plus sophistiquée.

## <a name="sharing-features"></a>Fonctions de partage

Android 4 inclut plusieurs nouvelles technologies qui permettent de partager des données sur les périphériques et les applications. Il donne également accès aux différents types de données qui n’étaient pas auparavant disponibles, tels que les informations relatives au calendrier et le profil d’utilisateur du propriétaire du périphérique. Dans cette section, que nous allons examiner un large éventail de fonctionnalités offertes par Android 4 cette adresse ces zones, y compris :

-  **[Faisceaux Android](~/android/platform/android-beam.md)**  : autorise le partage via NFC des données.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – crée un fournisseur qui permet aux développeurs de spécifier des actions de partage à partir de la barre d’Action. 
-   **[Profil utilisateur](~/android/user-interface/user-profile.md)**  – permet d’accéder aux données de profil du propriétaire du périphérique. 
-   **[API de calendrier](~/android/user-interface/controls/calendar.md)**  – fournit l’accès aux données de calendrier à partir du fournisseur de calendrier. 

## <a name="x86-emulators"></a>x86 émulateurs

Partage de connexion Internet ne prend pas en charge le développement avec x86 émulateur. x86 émulateurs sont uniquement pris en charge avec Android 2.3.3, niveau 10 de l’API. Consultez [configuration le x86 émulateur](~/android/get-started/installation/android-emulator/index.md) pour plus d’informations.

## <a name="summary"></a>Récapitulatif

Cet article couvert les nouvelles technologies sont désormais disponibles avec Android 4 diverses. Nous avons vu comment nouvelles fonctionnalités d’interface utilisateur tels que les *GridLayout*, *menu contextuel*, et *commutateur* widget. Nous avons également examiné certaines de la nouvelle prise en charge pour le contrôle du système de l’interface utilisateur, et la manière de travailler avec les *TextureView*. Nous avons étudié une variété de nouvelles technologies de partage. Nous couverts comment *FAISCEAUX Android* nous allons partager des informations sur les périphériques qui utilisent *NFC*, décrit le nouveau *API Calendar*et a également montré comment utiliser intégrée dans  *ShareActionProvider*.
Enfin, nous avons examiné comment utiliser le *ContactsContract* fournisseur pour accéder aux données de profil utilisateur.



## <a name="related-links"></a>Liens associés

- [Exemples de glace Sandwich](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (sample)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (exemple)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Didacticiel de mise en page d’onglet](~/android/user-interface/layouts/tab-layout/index.md)
- [Glace Sandwich](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Plateforme 4.0 Android](http://developer.android.com/about/versions/android-4.0.html)
