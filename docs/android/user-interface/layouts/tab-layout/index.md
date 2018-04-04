---
title: Mises en page à onglets
description: Une vue d’ensemble des mises en page à onglets dans Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 4095944bb630637a2e761af18796dacdef17785c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-layouts"></a>Mises en page à onglets


## <a name="overview"></a>Vue d'ensemble

Onglets sont un modèle d’interface utilisateur populaires dans les applications mobiles en raison de leur simplicité et la facilité d’utilisation. Ils fournissent un moyen simple et cohérent pour naviguer entre les différents écrans dans une application. Android dispose de plusieurs API pour les interfaces à onglets : 

-   **TabHost** &ndash; c’est l’API d’origine pour la création d’interfaces utilisateur à onglets. A `TabHost` widget est ajouté à une mise en page et agit comme un conteneur pour les vues à onglets. Cette API a depuis été déconseillée et son utilisation est déconseillée. 

-   **ActionBar** &ndash; cela fait partie d’un nouvel ensemble d’API qui a été introduite dans 3.0 Android (API niveau 11) dans le but de fournir une interface de commutation de l’affichage et navigation. Il a été porté précédent pour Android 2.2 (API niveau 8) avec le [bibliothèque de prise en charge Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; indique les pages de précédent et suivant en cours, d’un `ViewPager`. `ViewPager` est disponible uniquement via [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Pour plus d’informations sur `PagerTabStrip`, consultez [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Barre d’outils** &ndash; `Toolbar` est un composant de barre d’action plus récent et plus souple qui remplace `ActionBar`. `Toolbar` est disponible dans Android interface (lollipop) 5.0 ou version ultérieure, et il est également disponible pour les versions antérieures d’Android via le [bibliothèque de prise en charge Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package NuGet. 
    `Toolbar` est actuellement le composant de barre d’action recommandée à utiliser dans des applications Android.
    Pour plus d’informations, consultez [barre d’outils](~/android/user-interface/controls/tool-bar/index.md). 


Ces API est visuellement très différents et n’est pas compatibles entre eux. Ce qui suit montre l’illustration de l’écran `TabHost` et `ActionBar` côte à côte : 

![Captures d’écran de TabHost sur la gauche et ActionBar sur la droite](images/image01.png)

Ces API incompatible existe en raison de modifications importantes de l’interface utilisateur depuis 3.0 Android (API niveau 11). L’une de ces modifications de l’interface utilisateur était la [action barre le modèle de conception](http://www.androidpatterns.com/uap_pattern/action-bar), un modèle destiné à fournir un accès facile aux fonctionnalités de navigation et la clé d’une application. Le `ActionBar` API a été introduit pour prendre en charge ce modèle. 

Le `ActionBar` API est plus simple et sans doute fournit une meilleure expérience utilisateur. Il a été déplacée de nouveau vers l’Android 2.2 et est le choix par défaut pour les applications de Xamarin.Android. 

Le `TabHost` API est compatible entre les différentes versions d’Android, mais nécessite davantage d’efforts à utiliser et n’est pas cohérent avec actuel [instructions d’interface utilisateur Android](http://developer.android.com/design/index.html). Les développeurs sont déconseillés d’utiliser cette API et doivent favoriser la plus récente ActionBar pour leurs applications Xamarin.Android. 



## <a name="actionbarsherlock"></a>ActionBarSherlock

Avant de l’API de ActionBar ont été utilisées pour Android 2.2, les développeurs qui voulaient l’apparence plus récente de l’API ActionBar mais peut utiliser une bibliothèque tierce, [ActionBarSherlock](http://actionbarsherlock.com). ActionBarSherlock est une extension de la bibliothèque de prise en charge Android backport le modèle de conception action barre pour Android 2.x. Lors de l’exécution sur Android 3.0 ou version ultérieure, ActionBarSherlock utiliseront automatiquement natif `ActionBar` API fournie par Android. Les versions antérieures d’Android utilise une implémentation personnalisée sera imitant l’apparence de la plus récente `ActionBar` API. Le [ActionBarSherlock composant](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) permet de facilement ajouter ActionBarSherlock à une application Xamarin.Android. 



## <a name="related-links"></a>Liens associés

- [Vue d’ensemble de TabHost](tab-host.md)
- [Procédure pas à pas TabHost](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Package NuGet de prise en charge Android bibliothèque v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [bibliothèque d’appcompat V7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
