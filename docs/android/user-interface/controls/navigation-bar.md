---
title: Barre de navigation
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 476e73906e4fbe01847740f90a8da701768b29db
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762810"
---
# <a name="navigation-bar"></a>Barre de navigation

Android 4 a introduit une nouvelle fonctionnalité d’interface utilisateur système appelée un *barre de Navigation*, qui fournit des contrôles de navigation sur les appareils qui n’incluent pas de boutons **accueil**, **précédent** , et **Menu**.
La capture d’écran suivante montre la barre de Navigation à partir d’un appareil Nexus premiers :

 [![Exemple d’une barre de navigation Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Plusieurs nouveaux indicateurs sont disponibles afin de contrôler la visibilité de la barre de Navigation et de ses contrôles, ainsi que la visibilité de la barre de système qui a été introduite dans 3 Android. Les indicateurs sont définis dans la `Android.View.View` classe et sont répertoriées ci-dessous :

-   `SystemUiFlagVisible` &ndash; Affiche la barre de Navigation. 
-   `SystemUiFlagLowProfile` &ndash; DIMS contrôles dans la barre de Navigation. 
-   `SystemUiFlagHideNavigation` &ndash; Masque la barre de Navigation. 


Ces indicateurs peuvent être appliquées à n’importe quelle vue dans la hiérarchie en définissant le `SystemUiVisibility` propriété. Si plusieurs vues utilisent cette propriété, le système les combine avec une opération OR et les applique en tant que la fenêtre dans laquelle les indicateurs sont définis conserve le focus. Lorsque vous supprimez une vue, elle a ramené tous les indicateurs seront également supprimées.

L’exemple suivant montre une application simple où en cliquant sur les boutons de change le `SystemUiVisibility`:

 [![Captures d’écran montrant Visible, profil bas et SystemUiVisibility de masqué](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

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

En outre, un `SystemUiVisibility` modifier déclenche un `SystemUiVisibilityChange` événement. Tout comme paramètre le `SystemUiVisibility` propriété, un gestionnaire pour le `SystemUiVisibilityChange` événement peut être inscrits pour n’importe quelle vue de la hiérarchie. Par exemple, le code ci-dessous utilise le `TextView` instance à inscrire pour l’événement :

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Liens associés

- [SystemUIVisibilityDemo (sample)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
