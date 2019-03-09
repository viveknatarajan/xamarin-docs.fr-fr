---
title: Barre de navigation
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 9455cac81a0f9ea81e08cf63397e45c1698e1c1b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670010"
---
# <a name="navigation-bar"></a>Barre de navigation

Android 4 a introduit une nouvelle fonctionnalité d’interface d’utilisateur système appelée un *barre de Navigation*, qui fournit des contrôles de navigation sur les appareils qui n’incluent pas les boutons matériels pour **accueil**, **précédent** , et **Menu**.
La capture d’écran suivante montre la barre de Navigation à partir d’un appareil Nexus Prime :

 [![Exemple d’une barre de navigation Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Plusieurs nouveaux indicateurs sont disponibles afin de contrôler la visibilité de la barre de Navigation et de ses contrôles, ainsi que la visibilité de la barre système qui a été introduite dans 3 Android. Les indicateurs sont définis dans le `Android.View.View` classe et sont répertoriées ci-dessous :

-   `SystemUiFlagVisible` &ndash; La barre de Navigation rend visible. 
-   `SystemUiFlagLowProfile` &ndash; DIMS contrôles dans la barre de Navigation. 
-   `SystemUiFlagHideNavigation` &ndash; Masque la barre de Navigation. 


Ces indicateurs peuvent être appliquées à n’importe quelle vue dans la hiérarchie d’affichage en définissant le `SystemUiVisibility` propriété. Si plusieurs vues possède cette propriété, le système les combine avec une opération OR et les applique en tant que la fenêtre dans laquelle les indicateurs sont définis conserve le focus. Lorsque vous supprimez une vue, des indicateurs qu’elle a définie seront également supprimées.

L’exemple suivant montre une application simple où cliquer sur les boutons de la modification la `SystemUiVisibility`:

 [![Captures d’écran illustrant Visible, le profil de faible et SystemUiVisibility masqué](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Le code pour modifier le `SystemUiVisibility` définit la propriété sur un `TextView` à partir de chaque bouton Cliquez sur Gestionnaire d’événements comme indiqué ci-dessous :

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

En outre, un `SystemUiVisibility` modifier déclenche un `SystemUiVisibilityChange` événement. Tout comme paramètre le `SystemUiVisibility` propriété, un gestionnaire pour le `SystemUiVisibilityChange` événement peut être inscrits pour n’importe quelle vue de la hiérarchie. Par exemple, le code ci-dessous utilise le `TextView` instance pour vous inscrire à l’événement :

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Liens associés

- [SystemUIVisibilityDemo (exemple)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Présentation d’Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](https://developer.android.com/sdk/android-4.0.html)
