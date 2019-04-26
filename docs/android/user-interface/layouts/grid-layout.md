---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 45e625b28bbdf0009b5cfcf661b00ce17638771d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61311201"
---
# <a name="gridlayout"></a>GridLayout

Le `GridLayout` est un nouveau `ViewGroup` sous-classe qui prend en charge la disposition des vues dans une grille 2D, similaire à un tableau HTML, comme indiqué ci-dessous :

 [![Rognée GridLayout affichant quatre cellules](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` fonctionne avec une hiérarchie d’affichage de deux dimensions, où vues enfants définissez leurs emplacements dans la grille en spécifiant les lignes et colonnes, qu'ils doivent être. De cette façon, le *GridLayout* est en mesure de positionner les vues dans la grille sans exiger que toutes les vues intermédiaires fournissent une structure de table, comme indiqué dans les lignes de table utilisés dans le TableLayout. En conservant une hiérarchie plate, *GridLayout* est en mesure de disposition plus rapidement ses vues enfants. Jetons un œil à un exemple pour illustrer ce concept signification dans le code.


## <a name="creating-a-grid-layout"></a>Création d’une disposition de grille

Le code XML suivant ajoute plusieurs `TextView` de contrôles à un *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

La disposition s’ajuste la taille de ligne et de colonne pour que les cellules tiennent leur contenu, comme l’illustre le diagramme suivant :

 [![Diagramme de mise en page affichant les deux cellules sur la gauche inférieur à droite](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

Il en résulte dans l’interface utilisateur suivants lors de l’exécuter dans une application :

 [![Application de capture d’écran de GridLayoutDemo affichant quatre cellules](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>Spécification de l’Orientation

Notez que, dans le code XML ci-dessus, chaque `TextView` ne spécifie pas une ligne ou colonne. Lorsque ceux-ci ne sont pas spécifiés, le `GridLayout` assigne chaque vue enfant dans l’ordre, en fonction de l’orientation. Par exemple, nous allons modifier l’orientation de la GridLayout à partir de la valeur par défaut, ce qui est horizontale, verticale comme suit :

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

À présent, le `GridLayout` positionne les cellules de haut en bas dans chaque colonne, au lieu de gauche à droite, comme indiqué ci-dessous :

 [![Diagramme illustrant comment les cellules sont positionnés dans une orientation verticale](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Il en résulte dans l’interface utilisateur suivants lors de l’exécution :

 [![Capture d’écran de GridLayoutDemo avec cellules positionnés dans l’orientation verticale](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>Spécification de Position explicites

Si nous souhaitons contrôler explicitement les positions des vues enfants dans le `GridLayout`, nous pouvons définir leurs `layout_row` et `layout_column` attributs. Par exemple, le code XML suivant entraîne la mise en page, illustrée à la première capture d’écran (illustré ci-dessus), quel que soit l’orientation.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```



### <a name="specifying-spacing"></a>Spécification d’espacement

Nous avons quelques options qui fournira l’espacement entre les enfants de vues de la `GridLayout`. Nous pouvons utiliser le `layout_margin` attribut à définir la marge sur chaque vue enfant directement, comme indiqué ci-dessous

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

En outre, dans Android 4, une nouvelle vue de l’espacement à usage général appelé `Space` est désormais disponible. Pour l’utiliser, simplement l’ajouter en tant qu’une vue enfant.
Par exemple, le code XML ci-dessous ajoute une ligne supplémentaire à la `GridLayout` en définissant son `rowcount` à 3 et ajoute un `Space` affichage qui fournit l’espacement entre le `TextViews`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Ce code XML crée l’espacement dans le `GridLayout` comme indiqué ci-dessous :

 [![Capture d’écran de GridLayoutDemo illustrant les cellules plus grandes avec espacement](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

L’avantage d’utiliser le nouveau `Space` vue est qu’elle permet l’espacement et ne nécessite pas permis de définir des attributs sur chaque vue enfant.



### <a name="spanning-columns-and-rows"></a>La répartition des lignes et colonnes

Le `GridLayout` prend également en charge les cellules qui s’étendent sur plusieurs lignes et colonnes. Par exemple, nous ajoutons une autre ligne contenant un bouton à la `GridLayout` comme indiqué ci-dessous :

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Ainsi, la première colonne de la `GridLayout` étirée pour s’adapter à la taille du bouton, comme nous le voyons ici :

[![Capture d’écran de GridLayoutDemo avec le bouton couvrant uniquement la première colonne](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Pour empêcher l’étirement de la première colonne, nous pouvons définir le bouton à couvrir les deux colonnes en définissant son columnspan comme suit :

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Cette opération entraîne une disposition pour le `TextViews` qui est similaire à la disposition que nous avions précédemment, avec le bouton ajouté au bas de la `GridLayout` comme indiqué ci-dessous :

 [![Capture d’écran de GridLayoutDemo avec le bouton s’étendant sur les deux colonnes](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>Liens associés

- [GridLayoutDemo (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Présentation d’Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](https://developer.android.com/sdk/android-4.0.html)
