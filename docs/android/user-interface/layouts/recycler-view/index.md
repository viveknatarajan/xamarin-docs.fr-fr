---
title: RecyclerView
description: RecyclerView est un groupe de vue pour afficher les collections ; Il est conçu pour être un remplacement plus flexible pour afficher les groupes plus anciens tels que ListView et GridView.  Ce guide explique comment utiliser et personnaliser RecyclerView dans les applications Xamarin.Android.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 1332a7ef5b8e5bb2f1178582bcf058123f1e177c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110145"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView est un groupe de vue pour afficher les collections ; Il est conçu pour être un remplacement plus flexible pour afficher les groupes plus anciens tels que ListView et GridView.  Ce guide explique comment utiliser et personnaliser RecyclerView dans les applications Xamarin.Android._

## <a name="recyclerview"></a>RecyclerView

Beaucoup d’applications nécessitent d’afficher des regroupements du même type (par exemple, les messages, contacts, des images ou des chansons) ; souvent, cette collection est trop grande pour tenir sur l’écran, afin de la collection est présentée dans une petite fenêtre qui peut défiler aisément à tous les éléments dans la collection.
`RecyclerView` est un widget d’Android qui affiche une collection d’éléments dans une liste ou une grille, l’activation de l’utilisateur de parcourir la collection. Voici une capture d’écran d’un exemple d’application qui utilise `RecyclerView` pour afficher le contenu de boîte de réception de courrier électronique dans une liste de défilement verticale :

[![Exemple d’application à l’aide de RecyclerView pour répertorier les messages de boîte de réception](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` offre deux fonctionnalités intéressantes :

-  Il possède une architecture flexible qui vous permet de modifier son comportement en connectant vos composants par défaut.

-  Il est efficace avec collections volumineuses, car il réutilise les vues des éléments et nécessite l’utilisation de *afficher détenteurs* pour afficher les références du cache.

Ce guide explique comment utiliser `RecyclerView` dans les applications Xamarin.Android ; il explique comment ajouter la `RecyclerView` package pour votre projet Xamarin.Android et il décrit comment `RecyclerView` fonctions dans une application classique. Exemples de code réel sont fournis pour vous montrer comment intégrer `RecyclerView` dans votre application, comment implémenter la vue de l’élément, cliquez sur et comment actualiser `RecyclerView` lorsque ses données sous-jacentes changent. Ce guide suppose que vous êtes familiarisé avec le développement Xamarin.Android.


### <a name="requirements"></a>Configuration requise

Bien que `RecyclerView` est souvent associée à Android 5.0 Lollipop, il est proposé comme une bibliothèque de prise en charge &ndash; `RecyclerView` fonctionne avec les applications de ce niveau d’API cible 7 (Android 2.1) et versions ultérieures. Ce qui suit est nécessaire pour utiliser `RecyclerView` dans les applications Xamarin :

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac.

-  Votre projet d’application doit inclure le **Xamarin.Android.Support.v7.RecyclerView** package. Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Vue d'ensemble

`RecyclerView` peut être considéré comme un remplacement pour le `ListView` et `GridView` widgets dans Android. Tout comme ses prédécesseurs, `RecyclerView` est conçu pour afficher un jeu de données volumineux dans une petite fenêtre, mais `RecyclerView` offre plusieurs options de disposition et optimisée pour l’affichage de grandes collections. Si vous êtes familiarisé avec `ListView`, il existe plusieurs différences importantes entre `ListView` et `RecyclerView`:

-   `RecyclerView` est légèrement plus complexe à utiliser : vous devez écrire davantage de code à utiliser `RecyclerView` par rapport à `ListView`.

-   `RecyclerView` ne fournit pas d’une carte prédéfinie ; Vous devez implémenter le code d’adaptateur qui accède à votre source de données. Toutefois, Android inclut plusieurs adaptateurs prédéfinies qui fonctionnent avec `ListView` et `GridView`.

-   `RecyclerView` n’offre pas un événement de clic de l’élément lorsqu’un utilisateur appuie sur un élément ; au lieu de cela, les événements de clic de l’élément sont gérés par les classes d’assistance. En revanche, `ListView` offre un événement de clic de l’élément.

-   `RecyclerView` améliore les performances en recyclant les vues et en appliquant le modèle de détenteur de la vue, ce qui élimine les recherches de ressources de dispositions inutiles. Utilisation du modèle d’espace réservé de la vue est facultative dans `ListView`.

-   `RecyclerView` repose sur une conception modulaire qui rend plus facile à personnaliser. Par exemple, vous pouvez incorporer dans une stratégie de mise en page différente sans modifications de code significatives à votre application.
    En revanche, `ListView` est relativement monolithique dans la structure.

-   `RecyclerView` inclut les animations intégrées pour l’élément, ajouter et supprimer. `ListView` animations nécessitent certains effort supplémentaire de la part du développeur d’application.


### <a name="sections"></a>Sections

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[Fonctionnalités et les parties de RecyclerView](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

Cette rubrique explique comment la `Adapter`, `LayoutManager`, et `ViewHolder` fonctionnent ensemble en tant que classes d’assistance pour prendre en charge `RecyclerView`.
Il fournit une vue d’ensemble de chacune de ces classes d’assistance et explique comment les utiliser dans votre application.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Cette rubrique s’appuie sur les informations fournies dans [RecyclerView parties et fonctionnalité](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) en fournissant des exemples de code réel de divers `RecyclerView` éléments sont implémentées pour créer une application de navigation de photos réalistes.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Extension de l’exemple de RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

Cette rubrique ajoute du code supplémentaire à l’exemple d’application présenté dans [un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) pour montrer comment gérer les événements de clic de l’élément et mettre à jour `RecyclerView` lorsque les modifications de source de données sous-jacentes.


### <a name="summary"></a>Récapitulatif

Ce guide a introduit Android `RecyclerView` widget ; vous avez appris comment ajouter la `RecyclerView` prennent en charge de bibliothèque aux projets Xamarin.Android, comment `RecyclerView` recycle les vues, il applique le modèle de détenteur de la vue pour l’efficacité et découvrez comment les différents les classes d’assistance qui composent `RecyclerView` collaborent pour afficher des regroupements. Il fourni d’exemple de code pour illustrer comment `RecyclerView` est intégré dans une application, vous avez appris comment adapter `RecyclerView`de stratégie de mise en page en branchant des gestionnaires de présentation différente et il décrit comment gérer l’élément événements de clic et notifier `RecyclerView`des modifications de source de données.

Pour plus d’informations sur `RecyclerView`, consultez le [référence de classe RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Liens associés

- [RecyclerViewer (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Présentation d’interface (lollipop)](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
