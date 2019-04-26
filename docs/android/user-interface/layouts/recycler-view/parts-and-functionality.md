---
title: Fonctionnalités et les parties de RecyclerView
description: Vue d’ensemble du Gestionnaire de disposition RecyclerView, adaptateur et le détenteur de la vue.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 13678d3b1bca102e6f608ad1c11838db1f14cd08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61309155"
---
# <a name="recyclerview-parts-and-functionality"></a>Fonctionnalités et les parties de RecyclerView


`RecyclerView` poignées de certaines tâches en interne (telles que le défilement et le recyclage des vues), mais il est essentiellement un gestionnaire qui coordonne les classes d’assistance pour afficher une collection. `RecyclerView` tâches de délégués pour les classes d’assistance suivantes :

-   **`Adapter`** &ndash; Augmente les dispositions de l’élément (instancie le contenu d’un fichier de disposition) et lie les données aux vues qui sont affichés dans un `RecyclerView`. L’adaptateur signale également les événements de clic de l’élément.

-   **`LayoutManager`** &ndash; Mesure et positionne les vues des éléments au sein d’un `RecyclerView` et gère la stratégie pour le recyclage de la vue.

-   **`ViewHolder`** &ndash; Recherche et enregistre les références de la vue. Le détenteur de la vue permet également de détecter les clics de l’affichage des éléments.

-   **`ItemDecoration`** &ndash; Permet à une application Ajouter des décalages de dessin et de disposition spéciales à des vues spécifiques de dessin des séparateurs entre les éléments, les points forts et les limites de regroupement visuel.

-   **`ItemAnimator`** &ndash; Définit les animations qui ont lieu au cours des actions de l’élément ou en tant que les modifications sont apportées à l’adaptateur.

La relation entre la `RecyclerView`, `LayoutManager`, et `Adapter` classes est illustré dans le diagramme suivant :

![Diagramme de RecyclerView contenant LayoutManager, à l’aide de l’adaptateur pour accéder aux données](parts-and-functionality-images/01-recyclerview-diagram.png)

Comme l’illustre la figure, le `LayoutManager` peut être considéré comme intermédiaire entre le `Adapter` et `RecyclerView`. Le `LayoutManager` effectue des appels dans `Adapter` méthodes de la part de le `RecyclerView`. Par exemple, le `LayoutManager` appelle un `Adapter` méthode lorsqu’il est temps de créer une nouvelle vue pour une position de l’élément particulier dans le `RecyclerView`. Le `Adapter` augmente la disposition pour cet élément et crée un `ViewHolder` instance (non illustré) aux références du cache pour les vues à cette position. Lorsque le `LayoutManager` appelle le `Adapter` pour lier un élément particulier au jeu de données, le `Adapter` localise les données pour cet élément, il récupère le jeu de données et le copie dans la vue de l’élément associé.

Lorsque vous utilisez `RecyclerView` dans votre application, les types dérivés des classes suivantes est nécessaire de créer :

-   **`RecyclerView.Adapter`** &ndash; Fournit une liaison du jeu de données de votre application (ce qui est spécifique à votre application) pour les vues des éléments qui sont affichés dans le `RecyclerView`. L’adaptateur sait comment associer chaque position d’affichage des éléments dans le `RecyclerView` à un emplacement spécifique dans la source de données. En outre, l’adaptateur gère la disposition du contenu dans chaque vue de l’élément individuel et crée le détenteur de la vue pour chaque vue. L’adaptateur signale également les événements de clic de l’élément qui sont détectés par la vue de l’élément.

-   **`RecyclerView.ViewHolder`** &ndash; Met en cache des références aux vues dans votre fichier de disposition élément afin que les recherches de ressources ne sont pas répétées inutilement. Le détenteur de la vue également fait en sorte que les événements de clic de l’élément peut être envoyé à l’adaptateur quand un utilisateur appuie sur l’affichage d’un élément associé à la vue du titulaire du.

-   **`RecyclerView.LayoutManager`** &ndash; Positionne des éléments dans le `RecyclerView`. Vous pouvez utiliser un de plusieurs gestionnaires de disposition prédéfinie, ou vous pouvez implémenter votre propre gestionnaire de disposition personnalisée.
    `RecyclerView` délégués la stratégie de mise en page pour le Gestionnaire de disposition, afin que vous pouvez incorporer dans un gestionnaire de disposition différents sans avoir à apporter importantes modifications à votre application.

En outre, vous pouvez étendre éventuellement les classes suivantes pour modifier l’apparence de `RecyclerView` dans votre application :

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Si vous n’étendez pas `ItemDecoration` et `ItemAnimator`, `RecyclerView` utilise les implémentations par défaut. Ce guide n’explique pas comment créer des `ItemDecoration` et `ItemAnimator` classes ; pour plus d’informations sur ces classes, consultez [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) et [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Comment afficher recyclage Works

`RecyclerView` n’alloue pas une vue de l’élément pour chaque élément dans votre source de données. Au lieu de cela, il alloue uniquement le nombre de vues des éléments qui correspondent à l’écran, et il réutilise ces mises en page de l’élément en tant que l’utilisateur fait défiler. Lorsque la vue défile tout d’abord hors de vue, il transite par le processus de recyclage illustré dans la figure suivante :

[![Diagramme illustrant les six étapes de recyclage de la vue](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  Lorsqu’une vue défile hors de vue et n’est plus affichée, il devient un *mise au rebut de vue*.

2.  La vue de la mise au rebut est placée dans un pool et devient un *recycler vue*.
    Ce pool est un cache de vues qui affichent le même type de données.

3.  Lorsqu’un nouvel élément doit être affichée, une vue est effectuée à partir du pool de recyclage pour une réutilisation. Étant donné que cette vue doit être liée à nouveau par l’adaptateur avant d’être affichée, elle est appelée un *dirty vue*.

4.  La vue sale est recyclée : l’adaptateur recherche les données pour l’élément suivant à afficher et copie ces données dans les vues pour cet élément. Références pour ces vues sont récupérées depuis le détenteur de la vue associé à la vue recyclée.

5.  La vue recyclée est ajoutée à la liste des éléments dans le `RecyclerView` qui doivent accéder à l’écran.

6.  Le recyclage de l’affichage passe à l’écran en tant que l’utilisateur fait défiler le `RecyclerView` à l’élément suivant dans la liste. Pendant ce temps, une autre vue défile hors de vue et est recyclée en respectant les étapes ci-dessus.

En plus de réutilisation de l’affichage des éléments, `RecyclerView` utilise également une autre optimisation de l’efficacité : afficher les détenteurs. Un *détenteur de la vue* est une classe simple que caches afficher les références. Chaque fois que l’adaptateur augmente d’un fichier de disposition de l’élément, il crée également un détenteur de la vue correspondante. Le détenteur de la vue utilise `FindViewById` pour obtenir des références aux vues dans le fichier de disposition d’élément gonflé. Ces références sont utilisées pour charger les nouvelles données dans les vues chaque fois que la disposition est recyclée pour afficher les nouvelles données.
 


### <a name="the-layout-manager"></a>Le Gestionnaire de disposition

Il incombe au Gestionnaire de disposition pour positionner des éléments dans le `RecyclerView` afficher ; il détermine le type de présentation (une liste ou une grille), l’orientation (si les éléments sont affichés verticalement ou horizontalement), et les éléments de la direction doivent être affichés. (dans l’ordre normal ou dans l’ordre inverse). Le Gestionnaire de disposition est également chargé de calculer la taille et la position de chaque élément dans le **RecycleView** afficher.

Le Gestionnaire de disposition a un objectif supplémentaire : il détermine la stratégie pour le moment auquel le recyclage des vues des éléments qui ne sont plus visibles par l’utilisateur.
Étant donné que le Gestionnaire de disposition connaît les vues qui sont visibles (et qui ne sont pas), il est la mieux placée pour décider quand une vue peut être recyclée. Pour recycler une vue, le Gestionnaire de disposition généralement effectue des appels à l’adaptateur pour remplacer le contenu d’une vue recyclé avec des données différentes, comme décrit précédemment dans [vue recyclage fonctionnement](#recycling).

Vous pouvez étendre `RecyclerView.LayoutManager` pour créer votre propre disposition manager, ou vous pouvez utiliser un gestionnaire de disposition prédéfinie. `RecyclerView` fournit les gestionnaires de présentation prédéfinis suivants :

-   **`LinearLayoutManager`** &ndash; Organise les éléments dans une colonne pouvant défiler verticalement, ou dans une ligne permettre défiler horizontalement.

-   **`GridLayoutManager`** &ndash; Affiche les éléments dans une grille.

-   **`StaggeredGridLayoutManager`** &ndash; Affiche les éléments dans une grille échelonnée, où certains éléments ont différentes hauteurs et les largeurs.

Pour spécifier le Gestionnaire de disposition, instanciez votre gestionnaire de disposition choisie et transmettez-le à la `SetLayoutManager` (méthode). Notez que vous avez *doit* spécifier le Gestionnaire de disposition &ndash; `RecyclerView` ne sélectionne pas un gestionnaire de disposition prédéfinie par défaut.

Pour plus d’informations sur le Gestionnaire de disposition, consultez le [référence de classe RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).


### <a name="the-view-holder"></a>Le détenteur de la vue

Le détenteur de la vue est une classe que vous définissez pour afficher les références de la mise en cache. L’adaptateur utilise ces références de vue pour lier chaque vue de son contenu. Chaque élément dans le `RecyclerView` doté d’une instance de détenteur de vue associée qui met en cache les références de vue pour cet élément. Pour créer un détenteur de la vue, procédez comme suit pour définir une classe pour contenir le jeu exact de visites par élément :

1.  Sous-classe `RecyclerView.ViewHolder`.
2.  Implémentez un constructeur qui recherche et enregistre les références de la vue.
3.  Implémentez les propriétés que l’adaptateur peut utiliser pour accéder à ces références.

Un exemple détaillé d’un `ViewHolder` implémentation est présentée dans [un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Pour plus d’informations sur `RecyclerView.ViewHolder`, consultez le [référence de classe RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="the-adapter"></a>L’adaptateur

La plupart de la « lourdes » de la `RecyclerView` code d’intégration a lieu dans l’adaptateur. `RecyclerView` exige que vous fournissiez un adaptateur dérivé `RecyclerView.Adapter` pour accéder à votre source de données et remplir chaque élément avec le contenu à partir de la source de données.
Étant donné que la source de données est spécifique à l’application, vous devez implémenter les fonctionnalités d’adaptateur qui comprend comment accéder à vos données. L’adaptateur extrait les informations de la source de données et les charge dans chaque élément dans le `RecyclerView` collection.

Le dessin suivant illustre comment l’adaptateur mappe le contenu dans une source de données par le biais des détenteurs de vue pour les vues individuelles au sein de chaque élément de ligne dans le `RecyclerView`:

[![Diagramme illustrant l’adaptateur de connexion de Source de données à ViewHolders](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

L’adaptateur charge chaque `RecyclerView` ligne contenant des données pour un élément de ligne particulière. Pour la position de ligne *P*, par exemple, l’adaptateur localise les données associées à la position *P* au sein de la source de données et copie ces données à la ligne d’élément à la position *P* dans le `RecyclerView` collection.
Dans le dessin ci-dessus, par exemple, l’adaptateur utilise le détenteur de la vue pour rechercher les références pour le `ImageView` et `TextView` à cette position afin qu’elle ait à appeler à plusieurs reprises `FindViewById` pour ces vues en tant que l’utilisateur parcourt la collection et réutilise les vues.

Lorsque vous implémentez un adaptateur, vous devez substituer les éléments suivants `RecyclerView.Adapter` méthodes :

-   **`OnCreateViewHolder`** &ndash; Instancie le détenteur de fichier et affichage de disposition élément.

-   **`OnBindViewHolder`** &ndash; Charge les données à la position spécifiée dans les vues dont les références sont stockés dans le détenteur de la vue donnée.

-   **`ItemCount`** &ndash; Retourne le nombre d’éléments dans la source de données.

Le Gestionnaire de disposition appelle ces méthodes lorsqu’il positionne les éléments dans le `RecyclerView`. 



### <a name="notifying-recyclerview-of-data-changes"></a>RecyclerView de notification des modifications de données

`RecyclerView` ne pas automatiquement à jour son affichage lorsque le contenu de ses données source change ; l’adaptateur doit notifier `RecyclerView` lorsqu’il existe une modification dans le jeu de données. Le jeu de données peut modifier de nombreuses façons ; par exemple, le contenu au sein d’un élément peut modifier ou la structure globale des données peut être modifiée.
`RecyclerView.Adapter` fournit un certain nombre de méthodes que vous pouvez appeler afin que `RecyclerView` répond aux modifications de données de la manière la plus efficace :

-  **`NotifyItemChanged`** &ndash; Indique que l’élément à la position spécifiée a été modifié.

-  **`NotifyItemRangeChanged`** &ndash; Indique que les éléments dans la plage spécifiée de positions ont été modifiés.

-  **`NotifyItemInserted`** &ndash; Signale que l’élément à la position spécifiée a été récemment insérée.

-  **`NotifyItemRangeInserted`** &ndash; Signale que les éléments dans la plage spécifiée de positions ont été récemment insérées.

-  **`NotifyItemRemoved`** &ndash; Signale que l’élément à la position spécifiée a été supprimé.

-  **`NotifyItemRangeRemoved`** &ndash; Signale que les éléments dans la plage spécifiée de positions ont été supprimés.

-  **`NotifyDataSetChanged`** &ndash; Indique que le jeu de données a changé (force une mise à jour complète).

Si vous savez exactement comment votre jeu de données a changé, vous pouvez appeler les méthodes appropriées ci-dessus pour actualiser `RecyclerView` de la manière la plus efficace. Si vous ne connaissez pas exactement comment votre jeu de données a changé, vous pouvez appeler `NotifyDataSetChanged`, ce qui est beaucoup moins efficace car `RecyclerView` doit actualiser toutes les vues qui sont visibles par l’utilisateur. Pour plus d’informations sur ces méthodes, consultez [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Dans la rubrique suivante, [un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), un exemple d’application est implémentée pour illustrer des exemples de code réel des parties et des fonctionnalités décrites ci-dessus.


## <a name="related-links"></a>Liens associés

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Extension de l’exemple de RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
