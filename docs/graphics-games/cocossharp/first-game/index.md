---
title: "Introduction au développement de jeux avec CocosSharp"
description: "Cette procédure pas à pas plusieurs parties montre comment créer un simple jeu 2D à l’aide de CocosSharp. Elle traite des concepts de programmation jeu courants tels que des graphiques, d’entrée et physique."
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 3b1bb45ab87c85dff42b4f7ea5297eb3596b81a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>Introduction au développement de jeux avec CocosSharp

_Cette procédure pas à pas plusieurs parties montre comment créer un simple jeu 2D à l’aide de CocosSharp. Elle traite des concepts de programmation jeu courants tels que des graphiques, d’entrée et physique._

Le moteur de jeu 2D CocosSharp fournit des technologies permettant de jeux multiplateformes. Pour une liste complète des plates-formes prises en charge, consultez la [wiki CocosSharp sur GitHub](https://github.com/mono/CocosSharp/wiki). Ce didacticiel utilise c# pour obtenir des exemples de code, bien que CocosSharp est également entièrement fonctionnelle avec F #.

Le cœur de CocosSharp est fourni par le [MonoGame framework](http://www.monogame.net/), qui est lui-même un inter-plateformes, les API qui fournit des graphiques, la gestion d’état audio, de jeu, d’entrée et un pipeline de contenu pour l’importation des ressources d’accélération matérielle. CocosSharp est une couche d’abstraction efficace convient particulièrement pour les jeux 2D. En outre, jeux plus volumineux peuvent effectuer leurs propres optimisations en dehors de leurs bibliothèques principales que jeux augmente la complexité. En d’autres termes, CocosSharp fournit un mélange de facilité d’utilisation et de performances, permettant aux développeurs d’être rapidement opérationnel sans limiter la taille du jeu ou la complexité.

La première section de concentration de cette procédure pas à pas sur la configuration d’un projet vide.  La deuxième partie couvre l’écriture tous nos logique du jeu. 

À la fin de cette procédure pas à pas, nous avoir créé un jeu simple où du joueur vise faire disparaître une manette de jeu horizontalement pour tenter de maintenir une boule chute sur l’écran. Chaque rebond augmentera le score du joueur d’un point.

![](images/image1.png "Chaque rebond augmentera le score du joueur d’un point")

# <a name="walkthrough-parts"></a>Parties de la procédure pas à pas

* [Partie 1 : création d’un projet CocosSharp](~/graphics-games/cocossharp/first-game/part1.md)
* [Partie 2 : mise en œuvre de la BouncingGame](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>Liens associés

- [Contenu de jeu (exemple)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Bibliothèque de classes portables CocosSharp sur NuGet](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [Documentation de l’API de CocosSharp](http://developer.xamarin.comhttps://developer.xamarin.com/api/namespace/CocosSharp/)
