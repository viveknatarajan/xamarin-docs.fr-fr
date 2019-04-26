---
title: Bouton personnalisé
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b5ccefa1eb7e659584c1c82481bbd4473a3a8abc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076249"
---
# <a name="custom-button"></a>Bouton personnalisé

Dans cette section, vous allez créer un bouton avec une image personnalisée au lieu de texte, à l’aide de la [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) widget et un fichier XML qui définit trois différentes images à utiliser pour les différents États du bouton. Lorsque le bouton est enfoncé, un bref message s’affichera.

Avec le bouton droit et télécharger les trois images ci-dessous, puis copiez-les sur le **ressources/drawable** répertoire de votre projet. Ils seront utilisés pour les différents États du bouton.

 [![Icône Android vert pour un état normal](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![icône Orange Android pour l’état focus](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![une icône jaune Android pour un état enfoncé](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Créer un nouveau fichier dans le **ressources/drawable** répertoire nommé **android_button.xml**. Insérez le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Définit une seule ressource drawable, ce qui modifie son image basée sur l’état actuel du bouton. La première `<item>` définit **android_pressed.png** en tant que l’image lorsque le bouton est enfoncé (avoir activé) ; la seconde `<item>` définit **android_focused.png** en tant que l’image lorsque le bouton est actif (lorsque le bouton est mis en surbrillance à l’aide du trackball ou le pavé directionnel) ; le troisième `<item>` définit **android_normal.png** comme image pour l’état normal (lorsque ni activé ni axé). Ce fichier XML représente maintenant une seule ressource drawable et lorsque référencé par un [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) d’arrière-plan, l’image affichée changent en fonction de ces trois états.


> [!NOTE]
> L’ordre de la `<item>` éléments est important. Lorsque ce drawable est référencé, le `<item>`s sont parcourus dans l’ordre pour déterminer celle qui convient à l’état actuel du bouton.
> Étant donné que l’image « normal » est la dernière, elle s’applique uniquement lorsque les conditions `android:state_pressed` et `android:state_focused` évaluées toutes deux false.

Ouvrez le **Resources/layout/Main.axml** fichier, puis ajoutez le [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) élément :

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Le `android:background` attribut spécifie la ressource drawable à utiliser pour l’arrière-plan du bouton (qui, lors de l’enregistrement à **Resources/drawable/android.xml**, est référencé en tant que `@drawable/android`). Cela remplace l’image d’arrière-plan normal utilisé pour les boutons dans tout le système. Dans l’ordre pour drawable modifier son image basée sur l’état du bouton, l’image doit être appliqué à l’arrière-plan.

Pour faire une opération lorsque vous appuyez sur le bouton, ajoutez le code suivant à la fin de la [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)
méthode :

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Capture le [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) à partir de la mise en page, puis ajoute un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message à afficher lorsque le [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) vous cliquez sur.

Exécutez maintenant l’application.


*Certaines parties de cette page sont des modifications basées sur le travail créé et partagé par Android Open Source Project et utilisé conformément aux conditions décrites dans le*
[*licence Creative Commons 2.5 Attribution* ](http://creativecommons.org/licenses/by/2.5/).
