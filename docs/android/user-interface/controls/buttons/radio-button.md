---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1267491f2d9b7519f76651df059722420fa8e1eb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763112"
---
# <a name="radiobutton"></a>RadioButton

Dans cette section, vous allez créer deux boutons de case d’option qui s’excluent mutuellement (désactive l’activation d’un l’autre), à l’aide de la [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) et [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) widgets. Lorsque des boutons radio sont activé, un message de toast s’affichera.


Ouvrir le **Resources/layout/Main.axml** de fichiers et ajouter deux [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, imbriqués dans un [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (à l’intérieur de la [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

Il est important que le [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s sont regroupés par le [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) élément afin que plusieurs ne peut être sélectionné à la fois. Cette logique est gérée automatiquement par le système Android. Lorsqu’un [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) au sein d’un groupe est sélectionné, tous les autres sont automatiquement désélectionnées.

Pour effectuer une action lors de chaque [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) est sélectionné, nous avons besoin d’écrire un gestionnaire d’événements :

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Tout d’abord, l’expéditeur qui est passé est converti en un RadioButton.
Un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message affiche le texte du bouton de la case d’option sélectionnée.

Maintenant, au bas de la [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) (méthode), ajoutez le code suivant :

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Chaque capture le [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s à partir de la mise en page et ajoute chaque handlerto nouvellement créé l’événement.

Exécutez l'application.

**Conseil :** si vous avez besoin modifier l’état vous-même (par exemple lors du chargement une enregistré [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), utiliser le [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) accesseur Set de propriété ou [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) (méthode).

*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/). 
