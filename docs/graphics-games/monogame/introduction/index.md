---
title: Introduction au développement de jeux avec MonoGame
description: Cette procédure pas à pas plusieurs parties montre comment créer une simple application 2D à l’aide de MonoGame.  Il aborde la partie commune des concepts de programmation, tels que des graphiques, d’entrée, de jeux des entités et physique.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4ab98d59bc74672f9531f4dbd3c33a6270582612
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386264"
---
# <a name="introduction-to-game-development-with-monogame"></a>Introduction au développement de jeux avec MonoGame

_Cette procédure pas à pas plusieurs parties montre comment créer une simple application 2D à l’aide de MonoGame.  Il aborde la partie commune des concepts de programmation, tels que des graphiques, d’entrée, de jeux des entités et physique._

Cet article décrit la technologie de MonoGame API pour effectuer des jeux multiplateformes. Pour obtenir une liste complète des plateformes, consultez le [site Web MonoGame](http://www.monogame.net/). Ce didacticiel utilise C# pour obtenir des exemples de code, bien que MonoGame est totalement fonctionnel avec F# également.

MonoGame est multiplateforme, API fournissant des graphiques, audio, la partie gestion d’état, entrée et un pipeline de contenu pour l’importation d’éléments multimédias à accélération matérielle. Contrairement à la plupart des moteurs de jeux, MonoGame ne pas fournir ou imposer une structure de modèle ou un projet.  Bien que cela signifie que les développeurs sont libres d’organiser leur code qu’ils le souhaitent, cela signifie également qu’un peu de code de programme d’installation est nécessaire lorsque vous démarrez un nouveau projet.

La première section de cette procédure pas à pas se concentre sur la configuration d’un projet vide. La dernière section couvre l’écriture de toutes les notre logique de jeu et le contenu – plus de ce qui sera multiplateforme.

À la fin de cette procédure pas à pas, nous avons sera créé un jeu simple, où le joueur peut contrôler un caractère animé avec entrée tactile.  Bien que cela ne soit pas techniquement un jeu complet (dans la mesure où il n’a pas gagner ou perdre des conditions), il illustre de nombreux concepts de développement de jeux et peut être utilisé comme base pour de nombreux types de jeux. 

Voici le résultat de cette procédure pas à pas :

![Animation de caractère du jeu exemple suivant de la souris](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame et XNA

La bibliothèque MonoGame vise à imiter la bibliothèque XNA de Microsoft dans la syntaxe et des fonctionnalités.  Tous les objets de MonoGame existent sous l’espace de noms Microsoft.Xna – autoriser la plupart du code XNA utilisable dans MonoGame sans modification. 

Les développeurs familiarisés avec XNA seront déjà familiarisés avec la syntaxe de MonoGame, et les développeurs qui souhaitent pour plus d’informations sur l’utilisation avec MonoGame sera en mesure de référencer les procédures pas à pas XNA en ligne existant, la documentation des API et discussions.


## <a name="walkthrough-parts"></a>Parties de la procédure pas à pas

- [Partie 1 : création d’un projet de MonoGame multiplateforme](~/graphics-games/monogame/introduction/part1.md)
- [Partie 2 : implémentation du WalkingGame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Liens associés

- [Projet de MonoGame WalkingGame (exemple)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [Les polices XNB iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB polices Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [MonoGame Android sur NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [IOS MonoGame sur NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Documentation de l’API de MonoGame](http://www.monogame.net/documentation/?page=main)
