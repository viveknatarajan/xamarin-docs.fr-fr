---
title: Dessin 2D avec SkiaSharp
description: Ce document fournit une vue d’ensemble d’inter-plateformes 2D dessiner avec SkiaSharp. Il est lié à des guides pas à pas qui décrivent SkiaSharp et ses diverses API.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 0c8cbc14308c8c4131e5aaa2bcc0ddfa798af610
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130918"
---
# <a name="2d-drawing-with-skiasharp"></a>Dessin 2D avec SkiaSharp

SkiaSharp fournit une API c# puissant qui permet d’effectuer des graphismes 2D. Il est alimenté par [bibliothèque de Skia de Google](http://skia.org), la même bibliothèque qui alimente les piles de graphique Google Chrome, Firefox et Android.

[![](images/ide-sml.png "SkiaSharp fournit une API c# puissante qui permet d’effectuer des graphismes 2D")](images/ide.png#lightbox)

SkiaSharp est une bibliothèque Portable et est facilement fourni un [le package NuGet multiplateforme](https://www.nuget.org/packages/SkiaSharp)et prend en charge les plateformes suivantes prêts à l’emploi : macOS, le bureau Windows, Xamarin.Android et Xamarin.iOS.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Introduction à SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Une vue d’ensemble des principaux concepts de SkiaSharp et exemple de code pour restituer des graphiques, texte, images bitmap et utiliser des filtres de l’image.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Didacticiels de SkiaSharp pour Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Découvrez comment travailler avec cross platform graphiques qui s’affichent dans Xamarin.Forms :

- [Principes de base de dessin](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Dessin d’un cercle simple](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Intégration avec Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pixels et des unités indépendantes du périphérique](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Animation de base](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [L’intégration de texte et des graphiques](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Principes de base bitmap](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Lignes et chemins d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Lignes et embouts de trait](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Principes fondamentaux de chemin d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Les types de remplissage de chemin d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Polylignes et équations paramétriques](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Points et tirets](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Peinture au doigt](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transformations](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [La transformation de traduction](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [La transformation d’échelle](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [La transformation de rotation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [La transformation d’inclinaison](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Transformations de matrice](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulations tactiles](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Transformations non affines](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [Rotation 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Courbes et chemins d’accès](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Trois façons de dessiner un arc](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Trois types de courbes Bézier](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Données de chemin SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Détourage avec chemins d’accès et régions](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Effets de tracé](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Chemins et texte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Informations de chemin et énumération](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [Afficher des Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [Création et le dessin de Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [Rognage d’images bitmap](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [Affichage segmenté de Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [Enregistrement de Bitmaps dans des fichiers](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [L’accès aux Bits de Pixel de Bitmap](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [Animer des Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Remarques spécifiques de la plateforme](~/graphics-games/skiasharp/platform.md)

Cette page décrit les instructions d’installation pour SkiaSharp sur différentes plateformes, y compris iOS, Android, macOS et Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Documentation de l’API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Vous pouvez parcourir le [documentation de l’API](https://developer.xamarin.com/api/namespace/SkiaSharp/) pour SkiaSharp sur notre site web.

## <a name="work-in-progress"></a>Travail en cours

SkiaSharp est un travail en cours, nous partageons avec notre communauté. Même si nous avons lié des parties importantes de l’API Skia, la quantité de travail reste-t-il à effectuer. Nous utilisons l’API C stable exposés par Skia et notre objectif est de continuer faisant partie de notre travail aux liaisons de Skia pour fournir une couverture complète à l’API C.

Pour nous aider à guider nos efforts de liaison, veuillez laisser commentaires ou suggestions en tant que des problèmes sur le référentiel GitHub [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
