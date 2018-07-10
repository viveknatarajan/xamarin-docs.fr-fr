---
title: Résumé du chapitre 4. Défilement de la pile
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 4. Défilement de la pile'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7860df998fbfe580362aff0f4f01374a4ae1f923
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935551"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Résumé du chapitre 4. Défilement de la pile

Ce chapitre est principalement consacré au introduisant le concept de *disposition*, qui est le terme général pour les classes et les techniques de Xamarin.Forms utilise pour organiser l’affichage visuel de plusieurs vues sur la page.

Disposition implique plusieurs classes qui dérivent de [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) et [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). Ce chapitre se concentre sur [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

A également introduit dans ce chapitre sont les [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), et [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) classes.

## <a name="stacks-of-views"></a>Piles de vues

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) dérive de `Layout<View>` et hérite d’un [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) propriété de type `IList<View>`. Vous ajoutez plusieurs éléments d’affichage à cette collection, et `StackLayout` les affiche dans une pile horizontale ou verticale.

Définir le [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) propriété du `StackLayout` à un membre de la [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation) énumération, soit [ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical) ou [ `Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). La valeur par défaut est `Vertical`.

Définir le [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) propriété du `StackLayout` à un `double` valeur à spécifier un espacement entre les enfants. La valeur par défaut est 6.

Dans le code, vous pouvez ajouter des éléments à la `Children` collection de `StackLayout` dans un `for` ou `foreach` boucle comme illustré dans le [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) exemple, ou vous pouvez initialiser le `Children` collection avec une liste des vues individuelles comme illustré dans [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Les enfants doivent dériver de `View` mais peut inclure d’autres `StackLayout` objets.

## <a name="scrolling-content"></a>Faire défiler un contenu

Si un `StackLayout` contient trop grand nombre d’enfants à afficher sur une page, vous pouvez placer le `StackLayout` dans un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) pour permettre le défilement.

Définir le [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) propriété du `ScrollView` à la vue que vous souhaitez faire défiler. Il s’agit souvent un `StackLayout`, mais il peut être n’importe quelle vue.

Définir le [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) propriété du `ScrollView` à un membre de la [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) propriété, [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical), [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal), ou [ `Both` ](xref:Xamarin.Forms.ScrollOrientation.Both). La valeur par défaut est `Vertical`. Si le contenu d’un `ScrollView` est un `StackLayout`, les deux orientations doivent être cohérentes.

Le [ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) exemple illustre l’utilisation de `ScrollView` et `StackLayout` pour afficher les couleurs disponibles. L’exemple montre également comment utiliser la réflexion .NET pour obtenir toutes les propriétés statiques publiques et les champs de la `Color` structure sans avoir à les répertorier explicitement.

## <a name="the-expands-option"></a>L’option se développe

Lorsqu’un `StackLayout` piles ses enfants, chacun d’eux occupe une position particulière au sein de la hauteur totale de la `StackLayout` qui varie selon la taille de l’enfant et les paramètres de son `HorizontalOptions` et `VerticalOptions` propriétés. Ces propriétés sont attribuées aux valeurs de type [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

Le `LayoutOptions` structure définit deux propriétés :

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) du type énumération [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) avec quatre membres, [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), et [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) de type `bool`

Pour votre commodité, le `LayoutOptions` structure définit également les huit champs en lecture seule statiques de type `LayoutOptions` qui englobent toutes les combinaisons des propriétés de l’instance de deux :

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

La discussion suivante implique un `StackLayout` avec une orientation verticale par défaut. Horizontal `StackLayout` est analogue.

Pour un vertical `StackLayout`, le `HorizontalOptions` paramètre détermine la façon dont un enfant est positionné horizontalement dans la largeur de la `StackLayout`. Un `Alignment` paramètre `Start`, `Center`, ou `End` provoque l’enfant d’être horizontalement sans contrainte. L’enfant détermine sa propre largeur et est positionné sur la gauche, centre ou à droite de la `StackLayout`. Le `Fill` option entraîne l’enfant à limiter horizontalement et remplit la largeur de la `StackLayout`.

Pour un vertical `StackLayout`, chaque enfant est verticalement sans contrainte et obtient un vertical de l’emplacement en fonction de la hauteur de l’enfant, auquel cas la `VerticalOptions` paramètre n’est pas pertinent.

Si vertical `StackLayout` lui-même est sans contrainte&mdash;autrement dit si son `VerticalOptions` paramètre est `Start`, `Center`, ou `End`, puis la hauteur de la `StackLayout` est la hauteur totale de ses enfants.

Toutefois, si la verticale `StackLayout` est contraint verticalement&mdash;si son `VerticalOptions` paramètre est `Fill` &mdash;puis la hauteur de la `StackLayout` sera la hauteur de son conteneur, ce qui peut être supérieure au total hauteur de ses enfants. Si tel est le cas, et si au moins un enfant a un `VerticalOptions` avec un `Expands` indicateur de `true`, puis l’espace supplémentaire dans le `StackLayout` est répartis équitablement entre tous les enfants avec un `Expands` indicateur de `true`. La hauteur totale des enfants sera ensuite égale à la hauteur de la `StackLayout`et le `Alignment` dans le cadre de la `VerticalOptions` paramètre détermine la façon dont l’enfant est positionnée verticalement dans son emplacement.

Cela est illustré dans le [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) exemple.

## <a name="frame-and-boxview"></a>Frame et BoxView

Ces deux vues rectangulaires servent souvent à des fins de présentation.

Le [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) affiche un cadre rectangulaire autour d’une autre vue, ce qui peut être une disposition comme `StackLayout`. `Frame` hérite une [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) propriété à partir de [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) que vous définissez à la vue à afficher dans le `Frame`. Le `Frame` est transparent par défaut. Définissez les trois propriétés suivantes pour personnaliser l’apparence de l’image :

- Le [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) propriété pour le rendre visible. Il est courant de définir `OutlineColor` à `Color.Accent` lorsque vous ne connaissez pas le jeu de couleurs sous-jacent.
- Le [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) propriété peut être définie sur `true` pour afficher une ombre noir sur les appareils iOS.
- Définir le [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriété un `Thickness` le contenu de valeur à laisser un espace entre l’image et l’image. La valeur par défaut est de 20 unités de tous les côtés.

Le `Frame` a la valeur par défaut `HorizontalOptions` et `VerticalOptions` les valeurs de `LayoutOptions.Fill`, ce qui signifie que le `Frame` remplira son conteneur. Avec d’autres paramètres, la taille de la `Frame` est basée sur la taille de son contenu.

Le `Frame` est illustrée dans le [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) exemple.

Le [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) affiche une zone rectangulaire de la couleur spécifiée par son [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) propriété.

Si le `BoxView` est limité (son `HorizontalOptions` et `VerticalOptions` propriétés ont leurs paramètres par défaut de `LayoutOptions.Fill`), le `BoxView` remplit l’espace disponible pour lui. Si le `BoxView` n’est pas limitée (avec `HorizontalOptions` et `LayoutOptions` paramètres de `Start`, `Center`, ou `End`), il a une dimension par défaut du carré de 40 unités. Un `BoxView` peut être limité dans une dimension et sans contraintes dans l’autre.

Souvent, vous devez définir le le [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) et [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) propriétés de `BoxView` afin de lui donner une taille spécifique. Ceci est illustré par la [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) exemple.

Vous pouvez utiliser plusieurs instances de `StackLayout` pour combiner un `BoxView` et plusieurs `Label` instances dans un `Frame` pour afficher une couleur particulière, puis placez chacun de ces vues dans une `StackLayout` dans un `ScrollView` pour créer l’attrayante liste de couleurs affichées dans le [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) exemple :

[![Capture d’écran de triple des blocs de couleur](images/ch04fg11-small.png "liste de couleurs")](images/ch04fg11-large.png#lightbox "liste de couleurs")

## <a name="a-scrollview-in-a-stacklayout"></a>Un ScrollView dans un StackLayout ?

Placer un `StackLayout` dans un `ScrollView` est commun, mais en ajoutant un `ScrollView` dans un `StackLayout` est également parfois commode. En théorie, cela ne doit pas être possible, car les enfants d’une verticale `StackLayout` sont verticalement sans contrainte. Mais un `ScrollView` doit donc être contraint verticalement. Il doit disposer d’une hauteur spécifique afin qu’elle puisse ensuite déterminer la taille de son enfant pour le défilement.

L’astuce consiste à donner le `ScrollView` enfant de la `StackLayout` un `VerticalOptions` paramètre `FillAndExpand`. Cela est illustré dans le [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) exemple.

Le **BlackCat** exemple montre également comment définir et accéder aux ressources de programme qui sont incorporés dans la bibliothèque de classes Portable (PCL). Cela est également possible avec les projets de ressource partagé (SAP) mais le processus est un peu plus compliqué, comme le [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) montre des exemples.



## <a name="related-links"></a>Liens associés

- [Chapitre 4 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Exemples de chapitre 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Exemples de chapitre 4 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
