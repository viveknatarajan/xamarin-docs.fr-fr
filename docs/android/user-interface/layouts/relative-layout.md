---
title: À l’aide de la RelativeLayout dans Xamarin.Android
description: Comment utiliser RelativeLayout dans une application Xamarin.Android
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af2972ecc92435836a75013e6203ba47c2c04627
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113137"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) est un [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) qui affiche les enfants [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
éléments dans des positions relatives. La position d’un [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) peut être spécifié par rapport à des éléments frères (par exemple, quant à la gauche du ou en dessous d’un élément donné) ou dans positionne relatif à la [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
zone (par exemple aligné en bas, à gauche du centre).

Un [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) est un utilitaire très puissant pour concevoir une interface utilisateur, car il peut éliminer imbriqué [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Si vous vous trouvez à l’aide de plusieurs imbriqués [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
groupes, vous pourrez peut-être les remplacer par un seul [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Démarrer un nouveau projet nommé **HelloRelativeLayout**.

Ouvrez le **Resources/Layout/Main.axml** de fichiers et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Notez que chacun de la `android:layout_*` attributs, tels que `layout_below`, `layout_alignParentRight`, et `layout_toLeftOf`.
Lorsque vous utilisez un [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), vous pouvez utiliser ces attributs pour décrire la façon dont vous souhaitez positionner chaque [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Chacune de ces attributs définissent un autre type de position relative. Certains attributs, utilisez l’ID de ressource d’un frère [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) pour définir sa propre position relative. Par exemple, la dernière [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) est défini pour se trouvent à gauche d’et aligné-avec-the-haut de la [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) identifiée par l’ID `ok` (qui est la précédente [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Tous les attributs de disposition disponibles sont définies dans [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Vous assurer que vous chargez cette disposition dans le [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
méthode :

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Le [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) méthode charge le fichier de disposition pour le [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), spécifié par l’ID de ressource &mdash; `Resource.Layout.Main` fait référence à la **disposition/ressources / Main.axml** fichier de disposition.

Exécutez l'application. Vous devez voir la disposition suivante :

[![Capture d’écran de disposition relative avec TextView EditText et deux boutons](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Ressources

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Certaines parties de cette page sont des modifications basées sur le travail créé et partagé par Android Open Source Project et utilisé conformément aux conditions décrites dans le*
[*licence Creative Commons 2.5 Attribution* ](http://creativecommons.org/licenses/by/2.5/).
