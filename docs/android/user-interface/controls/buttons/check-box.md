---
title: Case à cocher
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765751"
---
# <a name="checkbox"></a>Case à cocher

Dans cette section, vous allez créer une case à cocher pour sélectionner des éléments, à l’aide de la [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox) widget. Lorsque la case à cocher est activé, un message de toast indique l’état actuel de la case à cocher.

Ouvrir le **Resources/layout/Main.axml** et ajoutez le [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) élément (à l’intérieur de la [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)) :

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Pour ce faire, lorsque l’état est modifié, ajoutez le code suivant à la fin de la [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) méthode :

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Capture la [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) élément à partir de la disposition, traite alors l’événement Click, qui définit l’action à effectuer lorsque l’utilisateur clique sur la case à cocher. Lorsque vous cliquez dessus, le [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) propriété est appelée pour vérifier le nouvel état de la case à cocher. Si elle a été activée, un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) affiche le message « Sélectionné », dans le cas contraire, elle affiche « Désactivée ». Le [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) gère son propre état change, il suffit d’interroger l’état actuel.

L’exécuter.

**Conseil :** si vous avez besoin modifier l’état vous-même (par exemple lors du chargement une enregistré [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference), utiliser le [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked) accesseur Set de propriété ou [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle) (méthode).

*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).
