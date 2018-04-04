---
title: Chemins d’accès et les lignes SkiaSharp
description: Utilisez SkiaSharp pour dessiner des chemins d’accès des lignes et des graphiques
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: f7c0fcc87253b1d5ae9d612c52ef3a32c5790c5e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="skiasharp-lines-and-paths"></a>Chemins d’accès et les lignes SkiaSharp

_Utilisez SkiaSharp pour dessiner des chemins d’accès des lignes et des graphiques_

Le [section précédente](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) a démontré que le SkiaSharp `SKCanvas` classe inclut plusieurs méthodes pour dessiner des rectangles, des ellipses et rectangles arrondis. Cette section et les sections suivantes couvrent les différentes classes liées à la création et le rendu *tracés graphiques*.

Le chemin d’accès du graphique est l’approche la plus généralisée pour dessiner des lignes et des courbes dans SkiaSharp. Cette section couvre l’utilisation une `SKPath` objet pour dessiner des lignes droites et d’utiliser une collection de petites lignes droites (appelé un *polyligne*) pour dessiner des courbes que vous pouvez définir mathématiquement. Une section ultérieure sera présente les différents tris de courbes pris en charge par `SKPath`.

Tous les exemples de programmes dans cette section apparaissent sous l’en-tête **lignes et des chemins d’accès** dans la page d’accueil de la [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme, puis, dans le [ **Chemins d’accès** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Paths) dossier de la solution.

## <a name="lines-and-stroke-capslinesmd"></a>[Lignes et embouts de trait](lines.md)

Découvrez comment utiliser SkiaSharp pour dessiner des lignes avec des embouts de trait différent.

## <a name="path-basicspathsmd"></a>[Principes de base du chemin d’accès](paths.md)

Explorer l’objet SkiaSharp SKPath pour combiner des lignes et des courbes.

## <a name="the-path-fill-typesfill-typesmd"></a>[Les types de remplissage de chemin d’accès](fill-types.md)

Découvrez les différents effets possibles avec les types de remplissage SkiaSharp chemin d’accès.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Polylignes et équations paramétriques](polylines.md)

SkiaSharp permet d’afficher n’importe quelle ligne, que vous pouvez définir avec des équations paramétriques.

## <a name="dots-and-dashesdotsmd"></a>[Points et tirets](dots.md)

Maîtrisez les complexités de dessiner des lignes en pointillés et dans SkiaSharp.

## <a name="finger-paintingfinger-paintmd"></a>[Peinture au doigt](finger-paint.md)

Utilisez vos doigts pour peindre sur le canevas.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
