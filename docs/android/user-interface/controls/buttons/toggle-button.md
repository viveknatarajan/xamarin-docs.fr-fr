---
title: Bouton bascule
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8323c456a97a033e19374a4bd3ea7468ecafa608
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762296"
---
# <a name="togglebutton"></a>Bouton bascule

Dans cette section, vous allez créer un bouton utilisé spécifiquement pour basculer entre les deux États, à l’aide de la [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) widget. Ce widget est une excellente alternative aux boutons de case d’option si vous disposez de deux États simples qui s’excluent mutuellement (« on » et « off », par exemple). Android 4.0 (API niveau 14) a introduit une alternative au bouton bascule appelé un [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/).

Un exemple d’un **ToggleButton** peuvent être consultés dans la paire de gauche des images, tandis que la paire de droite d’images présente un exemple d’un **commutateur**:

![Exemples de commutateurs et les contrôles ToggleButton dans sur site et hors tension des États](toggle-button-images/togglebutton-switch.png)  

Une application utilise le contrôle qui est une question de style. Les deux widgets sont fonctionnellement équivalents.

Ouvrir le **Resources/layout/Main.axml** et ajoutez le [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) élément (à l’intérieur de la [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)) :

Pour ce faire, lorsque l’état est modifié, ajoutez le code suivant à la fin de la [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) méthode :

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

Capture la [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) élément à partir de la mise en page et gère l’événement Click, qui définit l’action à effectuer lorsque le bouton est activé. Dans cet exemple, la méthode vérifie le nouvel état du bouton, puis affiche un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message qui indique l’état actuel.

Notez que la [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) gère son propre état changer entre checked et unchecked, afin de vous poser simplement qu’il est.

Exécutez l'application.


**Conseil :** si vous avez besoin modifier l’état vous-même (par exemple lors du chargement une enregistré [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), utiliser le [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) accesseur Set de propriété ou [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) (méthode).


## <a name="related-links"></a>Liens associés

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [Commutateur](http://developer.android.com/reference/android/widget/Switch.html)
