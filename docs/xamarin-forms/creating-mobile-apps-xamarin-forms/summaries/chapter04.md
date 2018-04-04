---
title: Résumé du chapitre 4. Défilement de la pile
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09a9d21b7979e0eb3f12d3b1207f2185e059f65c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Résumé du chapitre 4. Défilement de la pile

Ce chapitre est principalement consacré à introduit le concept de *disposition*, qui est le terme général pour les classes et les techniques Xamarin.Forms utilise pour organiser l’affichage de plusieurs vues sur la page.

Mise en page implique plusieurs classes qui dérivent de [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) et [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). Ce chapitre se concentre sur [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

A également introduit dans ce chapitre sont les [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), et [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) classes.

## <a name="stacks-of-views"></a>Piles de vues

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) dérive de `Layout<View>` et hérite d’un [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) propriété de type `IList<View>`. Vous ajoutez plusieurs éléments d’affichage à cette collection, et `StackLayout` les affiche dans une pile horizontale ou verticale.

Définir le [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) propriété du `StackLayout` à un membre de la [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) énumération, soit [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) ou [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). La valeur par défaut est `Vertical`.

Définir le [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) propriété du `StackLayout` à un `double` valeur pour spécifier un espacement entre les enfants. La valeur par défaut est 6.

Dans le code, vous pouvez ajouter des éléments à la `Children` collection de `StackLayout` dans un `for` ou `foreach` boucle comme illustré dans le [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) exemples ou vous pouvez initialiser la `Children` collection avec une liste des vues individuelles comme cela est indiqué dans [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Les enfants doivent dériver de `View` , mais peut inclure d’autres `StackLayout` objets.

## <a name="scrolling-content"></a>Contenu de défilement

Si un `StackLayout` contient trop d’enfants à afficher sur une page, vous pouvez placer le `StackLayout` dans un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) pour permettre le défilement.

Définir le [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) propriété du `ScrollView` à la vue que vous souhaitez faire défiler. Il s’agit souvent un `StackLayout`, mais il peut être n’importe quelle vue.

Définir le [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) propriété du `ScrollView` à un membre de la [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) propriété, [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), ou [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/). La valeur par défaut est `Vertical`. Si le contenu d’un `ScrollView` est un `StackLayout`, les deux orientations doivent être cohérentes.

Le [ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) exemple illustre l’utilisation de `ScrollView` et `StackLayout` pour afficher les couleurs disponibles. L’exemple montre également comment utiliser la réflexion .NET pour obtenir toutes les propriétés statiques publiques et les champs de la `Color` structure sans avoir à les répertorier explicitement.

## <a name="the-expands-option"></a>L’option se développe

Quand un `StackLayout` piles de ses enfants, chacun d’eux occupe un emplacement spécifique au sein de la hauteur totale de la `StackLayout` qui varie selon la taille de l’enfant et les paramètres de son `HorizontalOptions` et `VerticalOptions` propriétés. Ces propriétés sont affectées des valeurs de type [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

Le `LayoutOptions` structure définit deux propriétés :

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) du type énumération [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) avec quatre membres, [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), et [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) de type `bool`

Pour votre commodité, le `LayoutOptions` structure définit également les huit champs en lecture seule statiques de type `LayoutOptions` qui englobent toutes les combinaisons de l’instance de deux propriétés :

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

La discussion suivante implique un `StackLayout` avec une orientation verticale par défaut. Horizontal `StackLayout` est analogue.

Pour une verticale `StackLayout`, le `HorizontalOptions` détermine comment un enfant est positionné horizontalement dans la largeur de la `StackLayout`. Un `Alignment` paramètre `Start`, `Center`, ou `End` provoque l’enfant d’être horizontalement sans contrainte. L’enfant détermine son propre largeur et est positionné à gauche, centre ou droite de la `StackLayout`. Le `Fill` option entraîne l’enfant soit horizontalement contraint et remplit la largeur de la `StackLayout`.

Pour une verticale `StackLayout`, chaque enfant est verticalement sans contrainte et obtient un vertical emplacement en fonction de la hauteur de l’enfant, auquel cas la `VerticalOptions` paramètre n’est pas pertinent.

Si vertical `StackLayout` lui-même n’est pas limitée&mdash;autrement dit si son `VerticalOptions` paramètre est `Start`, `Center`, ou `End`, puis la hauteur de la `StackLayout` correspond à la hauteur totale de ses enfants.

Toutefois, si la verticale `StackLayout` est contraint verticalement&mdash;si son `VerticalOptions` paramètre est `Fill` &mdash;puis la hauteur de la `StackLayout` sera la hauteur de son conteneur, ce qui peut être supérieure au total hauteur de ses enfants. Si tel est le cas, et si au moins un enfant a un `VerticalOptions` avec un `Expands` indicateur de `true`, puis l’espace supplémentaire dans le `StackLayout` est réparti également de tous les enfants avec un `Expands` indicateur de `true`. La hauteur totale des enfants puis est égale à la hauteur de la `StackLayout`et le `Alignment` dans le cadre de la `VerticalOptions` détermine la façon dont l’enfant est positionné verticalement dans son emplacement.

Cela est illustré dans le [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) exemple.

## <a name="frame-and-boxview"></a>Frame et BoxView

Ces deux vues rectangulaires sont souvent utilisés à des fins de présentation.

Le [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) affiche un cadre rectangulaire autour d’un autre affichage, qui peut être une disposition comme `StackLayout`. `Frame` hérite une [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) propriété à partir de [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) que vous définissez à la vue à afficher dans le `Frame`. Le `Frame` est transparent par défaut. Définir les trois propriétés suivantes pour personnaliser l’apparence de l’image :

- Le [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) propriété pour le rendre visible. Il est courant de définir `OutlineColor` à `Color.Accent` lorsque vous ne connaissez pas le modèle de couleurs sous-jacent.
- Le [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) propriété peut être définie sur `true` pour afficher une ombre noire sur les appareils iOS.
- Définir le [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriété à un `Thickness` à laisser un espace entre l’image et la valeur du contenu. La valeur par défaut est de 20 unités sur tous les côtés.

Le `Frame` a la valeur par défaut `HorizontalOptions` et `VerticalOptions` les valeurs de `LayoutOptions.Fill`, ce qui signifie que le `Frame` remplit son conteneur. Avec d’autres paramètres, la taille de la `Frame` est basée sur la taille de son contenu.

Le `Frame` est illustré dans le [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) exemple.

Le [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) affiche une zone rectangulaire de couleur spécifié par son [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) propriété.

Si le `BoxView` est limitée (son `HorizontalOptions` et `VerticalOptions` propriétés ont leurs paramètres par défaut de `LayoutOptions.Fill`), le `BoxView` remplit l’espace disponible pour celui-ci. Si le `BoxView` n’est pas limitée (avec `HorizontalOptions` et `LayoutOptions` paramètres de `Start`, `Center`, ou `End`), il a une dimension par défaut du carré de 40 unités. A `BoxView` peuvent être limitées dans une dimension et sans contrainte dans l’autre.

Souvent, vous devez définir le le [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) et [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) propriétés de `BoxView` afin de lui donner une taille spécifique. Ceci est illustré par le [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) exemple.

Vous pouvez utiliser plusieurs instances de `StackLayout` pour combiner un `BoxView` et plusieurs `Label` les instances en un `Frame` pour afficher une couleur spécifique, puis placer chacune de ces vues dans une `StackLayout` dans un `ScrollView` pour créer le liste des couleurs affichées dans le [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) exemple :

[![Capture d’écran de triple des blocs de couleur](images/ch04fg11-small.png "liste de couleurs")](images/ch04fg11-large.png#lightbox "liste de couleurs")

## <a name="a-scrollview-in-a-stacklayout"></a>Un ScrollView dans un StackLayout ?

Placer un `StackLayout` dans un `ScrollView` est commune, mais la mise un `ScrollView` dans un `StackLayout` est également parfois plus pratique. En théorie, cela ne doit pas être possible, car les enfants d’un vertical `StackLayout` sont verticalement sans contrainte. Mais un `ScrollView` doit être limitée verticalement. Il doit comporter une hauteur spécifique afin qu’il peut ensuite déterminer la taille de son enfant pour le défilement.

L’astuce consiste à donner le `ScrollView` enfant de la `StackLayout` un `VerticalOptions` paramètre `FillAndExpand`. Cela est illustré dans le [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) exemple.

Le **BlackCat** exemple montre également comment définir et accéder aux ressources de programme qui sont incorporés dans la bibliothèque de classe Portable (PCL). Cela peut être obtenu avec les projets de ressources partagé (SAP), mais le processus est un peu plus complexe, comme le [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) illustre.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 4 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Exemples de chapitre 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Exemples de chapitre 4 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
