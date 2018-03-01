---
title: API de Mac
description: "Ce document décrit comment lire les sélecteurs Objective-C et comment rechercher leurs méthodes c# correspondants."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 724edb7d23a5be5d790b43b81486d60c06df60bf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="mac-apis"></a>API de Mac

## <a name="overview"></a>Vue d'ensemble

Pour une grande partie de votre temps à développer avec Xamarin.Mac, vous pouvez considérer, lire et écrire en c#, sans trop vous soucier avec les API sous-jacentes Objective-C. Cependant, parfois, allez à lire la documentation de l’API à partir d’Apple, traduire une réponse à partir de débordement de pile pour une solution pour votre problème, ou que vous comparer à un exemple existant.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Lors de la lecture suffisamment Objective-C dangereuse

Parfois, il sera nécessaire lire une définition Objective-C ou méthode appeler et à le traduire à la méthode c# équivalente. Nous allons jeter un coup de œil à une définition de fonction Objective-C et consiste à décomposer les éléments. Cette méthode (un *sélecteur* dans Objective-C) se trouvent sur `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

La déclaration peut être lues de gauche à droite :

- Le `-` préfixe signifie qu’il est une méthode d’instance (non statique). + signifie qu’il s’agit d’une méthode de classe (statique)
- `(BOOL)` est le type de retour (bool en c#)
- `canDragRowsWithIndexes` est la première partie du nom.
- `(NSIndexSet *)rowIndexes` est le premier paramètre et avec elle a le type. Le premier paramètre est au format : `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` est le deuxième paramètre et son type. Chaque paramètre après le premier est le format : `selectorPart:(Type) pararmName`
- Le nom complet de ce sélecteur de message est : `canDragRowsWithIndexes:atPoint:`. Remarque la `:` à la fin - il est important.
- La liaison réelle Xamarin.Mac c# est la suivante : `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Cet appel sélecteur peut lire la même façon :

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- L’instance `v` a son `canDragRowsWithIndexes:atPoint` sélecteur appelé avec deux paramètres, `set` et `point`, il est passé.
- En c#, l’appel de méthode ressemble à ceci : `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Recherche le membre c# pour un sélecteur indiquée

Maintenant que vous avez trouvé le sélecteur Objective-C que vous deviez appeler, l’étape suivante qui mappe vers le membre équivalent de c#. Il existe quatre approches que vous pouvez essayer (poursuivre la `NSTableView CanDragRows` exemple) :

1. Utilisez la liste de saisie semi-automatique pour rechercher rapidement un élément portant le même nom. Étant donné que nous savons qu’il est une instance de `NSTableView` que vous pouvez taper :

    - `NSTableView x;`
    - `x.` [ctrl + espace si la liste n’apparaît pas).
    - `CanDrag` [Entrée]
    - Avec le bouton droit de la méthode, accédez à la déclaration pour ouvrir le navigateur de l’Assembly dans lequel vous pouvez comparer le `Export` d’attribut pour le sélecteur en question

2. Recherche la liaison de l’ensemble de la classe. Étant donné que nous savons qu’il est une instance de `NSTableView` que vous pouvez taper :

    - `NSTableView x;`
    - Avec le bouton droit `NSTableView`, accédez à la déclaration de navigateur de l’Assembly
    - Recherchez le sélecteur en question

3. Vous pouvez utiliser la [documentation en ligne de Xamarin.Mac API](https://developer.xamarin.com/api/root/monomac-lib/) .

4. Miguel fournit une vue de « Pierre Rosetta » APIs Xamarin.Mac [ici](http://tirania.org/tmp/rosetta.html) que vous pouvez rechercher par le biais d’une API donnée. Si votre API n’est pas AppKit ou macOS spécifique, vous pouvez le trouver là.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
