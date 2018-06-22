---
title: RecyclerView
description: RecyclerView est un groupe d’affichage pour l’affichage de collections ; Il est conçu pour être un remplacement plus souple pour afficher les groupes plus anciens tels que ListView et GridView.  Ce guide explique comment utiliser et personnaliser RecyclerView dans les applications de Xamarin.Android.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: 187339244d53c154cc22672a3d2ceba7e0a75bcf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766333"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView est un groupe d’affichage pour l’affichage de collections ; Il est conçu pour être un remplacement plus souple pour afficher les groupes plus anciens tels que ListView et GridView.  Ce guide explique comment utiliser et personnaliser RecyclerView dans les applications de Xamarin.Android._

## <a name="recyclerview"></a>RecyclerView

De nombreuses applications doivent afficher les collections du même type (par exemple, les messages, contacts, des images ou des morceaux) ; souvent, cette collection est trop volumineuse pour tenir sur l’écran, la collection est présentée dans une petite fenêtre qui peut faire défiler en continu via tous les éléments dans la collection.
`RecyclerView` est un widget Android qui affiche une collection d’éléments dans une liste ou d’une grille, ce qui permet de parcourir la collection. Voici une capture d’écran d’une application d’exemple qui utilise `RecyclerView` pour afficher le contenu de la boîte de réception de courrier électronique dans une liste de défilement verticale :

[![Exemple d’application à l’aide de RecyclerView pour les messages de la boîte de réception de liste](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` propose deux fonctionnalités intéressantes :

-  Il possède une architecture flexible qui vous permet de modifier son comportement en connectant vos composants par défaut.

-  Il est efficace avec des regroupements volumineux, car il réutilise les vues des éléments et requiert l’utilisation de *afficher les détenteurs* pour afficher les références du cache.

Ce guide explique comment utiliser `RecyclerView` dans les applications de Xamarin.Android ; elle explique comment ajouter la `RecyclerView` package pour votre projet Xamarin.Android et il décrit comment `RecyclerView` fonctions dans une application classique. Exemples de code réel sont fournis pour vous montrer comment intégrer `RecyclerView` dans votre application, l’implémentation de vue de l’élément, cliquez sur et comment actualiser `RecyclerView` lorsque ses données sous-jacentes changent. Ce guide suppose que vous êtes familiarisé avec le développement de Xamarin.Android.


### <a name="requirements"></a>Spécifications

Bien que `RecyclerView` est souvent associée à l’interface Lollipop 5.0 Android, il est proposé comme une bibliothèque de prise en charge &ndash; `RecyclerView` fonctionne avec les applications de ce niveau de l’API cible 7 (Android 2.1) et versions ultérieures. Les éléments suivants sont requis pour utiliser `RecyclerView` dans les applications Xamarin :

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac.

-  Votre projet d’application doit inclure le **Xamarin.Android.Support.v7.RecyclerView** package. Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Vue d'ensemble

`RecyclerView` peut être considéré comme un remplacement pour le `ListView` et `GridView` les widgets d’Android. Tout comme ses prédécesseurs, `RecyclerView` est conçu pour afficher un jeu de données volumineux dans une petite fenêtre, mais `RecyclerView` offre davantage d’options de mise en page et optimisée pour l’affichage de grandes collections. Si vous êtes familiarisé avec `ListView`, il existe plusieurs différences importantes entre `ListView` et `RecyclerView`:

-   `RecyclerView` est légèrement plus complexe à utiliser : vous devez écrire plus de code pour utiliser `RecyclerView` par rapport à `ListView`.

-   `RecyclerView` ne fournit pas d’une carte prédéfinie ; Vous devez implémenter le code d’adaptateur qui accède à votre source de données. Toutefois, Android inclut plusieurs adaptateurs prédéfinies qui fonctionnent avec `ListView` et `GridView`.

-   `RecyclerView` n’offre pas un événement de clic de l’élément lorsqu’un utilisateur clique sur un élément ; au lieu de cela, les événements click de l’élément sont gérés par les classes d’assistance. En revanche, `ListView` offre un événement de clic de l’élément.

-   `RecyclerView` améliore les performances en recyclant les vues et en appliquant le modèle de vue titulaire, ce qui élimine les recherches de ressources de mise en forme inutiles. Utilisation du modèle d’espace réservé de la vue est facultative dans `ListView`.

-   `RecyclerView` repose sur une conception modulaire qui rend plus facile à personnaliser. Par exemple, vous pouvez incorporer dans une stratégie de mise en page différente sans modification du code significatif à votre application.
    En revanche, `ListView` est relativement monolithique de structure.

-   `RecyclerView` inclut les animations intégrées pour ajouter et de supprimer de l’élément. `ListView` animations nécessitent des efforts supplémentaires sur la partie au développeur d’application.


### <a name="sections"></a>Sections

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[Fonctionnalités et les parties RecyclerView](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

Cette rubrique explique comment les `Adapter`, `LayoutManager`, et `ViewHolder` fonctionnent ensemble en tant que classes d’assistance pour prendre en charge `RecyclerView`.
Il fournit une vue d’ensemble de chacune de ces classes d’assistance et explique comment les utiliser dans votre application.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Un exemple de base RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Cette rubrique s’appuie sur les informations fournies dans [RecyclerView parties et les fonctionnalités](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) en fournissant des exemples de code réel de différents `RecyclerView` éléments sont implémentés pour générer une application de navigation de photos réels.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Pour étendre l’exemple RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

Cette rubrique ajoute du code supplémentaire pour l’exemple d’application présenté dans [exemple RecyclerView A](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) pour montrer comment gérer les événements de clic de l’élément et de mettre à jour `RecyclerView` lorsque les modifications de source de données sous-jacentes.


### <a name="summary"></a>Récapitulatif

Ce guide a introduit le Android `RecyclerView` widget ; elle explique comment ajouter la `RecyclerView` prend en charge de bibliothèque aux projets de Xamarin.Android, comment `RecyclerView` recycle des vues, comment il applique le modèle de vue-espace réservé par souci d’efficacité et les différents classes d’assistance qui composent `RecyclerView` collaborent pour afficher des regroupements. Il fourni l’exemple de code pour illustrer comment `RecyclerView` est intégré dans une application, il a expliqué comment adapter `RecyclerView`de stratégie de mise en page en branchant des gestionnaires de mise en page différente et il décrit comment gérer les éléments les événements de clic et notifier `RecyclerView`des modifications de source de données.

Pour plus d’informations sur `RecyclerView`, consultez la [référence de classe RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Liens associés

- [RecyclerViewer (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Introduction à l’interface Lollipop](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
