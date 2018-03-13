---
title: RelativeLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c7de56038d5f59c6cff89b87f48535c3e757bb91
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) est un [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) qui affiche les enfants [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) éléments dans les positions relatives. La position d’un [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) peut être spécifié comme étant relatif au éléments frères (par exemple à gauche d’ou en dessous d’un élément donné) ou de positions par rapport à la [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) zone (telle que aligné en bas, gauche du centre).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) est un utilitaire très puissant pour concevoir une interface utilisateur, car elle peut éliminer imbriqué [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Si vous vous trouvez à l’aide de plusieurs imbriqués [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) groupes, vous pourrez peut-être les remplacer par un seul [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Démarrer un nouveau projet nommé **HelloRelativeLayout**.

Ouvrez le **Resources/Layout/Main.axml** de fichier et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
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

Notez chaque le `android:layout_*` des attributs, tels que `layout_below`, `layout_alignParentRight`, et `layout_toLeftOf`.
Lorsque vous utilisez un [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), vous pouvez utiliser ces attributs pour décrire la façon dont vous souhaitez positionner [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Chacune de ces attributs définissent un autre type de position relative. Certains attributs, utilisez l’ID de ressource d’un frère [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) pour définir son propre position relative. Par exemple, le dernier [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) est définie pour se trouve à gauche d’et aligné-avec-l’en haut de la [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) identifiée par l’ID `ok` (qui est le précédent [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Tous les attributs de disposition disponibles sont définies dans [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Assurez-vous que vous chargez cette disposition dans le [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) méthode :

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Le [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) méthode charge le fichier de disposition pour le [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), spécifié par l’ID de ressource &mdash; `Resource.Layout.Main` fait référence à la **ressources/disposition / Main.axml** fichier de disposition.

Exécutez l'application. Vous devez voir la présentation suivante :

[![Capture d’écran d’une disposition relative en TextView EditText et deux boutons](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Ressources

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).
