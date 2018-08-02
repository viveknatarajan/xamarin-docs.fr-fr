---
title: Menu contextuel
description: Comment ajouter un menu contextuel qui est ancré à une vue particulière.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: d7cadde88e9ae7ee30815ee9323785038dbb1a39
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393657"
---
# <a name="popup-menu"></a>Menu contextuel

Le [menu contextuel](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (également appelé un _menu contextuel_) est un menu qui est ancré à une vue particulière. Dans l’exemple suivant, une seule activité contient un bouton. Lorsque l’utilisateur actionne le bouton, un menu contextuel de trois éléments s’affiche :

[![Exemple d’application avec un bouton et un menu contextuel de trois éléments](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>Création d’un Menu contextuel

La première étape consiste à créer un fichier de ressources de menu pour le menu et le placer dans **/menu ressources**. Par exemple, le code XML suivant est le code pour le menu de trois éléments s’affiché dans la capture d’écran précédente, **Resources/menu/popup_menu.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

Ensuite, créez une instance de `PopupMenu` et ancrez-la à sa vue. Lorsque vous créez une instance de `PopupMenu`, vous passez une référence à son constructeur le `Context` , ainsi que la vue à laquelle le menu sera attaché. Par conséquent, le menu contextuel est ancré à cette vue lors de sa création.

Dans l’exemple suivant, le `PopupMenu` est créé dans le Gestionnaire d’événements click pour le bouton (qui est nommé `showPopupMenu`). Ce bouton est également la vue à laquelle le `PopupMenu` est ancrée, comme indiqué dans l’exemple de code suivant :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

Enfin, le menu contextuel doit être *gonflé* avec la ressource de menu qui a été créée précédemment. Dans l’exemple suivant, l’appel à du menu [Inflate](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/) méthode est ajoutée et ses [afficher](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/) méthode est appelée pour l’afficher :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>Gestion des événements de Menu

Lorsque l’utilisateur sélectionne un élément de menu, le [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/) cliquez sur événement sera déclenché et le menu va être fermé. En appuyant sur n’importe où en dehors du menu sera simplement fait disparaître. Dans les deux cas, lorsque le menu est fermé, sa [DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/) sera déclenchée. Le code suivant ajoute des gestionnaires d’événements pour les deux le `MenuItemClick` et `DismissEvent` événements :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
    menu.Show ();
};
```



## <a name="related-links"></a>Liens associés

- [PopupMenuDemo (exemple)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
