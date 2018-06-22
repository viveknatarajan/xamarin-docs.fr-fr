---
title: Mises en page à onglets
description: Une vue d’ensemble des mises en page à onglets dans Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149258"
---
# <a name="tabbed-layouts"></a>Mises en page à onglets


## <a name="overview"></a>Vue d'ensemble

Onglets sont un modèle d’interface utilisateur populaires dans les applications mobiles en raison de leur simplicité et la facilité d’utilisation. Ils fournissent un moyen simple et cohérent pour naviguer entre les différents écrans dans une application. Android dispose de plusieurs API pour les interfaces à onglets : 

-   **ActionBar** &ndash; cela fait partie d’un nouvel ensemble d’API qui a été introduite dans 3.0 Android (API niveau 11) dans le but de fournir une interface de commutation de l’affichage et navigation. Il a été porté précédent pour Android 2.2 (API niveau 8) avec le [bibliothèque de prise en charge Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; indique les pages de précédent et suivant en cours, d’un `ViewPager`. `ViewPager` est disponible uniquement via [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Pour plus d’informations sur `PagerTabStrip`, consultez [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Barre d’outils** &ndash; `Toolbar` est un composant de barre d’action plus récent et plus souple qui remplace `ActionBar`. `Toolbar` est disponible dans Android interface (lollipop) 5.0 ou version ultérieure, et il est également disponible pour les versions antérieures d’Android via le [bibliothèque de prise en charge Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package NuGet. 
    `Toolbar` est actuellement le composant de barre d’action recommandée à utiliser dans des applications Android.
    Pour plus d’informations, consultez [barre d’outils](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Liens associés

- [Les onglets conception matérielle -](https://material.io/guidelines/components/tabs.html)- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Package NuGet de prise en charge Android bibliothèque v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [bibliothèque d’appcompat V7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
