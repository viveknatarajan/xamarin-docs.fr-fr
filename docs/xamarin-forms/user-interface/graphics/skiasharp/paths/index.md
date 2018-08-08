---
title: Chemins d’accès et des lignes de SkiaSharp
description: Cet article explique comment utiliser SkiaSharp pour dessiner des lignes et des tracés graphiques dans les applications Xamarin.Forms et illustre ceci avec l’exemple de code.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9febfabb7b44b1ec09abda4b352691b37565cb48
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615133"
---
# <a name="skiasharp-lines-and-paths"></a>Chemins d’accès et des lignes de SkiaSharp

_Permet de dessiner des lignes et des graphismes chemins d’accès SkiaSharp_

Le [section précédente](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) a démontré que le SkiaSharp `SKCanvas` classe inclut plusieurs méthodes pour dessiner des rectangles, des ellipses et rectangles arrondis. Cette section et les sections suivantes couvrent les différentes classes liées à la création et le rendu *tracés graphiques*.

Le chemin d’accès de graphiques est l’approche la plus généralisée pour dessiner des lignes et des courbes dans SkiaSharp. Cette section traite de l’utilisation un `SKPath` objet pour dessiner des lignes droites et d’utiliser une collection de lignes droites minuscules (appelé un *polyligne*) pour dessiner des courbes que vous pouvez définir de point de vue mathématique. Une section ultérieure traite des différentes sortes de courbes pris en charge par `SKPath`.

Tous les exemples de programmes dans cette section apparaissent sous l’en-tête **lignes et chemins d’accès** dans la page d’accueil de la [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme, puis, dans le [ **Chemins** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) dossier de la solution.

## <a name="lines-and-stroke-capslinesmd"></a>[Lignes et embouts de trait](lines.md)

Découvrez comment utiliser SkiaSharp pour dessiner des lignes avec des embouts de trait différentes.

## <a name="path-basicspathsmd"></a>[Principes de base du chemin d’accès](paths.md)

Explorez l’objet SkiaSharp SKPath pour la combinaison de lignes et des courbes.

## <a name="the-path-fill-typesfill-typesmd"></a>[Les types de remplissage de chemin d’accès](fill-types.md)

Découvrez les différents effets possibles avec les types de remplissage de chemin d’accès SkiaSharp.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Polylignes et équations paramétriques](polylines.md)

SkiaSharp permet d’afficher n’importe quelle ligne, que vous pouvez définir avec des équations paramétriques.

## <a name="dots-and-dashesdotsmd"></a>[Points et tirets](dots.md)

Maîtrisez les complexités de dessiner des lignes en pointillés et dans SkiaSharp.

## <a name="finger-paintingfinger-paintmd"></a>[Peinture au doigt](finger-paint.md)

Utilisez vos doigts pour peindre sur le canevas.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
