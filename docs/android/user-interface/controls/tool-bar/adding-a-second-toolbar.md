---
title: "Ajout d’une seconde barre d’outils"
ms.topic: article
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b471742ae9fb365d75e8dd3ca0f93f5e55208f19
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-second-toolbar"></a>Ajout d’une seconde barre d’outils

<a name="overview" />

## <a name="overview"></a>Vue d'ensemble 

Le `Toolbar` peut faire plus que remplacer la barre d’action &ndash; il peut être utilisé plusieurs fois dans une activité, il peut être personnalisé pour la sélection élective n’importe où sur l’écran, et il peut être configuré pour qu’ils concernent uniquement une largeur partielle de l’écran. Les exemples ci-dessous montrent comment créer un second `Toolbar` et le placer en bas de l’écran. Cela `Toolbar` implémente **copie**, **couper**, et **coller** des éléments de menu. 

<a name="define_second" />

## <a name="define-the-second-toolbar"></a>Définir la seconde barre d’outils 

Modifier le fichier de disposition **Main.axml** et remplacez son contenu avec le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

Ce code XML ajoute une seconde `Toolbar` vers le bas de l’écran avec vide `ImageView` remplissage au milieu de l’écran. La hauteur de cette `Toolbar` a la valeur de la hauteur de la barre d’action : 

```xml
android:minHeight="?android:attr/actionBarSize"
```

La couleur d’arrière-plan de ce `Toolbar` est définie sur une couleur accentuée qui sera ensuite définie :

```xml
android:background="?android:attr/colorAccent
```

Notez que cette `Toolbar` est basé sur un autre thème (**ThemeOverlay.Material.Dark.ActionBar**) que celle utilisée par le `Toolbar` créé dans [en remplaçant la barre d’Action](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;il n’est pas lié à l’intérieur de fenêtre de l’activité ou au thème utilisé dans la première `Toolbar`.

Modifier **Resources/values/styles.xml** et ajouter la couleur d’accentuation suivant à la définition de style : 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Ainsi, la barre d’outils en bas une couleur orange foncée. Génération et exécution de l’application affiche une barre d’outils deuxième vide en bas de l’écran : 

[![Capture d’écran de l’application avec jaune seconde barre d’outils en bas de l’écran](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png)


<a name="second_menus" />
 
## <a name="add-edit-menu-items"></a>Ajouter des éléments de Menu Edition 

Cette section explique comment ajouter des éléments de menu Edition vers le bas `Toolbar`. 

Pour ajouter des éléments de menu à une base de données secondaire `Toolbar`: 

1.  Ajouter des icônes de menu pour le `mipmap-` dossiers de projet d’application (si nécessaire).

2.  Définir le contenu des éléments de menu en ajoutant un fichier de ressources de menu supplémentaires à **menu/ressources**. 

3.  Dans l’activité `OnCreate` (méthode), rechercher le `Toolbar` (en appelant `FindViewById`) et la valeur de l’augmentation du `Toolbar`de menus.

4.  Implémentez un gestionnaire de clic dans `OnCreate` pour les éléments de menu. 

Les sections suivantes illustrent ce processus en détail : **couper**, **copie**, et **coller** des éléments de menu sont ajoutés à la partie inférieure `Toolbar`. 


<a name="second_resource" />

### <a name="define-the-edit-menu-resource"></a>Définir la ressource de Menu Edition

Dans le **menu/ressources** sous-répertoire, créez un nouveau fichier XML appelé **edit_menus.xml** et remplacez le contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Ce code XML crée les **couper**, **copie**, et **coller** des éléments de menu (à l’aide des icônes qui ont été ajoutés à la `mipmap-` dossiers dans [en remplaçant la barre d’Action ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).


<a name="inflate_menus" />

### <a name="inflate-the-menus"></a>Valeur de l’augmentation les Menus

À la fin de la `OnCreate` méthode dans **MainActivity.cs**, ajoutez les lignes de code suivantes : 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Ce code localise le `edit_toolbar` vue définie dans **Main.axml**, définit son titre **édition**et augmente ses éléments de menu (défini dans **edit_menus.xml**). Il définit un menu cliquez sur Gestionnaire qui affiche une notification toast pour indiquer que l'on a cliqué sur l’icône de modification. 

Générez et exécutez l’application. Lorsque l’application s’exécute, le texte et les icônes ajoutées ci-dessus seront affiche comme illustré ici : 

[![Diagramme de bas barre d’outils avec des icônes de couper, copier et coller](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png)

Si vous appuyez sur la **couper** icône du menu provoque le toast suivant s’affiche : 

[![Capture d’écran de Toast indiquant que l'on a cliqué sur l’icône du menu Couper](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png)

En appuyant sur les éléments de menu dans une barre d’outils affiche les toasts obtenus : 

[![Captures d’écran de Toasts pour enregistrer, copier et coller des éléments de menu est activé par un clic](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png)


<a name="up_button" />

## <a name="the-up-button"></a>Le bouton haut 

La plupart des applications Android s’appuient sur la **précédent** bouton de navigation de l’application ; en appuyant sur la **précédent** bouton guide l’utilisateur à l’écran précédent.
Toutefois, vous pouvez également fournir un **des** bouton qui facilite la navigation « jusqu'à » à l’écran de l’application principale pour les utilisateurs. Lorsque l’utilisateur sélectionne le **des** bouton, l’utilisateur déplace vers le haut pour un niveau supérieur dans la hiérarchie de l’application &ndash; , autrement dit, l’application s’affiche l’utilisateur dans plusieurs activités dans la pile back plutôt qu’à arrière à la visités précédemment Activité. 

Pour activer la **des** bouton dans une deuxième activité qui utilise un `Toolbar` en tant que la barre d’action, appelez le `SetDisplayHomeAsUpEnabled` et `SetHomeButtonEnabled` méthodes dans la deuxième activité `OnCreate` (méthode) :

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

Le [prennent en charge de la barre d’outils v7](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) exemple de code montre comment le **des** bouton en action. Cet exemple (qui utilise la bibliothèque AppCompat décrite ci-après) implémente une deuxième activité qui utilise la barre d’outils **des** bouton de navigation hiérarchique à l’activité précédente. Dans cet exemple, le `DetailActivity` bouton Accueil permet la **des** bouton en effectuant ce qui suit `SupportActionBar` les appels de méthode : 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Lorsque l’utilisateur navigue parmi `MainActivity` à `DetailActivity`, le `DetailActivity` affiche un **des** bouton (flèche gauche), comme illustré dans la capture d’écran :

[![Capture d’écran exemple de flèche haut bouton gauche dans la barre d’outils](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png)

En appuyant sur cette **des** bouton entraîne l’application revenir à `MainActivity`. Dans une application plus complexe avec plusieurs niveaux de hiérarchie, ce bouton renvoie l’utilisateur au niveau supérieur dans l’application plutôt qu’à l’écran précédent. 



## <a name="related-links"></a>Liens associés

- [La barre d’outils de l’interface Lollipop (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [La barre d’outils de AppCompat (exemple)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
