---
title: "Fonctionnalités et les parties RecyclerView"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b1ddcca25fd83a806e8383a5717462b518b46d0b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview-parts-and-functionality"></a>Fonctionnalités et les parties RecyclerView


`RecyclerView` poignées de certaines tâches en interne (telles que le défilement et le recyclage des vues), mais il est essentiellement un gestionnaire qui coordonne les classes d’assistance pour afficher une collection. `RecyclerView` tâches de délégués pour les classes d’assistance suivantes :

-   **`Adapter`** &ndash; Augmente les mises en page de l’élément (instancie le contenu d’un fichier de mise en page) et lie des données à des vues qui sont affichés dans un `RecyclerView`. L’adaptateur signale également les événements click de l’élément.

-   **`LayoutManager`** &ndash; Mesures et place des vues des éléments au sein d’un `RecyclerView` et gère la stratégie pour le recyclage de la vue.

-   **`ViewHolder`** &ndash; Recherche et enregistre les références de la vue. Le détenteur de l’affichage permet également de détecter les clics de la vue de l’élément.

-   **`ItemDecoration`** &ndash; Permet à une application Ajouter des décalages de disposition et le dessin spéciaux à des vues spécifiques pour le dessin des séparateurs entre les éléments, met en évidence et limites de regroupement visuel.

-   **`ItemAnimator`** &ndash; Définit les animations qui ont lieu pendant les actions de l’élément ou en tant que les modifications sont apportées à l’adaptateur.

La relation entre la `RecyclerView`, `LayoutManager`, et `Adapter` classes est illustré dans le diagramme suivant :

![Diagramme de RecyclerView contenant LayoutManager, à l’aide de l’adaptateur pour accéder au jeu de données](parts-and-functionality-images/01-recyclerview-diagram.png)

Comme l’illustre la figure, le `LayoutManager` peut être considéré comme intermédiaire entre la `Adapter` et `RecyclerView`. Le `LayoutManager` appelle `Adapter` méthodes de la part de le `RecyclerView`. Par exemple, le `LayoutManager` appelle un `Adapter` méthode au moment de créer une vue pour une position de l’élément particulier dans le `RecyclerView`. Le `Adapter` augmente la disposition de cet élément et crée un `ViewHolder` instance (non affiché) pour le cache des références aux vues à cette position. Lorsque le `LayoutManager` appelle la `Adapter` pour lier un élément particulier au jeu de données, le `Adapter` localise les données pour cet élément, il récupère le jeu de données et la copie à l’affichage de l’élément associé.

Lorsque vous utilisez `RecyclerView` dans votre application, la création des types dérivés des classes suivantes est requise :

-   **`RecyclerView.Adapter`** &ndash; Fournit une liaison à partir du jeu de données de votre application (qui est spécifique à votre application) pour les vues des éléments qui sont affichés dans le `RecyclerView`. L’adaptateur sait comment associer chaque position d’affichage des éléments dans le `RecyclerView` à un emplacement spécifique dans la source de données. En outre, l’adaptateur gère la disposition du contenu dans la vue de chaque élément individuel et crée le détenteur de la vue pour chaque vue. L’adaptateur signale également des événements de clic de l’élément qui sont détectés par la vue de l’élément.

-   **`RecyclerView.ViewHolder`** &ndash; Met en cache des références aux vues dans votre fichier de mise en page élément afin que les recherches de ressources ne sont pas répétées inutilement. Le détenteur de la vue réorganise également pour les événements de clic de l’élément à transmettre à l’adaptateur lorsqu’un utilisateur appuie sur l’affichage d’un élément associé à la vue titulaire du.

-   **`RecyclerView.LayoutManager`** &ndash; Positionne des éléments dans le `RecyclerView`. Vous pouvez utiliser l’une de plusieurs gestionnaires de mise en page prédéfinies, ou vous pouvez implémenter votre propre gestionnaire de disposition personnalisée.
    `RecyclerView` délégués change de la stratégie de mise en page pour le Gestionnaire de disposition, afin que vous pouvez incorporer dans un gestionnaire de mise en page différente sans devoir faire significatif à votre application.

En outre, vous pouvez étendre éventuellement les classes suivantes pour modifier l’apparence de `RecyclerView` dans votre application :

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Si vous n’étendez pas `ItemDecoration` et `ItemAnimator`, `RecyclerView` utilise les implémentations par défaut. Ce guide n’explique pas comment créer des `ItemDecoration` et `ItemAnimator` classes ; pour plus d’informations sur ces classes, consultez [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) et [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Comment afficher le recyclage des travaux

`RecyclerView` n’alloue pas une vue d’article pour chaque élément de source de données. Au lieu de cela, il alloue uniquement le nombre de vues des éléments qui correspondent à l’écran, et il réutilise les dispositions de l’élément en tant que l’utilisateur fait défiler. Lorsque la vue défile tout d’abord basculer vers elle, il passe par le processus de recyclage illustré dans la figure suivante :

[ ![Diagramme illustrant les six étapes de la vue de recyclage](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png)

1.  Lorsqu’une vue fait défiler la vue et n’est plus affichée, il devient un *mise au rebut de vue*.

2.  La vue de la mise au rebut est placée dans un pool et devient un *recycler vue*.
    Ce pool est un cache de vues qui affichent le même type de données.

3.  Lorsqu’un nouvel élément doit être affichée, une vue est effectuée à partir du pool de recyclage pour réutilisation. Étant donné que cette vue doit être liée de nouveau par l’adaptateur avant d’être affichée, elle est appelée un *dirty vue*.

4.  La vue dirty est recyclée : l’adaptateur recherche les données pour l’élément suivant à afficher et copie ces données aux affichages de cet élément. Références de ces vues sont extraites le détenteur de la vue associé à la vue recyclée.

5.  La vue recyclée est ajoutée à la liste des éléments dans le `RecyclerView` qui doivent accéder à l’écran.

6.  La vue recyclée passe à l’écran en tant que l’utilisateur fait défiler le `RecyclerView` à l’élément suivant dans la liste. Pendant ce temps, une autre vue défile hors de vue et est recyclée après les étapes ci-dessus.

En plus de la réutilisation de l’affichage des éléments, `RecyclerView` utilise également une autre optimisation de l’efficacité : afficher les détenteurs. A *détenteur de la vue* est une classe simple que caches afficher les références. Chaque fois que l’adaptateur augmente d’un fichier de disposition de l’élément, il crée également un détenteur de vue correspondante. Le détenteur de la vue utilise `FindViewById` pour obtenir des références aux vues dans le fichier de disposition d’élément augmentée. Ces références sont utilisées pour charger les nouvelles données dans les vues chaque fois que la disposition est recyclée pour afficher les nouvelles données.
 

<a name="layoutmanager" />

### <a name="the-layout-manager"></a>Le Gestionnaire de disposition

Le Gestionnaire de disposition est chargé pour le positionnement des éléments dans le `RecyclerView` afficher ; il détermine le type de présentation (une liste ou une grille), l’orientation (si les éléments sont affichées verticalement ou horizontalement) et les éléments de la direction doivent être affichés. (dans l’ordre normal ou dans l’ordre inverse). Le Gestionnaire de disposition est également chargé de calculer la taille et la position de chaque élément dans le **RecycleView** afficher.

Le Gestionnaire de disposition a un objectif supplémentaire : elle détermine la stratégie pour les périodes de recycler les vues des éléments qui ne sont plus visibles par l’utilisateur.
Étant donné que le Gestionnaire de disposition est prenant en charge les modes d’affichage sont visibles (et qui ne sont pas), il est la mieux placée pour déterminer quand une vue peut être recyclée. Pour recycler un affichage, le Gestionnaire de disposition généralement appelle l’adaptateur pour remplacer le contenu d’une vue recyclé avec des données différentes, comme décrit précédemment dans [vue recyclage fonctionnement](#recycling).

Vous pouvez étendre `RecyclerView.LayoutManager` pour créer votre propre disposition les manager, ou vous pouvez utiliser un gestionnaire de disposition prédéfinie. `RecyclerView` fournit les gestionnaires de mise en page prédéfinies suivantes :

-   **`LinearLayoutManager`** &ndash; Organise les éléments dans une colonne qui peut défiler verticalement, ou dans une ligne de défilement horizontale.

-   **`GridLayoutManager`** &ndash; Affiche les éléments dans une grille.

-   **`StaggeredGridLayoutManager`** &ndash; Affiche les éléments dans une grille échelonnée, où certains éléments ont différentes hauteurs et largeurs.

Pour spécifier le Gestionnaire de disposition, instancier votre gestionnaire de configuration choisie et passer à la `SetLayoutManager` (méthode). Notez que vous avez *doit* spécifier le Gestionnaire de disposition &ndash; `RecyclerView` ne sélectionne pas un gestionnaire de disposition prédéfinie par défaut.

Pour plus d’informations sur le Gestionnaire de disposition, consultez la [référence de classe RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).

<a name="viewholder" />

### <a name="the-view-holder"></a>Le détenteur de l’affichage

Le détenteur de la vue est une classe que vous définissez pour afficher les références de la mise en cache. L’adaptateur utilise ces références de vue pour lier chaque vue de son contenu. Chaque élément dans le `RecyclerView` doté d’une instance de détenteur de vue associée qui met en cache les références de l’affichage de cet élément. Pour créer un support de la vue, utilisez les étapes suivantes pour définir une classe pour contenir le jeu exact de vues par article :

1.  Sous-classe `RecyclerView.ViewHolder`.
2.  Implémentez un constructeur qui recherche et enregistre les références de la vue.
3.  Implémenter des propriétés de l’adaptateur peut utiliser pour accéder à ces références.

Un exemple détaillé d’un `ViewHolder` implémentation est présenté dans [A base RecyclerView exemple](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Pour plus d’informations sur `RecyclerView.ViewHolder`, consultez la [référence de classe RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).

<a name="adapter" />

### <a name="the-adapter"></a>L’adaptateur

La plupart de la « lourdes » de la `RecyclerView` code d’intégration a lieu dans l’adaptateur. `RecyclerView` Vous devez fournir un adaptateur dérivé `RecyclerView.Adapter` pour accéder à votre source de données et remplir chaque élément avec le contenu de la source de données.
Étant donné que la source de données est spécifique à l’application, vous devez implémenter les fonctionnalités d’adaptateur capable d’accéder à vos données. L’adaptateur extrait les informations de la source de données et les charge dans chaque élément dans le `RecyclerView` collection.

Le dessin suivant montre comment l’adaptateur mappe le contenu dans une source de données par le biais des détenteurs de vue à des vues dans chaque élément de ligne dans le `RecyclerView`:

[ ![Diagramme illustrant la carte de connexion de Source de données à ViewHolders](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png)

L’adaptateur charge chacun `RecyclerView` ligne contenant des données pour un élément de ligne particulière. Pour la position de ligne *P*, par exemple, l’adaptateur recherche les données associées à la position *P* au sein de la source de données et les copies de ces données à la ligne d’élément à la position *P* dans le `RecyclerView` collection.
Dans le dessin ci-dessus, par exemple, l’adaptateur utilise le détenteur de la vue pour rechercher les références pour le `ImageView` et `TextView` à cet emplacement afin qu’il ne doit pas nécessairement appeler `FindViewById` pour ces vues en tant que l’utilisateur fait défiler la collection et réutilise les vues.

Lorsque vous implémentez un adaptateur, vous devez substituer les éléments suivants `RecyclerView.Adapter` méthodes :

-   **`OnCreateViewHolder`** &ndash; Instancie le détenteur de fichier et affichage de mise en page élément.

-   **`OnBindViewHolder`** &ndash; Charge les données à la position spécifiée dans les vues dont les références sont stockés dans le détenteur de la vue donnée.

-   **`ItemCount`** &ndash; Retourne le nombre d’éléments dans la source de données.

Le Gestionnaire de disposition appelle ces méthodes pendant qu’il est positionner des éléments dans le `RecyclerView`. 


<a name="datachanges" />

### <a name="notifying-recyclerview-of-data-changes"></a>RecyclerView de notification des modifications de données

`RecyclerView` ne pas automatiquement à jour son affichage lorsque le contenu de ses données source change ; l’adaptateur doit informer `RecyclerView` lors d’une modification dans le jeu de données. Le jeu de données peut modifier de nombreuses manières ; par exemple, le contenu d’un élément peut modifier ou la structure globale des données peut être modifiée.
`RecyclerView.Adapter` fournit un certain nombre de méthodes que vous pouvez appeler afin que `RecyclerView` répond aux modifications de données de la manière la plus efficace :

-  **`NotifyItemChanged`** &ndash; Indique que l’élément à la position spécifiée a été modifié.

-  **`NotifyItemRangeChanged`** &ndash; Indique que les éléments de la plage spécifiée des positions ont été modifiés.

-  **`NotifyItemInserted`** &ndash; Signale que l’élément à la position spécifiée a été récemment insérée.

-  **`NotifyItemRangeInserted`** &ndash; Indique que les éléments de la plage spécifiée des positions ont été récemment insérées.

-  **`NotifyItemRemoved`** &ndash; Signale que l’élément à la position spécifiée a été supprimé.

-  **`NotifyItemRangeRemoved`** &ndash; Indique que les éléments de la plage spécifiée des positions ont été supprimés.

-  **`NotifyDataSetChanged`** &ndash; Indique que le jeu de données a changé (force une mise à jour complète).

Si vous savez exactement comment votre jeu de données a changé, vous pouvez appeler les méthodes appropriées ci-dessus pour actualiser `RecyclerView` de la manière la plus efficace. Si vous ne connaissez pas exactement comment votre jeu de données a changé, vous pouvez appeler `NotifyDataSetChanged`, qui est beaucoup moins efficace, car `RecyclerView` doit actualiser toutes les vues qui sont visibles par l’utilisateur. Pour plus d’informations sur ces méthodes, consultez [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Dans la rubrique suivante, [exemple RecyclerView A](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), un exemple d’application est implémentée pour illustrer des exemples de code réel des parties et des fonctionnalités présentées ci-dessus.


## <a name="related-links"></a>Liens associés

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Un exemple de base RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Pour étendre l’exemple RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
