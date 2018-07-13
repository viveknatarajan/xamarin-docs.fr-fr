---
title: Convertisseurs rapide de Xamarin.Forms
description: Cet article présente des renderers rapides qui réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms sur Android en aplanissant la hiérarchie des contrôles natifs obtenue.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4b060c703077e140e0f0d2f8c4c2b824c890e8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997116"
---
# <a name="xamarinforms-fast-renderers"></a>Convertisseurs rapide de Xamarin.Forms

_Cet article présente des renderers rapides qui réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms sur Android en aplanissant la hiérarchie des contrôles natifs obtenue._

En règle générale, la plupart des convertisseurs de contrôle d’origine sur Android se composent de deux vues :

- Contrôle natif, tel qu’un `Button` ou `TextView`.
- Un conteneur `ViewGroup` qui gère le travail de mise en page, de gestion des mouvements ou autres tâches.

Toutefois, cette approche présente une implication de performances dans la mesure où deux vues sont créées pour chaque contrôle logique, ce qui entraîne une arborescence visuelle plus complexe qui nécessite davantage de mémoire et bien plus encore pour effectuer le rendu à l’écran de traitement.

Renderers rapides réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms dans une vue unique. Par conséquent, au lieu de créer des deux vues et les ajouter à l’arborescence de la vue, seul l’un est créé. Cela améliore les performances en créant moins d’objets, ce qui implique une arborescence de vue moins complexe, et moins l’utilisation de la mémoire (ce qui entraîne également moins pauses du garbage collection).

Renderers rapides sont disponibles pour les contrôles suivants dans Xamarin.Forms 2.4 sur Android :

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)

Fonctionnellement, ces convertisseurs rapides ne sont pas différentes pour les convertisseurs d’origine. Cependant, ils sont actuellement en phase expérimentale et peut uniquement être utilisées en ajoutant la ligne suivante de code à votre `MainActivity` classe avant d’appeler `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Renderers rapides sont uniquement applicables à l’app compat Android back-end, donc ce paramètre sera ignoré sur les activités de pre-app compat.

Améliorations des performances varient pour chaque application, en fonction de la complexité de la mise en page. Par exemple, les améliorations des performances de x2 sont possibles lors du défilement d’un [ `ListView` ](xref:Xamarin.Forms.ListView) contenant des milliers de lignes de données, où les cellules de chaque ligne sont constitués de contrôles qui utilisent des renderers rapides, ce qui entraîne des visiblement défilement plus lisse.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
