---
title: Dispositions à onglets
description: Une vue d’ensemble des mises en page à onglets dans Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 5d88ffb44d12ee142314c74ca8e749164cbfe3b9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105959"
---
# <a name="tabbed-layouts"></a>Dispositions à onglets


## <a name="overview"></a>Vue d'ensemble

Onglets sont un modèle d’interface utilisateur populaires dans les applications mobiles en raison de leur simplicité et la facilité d’utilisation. Ils fournissent un moyen cohérent et facile de naviguer entre les différents écrans dans une application. Android a plusieurs API pour les interfaces à onglets : 

-   **Barre d’action** &ndash; cela fait partie d’un nouvel ensemble d’API qui a été introduit dans 3.0 Android (niveau d’API 11) dans le but de fournir une cohérence navigation et l’interface de basculement entre les vues. Il a été adapté de revenir à Android 2.2 (API niveau 8) avec le [v7 de bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; indique l’actuel, suivant et précédent de pages d’un `ViewPager`. `ViewPager` est uniquement disponible via un [v4 de la bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Pour plus d’informations sur `PagerTabStrip`, consultez [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Barre d’outils** &ndash; `Toolbar` est un composant de barre d’action plus récente et plus flexible qui remplace `ActionBar`. `Toolbar` est disponible dans Android Lollipop 5.0 ou version ultérieure, et il est également disponible pour les versions antérieures d’Android via le [v7 de bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package NuGet. 
    `Toolbar` est actuellement le composant de barre d’action recommandée à utiliser dans les applications Android.
    Pour plus d’informations, consultez [barre d’outils](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Liens associés

- [Onglets de la conception matérielle -](https://material.io/guidelines/components/tabs.html)- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Package NuGet AppCompat prise en charge Android bibliothèque v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [bibliothèque d’appcompat V7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
