---
title: Convertisseurs rapide de Xamarin.Forms
description: Cet article présente des renderers rapides qui réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms sur Android en aplanissant la hiérarchie des contrôles natifs obtenue.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 861d9e3f898dcd61015d9aca27ae66c3fe72d1a9
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970722"
---
# <a name="xamarinforms-fast-renderers"></a>Convertisseurs rapide de Xamarin.Forms

En règle générale, la plupart des convertisseurs de contrôle d’origine sur Android se composent de deux vues :

- Contrôle natif, tel qu’un `Button` ou `TextView`.
- Un conteneur `ViewGroup` qui gère le travail de mise en page, de gestion des mouvements ou autres tâches.

Toutefois, cette approche présente une implication de performances dans la mesure où deux vues sont créées pour chaque contrôle logique, ce qui entraîne une arborescence visuelle plus complexe qui nécessite davantage de mémoire et bien plus encore pour effectuer le rendu à l’écran de traitement.

Renderers rapides réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms dans une vue unique. Par conséquent, au lieu de créer des deux vues et les ajouter à l’arborescence de la vue, seul l’un est créé. Cela améliore les performances en créant moins d’objets, ce qui implique une arborescence de vue moins complexe, et moins l’utilisation de la mémoire (ce qui entraîne également moins pauses du garbage collection).

Renderers rapides sont disponibles pour les contrôles suivants dans Xamarin.Forms sur Android :

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

Fonctionnellement, ces convertisseurs rapides ne sont pas différentes pour les convertisseurs hérités. À partir de Xamarin.Forms 4.0 et versions ultérieures, toutes les applications ciblant `FormsAppCompatActivity` utilisera ces convertisseurs rapides par défaut. Convertisseurs pour tous les nouveaux contrôles, y compris [ `ImageButton` ](xref:Xamarin.Forms.ImageButton) et [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), utilisez l’approche de convertisseur rapide.

Améliorations des performances lors de l’utilisation des renderers rapides varient pour chaque application, en fonction de la complexité de la mise en page. Par exemple, les améliorations des performances de x2 sont possibles lors du défilement d’un [ `ListView` ](xref:Xamarin.Forms.ListView) contenant des milliers de lignes de données, où les cellules de chaque ligne sont constitués de contrôles qui utilisent des renderers rapides, ce qui entraîne des visiblement défilement plus lisse.

> [!NOTE]
> Renderers personnalisés peuvent être créés pour des renderers rapides à l’aide de la même approche que celles utilisées pour les convertisseurs hérités. Pour plus d’informations, consultez [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="backwards-compatibility"></a>Compatibilité descendante

Renderers rapides peuvent être remplacées par les approches suivantes :

1. Activer les convertisseurs hérités en ajoutant la ligne suivante de code à votre `MainActivity` classe avant d’appeler `Forms.Init`:

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. À l’aide des convertisseurs personnalisés qui ciblent les convertisseurs hérités. Les renderers personnalisés existants continueront à fonctionner avec les convertisseurs hérités.
1. En spécifiant un autre `View.Visual`, tel que `Material`, qui utilise différents convertisseurs. Pour plus d’informations sur Visual de matériau, consultez [Xamarin.Forms matériau Visual](~/xamarin-forms/user-interface/visual/material-visual.md).

## <a name="related-links"></a>Liens connexes

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
