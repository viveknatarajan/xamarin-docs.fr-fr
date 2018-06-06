---
title: Contraintes de disposition par programmation dans Xamarin.iOS
description: Ce guide présente les contraintes de disposition automatique de travailler avec iOS dans le code c# au lieu de les créer dans le concepteur iOS.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a40a3c66369902d2d6f8dbee5a6a7e9bad8a9e05
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790599"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Contraintes de disposition par programmation dans Xamarin.iOS

_Ce guide présente les contraintes de disposition automatique de travailler avec iOS dans le code c# au lieu de les créer dans le concepteur iOS._

Mise en page automatique (également appelé « disposition adaptive ») est une approche de conception réactive. Contrairement au système de disposition transitoire, où les emplacement de chaque élément sont codé en dur à un point sur l’écran, la mise en page automatique est sur *relations* -la position des éléments par rapport aux autres éléments sur l’aire de conception. Au cœur de la mise en page automatique est l’idée des contraintes ou des règles qui définissent le placement d’un élément ou un ensemble d’éléments dans le contexte des autres éléments sur l’écran. Étant donné que les éléments ne sont pas liés à un emplacement spécifique sur l’écran, les contraintes permettent de créer une disposition adaptive convenable sur différentes tailles d’écran et les orientations de l’appareil.

En général, lorsque vous travaillez avec disposition automatique dans iOS, vous utiliserez le concepteur iOS graphiquement placer des contraintes de mise en page sur vos éléments de l’interface utilisateur. Toutefois, il peut arriver lorsque vous devez créer et appliquer des contraintes en code c#. Par exemple, utilise dynamiquement la création des éléments d’interface utilisateur ajoutés à un `UIView`.

Ce guide vous explique comment créer et utiliser des contraintes à l’aide de code c# au lieu de les créer graphiquement dans le concepteur iOS.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Création de contraintes par programme

Comme indiqué ci-dessus, en général, vous allez travailler avec des contraintes de mise en page automatique dans le concepteur iOS. Dans les cas que vous n’avez pas à créer vos contraintes par programme, vous disposez de choisir entre trois options :

* [Mise en page ancres](#Layout-Anchors) -cette API permet d’accéder aux propriétés d’ancrage (tel que `TopAnchor`, `BottomAnchor` ou `HeightAnchor`) des éléments de l’interface utilisateur en cours de la contrainte.
* [Contraintes de disposition](#Layout-Constraints) -vous pouvez créer des contraintes directement à l’aide de la `NSLayoutConstraint` classe.
* [Mise en forme visuelle](#Visual-Format-Language) -fournit une illustration ASCII comme méthode pour définir vos contraintes.

Les sections suivantes passe en revue chaque option de détail.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Points d’ancrage de la mise en page

À l’aide de la `NSLayoutAnchor` (classe), vous avez une interface fluent pour la création de contraintes basés sur les propriétés d’ancrage des éléments de l’interface utilisateur en cours de la contrainte. Par exemple, d’un contrôleur d’affichage supérieure et inférieure repères expose le `TopAnchor`, `BottomAnchor` et `HeightAnchor` d’ancrage propriétés pendant une vue expose les propriétés de bord, centre, la taille et de ligne de base.

> [!IMPORTANT]
> En plus de l’ensemble standard de propriétés d’ancrage, les vues iOS incluent également le `LayoutMarginsGuides` et `ReadableContentGuide` propriétés. Ces propriétés présentent des `UILayoutGuide` objets pour travailler avec des marges de la vue et lisibles content guides respectivement.

Mise en page ancres fournissent plusieurs méthodes pour la création de contraintes dans un format compact facile à lire :

- **ConstraintEqualTo** -définit une relation où `first attribute = second attribute + [constant]` avec éventuellement fourni `constant` valeur de décalage.
- **ConstraintGreaterThanOrEqualTo** -définit une relation où `first attribute >= second attribute + [constant]` avec éventuellement fourni `constant` valeur de décalage.
- **ConstraintLessThanOrEqualTo** -définit une relation où `first attribute <= second attribute + [constant]` avec éventuellement fourni `constant` valeur de décalage.

Exemple :

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Une contrainte de mise en page de type peut être exprimée comme une expression linéaire. Prenons l’exemple suivant :

[![](programmatic-layout-constraints-images/graph01.png "Une contrainte de mise en page exprimée sous la forme d’une expression linéaire")](programmatic-layout-constraints-images/graph01.png#lightbox)

Qui est converti à la ligne de code c# à l’aide de points d’ancrage de la mise en page suivante :

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Où les parties du code c# correspondent aux parties de l’équation données de la façon suivante :

|Équation|Code|
|---|---|
|Élément 1|PurpleView|
|Attribut 1|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|Multiplicateur|Valeur par défaut est 1.0 donc ne pas spécifié|
|Élément 2|OrangeView|
|Attribut 2|TrailingAnchor|
|Constante|10.0|

En plus de fournir uniquement les paramètres qui sont nécessaires pour résoudre une équation de contrainte de mise en page donnée, chacune des méthodes d’ancrage de mise en page appliquer la sécurité de type des paramètres passés à ceux-ci. Contrainte donc horizontal ancre comme `LeadingAnchor` ou `TrailingAnchor` ne peut être utilisé avec un autre point d’ancrage horizontal types et des multiplicateurs sont uniquement fournies aux contraintes de taille.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Contraintes de disposition

Vous pouvez ajouter manuellement des contraintes de mise en page automatique en créant directement un `NSLayoutConstraint` en code c#. Contrairement à l’utilisation de points d’ancrage de la mise en page, vous devez spécifier une valeur pour chaque paramètre, même si elle aura aucun effet sur la contrainte est définie. Par conséquent, vous obtiendrez produisant une quantité considérable de difficile à lire, un code réutilisable. Exemple :

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Où les `NSLayoutAttribute` enum définit la valeur pour les marges de la vue et correspond à la `LayoutMarginsGuide` propriétés, telles que `Left`, `Right`, `Top` et `Bottom` et `NSLayoutRelation` enum définit la relation qui sera créé entre les attributs donnés en tant que `Equal`, `LessThanOrEqual` ou `GreaterThanOrEqual`.

À la différence avec l’API d’ancrage de la mise en page, le `NSLayoutConstraint` méthodes de création de ne pas mettre en surbrillance les aspects importants d’une contrainte particulière et il n’existe aucune compilation vérifications effectuées sur la contrainte de temps. Par conséquent, il est facile de construire une contrainte non valide qui lève une exception lors de l’exécution.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Langue de Format visuel

La langue de Format Visual vous permet de définir des contraintes à l’aide d’art ASCII comme les chaînes qui fournissent une représentation visuelle de la contrainte en cours de création. Cela présente les avantages et les inconvénients suivants :

- La langue de Format Visual impose la création de contraintes valides uniquement.
 - Disposition automatique génère des contraintes sur la console à l’aide de la langue de Format visuel pour les messages de débogage seront présenteront comme le code utilisé pour créer la contrainte.
 - La langue de Format Visual vous permet de créer plusieurs contraintes en même temps avec une expression très compacte.
 - Comme il n’existe aucune validation de compilation côté des chaînes de Format visuelle, les problèmes ne peuvent être détectés lors de l’exécution.
 - Étant donné que la langue de Format Visual privilégie la visualisation sur l’exhaustivité de que certains types de contrainte ne peut pas être créés avec lui (tels que les rapports).

Les étapes suivantes s’affiche lors de l’utilisation de la langue de Format visuel pour créer une contrainte :

1. Créer un `NSDictionary` qui contient les objets de vue et des Guides de mise en page et une clé de chaîne qui sera utilisée lors de la définition des formats.
2. Vous pouvez également créer un `NSDictionary` qui définit un jeu de clés et valeurs (`NSNumber`) utilisé en tant que la valeur de constante pour la contrainte.
3. Créer la chaîne de format à disposition une seule colonne ou une ligne d’éléments.
4. Appelez le `FromVisualFormat` méthode de la `NSLayoutConstraint` classe génère les contraintes.
5. Appelez le `ActivateConstraints` méthode de la `NSLayoutConstraint` classe pour activer et appliquer les contraintes.

Par exemple, pour créer un début et une contrainte de fin dans la langue de Format visuel, vous pouvez utiliser les éléments suivants :

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Étant donné que la langue de Format Visual crée toujours zéro contraintes point pour les marges de l’affichage de parent lors de l’utilisation de l’espacement par défaut, ce code génère des résultats identiques aux exemples présentés.

Pour les conceptions d’interface utilisateur plus complexes, tels que plusieurs vues enfants sur une seule ligne, la langue de Format Visual Spécifie l’espacement horizontal et l’alignement vertical. Comme dans l’exemple ci-dessus, il spécifie où la `AlignAllTop` `NSLayoutFormatOptions` aligne toutes les vues dans une ligne ou colonne vers leurs tops.

Consultez d’Apple [annexe de langue de Format Visual](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) pour obtenir des exemples de spécification de contraintes courantes et la grammaire de chaîne de Format Visual.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Ce guide de présentation Création et utilisation des contraintes de mise en page automatique en c# plutôt que de les créer graphiquement dans le concepteur iOS. Tout d’abord, il se présentait à l’aide de points d’ancrage de la mise en page (`NSLayoutAnchor`) pour gérer la disposition automatique. Ensuite, il vous a montré comment travailler avec des contraintes de mise en page (`NSLayoutConstraint`). Enfin, elle présente à l’aide de la langue de Format Visual pour la disposition automatique.

## <a name="related-links"></a>Liens associés

- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
- [procédure pas à pas de contrôles possible d’iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple - création par programme de contraintes](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple - Format visuel langage annexe](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
