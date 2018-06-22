---
title: TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9a77655091e4b552bd4a9d440f50b6a3cbeabcc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763645"
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) est un [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) qui affiche les enfants [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) éléments en lignes et colonnes.

Démarrer un nouveau projet nommé **HelloTableLayout**.

Ouvrez le **Resources/Layout/Main.axml** de fichier et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Remarquez comment cela ressemble à la structure d’une table HTML. Le [ `TableLayout` ](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) élément ressemble à du code HTML `<table>` élément ; [ `TableRow` ](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) est similaire à un `<tr>` élément ; mais pour les cellules, vous pouvez utiliser les types de [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) élément. Dans cet exemple, un [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) est utilisé pour chaque cellule. Entre certaines lignes, il existe également un basic [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/), qui est utilisé pour dessiner une ligne horizontale.

Assurez-vous que votre **HelloTableLayout** activité charge cette disposition dans le [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) méthode :

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Le [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) méthode charge le fichier de disposition pour le [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), spécifié par l’ID de ressource &mdash; `Resource.Layout.Main` fait référence à la **ressources/disposition / Main.axml** fichier de disposition.

Exécutez l'application. Vous devez voir les éléments suivants :

[![Capture d’écran de l’exemple d’application TableLayout affichage de plusieurs lignes de la table](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>Références

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).
