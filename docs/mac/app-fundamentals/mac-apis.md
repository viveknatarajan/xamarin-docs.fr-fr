---
title: macOS API pour les développeurs Xamarin.Mac
description: Ce document décrit comment lire des sélecteurs Objective-C et rechercher leurs méthodes c# correspondants dans une application Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/02/2017
ms.openlocfilehash: c387bbead1ac56d7f4c4c05a79c430302e50aec1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668281"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS API pour les développeurs Xamarin.Mac

## <a name="overview"></a>Vue d'ensemble

Pour une grande partie de votre temps à développer avec Xamarin.Mac, vous pouvez donc réfléchir, lire et écrire en c#, sans se soucier de bien avec les API Objective-C sous-jacente. Cependant, parfois, allez à lire la documentation de l’API d’Apple, traduire une réponse de Stack Overflow à une solution à votre problème, ou que vous comparer à un exemple existant.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Lecture suffisamment Objective-C pour être dangereux

Parfois, il sera nécessaire lire une définition d’Objective-C ou méthode appeler et qui les traduire la méthode c# équivalente. Nous allons jeter un coup de œil à une définition de fonction Objective-C et décomposer les différentes parties. Cette méthode (un *sélecteur* en Objective-C) sont accessibles sur `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

La déclaration permettre être lues de gauche à droite :

- Le `-` préfixe signifie qu’il est une méthode d’instance (non statique). + signifie que c’est une méthode de classe (statique)
- `(BOOL)` est le type de retour (bool en c#)
- `canDragRowsWithIndexes` est la première partie du nom.
- `(NSIndexSet *)rowIndexes` est le premier paramètre et dont il a le type. Le premier paramètre est au format : `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` est le deuxième paramètre et son type. Chaque paramètre après le premier est le format : `selectorPart:(Type) pararmName`
- Le nom complet de ce sélecteur de message est : `canDragRowsWithIndexes:atPoint:`. Remarque la `:` à la fin - il est important.
- La liaison réelle de Xamarin.Mac c# est : `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Cet appel sélecteur peut être lu de la même façon :

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- L’instance `v` rencontre son `canDragRowsWithIndexes:atPoint` sélecteur appelée avec deux paramètres, `set` et `point`, transmis.
- En c#, l’appel de méthode ressemble à ceci : `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Recherche le membre c# pour un sélecteur donné

Maintenant que vous avez trouvé le sélecteur d’Objective-C que vous devez appeler, l’étape suivante qui mappe vers le membre équivalent c#. Il existe quatre approches que vous pouvez essayer (poursuivre le `NSTableView CanDragRows` exemple) :

1. Utilisez la liste de saisie semi-automatique pour rechercher rapidement un élément du même nom. Étant donné que nous savons qu’il est une instance de `NSTableView` vous pouvez taper :

    - `NSTableView x;`
    - `x.` [ctrl + espace si la liste n’apparaît pas).
    - `CanDrag` [enter]
    - Avec le bouton droit de la méthode, atteindre la déclaration pour ouvrir le navigateur de l’Assembly dans lequel vous pouvez comparer la `Export` d’attribut pour le sélecteur en question

2. Rechercher la liaison de la classe entière. Étant donné que nous savons qu’il est une instance de `NSTableView` vous pouvez taper :

    - `NSTableView x;`
    - Avec le bouton droit `NSTableView`, accédez à la déclaration de l’Explorateur d’Assembly
    - Recherchez le sélecteur en question

3. Vous pouvez utiliser la [documentation en ligne API Xamarin.Mac](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) .

4. Miguel fournit une vue de « Rosetta Pierre » des APIs Xamarin.Mac [ici](https://tirania.org/tmp/rosetta.html) que vous pouvez rechercher via une API donnée. Si votre API n’est pas AppKit ou macOS spécifique, vous pouvez le trouver là.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
