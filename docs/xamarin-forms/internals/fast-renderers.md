---
title: Renderers rapides
description: "Cet article présente les convertisseurs rapides, réduire les complications et les coûts de rendu d’un contrôle Xamarin.Forms sur Android par la mise à plat de la hiérarchie de contrôle natif résultante."
ms.topic: article
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 956fa919ef9aa994fea92a9a64ca1325819f3ffc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="fast-renderers"></a>Renderers rapides

_Cet article présente les convertisseurs rapides, réduire les complications et les coûts de rendu d’un contrôle Xamarin.Forms sur Android par la mise à plat de la hiérarchie de contrôle natif résultante._

En règle générale, la plupart des convertisseurs de contrôle d’origine sur Android se composent de deux vues :

- Contrôle natif, tel qu’un `Button` ou `TextView`.
- Un conteneur `ViewGroup` qui gère le travail de mise en page, de la gestion des mouvements ou autres tâches.

Toutefois, cette approche a une incidence sur performances dans la mesure où deux vues sont créées pour chaque contrôle logique, ce qui génère une arborescence d’éléments visuel plus complexe qui nécessite davantage de mémoire, plus le traitement pour effectuer le rendu à l’écran.

Convertisseurs rapides de réduire les complications et les coûts de rendu d’un contrôle Xamarin.Forms dans une vue unique. Par conséquent, au lieu de créer deux vues et les ajouter à l’arborescence de la vue, une seule est créée. Cela améliore les performances en créant le moins d’objets, qui à son tour signifie une arborescence moins complexe, et moins de l’utilisation mémoire (ce qui entraîne également moins pauses du garbage collection).

Les convertisseurs rapides sont disponibles pour les contrôles suivants dans Xamarin.Forms 2.4 sur Android :

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

Fonctionnellement, ces convertisseurs rapides sont identiques pour les convertisseurs d’origine. Cependant, ils sont actuellement expérimentales et peut uniquement être utilisés en ajoutant la ligne suivante de code à votre `MainActivity` classe avant d’appeler `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Les convertisseurs rapides sont uniquement applicables aux application compat Android serveur principal, donc ce paramètre sera ignoré sur les activités de pre-app compat.

Améliorations des performances varient pour chaque application, en fonction de la complexité de la mise en page. Par exemple, les améliorations des performances de x2 sont possibles lors du défilement d’un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) contenant des milliers de lignes de données, où les cellules de chaque ligne sont constitués de contrôles qui utilisent des convertisseurs rapides, ce qui entraîne une visiblement défilement plus lisse.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
