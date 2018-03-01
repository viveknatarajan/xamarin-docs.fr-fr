---
title: GridLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 013db64add615e94ef3494f14bc82fc17ec2dca1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="gridlayout"></a>GridLayout

Le `GridLayout` est un nouveau `ViewGroup` sous-classe qui prend en charge la disposition de vues dans une grille 2D, semblable à un tableau HTML, comme indiqué ci-dessous :

 [ ![Rognée GridLayout affichant quatre cellules](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png)

 `GridLayout` fonctionne avec une hiérarchie plate-view, où vues enfants définir leurs emplacements dans la grille en spécifiant les lignes et colonnes, qu'elles doivent avoir. De cette manière, le *GridLayout* est en mesure de positionner les vues dans la grille sans exiger que toutes les vues intermédiaires fournissent une structure de table, comme indiqué dans les lignes de table utilisés dans le TableLayout. En conservant une hiérarchie plate, *GridLayout* est en mesure de disposition plus rapidement ses vues enfants. Examinons un exemple pour illustrer ce concept signification dans le code.

<a name="Creating_a_Grid_Layout" />

## <a name="creating-a-grid-layout"></a>Création d’une disposition en grille

Le code XML suivant ajoute plusieurs `TextView` des contrôles à un *GridLayout*.

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

La mise en page ajuster les tailles de ligne et de colonne afin que les cellules peuvent s’ajuster à leur contenu, comme illustré dans le diagramme suivant :

 [ ![Diagramme de mise en page affichant les deux cellules à gauche inférieur à droite](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png)

Il en résulte dans l’interface utilisateur suivantes lors de l’exécution dans une application :

 [ ![Application de capture d’écran de GridLayoutDemo affichant quatre cellules](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png)

 <a name="Specifying_Orientation" />


## <a name="specifying-orientation"></a>Spécification de l’Orientation

Notez que, dans le document XML ci-dessus, chaque `TextView` ne spécifie pas une ligne ou colonne. Lorsqu’elles ne sont pas spécifiées, la `GridLayout` assigne chaque vue enfant dans l’ordre, en fonction de l’orientation. Par exemple, nous allons maintenant changer l’orientation de la valeur de GridLayout à partir de la valeur par défaut, ce qui est horizontale, verticale comme suit :

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

À présent, le `GridLayout` positionne les cellules de haut en bas de chaque colonne, au lieu de gauche à droite, comme indiqué ci-dessous :

 [ ![Diagramme illustrant comment les cellules sont positionnés dans une orientation verticale](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png)

Il en résulte dans l’interface utilisateur suivantes lors de l’exécution :

 [ ![Capture d’écran de GridLayoutDemo avec des cellules positionnées en orientation verticale](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png)

 <a name="Specifying_Explicit_Position" />


### <a name="specifying-explicit-position"></a>Spécification de Position explicites

Si vous souhaitez contrôler explicitement les positions des vues enfants dans le `GridLayout`, nous pouvons définir leurs `layout_row` et `layout_column` attributs. Par exemple, le code XML suivant entraîne la mise en page indiqué dans la première capture d’écran (illustré ci-dessus), quel que soit l’orientation.

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

 <a name="Specifying_spacing" />


### <a name="specifying-spacing"></a>Spécification d’espacement

Nous avons deux options qui fournissent l’espacement entre les enfants des vues de la `GridLayout`. Nous pouvons utiliser le `layout_margin` attribut à définir la marge sur chaque vue enfant directement, comme indiqué ci-dessous

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

En outre, dans les 4 Android, une nouvelle vue espacement à usage général appelée `Space` est désormais disponible. Pour l’utiliser, ajoutez-le simplement comme une vue enfant.
Par exemple, le code XML ci-dessous ajoute une ligne supplémentaire pour le `GridLayout` en définissant son `rowcount` à 3 et ajoute un `Space` affichage qui fournit l’espacement entre la `TextViews`.

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

 [ ![Capture d’écran de GridLayoutDemo illustrant les cellules plus grandes espacement](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png)

L’avantage d’utiliser le nouveau `Space` vue est qu’il autorise pour l’espacement et ne nécessite pas définir des attributs sur chaque vue enfant.

 <a name="Spanning_Columns_and_Rows" />


### <a name="spanning-columns-and-rows"></a>Fractionnement des colonnes et des lignes

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

Ainsi, la première colonne de la `GridLayout` étirée pour s’adapter à la taille du bouton, nous voyons ici :

[ ![Capture d’écran de GridLayoutDemo avec le bouton, seule la première colonne de fractionnement](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png)

Pour empêcher l’étirement de la première colonne, nous pouvons définir le bouton pour couvrir les deux colonnes en définissant son columnspan comme suit :

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Cette opération entraîne une disposition pour le `TextViews` qui est similaire à la mise en page, nous avons dû précédemment, avec le bouton ajouté au bas de la `GridLayout` comme indiqué ci-dessous :

 [ ![Capture d’écran de GridLayoutDemo avec le bouton couvrant les deux colonnes](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png)


## <a name="related-links"></a>Liens associés

- [GridLayoutDemo (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
