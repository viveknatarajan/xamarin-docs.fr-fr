---
title: "Contrôles d’usure Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5B62A5F8-5E55-4B3C-BFC4-E21CDB27C08B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 867246b3784fb244171058c7f4c6ec54092e813b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-wear-controls"></a>Contrôles d’usure Android

Android usure les applications peuvent utiliser la plupart des contrôles déjà en cours d’utilisation pour les applications Android régulières, y compris `Button`, `TextView`et drawables de l’image. Contrôles de disposition, notamment `ScrollView`, `LinearLayout`, et `RelativateLayout` peut également être utilisé.

Cette page établit un lien vers les contrôles spécifiques à usure Android à partir de la [portable bibliothèque d’interface utilisateur](https://developer.android.com/training/wearables/apps/layouts.html#UiLibrary) disponibles dans les projets de Xamarin via le [prise en charge portable](http://www.nuget.org/packages/Xamarin.Android.Wear/) package NuGet. Ces contrôles sont les suivants :

-   **GridViewPager** &ndash; créer une interface de navigation à deux dimensions où l’utilisateur fait défiler verticalement puis horizontalement à effectuer une sélection (pour plus d’informations, consultez [GridViewPager](~/android/wear/user-interface/controls/gridviewpager.md)) :

    ![Capture d’écran de l’exemple d’un GridViewPager](images/gridviewpager.png)

D’autres contrôles importants pour les applications d’usure incluent :

* `BoxInsetLayout` (consultez [utilisation de tailles d’écran](~/android/wear/screen-sizes.md)),

* `WatchViewStub` (consultez [utilisation de tailles d’écran](~/android/wear/screen-sizes.md)),

* `CardFrame` (consultez [Android cartes création](https://developer.android.com/training/wearables/ui/cards.html)),

* `CardScrollView` (consultez [Android cartes création](https://developer.android.com/training/wearables/ui/cards.html)),

* `WearableListView` (consultez [Android créer des listes](https://developer.android.com/training/wearables/ui/lists.html)).


## <a name="related-links"></a>Liens associés

- [Documents Android.Support.Wearable](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
