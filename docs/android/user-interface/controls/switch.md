---
title: Basculer
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0cf1c4d749eb85a7e0f4c035e10e2e7a40e0c711
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="switch"></a>Basculer

Le `Switch` widget (voir ci-dessous) permet à un utilisateur de basculer entre les deux États, comme sous ou hors tension. Le `Switch` valeur par défaut est OFF. Dans ses États ON et OFF, le widget est indiqué ci-dessous :

[![Captures d’écran d’un widget de commutateur et activer des États](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Création d’un commutateur

Pour créer un commutateur, il vous suffit de déclarer un `Switch` élément XML comme suit :

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Cette opération crée un commutateur de base comme indiqué ci-dessous :

[![Capture d’écran de l’application de démonstration affichant un commutateur dans l’état désactivé](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Modification des valeurs par défaut

Le texte affiché par le contrôle pour les États du ON et OFF et la valeur par défaut sont configurables. Par exemple, pour que le commutateur par défaut la valeur ON et lecture non/Oui au lieu de la valeur OFF/ON, nous pouvons définir le `checked`, `textOn`, et `textOff` les attributs dans le code XML suivant.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>En fournissant un titre

Le `Switch` widget prend également en charge, y compris une étiquette de texte en définissant le `text` attribut comme suit :

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Ce balisage génère la capture d’écran suivante lors de l’exécution :

[![Capture d’écran de l’application de démonstration avec le texte horizontalement précédant le widget de commutateur](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Lorsqu’un `Switch`de valeur change, elle déclenche un `CheckedChange` événement.
Par exemple, nous capturer cet événement dans le code suivant et présenter un `Toast` widget avec un message en fonction de la `isChecked` valeur `Switch`, qui est passé au gestionnaire d’événements dans le cadre de la `CompoundButton.CheckedChangeEventArg` argument.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Liens associés

- [SwitchDemo (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [Didacticiel de mise en page d’onglet](~/android/user-interface/layouts/tab-layout/index.md)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
