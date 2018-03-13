---
title: "Mise en page d’onglet avec TabHost"
description: "Cet article fournit une vue d’ensemble de haut niveau de la la TabHost, une API antérieure utilisé pour créer des mises en page à onglets dans une application Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 77B890A4-27A6-41DF-81BA-22C6116A8FB2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: e27557c65d2b3049457640a3492d090c5fa26a43
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="tab-layout-with-tabhost"></a>Mise en page d’onglet avec TabHost

_Cet article fournit une vue d’ensemble de haut niveau de la la TabHost, une API antérieure utilisé pour créer des mises en page à onglets dans une application Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

> [!NOTE]
> `TabHost` est une ancienne API a été déconseillée par Google. Les développeurs sont encouragés à créer des applications à onglets à l’aide de la [ActionBar](~/android/user-interface/controls/action-bar.md). Le `ActionBar` est disponible dans toutes les version d’Android. Il a été introduite dans 3.0 Android (API niveau 11) et a été déplacée dans Android 2.2 (API niveau 8) et Android 2.3 (API de niveau 10) dans le [V7 AppCompat bibliothèque](http://developer.android.com/tools/support-library/features.html#v7-appcompat), qui est disponible pour Xamarin.Android via la [Xamarin Bibliothèque de prise en charge Android - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package.

Le `TabHost` est l’API plus ancienne et d’origine pour la création d’utilisateur à onglets interfacesIt est mieux adaptée aux Applications Xamarin.Android qui doit prendre en charge Android 2.2 et 2.3 Android et ne peut pas utiliser **ActionBarSherlock**.
Les cinq composants suivants sont impliqués avec la `TabHost` API :

-  **TabActivity** &ndash; il s’agit d’une vue spécialisée qui agit comme un conteneur pour un `TabHost` (décrites ci-dessous).

-  **TabHost** &ndash; il s’agit d’un conteneur pour l’interface utilisateur à onglets. Il héberge deux enfants : une liste des étiquettes d’onglets et un `FrameLayout` qui affiche le contenu de l’onglet.

-  **TabWidget** &ndash; ce contrôle affiche une liste des étiquettes d’onglets, un pour chaque onglet dans le `TabHost` . Chaque onglet dans un `TabHost` sont représentées par un `TabHost.TabSpec` objet. Cet objet contient les métadonnées pour chaque onglet. Lorsque l’utilisateur sélectionne un onglet, le `TabHost` répond à la sélection en affichant l’onglet approprié.

-  **FrameLayout** &ndash; une interface utilisateur à onglets doit avoir un `FrameLayout` contenue à l’intérieur d’un `TabHost`. Il est utilisé pour afficher le contenu de l’onglet.

-  **Activités ou les vues** &ndash; lorsqu’un onglet est sélectionné, il affiche une activité ou une vue dans le `FrameLayout`.

Le diagramme suivant montre comment tous ces composants sont reliés entre eux :

![Diagramme illustrant la disposition du Frame dans TabWidget dans TabHost](tab-host-images/image03.png)

Contenu de l’onglet peut être soit des activités ou des vues. Les vues sont relativement léger et simple, mais peuvent entraîner une grande quantité de code indépendants co-habitating dans l’activité. Cela entraîne une séparation médiocre des problèmes et une classe trop importants qui est difficile à maintenir. En revanche, les activités requièrent des ressources système mais permettant une approche plus modulaire avec la logique de chaque onglet encapsulé dans son propre classe distinctes.


## <a name="summary"></a>Récapitulatif

Cet article a expliqué les composants de haut niveau de l’ancien `TabHost` API pour Android et tous leurs relations entre eux.



## <a name="related-links"></a>Liens associés

- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Package NuGet de prise en charge Android bibliothèque v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [bibliothèque d’appcompat V7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
