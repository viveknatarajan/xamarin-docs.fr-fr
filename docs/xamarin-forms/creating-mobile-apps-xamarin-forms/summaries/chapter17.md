---
title: Résumé du chapitre 17. Maîtrise de la grille
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 17. Maîtrise de la grille'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b71859d0848d7bf790b3cc4beddc67a5ea86d340
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935476"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Résumé du chapitre 17. Maîtrise de la grille

Le [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) est un mécanisme de disposition puissants qui réorganise ses enfants en lignes et colonnes de cellules. Le code HTML similaire à la différence `table` élément, le `Grid` est destiné uniquement à des fins de disposition plutôt que de présentation.

## <a name="the-basic-grid"></a>La grille de base

`Grid` dérive de [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), qui définit un [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) propriété qui `Grid` hérite. Vous pouvez remplir cette collection dans XAML ou de code.

### <a name="the-grid-in-xaml"></a>La grille dans XAML

La définition d’un `Grid` dans XAML commence généralement par remplissage le [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) et [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) collections de la `Grid` avec [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) et [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) objets. Voici comment vous établissez le nombre de lignes et colonnes de la `Grid`et leurs propriétés.

`RowDefinition` a un [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) propriété et `ColumnDefinition` a un [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) propriété, tous deux de type [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), une structure.

Dans XAML, le [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) convertit les chaînes de texte simples dans `GridLength` valeurs. Dans les coulisses, le [ `GridLength` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) crée le `GridLength` valeur basée sur un nombre et une valeur de type [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), une énumération avec trois membres :

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; la largeur ou la hauteur est spécifiée en unités indépendantes du périphérique (il s’agit d’un nombre en XAML)
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; la largeur ou hauteur est redimensionnée automatiquement selon le contenu de la cellule (« Auto » dans XAML)
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; restes hauteur ou la largeur est allouée proportionnellement (un nombre avec «\*», appelé *en étoile*, dans XAML)

Chaque enfant de le `Grid` doit également être affecté une ligne et colonne (explicitement ou implicitement). Ligne s’étend et étendues de colonne sont facultatifs. Ceux-ci sont spécifiées à l’aide des propriétés pouvant être liées jointes &mdash; propriétés qui sont définies par le `Grid` mais définies sur les enfants de le `Grid`. `Grid` définit quatre propriétés pouvant être liées attachées statiques :

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; la ligne de base zéro ; valeur par défaut est 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; la colonne de base zéro ; valeur par défaut est 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; le nombre de lignes enfant s’étend sur ; valeur par défaut est 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; le nombre de colonnes enfant s’étend sur ; valeur par défaut est 1

Dans le code, un programme peut utiliser les huit méthodes statiques pour définir et obtenir ces valeurs :

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) et [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

Dans XAML, vous utilisez les attributs suivants pour la définition de ces valeurs :

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

Le [ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) exemple montre comment créer et initialiser un `Grid` dans XAML.

Le `Grid` hérite le [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriété `Layout` et définit deux propriétés supplémentaires qui fournissent l’espacement entre les lignes et colonnes :

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) a la valeur par défaut de 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) a la valeur par défaut de 6

Le `RowDefinitions` et `ColumnDefinitions` collections ne sont pas strictement obligatoires. S’il est absent, le `Grid` crée des lignes et des colonnes pour le `Grid` enfants et leur donne toutes les une valeur par défaut `GridLength` de «\*» (en étoile).

### <a name="the-grid-in-code"></a>La grille dans le code

Le [ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) exemple montre comment créer et remplir un `Grid` dans le code. Vous pouvez définir les propriétés jointes pour chaque enfant directement ou indirectement en appelant supplémentaires `Add` méthodes telles que [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) défini par le [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) interface.

### <a name="the-grid-bar-chart"></a>Le graphique à barres grille

Le [ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) exemple montre comment ajouter plusieurs `BoxView` éléments à un `Grid` à l’aide de la majeure partie [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) (méthode). Par défaut, ces `BoxView` éléments ont la même largeur. La hauteur de chaque `BoxView` peut ensuite être contrôlé et ressemble à un graphique à barres.

Le `Grid` dans le **GridBarChart** exemple partages un `AbsoluteLayout` parent avec initialement invisible `Frame`. Le programme définit également un `TapGestureRecognizer` sur chaque `BoxView` à utiliser le `Frame` pour afficher des informations sur la barre drainée.

### <a name="alignment-in-the-grid"></a>Alignement de la grille

Le [ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) exemple montre comment utiliser le `VerticalOptions` et `HorizontalOptions` propriétés afin d’aligner les enfants dans un `Grid` cellule.

Le [ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) exemple tout aussi espaces `Button` éléments centrés dans `Grid` cellules.

### <a name="cell-dividers-and-borders"></a>Les bordures et les séparateurs de cellule

Le `Grid` n’inclut pas une fonctionnalité qui dessine les bordures ou des séparateurs de cellule. Toutefois, vous pouvez créer vos propres.

Le [ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) montre comment définir des lignes supplémentaires et colonne en particulier pour dynamique `BoxView` éléments afin de reproduire les lignes de démarcation.

Le [ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) programme ne crée pas de toutes les cellules, mais au lieu de cela aligne `BoxView` éléments dans chaque cellule pour imiter une bordure de cellule.

## <a name="almost-real-life-grid-examples"></a>Exemples de grille presque réel

Le [ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) exemple utilise un `Grid` pour afficher un pavé numérique :

[![Capture d’écran triple de grille du pavé numérique](images/ch17fg12-small.png "pavé grille")](images/ch17fg12-large.png#lightbox "grille du pavé numérique")

### <a name="responding-to-orientation-changes"></a>Répondre aux changements d’orientation

Le `Grid` peut aider à structurer un programme pour répondre aux changements d’orientation. Le [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) exemple illustre une technique qui déplace un élément entre une deuxième ligne d’un téléphone orienté en mode portrait et la deuxième colonne d’un téléphone en mode paysage.

Initialise le programme `Slider` éléments à une plage de 0 à 255 et utilise les liaisons de données pour afficher la valeur des curseurs au format hexadécimal. Étant donné que le `Slider` valeurs sont virgule flottante et la mise en forme de chaîne de format hexadécimal fonctionne uniquement avec des entiers, de .NET un [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque est utile.



## <a name="related-links"></a>Liens associés

- [Chapitre 17 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Exemples de chapitre 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Grid](~/xamarin-forms/user-interface/layouts/grid.md)
