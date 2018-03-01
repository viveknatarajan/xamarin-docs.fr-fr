---
title: GridViewPager
ms.topic: article
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: f156d802d807c4087331ca5796b046f8f5f2fa0d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="gridviewpager"></a>GridViewPager

Le [GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) exemple montre comment implémenter le modèle de navigation de sélecteur 2D pour porter Android.

![Capture d’écran de GridViewPager sur un écran carré](gridviewpager-images/gridviewpager.png)

Tout d’abord ajouter le [Xamarin Android usure prennent en charge](http://www.nuget.org/packages/Xamarin.Android.Wear/) package NuGet à votre projet.

La mise en page XML ressemble à ceci :

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Créer un [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (ou une sous-classe comme [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) pour fournir des affichages à afficher lorsque l’utilisateur navigue.

Le [exemple d’adaptateur](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) montre comment implémenter les méthodes obligatoires, y compris les remplacements pour `RowCount`, `GetColumnCount`, `GetBackground`, et `GetFragment`

Rattachez l’adaptateur comme indiqué :

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Liens associés

- [Document de sélecteur de Google 2D](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [documents Android.support.wearable](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (sample)](https://developer.xamarin.com/samples/GridViewPager/)
