---
title: Moteur de jeu 2D CocosSharp
description: Ce document contient des liens vers des articles différents sur le développement de jeux avec CocosSharp. Contenu lié décrit des exemples d’applications, dessin, animation et bien plus encore.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: add73360ea98d8c516e413f0cc0264f68c58d79d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107012"
---
# <a name="cocossharp-2d-game-engine"></a>Moteur de jeu 2D CocosSharp

_CocosSharp est une bibliothèque pour générer des jeux 2D à l’aide de C# et F#. Il s’agit d’un port .NET du moteur Cocos2D populaires._

## <a name="introduction-to-cocossharp"></a>Présentation de CocosSharp

Le moteur de jeu 2D CocosSharp fournit des technologies pour effectuer des jeux multiplateformes. Pour une liste complète des plateformes prises en charge, consultez le [wiki CocosSharp sur GitHub](https://github.com/mono/CocosSharp/wiki).
Utilisent ces guides C# pour obtenir des exemples de code, bien que CocosSharp est totalement fonctionnel avec F# également.

Le cœur de CocosSharp est fourni par le [MonoGame framework](http://www.monogame.net/), qui est lui-même une inter-plateformes, API fournissant des graphiques, audio, la partie gestion d’état, entrée et un pipeline de contenu pour l’importation d’éléments multimédias à accélération matérielle.
CocosSharp est une couche d’abstraction efficace parfaitement adaptée à des jeux 2D.
En outre, les jeux plus volumineux peuvent exécuter leurs propre optimisations en dehors de leurs bibliothèques principales comme jeux gagne en complexité. En d’autres termes, CocosSharp fournit un mélange de facilité d’utilisation et de performances, permettant aux développeurs de démarrer rapidement sans limiter la taille du jeu ou complexité.

Cette vidéo pratique montre comment créer une simple CocosSharp inter-plateformes jeu :

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Ce guide décrit BouncingGame, y compris comment utiliser le contenu de jeu, différents éléments visuels utilisés pour générer un jeu, l’ajout de logique de jeu et bien plus encore.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Jeu de Fruity concernent la plage](~/graphics-games/cocossharp/fruity-falls.md)

![Jeu Fruity capture d’écran](images/fruity-falls.png "du jeu Fruity capture d’écran")

Ce guide décrit le jeu Fruity concernent la plage, couvrant CocosSharp courants et les concepts de développement de jeux tels que physique, la gestion de contenu, état du jeu et conception de jeux.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Jeu de temps en monnaie](~/graphics-games/cocossharp/cointime.md)

![Capture d’écran du jeu de temps de monnaie](images/cointime.png "médaille temps capture d’écran de jeu")

Heure de la médaille est un jeux de plateforme complète jeu pour iOS et Android. L’objectif du jeu consiste à collecter toutes les pièces d’un niveau et puis atteindre le capot de sortie tout en évitant les ennemis et obstacles.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Dessin de géométrie avec CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Formes dessinées avec CCDrawNode](images/ccdrawnode.png "formes dessinées avec CCDrawNode")

CCDrawNode fournit des méthodes pour le dessins primitifs tels que des triangles, des cercles et des lignes.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Animation avec CCAction](~/graphics-games/cocossharp/ccaction.md)

![Une animation CCAction](images/ccaction.png "CCAction d’une animation")

`CCAction` est une classe de base qui peut être utilisée pour animer des objets de CocosSharp. Ce guide couvre intégré `CCAction` implémentations pour les tâches courantes telles que le positionnement, la mise à l’échelle et la rotation. Il examine également comment créer des implémentations personnalisées en héritant de `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Utilisation de Tiled avec CocosSharp](~/graphics-games/cocossharp/tiled.md)

![Un niveau dans un jeu](images/tiled.png "un niveau dans un jeu")

En mosaïque est un puissant, flexible et application mature pour la création de vignette ORTHOGONALE et isométrique mappages pour les jeux. CocosSharp fournit l’intégration pour le format de fichier natif de mosaïque.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Entités dans CocosSharp](~/graphics-games/cocossharp/entities.md)

![Un vaisseau spatial à partir d’un jeu](images/entities.png "un vaisseau spatial à partir d’un jeu")

Le modèle d’entité est un moyen puissant d’organiser le code de jeu. Il améliore la lisibilité, rend le code plus facile à gérer et tire parti des fonctionnalités intégrées de parent/enfant.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Gestion de résolutions multiples dans CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Une grille qui représente la résolution d’écran](images/resolutions.png "une grille qui représente la résolution d’écran")

Ce guide montre comment travailler avec CocosSharp pour développer des jeux qui s’affiche correctement sur les périphériques de résolution différente.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[Pipeline de contenu de CocosSharp](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Pipelines de contenu sont souvent utilisés dans le développement de jeux pour optimiser le contenu et mettre en forme tel qu’il peut être chargé sur un matériel spécifique ou avec certaines infrastructures de développement de jeux.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Amélioration de la fréquence d’images avec CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Une arborescence à partir d’un CCSpriteSheet](images/ccspritesheet.png "une arborescence à partir d’un CCSpriteSheet")

`CCSpriteSheet` Fournit des fonctionnalités permettant de combiner et à l’aide de fichiers image dans une texture. En réduisant le nombre de texture peut améliorer les temps de chargement d’un jeu et de la fréquence d’images.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[La mise en cache de texture à l’aide de CCTextureCache](~/graphics-games/cocossharp/texture-cache.md)

![Une représentation sous forme de comment CocosSharp met en cache des images](images/texture-cache.png "une représentation sous forme de comment CocosSharp met en cache des images")

De CocosSharp `CCTextureCache` classe offre un moyen standard d’organiser, de mettre en cache et de décharger le contenu. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[Mathématiques 2D avec CocosSharp](~/graphics-games/cocossharp/math.md)

![Une image en rotation](images/math.png "une image en rotation")

Ce guide couvre les mathématiques 2D pour le développement de jeux. Il utilise CocosSharp de montrer comment effectuer des tâches courantes de développement de jeux et explique les calculs derrière ces tâches.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Performances et des effets visuels avec CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Un sprite à partir d’un jeu](images/ccrendertexture.png "un sprite à partir d’un jeu")

Le `CCRenderTexture` classe fournit les fonctionnalités pour le rendu de plusieurs objets de CocosSharp à une texture unique. Une fois créé, `CCRenderTexture` instances peuvent être utilisées pour restituer des graphiques de manière efficace et pour implémenter des effets visuels.
