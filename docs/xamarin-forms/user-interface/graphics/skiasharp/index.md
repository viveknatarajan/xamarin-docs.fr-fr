---
title: À l’aide de SkiaSharp dans Xamarin.Forms
description: Utilisez SkiaSharp pour les graphiques 2D dans vos applications de Xamarin.Forms
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: e3444411c80ecaee105cce7c10f7bec7583dc2a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>À l’aide de SkiaSharp dans Xamarin.Forms

_Utilisez SkiaSharp pour les graphiques 2D dans vos applications de Xamarin.Forms_

SkiaSharp est un système de graphiques 2D pour .NET et le moteur de graphiques Skia open source qui est largement utilisé dans les produits Google pour c#. Vous pouvez utiliser SkiaSharp dans vos applications de Xamarin.Forms pour dessiner le texte, images bitmap et graphiques vectoriels 2D. Consultez le [dessin 2D](~/graphics-games/skiasharp/index.md) guide pour plus d’informations sur la bibliothèque SkiaSharp et certains autres didacticiels.

Ce guide suppose que vous êtes familiarisé avec la programmation de Xamarin.Forms.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Séminaire Web : Les SkiaSharp de Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>SkiaSharp préliminaires

SkiaSharp pour Xamarin.Forms est empaqueté comme un package NuGet. Une fois que vous avez créé une solution Xamarin.Forms dans Visual Studio ou Visual Studio pour Mac, vous pouvez utiliser le Gestionnaire de package NuGet pour rechercher la **SkiaSharp.Views.Forms** du package et l’ajouter à votre solution. Si vous activez le **références** section de chaque projet après l’ajout de SkiaSharp, vous pouvez voir que divers **SkiaSharp** bibliothèques ont été ajoutés à chacun des projets dans la solution.

Si votre application Xamarin.Forms cible iOS, utilisez la page de propriétés de projet pour modifier la cible de déploiement minimum à iOS 8.0.

Dans n’importe quelle page c# qui utilise SkiaSharp que vous souhaitez inclure un `using` directive pour le [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) espace de noms qui englobe tous les SkiaSharp classes, structures et énumérations que vous allez utiliser dans vos graphiques la programmation. Vous allez également un `using` directive pour le [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) espace de noms pour les classes spécifiques de Xamarin.Forms. Ceci est un volume plus petit espace de noms avec la classe la plus importante étant [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Cette classe dérive de la Xamarin.Forms `View` classe et héberge votre sortie de graphiques SkiaSharp.

> [!IMPORTANT]
> Le `SkiaSharp.Views.Forms` espace de noms contient également un `SKGLView` classe qui dérive de `View` , mais utilise OpenGL pour le rendu de graphiques. Pour des raisons de simplicité, la limite de ce guide pour `SKCanvasView`, mais à l’aide `SKGLView` au lieu de cela est assez similaire.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Principes de base de dessin SkiaSharp](basics/index.md)

Certains des figures graphics la plus simple, que vous pouvez dessiner avec SkiaSharp sont des ovales, des cercles et des rectangles. Dans l’affichage de ces chiffres, vous découvrirez les coordonnées SkiaSharp, tailles et couleurs.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[Lignes et chemins d’accès SkiaSharp](paths/index.md)

Un chemin d’accès du graphique est une série de lignes droites connectées et les courbes. Chemins d’accès peuvent être dessinés pleines, ou les deux. Cette rubrique comprend de nombreux aspects du dessin au trait, y compris les extrémités de trait et de jointures et tirets et des traits en pointillés, mais bloque les géométries de courbe.

## <a name="skiasharp-transformstransformsindexmd"></a>[Transformations SkiaSharp](transforms/index.md)

Transformations autorise des objets graphics uniformément traduites, mis à l’échelle, pivoté ou asymétrique. Cet article explique également comment vous pouvez utiliser une matrice de transformation de 3-par-3 standard pour créer des transformations non affines et appliquer des transformations aux chemins d’accès.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[Courbes et chemins d’accès SkiaSharp](curves/index.md)

L’exploration des chemins d’accès se poursuit avec l’ajout des courbes à des objets de chemin d’accès et d’exploiter les autres fonctionnalités puissantes de chemin d’accès. Vous allez voir comment vous pouvez spécifier un chemin d’accès complet dans une chaîne de texte concis, comment utiliser les effets de chemin d’accès et approfondir les mécanismes internes de chemin d’accès.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SkiaSharp avec Xamarin.Forms webinaire (vidéo)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
