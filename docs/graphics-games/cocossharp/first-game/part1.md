---
title: Création d’un projet de CocosSharp multi-plateforme
description: 'Cette procédure pas à pas montre comment créer une solution CocosSharp multi-plateforme. Le résultat de cette procédure pas à pas est un Visual Studio pour la solution Mac qui comprend trois projets : un projet de bibliothèque de classes portable, un projet Android spécifique et un projet iOS. Le projet résultant affiche un écran noir vide lors de l’exécution.'
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>Création d’un projet de CocosSharp multi-plateforme

_Cette procédure pas à pas montre comment créer une solution CocosSharp multi-plateforme. Le résultat de cette procédure pas à pas est un Visual Studio pour la solution Mac qui comprend trois projets : un projet de bibliothèque de classes portable, un projet Android spécifique et un projet iOS. Le projet résultant affiche un écran noir vide lors de l’exécution._

Le moteur de jeu 2D CocosSharp permet de code et le contenu à partager entre plusieurs plateformes. Cette procédure pas à pas montre comment créer un projet qui peut prendre en charge iOS et Android développement. Plus précisément, cette procédure pas à pas couvre les rubriques suivantes :

 - CocosSharp lors de l’installation
 - Création d’une solution
 - Méthode `LoadGame`

# <a name="installing-cocossharp"></a>CocosSharp lors de l’installation

Tout d’abord, nous allons ajouter CocosSharp à Visual Studio pour Mac. Si vous exécutez sur un Mac, sélectionnez **Visual Studio pour Mac** > **Gestionnaire de compléments...**  . Si en cours d’exécution sur Windows, sélectionnez **outils** > **Gestionnaire de compléments...**  . Cliquez sur le **galerie** onglet, développez le **CocosSharp élément**, sélectionnez **les modèles de projet CocosSharp**et enfin cliquez sur **installer...**  .

![CocosSharp addin](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>Création d’une solution

Maintenant que CocosSharp est installé, nous allons créer une solution. Dans Visual Studio pour Mac, sélectionnez **fichier** > **nouveau** > **Solution...** . Sélectionnez le **application** sous le **inter-plateformes** section, sélectionnez **projet vide de CocosSharp**, puis cliquez sur **suivant**:

![](part1-images/image1.png "Sélectionnez l’option de l’application sous la section inter-plateformes, sélectionnez projet vide de CocosSharp, puis cliquez sur Suivant")

Entrez le nom **BouncingGame** pour le **nom du projet**, puis cliquez sur **créer**:

![](part1-images/image2.png "Entrez le nom BouncingGame pour le nom du projet, puis cliquez sur Créer")

Une fois que le projet a été créé et Visual Studio pour Mac, nous pouvons compiler et exécuter pour afficher un fond gris : 

![](part1-images/image3.png "Une fois que le projet a été créé et Visual Studio pour Mac, compiler et exécuter pour afficher un fond gris")


# <a name="loadgame-method"></a>LoadGame (méthode)

Le projet de CocosSharp par défaut inclut les classes spécifiques pour iOS et Android pour la configuration d’un `CCGameView`, qui sert à démarrer CocosSharp. Le `CCGameView` instance est créée de manière spécifique à la plateforme : le projet iOS crée le `CCGameView` dans les `Main.storyboard` de fichiers, alors que Android crée le `CCGameView` dans le `Main.axml` fichier. Chaque plateforme utilise l’instance CCGameView dans un `LoadGame` méthode qui effectue une installation de base. Bien que nous ne modifient ce code, examinons certains détails importants :

 - Le code définit le `gameView.DesignResolution` à 1024 x 768. Cela permet de normaliser de positionnement pour les appareils indépendamment de proportions, résolution physique ou l’orientation du périphérique en cours. 
 - Le code ajoute plusieurs chemins de recherche. Chemins de recherche permettent également de contenu à charger sans les préfixes d’annuaire. Par exemple, depuis le `"Sounds"` chemin d’accès est ajouté en tant qu’un chemin de recherche, puis un fichier dans le répertoire `"Content/Sounds/mysound.xnb"` peut simplement être chargé en tant que `"mysound.xnb"`. Chemins de recherche sont similaires aux `using` instructions dans le code : ils peuvent réduire le code, mais elles peuvent également introduire une ambiguïté.
 - Le code construit une `GameLayer` instance. `GameLayer` est un `CCLayer`-héritage de classe où nous allons ajouter tous nos logique du jeu. Les jeux plus volumineux peuvent nécessiter plusieurs `CCLayer` instances, ou même plusieurs `CCScene` instances (qui peut contenir plusieurs `CCLayer` instances), mais nous allons coller à un seul `CCLayer` pour ce jeu.

#  <a name="summary"></a>Récapitulatif

Cette procédure pas à pas expliqué comment créer un projet de CocosSharp multiplateforme à l’aide de Visual Studio pour Mac. Le résultat est un écran vide, ce qui peut être utilisé comme point de départ pour un projet de jeu.