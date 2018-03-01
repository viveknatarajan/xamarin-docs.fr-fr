---
title: "Résumé du chapitre 17. La maîtrise de la grille"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c0a184b80b57980c7ae00572517ad52a18b5755c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Résumé du chapitre 17. La maîtrise de la grille

Le [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) est un mécanisme de disposition puissants qui réorganise ses enfants dans les lignes et colonnes de cellules. Contrairement au HTML similaire `table` élément, le `Grid` est uniquement à des fins de disposition plutôt que de présentation.

## <a name="the-basic-grid"></a>La grille de base

`Grid` dérive de [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), qui définit un [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) propriété qui `Grid` hérite. Vous pouvez remplir cette collection en XAML ou de code.

### <a name="the-grid-in-xaml"></a>La grille en XAML

La définition d’un `Grid` dans XAML en général commence avec remplissage la [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) et [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) collections de la `Grid` avec [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) et [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) objets. Voici comment vous établissez le nombre de lignes et colonnes de la `Grid`et leurs propriétés.

`RowDefinition` a un [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) propriété et `ColumnDefinition` a un [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) propriété, les deux de type [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), une structure.

En XAML, le [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) convertit les chaînes de texte simples dans `GridLength` valeurs. En arrière-plan, le [ `GridLength` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) crée le `GridLength` valeur basée sur un nombre et une valeur de type [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), une énumération avec trois membres :

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) & #x 2014 ; la largeur ou la hauteur est spécifiée en unités d’indépendants du périphérique (il s’agit d’un nombre en XAML)
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) & #x 2014 ; la hauteur ou la largeur est redimensionné automatiquement en fonction de contenu de la cellule (« Auto » en XAML)
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) & #x 2014 ; restant hauteur ou la largeur est alloué proportionnellement (un nombre avec «\*», appelé *en étoile*, en XAML)

Chaque enfant de le `Grid` doit également être affecté une ligne et colonne (explicitement ou implicitement). S’étend sur des lignes et des étendues de colonne sont facultatifs. Elles sont spécifiées pour tous l’utilisation de propriétés pouvant être liées & #x 2014 ; attachés propriétés qui sont définies par le `Grid` mais définies sur les enfants de le `Grid`. `Grid` définit les quatre propriétés pouvant être liées attachées statiques :

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) & #x 2014 ; la ligne de base zéro. valeur par défaut est 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) & #x 2014 ; la colonne de base zéro. valeur par défaut est 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) & #x 2014 ; le nombre de lignes enfant s’étend sur ; valeur par défaut est 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) & #x 2014 ; le nombre de colonnes enfants s’étend sur ; valeur par défaut est 1

Dans le code, un programme peut utiliser huit méthodes statiques pour définir et obtenir des valeurs suivantes :

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

En XAML, vous utilisez les attributs suivants pour définir ces valeurs :

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

Le [ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) exemple montre comment créer et initialiser un `Grid` en XAML.

Le `Grid` hérite le [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriété `Layout` et définit deux propriétés supplémentaires qui fournissent l’espacement entre les lignes et colonnes :

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) a la valeur par défaut 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) a la valeur par défaut 6

Le `RowDefinitions` et `ColumnDefinitions` collections ne sont pas strictement obligatoires. S’il est absent, le `Grid` crée des lignes et des colonnes pour le `Grid` enfants et tous les donne une valeur par défaut `GridLength` de «\*» (astérisque).

### <a name="the-grid-in-code"></a>La grille dans le code

Le [ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) exemple montre comment créer et remplir un `Grid` dans le code. Vous pouvez définir les propriétés jointes pour chaque enfant directement ou indirectement en appelant supplémentaires `Add` méthodes telles que [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) définie par le [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) interface.

### <a name="the-grid-bar-chart"></a>Le graphique à barres grille

Le [ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) montre comment ajouter plusieurs `BoxView` éléments à un `Grid` à l’aide de l’ensemble [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) (méthode). Par défaut, ces `BoxView` éléments ont la même largeur. La hauteur de chaque `BoxView` peut ensuite être contrôlé et ressemble à un graphique à barres.

Le `Grid` dans les **GridBarChart** exemple partages un `AbsoluteLayout` parent avec initialement invisible `Frame`. Le programme définit également un `TapGestureRecognizer` sur chaque `BoxView` à utiliser le `Frame` pour afficher des informations sur la barre tapée.

### <a name="alignment-in-the-grid"></a>Alignement de la grille

Le [ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) exemple montre comment utiliser le `VerticalOptions` et `HorizontalOptions` propriétés afin d’aligner les enfants dans un `Grid` cellule.

Le [ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) exemple également espaces `Button` éléments centré dans `Grid` cellules.

### <a name="cell-dividers-and-borders"></a>Les bordures et les séparateurs de cellule

Le `Grid` n’inclut pas une fonctionnalité qui dessine les bordures ou les séparateurs de cellule. Toutefois, vous pouvez créer vos propres.

Le [ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) montre comment définir des lignes supplémentaires et colonne en particulier pour dynamique `BoxView` éléments afin de reproduire les lignes de démarcation.

Le [ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) programme ne crée pas de toutes les cellules supplémentaires, mais au lieu de cela aligne `BoxView` éléments dans chaque cellule afin de reproduire une bordure de cellule.

## <a name="almost-real-life-grid-examples"></a>Exemples de grille presque réels

Le [ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) exemple utilise un `Grid` pour afficher un pavé :

[![Capture d’écran de triple de la grille du pavé](images/ch17fg12-small.png "pavé grille")](images/ch17fg12-large.png "pavé grille")

### <a name="responding-to-orientation-changes"></a>Réponse aux modifications de l’orientation

Le `Grid` peut aider à un programme pour répondre aux modifications de l’orientation de la structure. Le [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) exemple illustre une technique qui déplace un élément entre une deuxième ligne d’un téléphone orienté en mode portrait et la deuxième colonne d’un téléphone en mode paysage.

Le programme initialise `Slider` éléments à une plage de 0 à 255 et utilise les liaisons de données pour afficher la valeur des curseurs en hexadécimal. Étant donné que la `Slider` valeurs sont virgule flottante et la mise en forme de chaîne de format hexadécimal fonctionne uniquement avec des entiers, de .NET un [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque permet.



## <a name="related-links"></a>Liens associés

- [Chapitre 17 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Exemples de chapitre 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Grid](~/xamarin-forms/user-interface/layouts/grid.md)
