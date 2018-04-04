---
title: Dessin 2D
description: Cross-Platform de dessin 2D avec SkiaSharp
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: a6249525e8a5f85284c462888a7698312321642f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="2d-drawing"></a>Dessin 2D

SkiaSharp fournit une API puissante c# pour créer des graphiques 2D. Il est alimenté par [bibliothèque de Skia de Google](http://skia.org), la même bibliothèque optimisant Google Chrome, Firefox et d’Android piles de graphique.

[![](images/ide-sml.png "SkiaSharp fournit une API puissante c# pour créer des graphiques 2D")](images/ide.png#lightbox)

SkiaSharp est une bibliothèque Portable et est fournie pour des raisons pratiques comme un [package NuGet d’inter-plateformes](https://www.nuget.org/packages/SkiaSharp)et prend en charge les plateformes suivantes en dehors de la zone : macOS, Xamarin.Android et Xamarin.iOS le bureau Windows.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Introduction aux SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Une vue d’ensemble des principaux concepts de SkiaSharp et exemple de code pour restituer des graphiques, texte, images bitmap et utiliser des filtres de l’image.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Didacticiels SkiaSharp de Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Apprendre à utiliser entre les graphiques de plateforme qui restituent dans Xamarin.Forms :

- [Principes fondamentaux de dessin](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Dessin d’un cercle simple](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Intégration avec Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Unités indépendantes du périphérique et pixels](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Animation de base](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Intégration de texte et des graphiques](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Bitmap des principes de base](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Chemins d’accès et des lignes](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Lignes et les extrémités de trait](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Principes fondamentaux de chemin d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Les types de remplissage du chemin d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Polylignes et équations paramétriques](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Points et des tirets](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Peinture au doigt](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transformations](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [La transformation de traduction](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [La transformation d’échelle](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [La transformation de rotation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [La transformation d’inclinaison](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Transformations de matrice](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulations tactile](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Transformations affines non](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [Rotation 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Chemins d’accès et des courbes](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Trois façons de dessiner un arc](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Trois types de courbes Bézier](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Données de chemin SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Détourage avec chemins d’accès et régions](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Effets de tracé](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Chemins et texte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Informations de chemin et énumération](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Remarques spécifiques de la plateforme](~/graphics-games/skiasharp/platform.md)

Cette page décrit les instructions d’installation pour SkiaSharp sur différentes plateformes, y compris Windows, Mac OS, iOS et Android.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Documentation de l’API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Vous pouvez parcourir le [documentation de l’API](https://developer.xamarin.com/api/namespace/SkiaSharp/) pour SkiaSharp sur notre site web.

## <a name="work-in-progress"></a>Travail en cours

SkiaSharp est un travail en cours que nous allons partager avec notre communauté. Alors que nous avons liés des parties importantes de l’API Skia, la quantité de travail reste-t-il à effectuer. Nous utilisons l’API C stable par Skia et notre plan consiste à continuer qui contribuent à notre travail aux liaisons de Skia pour fournir une couverture complète aux API C.

Pour nous aider à guider nos efforts de liaison, veuillez laisser commentaires ou suggestions en tant que des problèmes sur le référentiel GitHub [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
