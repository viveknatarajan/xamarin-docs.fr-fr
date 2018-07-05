---
title: Basculer
description: Comment utiliser le widget de commutateur dans une application Xamarin.Android
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: e3bcce48a675a9ba3d1d41f93babc7fcb26448c8
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403274"
---
# <a name="switch"></a>Basculer

Le `Switch` widget (voir ci-dessous) permet à un utilisateur de basculer entre deux États, comme sous ou hors tension. Le `Switch` valeur par défaut est OFF. Dans ses États ON et OFF, le widget est indiqué ci-dessous :

[![Captures d’écran d’un widget de commutateur et activer des États](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Création d’un commutateur

Pour créer un commutateur, il vous suffit de déclarer un `Switch` élément XML comme suit :

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Cette opération crée un commutateur de base comme indiqué ci-dessous :

[![Capture d’écran de l’application de démonstration affichant un commutateur à l’état désactivé](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Modification des valeurs par défaut

Le texte qui affiche le contrôle des États le ON et OFF et la valeur par défaut sont configurables. Par exemple, pour rendre le commutateur par défaut la valeur ON et lire des non-Oui au lieu de la valeur OFF/ON, nous pouvons définir le `checked`, `textOn`, et `textOff` attributs dans le code XML suivant.

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

Quand un `Switch`de valeur change, il déclenche une `CheckedChange` événement.
Par exemple, nous capturer cet événement dans le code suivant et présenter un `Toast` widget avec un message en fonction de la `isChecked` valeur `Switch`, qui est transmis au gestionnaire d’événements dans le cadre de la `CompoundButton.CheckedChangeEventArg` argument.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Liens associés

- [SwitchDemo (exemple)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [Didacticiel de mise en page d’onglet](~/android/user-interface/layouts/tab-layout/index.md)
