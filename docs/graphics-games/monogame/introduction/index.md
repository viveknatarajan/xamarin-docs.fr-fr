---
title: Introduction au développement de jeux avec MonoGame
description: Cette procédure pas à pas plusieurs parties montre comment créer une simple application 2D à l’aide de MonoGame.  Elle couvre le jeu commun de jeu des concepts de programmation, tels que des graphiques, des entrées, des entités et physique.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 9fb19b86ca303f8be3506d267dd75dc9db6cfca6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>Introduction au développement de jeux avec MonoGame

_Cette procédure pas à pas plusieurs parties montre comment créer une simple application 2D à l’aide de MonoGame.  Elle couvre le jeu commun de jeu des concepts de programmation, tels que des graphiques, des entrées, des entités et physique._

Cet article décrit la technologie MonoGame API permettant de jeux multiplateformes. Pour obtenir une liste complète des plateformes, consultez le [site Web de MonoGame](http://www.monogame.net/). Ce didacticiel utilise c# pour obtenir des exemples de code, bien que MonoGame est également entièrement fonctionnelle avec F #.

MonoGame est un inter-plateformes, les API qui fournit des graphiques, la gestion d’état audio, de jeu, d’entrée et un pipeline de contenu pour l’importation des ressources d’accélération matérielle. Contrairement à la plupart des moteurs de jeu, MonoGame ne pas fournir ou imposer une structure de modèle ou un projet.  Bien que cela signifie que les développeurs sont libres d’organiser leur code qu’ils le souhaitent, cela signifie également qu’un peu de code d’installation est nécessaire lors du premier démarrage un nouveau projet.

La première section de cette procédure pas à pas se concentre sur la configuration d’un projet vide. La dernière section couvre l’écriture tous nos logique de jeu et un contenu plus de laquelle sera cross-platform.

À la fin de cette procédure pas à pas, nous avons sera créé un jeu simple dans lequel le lecteur peut contrôler un caractère animé avec une entrée tactile.  Bien que ce ne soit pas techniquement un jeu complet (puisqu’elle n’a aucune win ou de perdre des conditions), il présente de nombreux concepts de développement de jeux et peut être utilisé comme base pour de nombreux types de jeux. 

Voici le résultat de cette procédure pas à pas :

![](images/image1.gif "L’application qui sera générée dans la procédure pas à pas")

# <a name="monogame-and-xna"></a>Monogame et XNA

La bibliothèque MonoGame est destinée à simuler la bibliothèque de XNA de Microsoft dans la syntaxe et les fonctionnalités.  Tous les objets de MonoGame existent sous l’espace de noms Microsoft.Xna – autoriser la plupart du code XNA à utiliser dans MonoGame sans modification. 

Les développeurs familiarisés avec XNA seront déjà familiarisés avec la syntaxe de MonoGame, et les développeurs qui souhaitent pour plus d’informations sur l’utilisation de MonoGame sera en mesure de référencer des procédures pas à pas XNA en ligne existant, la documentation de l’API et des discussions.


# <a name="walkthrough-parts"></a>Parties de la procédure pas à pas

- [Partie 1 : création d’un projet de MonoGame inter-plateformes](~/graphics-games/monogame/introduction/part1.md)
- [Partie 2 : mise en œuvre de la WalkingGame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Liens associés

- [Projet de MonoGame WalkingGame (exemple)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [Les polices XNB iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB polices Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [MonoGame Android sur NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [IOS MonoGame sur NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Documentation de l’API de MonoGame](http://www.monogame.net/documentation/?page=main)
