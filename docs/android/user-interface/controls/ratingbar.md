---
title: RatingBar
description: Comment ajouter un widget RatingBar à une activité Android.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 97d2a126be70e210d2e8f4ebf4d7a25ff8777a02
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131090"
---
# <a name="ratingbar"></a>RatingBar

Un RatingBar est un widget d’interface utilisateur qui affiche une évaluation d’une à cinq étoiles. L’utilisateur peut sélectionner une évaluation en touchant sur une étoile dans cette section, vous allez créer un widget qui permet à l’utilisateur fournir une valeur, avec le [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/) widget.

![Exemple d’un RatingBar](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>Création d’un RatingBar

1. Ouvrez le **Resource/layout/Main.axml** fichier, puis ajoutez le [`RatingBar`](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/)
   élément (à l’intérieur de la [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)) :

    ```xml
    <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
    ```
   Le `android:numStars` attribut définit combien étoiles pour afficher la barre de contrôle d’accès. Le `android:stepSize` attribut définit la granularité de chaque étoile (par exemple, une valeur de `0.5` permettrait à moitié étoiles).

2. Pour faire quelque chose lorsqu’une nouvelle évaluation a été définie, ajoutez le code suivant à la fin de la [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
   méthode :

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    Il capture le [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/) widget à partir de la mise en page avec [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/) , définit une méthode d’événement puis définit l’action à effectuer lorsque l’utilisateur définit une évaluation. Dans ce cas, une simple [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message affiche la nouvelle évaluation.

3.  Exécutez l'application.

