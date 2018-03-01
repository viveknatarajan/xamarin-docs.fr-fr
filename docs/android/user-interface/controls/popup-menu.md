---
title: Menu contextuel
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: 54b6809b7e27dc87be6d510e4a4b6071e4ae22e7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="popup-menu"></a>Menu contextuel

La `PopupMenu` classe ajoute la prise en charge pour l’affichage des menus contextuels qui sont attachés à une vue particulière. L’illustration suivante montre un menu contextuel présenté à partir d’un bouton, le deuxième élément mis en surbrillance uniquement lorsqu’elle est sélectionnée :

 [ ![Exemple d’un PopopMenu avec trois trois éléments](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png)

Android 4 ajouté deux nouvelles fonctionnalités à `PopupMenu` qui rendent un peu plus facile à manipuler, à savoir :

-   **GONFLAGE** &ndash; augmentation de la méthode est désormais disponible directement sur la classe de menu contextuel.
-   **DismissEvent** &ndash; menu contextuel de la classe a maintenant un DismissEvent.

Examinons ces améliorations. Dans cet exemple, nous avons une activité unique qui contient un bouton. Lorsque l’utilisateur clique sur le bouton, un menu contextuel s’affiche comme indiqué ci-dessous :

 [ ![Exemple d’application en cours d’exécution dans un émulateur avec le bouton et menu contextuel d’élément de 3](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png)

<a name="Creating_a_Popup_Menu" />

## <a name="creating-a-popup-menu"></a>Création d’un Menu contextuel

Lorsque nous créons une instance de la `PopupMenu`, nous avons besoin de passer à son constructeur une référence à le `Context`, ainsi que la vue à laquelle le menu est attaché. Dans ce cas, nous créons le `PopupMenu` dans le Gestionnaire d’événements click pour notre bouton, qui est nommé `showPopupMenu`.
Ce bouton est également la vue à laquelle nous allons attacher le `PopupMenu`, comme illustré dans le code suivant :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

Dans Android 3, le code de la valeur de l’augmentation du menu à partir d’une ressource XML requis que vous obtenez tout d’abord une référence à un `MenuInflator`, puis appelez ses `Inflate` méthode avec l’ID de ressource du code XML qui contenait le menu et l’instance de menu à la valeur de l’augmentation dans. Une telle approche fonctionne toujours dans Android 4 et ultérieurement en tant que le code ci-dessous montre :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

À compter d’Android 4 Toutefois, vous pouvez maintenant appeler `Inflate` directement sur l’instance de la `PopupMenu`. Cela rend le code plus concis, comme illustré ici :

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

Dans le code ci-dessus, après avoir gonfler le menu nous simplement appeler `menu.Show` pour l’afficher à l’écran.

<a name="Handling_Menu_Events" />

## <a name="handling-menu-events"></a>La gestion des événements de Menu

Lorsque l’utilisateur sélectionne un élément de menu, le `MenuItemClick` événement est déclenché et le menu va être fermé. Si vous appuyez sur n’importe où en dehors du menu sera simplement faire disparaître. Dans les deux cas, à compter d’Android 4, lorsque le menu est fermé, son `DismissEvent` sera déclenchée. Le code suivant ajoute des gestionnaires d’événements pour les deux le `MenuItemClick` et `DismissEvent` événements :

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

- [PopupMenuDemo (sample)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
