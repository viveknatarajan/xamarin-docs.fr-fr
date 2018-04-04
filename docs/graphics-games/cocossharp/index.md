---
title: CocosSharp
description: Ce document est lié à des articles différents sur le développement de jeux avec CocosSharp.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: 2772af61dfc60b7ecd0fd5f7ecdfe5701d2504f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp est une bibliothèque pour la création de jeux 2D à l’aide de c# et F #. Il s’agit d’un port .NET du moteur Cocos2D populaires._

## <a name="introduction-to-cocossharp"></a>Introduction aux CocosSharp

Le moteur de jeu 2D CocosSharp fournit des technologies permettant de jeux multiplateformes. Pour une liste complète des plates-formes prises en charge, consultez la [wiki CocosSharp sur GitHub](https://github.com/mono/CocosSharp/wiki).
Ces guides utilisent c# pour obtenir des exemples de code, bien que CocosSharp est également entièrement fonctionnelle avec F #.

Le cœur de CocosSharp est fourni par le [MonoGame framework](http://www.monogame.net/), qui est lui-même un inter-plateformes, les API qui fournit des graphiques, la gestion d’état audio, de jeu, d’entrée et un pipeline de contenu pour l’importation des ressources d’accélération matérielle.
CocosSharp est une couche d’abstraction efficace convient particulièrement pour les jeux 2D.
En outre, jeux plus volumineux peuvent effectuer leurs propres optimisations en dehors de leurs bibliothèques principales que jeux augmente la complexité. En d’autres termes, CocosSharp fournit un mélange de facilité d’utilisation et de performances, permettant aux développeurs d’être rapidement opérationnel sans limiter la taille du jeu ou la complexité.

Cette vidéo pratique montre comment créer un CocosSharp inter-plateformes simple jeu :

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Ce guide décrit BouncingGame, y compris l’utilisation des contenus, différents éléments visuels utilisés pour générer un jeu, ajout logique du jeu et bien plus encore.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Se situe, agréables jeu](~/graphics-games/cocossharp/fruity-falls.md)

![Capture d’écran de jeu se situe, agréables](images/fruity-falls.png ", agréables concernent la plage de capture d’écran de jeu")

Ce guide décrit le jeu Fruity revient, couvrant CocosSharp courants et les concepts de développement de jeux telles que physique, la gestion de contenu, l’état du jeu et la conception de jeu.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Jeu de temps de prise en charge totale](~/graphics-games/cocossharp/cointime.md)

![Capture d’écran du jeu de temps de monnaie](images/cointime.png "prise en charge totale heure capture d’écran de jeu")

Heure de la prise en charge totale est un Platforming complète jeu pour iOS et Android. L’objectif du jeu consiste à collecter toutes les pièces d’un niveau et puis atteindre le capot de sortie tout en évitant les ennemis et éventuels obstacles.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Dessiner la géométrie avec CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Formes dessinées avec CCDrawNode](images/ccdrawnode.png "formes dessinées avec CCDrawNode")

CCDrawNode fournit des méthodes pour dessiner des objets de type primitif tels que des lignes, des cercles et des triangles.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Animation avec CCAction](~/graphics-games/cocossharp/ccaction.md)

![Une animation CCAction](images/ccaction.png "CCAction d’une animation")

`CCAction` est une classe de base qui peut être utilisée pour animer des objets CocosSharp. Ce guide couvre intégré `CCAction` implémentations pour les tâches courantes telles que le positionnement, la mise à l’échelle et la rotation. Il examine également comment créer des implémentations personnalisées en héritant de `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Utilisation de Tiled avec CocosSharp](~/graphics-games/cocossharp/tiled.md)

![Un niveau dans un jeu](images/tiled.png "un niveau dans un jeu")

Mosaïque est un outil puissant, flexible et une application déjà rodée pour la création de vignette orthogonal et isométrique est mappé pour les jeux. CocosSharp fournit une intégration intégrée pour le format de fichier natif de mosaïque.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Entités dans CocosSharp](~/graphics-games/cocossharp/entities.md)

![Un vaisseau spatial à partir d’un jeu](images/entities.png "un vaisseau spatial à partir d’un jeu")

Le modèle d’entité est une méthode efficace pour organiser le code de jeu. Il améliore la lisibilité, rend le code plus facile à gérer et il exploite les fonctionnalités intégrées de parent/enfant.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Gestion de plusieurs résolutions dans CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Une grille représentant la résolution d’écran](images/resolutions.png "une grille représentant la résolution d’écran")

Ce guide montre comment travailler avec CocosSharp pour développer des jeux qui s’affiche correctement sur les appareils des résolutions différentes.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[Pipeline de contenu de CocosSharp](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Pipelines de contenu sont souvent utilisées dans le développement de jeux pour optimiser le contenu et mettre en forme tel qu’il peut être chargé sur la configuration matérielle spécifique ou avec certaines infrastructures de développement de jeux.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Amélioration de la fréquence d’images avec CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Une arborescence à partir d’un CCSpriteSheet](images/ccspritesheet.png "une arborescence à partir d’un CCSpriteSheet")

`CCSpriteSheet` Fournit des fonctionnalités permettant de combiner et à l’aide de fichiers image dans une texture. Ce qui réduit le nombre de textures peut améliorer les temps de chargement d’un jeu et la fréquence d’images.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[La mise en cache de texture à l’aide de CCTextureCache](~/graphics-games/cocossharp/texture-cache.md)

![Une représentation de la façon dont CocosSharp met en cache des images](images/texture-cache.png "une représentation sous forme de comment CocosSharp met en cache des images")

De CocosSharp `CCTextureCache` classe offre un moyen standard d’organiser, de mettre en cache et de décharger le contenu. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[Mathématiques 2D avec CocosSharp](~/graphics-games/cocossharp/math.md)

![Une image en rotation](images/math.png "une image en rotation")

Ce guide couvre 2D mathématiques pour le développement de jeux. Il utilise CocosSharp pour montrer comment effectuer des tâches courantes de développement de jeux et explique les mathématiques qui sous-tendent ces tâches.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Performances et des effets visuels avec CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Un composant à partir d’un jeu](images/ccrendertexture.png "un composant à partir d’un jeu")

La `CCRenderTexture` classe fournit les fonctionnalités pour le rendu de plusieurs objets CocosSharp une texture unique. Une fois créé, `CCRenderTexture` instances peuvent être utilisées pour restituer des graphiques de manière efficace et pour implémenter des effets visuels.
