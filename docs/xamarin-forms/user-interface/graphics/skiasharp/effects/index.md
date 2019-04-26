---
title: Effets de SkiaSharp
description: Découvrez comment modifier l’affichage normal du graphique avec des dégradés, la mosaïque de bitmap, les modes de fusion, flou et autres effets.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
ms.openlocfilehash: 121d505d578aa20e86977c0da5d69626bbad1f53
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61289303"
---
# <a name="skiasharp-effects"></a>Effets de SkiaSharp

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Le SkiaSharp [ `SKPaint` ](xref:SkiaSharp.SKPaint) classe définit six des propriétés qui peuvent être classées sous le terme général de _effets_. Il s’agit des propriétés qui modifient l’affichage normal des graphiques d’une certaine façon. Les effets de SkiaSharp se répartissent en six catégories :

## <a name="path-effectscurveseffectsmd"></a>[Effets de tracé](../curves/effects.md)

Définir le [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) propriété du `SKPaint` à un objet de type [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) pour afficher les lignes en pointillés, ou pour rayer ou remplir une zone avec un modèle créé à partir de chemins d’accès. L’effet de chemin d’accès a été couvert plus haut dans cette série dans l’article [ **les effets de chemin d’accès dans SkiaSharp**](../curves/effects.md).

## <a name="shadersshadersindexmd"></a>[Nuanceurs](shaders/index.md)

Définir le [ `Shader` ](xref:SkiaSharp.SKPaint.Shader) propriété du `SKPaint` à un objet de type [ `SKShader` ](xref:SkiaSharp.SKShader) pour afficher des dégradés linéaires ou circulaires, des bitmaps en mosaïque et des modèles de bruit de Perlin.

## <a name="blend-modesblend-modesindexmd"></a>[Les Modes de fusion](blend-modes/index.md)

Définir le [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode) propriété du `SKPaint` à un membre de la [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode) énumération pour régir que se passe-t-il quand un graphique de la source est affiché sur une destination. SkiaSharp prend en charge tous les CSS composition et blend modes, y compris les modes, blend séparables modes et les modes de fusion non séparables Duff de Porter.

## <a name="mask-filtersmask-filtersmd"></a>[Masquage des filtres](mask-filters.md)

Définir le [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter) propriété du `SKPaint` à un objet de type [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) pour un flou et autres effets alphabétiques.

## <a name="image-filtersimage-filtersmd"></a>[Filtres d’image](image-filters.md)

Définir le [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter) propriété du `SKPaint` à un objet de type [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) flou bitmaps et en créant des ombres portées, relief ou gravure d’effets.

## <a name="color-filterscolor-filtersmd"></a>[Filtres de couleur](color-filters.md)

Définir le [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter) propriété du `SKPaint` à un objet de type [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) pour modifier les couleurs à l’aide de tables ou des transformations de matrice.

Tous les exemples de code pour ces articles se trouvent dans le [ **SkiaSharpFormsDemos**](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/). Dans la page d’accueil, sélectionnez **SkiaSharp effets**.

## <a name="related-links"></a>Liens connexes

- [API de SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (exemple)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
