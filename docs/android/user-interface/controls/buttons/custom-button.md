---
title: "Bouton personnalisé"
ms.topic: article
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5131b4d09f01af6a6e8bed28a2df27bc801dfb80
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="custom-button"></a>Bouton personnalisé

Dans cette section, vous allez créer un bouton avec une image personnalisée au lieu de texte, à l’aide de la [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) widget et un fichier XML qui définit trois images différentes à utiliser pour les différents États du bouton. Lorsque le bouton est activé, un bref message s’affichera.

Cliquez sur Télécharger les trois images ci-dessous, puis les copier sur le **drawable/ressources** répertoire de votre projet. Ils seront utilisés pour les différents États du bouton.

 [![Une icône verte Android pour l’état normal](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![icône Orange Android pour l’état de focus](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![icône jaune Android pour l’état enfoncé](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Créer un nouveau fichier dans le **drawable/ressources** répertoire nommé **android_button.xml**. Insérez le code XML suivant :

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

Définit une seule ressource drawable, ce qui modifie son image selon l’état actuel du bouton. La première `<item>` définit **android_pressed.png** en tant que l’image lorsque le bouton est enfoncé (il est été activé) ; la seconde `<item>` définit **android_focused.png** en tant que l’image lors de la bouton est actif (lorsque le bouton est mis en surbrillance à l’aide de la trackball ou pavé directionnel) ; et la troisième `<item>` définit **android_normal.png** comme image pour l’état normal (lorsqu’il est appuyé ni le focus). Ce fichier XML représente maintenant une seule ressource drawable et référencé par un [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) pour son arrière-plan, l’image affichée changent en fonction de ces trois états.


> [!NOTE]
> L’ordre de la `<item>` éléments est important. Lorsque ce drawable est référencée, le `<item>`sont parcourus dans l’ordre pour déterminer celui qui convient à l’état actuel du bouton.
> Étant donné que l’image « normal » est la dernière, elle s’applique uniquement lorsque les conditions `android:state_pressed` et `android:state_focused` évaluées toutes deux false.

Ouvrez le **Resources/layout/Main.axml** et ajoutez le [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) élément :

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

Le `android:background` attribut spécifie la ressource drawable à utiliser pour l’arrière-plan du bouton (qui, quand enregistré à **Resources/drawable/android.xml**, est référencée sous la forme `@drawable/android`). Cette opération remplace l’image d’arrière-plan normal utilisé pour les boutons dans tout le système. Dans l’ordre pour drawable modifier son image basée sur l’état du bouton, l’image doit être appliquée à l’arrière-plan.

Pour que le bouton faire quelque chose lorsque enfoncé, ajoutez le code suivant à la fin de la [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/) méthode :

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Capture la [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) à partir de la mise en page, puis ajoute un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message à afficher lorsque le [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) est activé.

Maintenant, exécutez l’application.


*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).
