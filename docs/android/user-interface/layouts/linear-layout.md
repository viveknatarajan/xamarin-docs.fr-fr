---
title: LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: d8ee5f867c9a4d724c54c14b5afdd005f1b805d3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115768"
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) est un [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
qui affiche les enfants [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
éléments dans une direction linéaire, verticalement ou horizontalement.

Vous devez être prudent lorsque vous excessif à l’aide de la [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
Si vous commencez l’imbrication multiple [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s, vous souhaiterez envisager d’utiliser un [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
à la place.

Démarrer un nouveau projet nommé **HelloLinearLayout**.

Ouvrez **Resources/Layout/Main.axml** et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "match_parent"
      android:layout_height=    "match_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Examinez attentivement ce code XML. Il existe une racine [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
qui définit son orientation à la verticale &ndash; tous les enfants [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)s (dont il dispose de deux) sera empilé verticalement. Le premier enfant est un autre [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
qui utilise une orientation horizontale et le deuxième enfant est un [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
qui utilise une orientation verticale. Chacun d'entre eux imbriqué [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s contient plusieurs [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
éléments qui sont orientées entre eux de la manière définie par leur parent [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Ouvrez maintenant **HelloLinearLayout.cs** et n’oubliez pas qu’il charge le **Resources/Layout/Main.axml** mise en page dans le [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
méthode :

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Le [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) méthode charge le fichier de disposition pour le [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), spécifié par l’ID de ressource &ndash; `Resources.Layout.Main` fait référence à la **disposition/ressources / Main.axml** fichier de disposition.

Exécutez l'application. Vous devez voir les éléments suivants :

[![Capture d’écran de l’application LinearLayout première organisée horizontalement, verticalement seconde](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Notez comment les attributs XML définissent le comportement de chaque vue. Essayez d’expérimenter avec différentes valeurs pour `android:layout_weight` pour voir comment la taille d’écran sont répartie en fonction du poids de chaque élément. Consultez le [les objets de mise en forme courantes](http://developer.android.com/guide/topics/ui/declaring-layout.html) document pour en savoir plus sur la façon [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
gère la `android:layout_weight` attribut.


## <a name="references"></a>Références

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Certaines parties de cette page sont des modifications basées sur le travail créé et partagé par Android Open Source Project et utilisé conformément aux conditions décrites dans le*
[*licence Creative Commons 2.5 Attribution* ](http://creativecommons.org/licenses/by/2.5/).

