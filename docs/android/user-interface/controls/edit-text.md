---
title: Modifier le texte
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d6be8ae1587742a8c2a37b22b2da3701187dde2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="edit-text"></a>Modifier le texte

Dans cette section, vous allez créer un champ de texte pour l’entrée utilisateur, à l’aide de la [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) widget. Une fois que le texte a été entré dans le champ, la touche « Entrée » affiche le texte dans un message de toast.

Ouvrir le <code>Resources\layout\main.xml</code> et ajoutez le [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) élément (à l’intérieur de la [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)) :

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

Effectuer une opération avec le texte que l’utilisateur tape, ajoutez le code suivant à la fin de la [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) méthode :

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

Capture la [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) élément à partir de la mise en page et le définit un [ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) gestionnaire, qui définit l’action à effectuer lorsqu’une touche est enfoncée alors que le widget a le focus. Dans ce cas, la méthode est définie pour écouter la touche entrée (lorsqu’il est enfoncé), puis affiche un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message avec le texte qui a été entré. Le [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) propriété doit toujours être `true` si l’événement a été géré, afin que l’événement ne bulles à distance (ce qui entraînerait un retour chariot dans le champ de texte).

Exécutez l'application.

*Des parties de cette page sont des modifications en fonction de travail créé et* [ *partagé par le projet Source ouvert Android* ](http://code.google.com/policies.html) *et utilisé en fonction des conditions décrites dans le* [ *2.5 de creative Commons Attribution licence* ](http://creativecommons.org/licenses/by/2.5/) *. Ce didacticiel est basé sur le* [ *didacticiel de sélections de formulaire Android* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*



## <a name="related-links"></a>Liens associés

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
