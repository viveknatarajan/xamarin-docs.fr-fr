---
title: "Résumé du chapitre 26. Dispositions personnalisées"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b17924ee2ee7c944b3924ec79568f07feb4449a7
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Résumé du chapitre 26. Dispositions personnalisées

Xamarin.Forms inclut plusieurs classes dérivées de [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout` et
* `RelativeLayout`.

Ce chapitre explique comment créer vos propres classes qui dérivent de `Layout<View>`.

## <a name="an-overview-of-layout"></a>Vue d’ensemble de la mise en page

Il n’existe aucun système centralisé qui gère la disposition de Xamarin.Forms. Chaque élément est chargé de déterminer ce que sa taille doit être et comment effectuer le rendu de lui-même au sein d’une zone particulière.

### <a name="parents-and-children"></a>Parents et enfants

Chaque élément qui a des enfants est chargé pour le positionnement de ces enfants à l’intérieur de lui-même. Il est le parent qui détermine la taille adaptée à ses enfants doit être basé sur la taille qu’il est disponible et la taille de l’enfant souhaite.

### <a name="sizing-and-positioning"></a>Dimensionnement et de positionnement

Mise en page commence en haut de l’arborescence d’éléments visuels avec la page et se poursuit dans toutes les branches. La méthode publique plus importante dans la disposition est [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) défini par `VisualElement`. Chaque élément est un parent pour autres appels éléments `Layout` pour chacun de ses enfants pour permettre à l’enfant, une taille et la position par rapport à lui-même sous la forme d’un [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) valeur. Ces `Layout` propagent des appels dans l’arborescence d’éléments visuels.

Un appel à `Layout` est requis pour un élément à afficher sur l’écran et les propriétés en lecture seule suivantes à définir. Elles sont cohérentes avec le `Rectangle` passé à la méthode :

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) de type `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) de type `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) de type `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) de type `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) de type `double`

Avant la `Layout` appeler, `Height` et `Width` ont des valeurs factices &ndash;1.

Un appel à `Layout` déclenche également des appels aux méthodes protégées suivantes :

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), qui appelle
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), qui peut être remplacée.

Enfin, l’événement suivant est déclenché :

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

Le `OnSizeAllocated` méthode est substituée par `Page` et `Layout`, qui sont les deux seules classes dans Xamarin.Forms qui peuvent avoir des enfants. Les appels de méthode substituée

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) pour `Page` dérivés et [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) pour `Layout` dérivés qui appelle
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) pour `Page` dérivés et [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) pour `Layout` dérivés.

`LayoutChildren` appelle ensuite `Layout` pour chacun des enfants de l’élément. Si au moins un enfant dispose d’un nouveau `Bounds` définition, puis l’événement suivant est déclenché :

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) pour `Page` dérivés et [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) pour `Layout` dérivés

### <a name="constraints-and-size-requests"></a>Les contraintes et les demandes de taille

Pour `LayoutChildren` intelligemment appeler `Layout` sur tous ses enfants, il doit connaître un *préféré* ou *souhaitée* taille pour les enfants. Par conséquent, les appels à `Layout` pour chacun des enfants sont généralement précédé par des appels à

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Une fois que le livre a été publié, la `GetSizeRequest` méthode a été déconseillée et remplacée par

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

Le `Measure` méthode prend en charge la [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) propriété et comprend un argument de type [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), qui a deux membres :

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) Pour ne pas inclure les marges

Pour de nombreux éléments, `GetSizeRequest` ou `Measure` Obtient la taille de l’élément native à partir de son convertisseur. Les deux méthodes ont des paramètres pour la largeur et la hauteur *contraintes*. Par exemple, un `Label` utilisent la contrainte de largeur pour déterminer comment exposer plusieurs lignes de texte.

Les deux `GetSizeRequest`et `Measure` retournent une valeur de type [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), qui a deux propriétés :

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) de type `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) de type `Size`

Très souvent ces deux valeurs sont identiques et le `Minimum` valeur peut généralement être ignorée.

`VisualElement` définit également une méthode protégée semblable à `GetSizeRequest` qui est appelée à partir de `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) Retourne un `SizeRequest` valeur

Cette méthode est désormais déconseillée et remplacée par :

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Chaque classe qui dérive de `Layout` ou `Layout<T>` doit remplacer `OnSizeRequest` ou `OnMeasure`. Il s’agit d’où une classe de disposition détermine sa taille, ce qui est généralement basée sur la taille de ses enfants, il obtient en appelant `GetSizeRequest` ou `Measure` sur les enfants. Avant et après l’appel `OnSizeRequest` ou `OnMeasure`, `GetSizeRequest` ou `Measure` ajuste basée sur les propriétés suivantes :

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)de type `double`, affecte le `Request` propriété de `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) de type `double`, affecte le `Request` propriété de `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) de type `double`, affecte le `Minimum` propriété de `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) de type `double`, affecte le `Minimum` propriété de `SizeRequest`

### <a name="infinite-constraints"></a>Contraintes infinies

Les arguments de contrainte transmis à `GetSizeRequest` (ou `Measure`) et `OnSizeRequest` (ou `OnMeasure`) peut être infini (par exemple, les valeurs de `Double.PositiveInfinity`). Toutefois, le `SizeRequest` retourné à partir de ces méthodes ne peut pas contenir des dimensions infinies.

Contraintes infinis indiquent que la taille demandée doit refléter la taille physique de l’élément. Un vertical `StackLayout` appelle `GetSizeRequest` (ou `Measure`) sur ses enfants avec une contrainte de hauteur infinie. Une disposition horizontale pile appelle `GetSizeRequest` (ou `Measure`) sur ses enfants avec une contrainte de largeur infinie. Un `AbsoluteLayout` appelle `GetSizeRequest` (ou `Measure`) sur ses enfants avec des contraintes de largeur et hauteur infinies.

### <a name="peeking-inside-the-process"></a>Lecture à l’intérieur du processus

Le [ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) affiche contrainte et la taille de demandent des informations pour une mise en page simple.

## <a name="deriving-from-layoutview"></a>Dérivation à partir de la mise en page<View>

Une classe de la disposition personnalisée dérive `Layout<View>`. Il a deux tâches :

- Substituer `OnMeasure` pour appeler `Measure` sur les enfants du tout la disposition. Retourner une taille demandée pour le schéma lui-même
- Substituer `LayoutChildren` pour appeler `Layout` sur les enfants du tout la disposition

Le `for` ou `foreach` boucle de ces substitutions doit ignorer tout enfant dont `IsVisible` est définie sur `false`.

Un appel à `OnMeasure` n’est pas garantie. `OnMeasure` ne pas être appelée si le parent de la disposition est régissant le taille de la mise en page (par exemple, une disposition qui remplit une page). Pour cette raison, `LayoutChildren` ne peut pas s’appuient sur les tailles enfant obtenus pendant le `OnMeasure` appeler. Très souvent, `LayoutChildren` doit appeler lui-même `Measure` sur les enfants de la mise en page, ou vous pouvez implémenter une sorte de taille logique (décrit plus loin) de la mise en cache.

### <a name="an-easy-example"></a>Un exemple simple

Le [ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) exemple contient une représentation simplifiée [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) classe et une démonstration de son utilisation.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Simplifié de positionnement vertical et horizontal

Une des tâches qui `VerticalStack` doit effectuer se produit pendant la `LayoutChildren` remplacer. La méthode utilise l’enfant `HorizontalOptions` propriété pour déterminer la position de l’enfant dans son emplacement dans le `VerticalStack`. Vous pouvez appeler la méthode statique [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Cette méthode appelle `Measure` sur l’enfant et utilise son `HorizontalOptions` et `VerticalOptions` propriétés pour positionner l’enfant dans le rectangle spécifié.

### <a name="invalidation"></a>Invalidation

Souvent une modification de propriété d’un élément affecte l’élément dans la disposition. La disposition doit être invalidée pour déclencher une nouvelle disposition.

`VisualElement` définit une méthode protégée [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), qui est généralement appelé par le Gestionnaire de modification de propriété d’une propriété pouvant être liée dont la modification affecte la taille de l’élément. Le `InvalidateMeasure` se déclenche la méthode un [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) événement.

Le `Layout` classe définit une méthode protégée similaire nommée [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), un `Layout` dérivé doit appeler pour toute modification qui affecte la façon dont il positionne et des tailles de ses enfants.

### <a name="some-rules-for-coding-layouts"></a>Certaines règles de codage des dispositions

1. Les propriétés définies par `Layout<T>` dérivés doivent être sauvegardées par les propriétés pouvant être liées, et les gestionnaires de modification de propriété doivent appeler `InvalidateLayout`.

2. A `Layout<T>` dérivé qui définit les propriétés pouvant être liées attachées doit substituer [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) pour ajouter un gestionnaire de modification de propriété à ses enfants et [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) à supprimer Gestionnaire. Le gestionnaire doit vérifier les modifications apportées à ces présent les propriétés pouvant être liées et répondre en appelant `InvalidateLayout`.

3. A `Layout<T>` dérivée qui implémente un cache de tailles d’enfants doit substituer `InvalidateLayout` et [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) et effacer le cache lorsque ces méthodes sont appelées.

### <a name="a-layout-with-properties"></a>Une mise en page avec des propriétés

Le [ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) suppose que tous ses enfants sont la même taille et les enfants sont renvoyées à partir d’une ligne (ou colonne) à la prochaine. Il définit un `Orientation` propriété comme `StackLayout`, et `ColumnSpacing` et `RowSpacing` propriétés telles que `Grid`, et il met en cache les tailles des enfants.

Le [ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) exemple met un `WrapLayout` dans un `ScrollView` pour afficher les photos de stock.

### <a name="no-unconstrained-dimensions-allowed"></a>Aucune dimension sans contrainte autorisée !

Le [ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) dans les [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque permet d’afficher tous ses enfants lui-même. Par conséquent, il ne peut pas traiter les dimensions sans contrainte et lève une exception si un est rencontré.

Le [ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) illustre `UniformGridLayout`:

[![Capture d’écran de triple de la grille de Photo](images/ch26fg08-small.png "mise en page de grille uniforme")](images/ch26fg08-large.png#lightbox "mise en page de grille uniforme")

### <a name="overlapping-children"></a>Enfants superposés

Un `Layout<T>` dérivé peut se chevaucher ses enfants. Toutefois, les enfants sont rendus dans leur ordre dans le `Children` collection et pas l’ordre dans lequel leur `Layout` méthodes sont appelées.

La `Layout` classe définit deux méthodes qui vous permettent de déplacer un enfant dans la collection :

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) Pour déplacer un enfant au début de la collection
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) Pour déplacer un enfant à la fin de la collection

Pour les enfants qui se chevauchent, les enfants à la fin de la collection sont visuellement apparaissent avant enfants au début de la collection.

Le [ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque définit une propriété jointe pour indiquer l’ordre de rendu et permettre ainsi à l’un de ses enfants à afficher sur les autres. Le [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) illustre cela :

[![Capture d’écran de triple de grille de fichier de carte de Student](images/ch26fg10-small.png "enfants de la disposition qui se chevauchent")](images/ch26fg10-large.png#lightbox "enfants de la disposition qui se chevauchent")

### <a name="more-attached-bindable-properties"></a>Plus joint propriétés pouvant être liées

Le [ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque définit des propriétés attachées pouvant être liées pour spécifier deux `Point` valeurs et un valeur d’épaisseur et manipule `BoxView` éléments qui ressemble à celle des lignes.

Le [ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) exemple qui utilise pour dessiner un cube 3D.

### <a name="layout-and-layoutto"></a>Mise en page et LayoutTo

A `Layout<T>` dérivé peut appeler `LayoutTo` plutôt que `Layout` pour animer la disposition. Le [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) classe pour cela et le [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) exemple montre comment il.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 26 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Exemples de chapitre 26](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Créer des dispositions personnalisées](~/xamarin-forms/user-interface/layouts/custom.md)
