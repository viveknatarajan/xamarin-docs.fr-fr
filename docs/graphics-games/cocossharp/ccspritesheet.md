---
title: "Amélioration de la fréquence d’images avec CCSpriteSheet"
description: "CCSpriteSheet fournit des fonctionnalités permettant de combiner et à l’aide de fichiers image dans une texture. Ce qui réduit le nombre de textures peut améliorer les temps de chargement d’un jeu et la fréquence d’images."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: ec8a641fbd15f826e92ada62f65b17dd46b369e4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="improving-framerate-with-ccspritesheet"></a>Amélioration de la fréquence d’images avec CCSpriteSheet

_CCSpriteSheet fournit des fonctionnalités permettant de combiner et à l’aide de fichiers image dans une texture. Ce qui réduit le nombre de textures peut améliorer les temps de chargement d’un jeu et la fréquence d’images._

De nombreux jeux requièrent des efforts d’optimisation de fonctionnement et de charger rapidement sur un appareil mobile. La `CCSpriteSheet` classe peut aider à résoudre de nombreux problèmes de performances courants rencontrés par les jeux CocosSharp. Ce guide aborde les problèmes de performances courants et comment les résoudre à l’aide de la `CCSpriteSheet` classe.


# <a name="what-is-a-sprite-sheet"></a>Qu’est une feuille Sprite ?

A *feuille de sprite*, qui peut également être appelé une *atlas de textures*, est une image qui combine plusieurs images dans un seul fichier. Cela peut améliorer les performances d’exécution ainsi que les temps de chargement de contenu.

Par exemple, l’image suivante est une feuille de sprite simple créée par trois images distinctes. Les images individuelles peuvent être n’importe quelle taille, et la feuille sprite résultant n’est pas nécessaire pour être complètement remplie :

![](ccspritesheet-images/image1.png "Les images individuelles peuvent être n’importe quelle taille, et la feuille sprite résultant n’est pas nécessaire pour être complètement remplie")


## <a name="render-states"></a>États de rendu

Objets CocosSharp visuels (tel que `CCSprite`) simplifient le code de rendu sur le code de rendu l’API graphique traditionnel telles que MonoGame ou OpenGL, qui nécessitent la création de mémoires tampons de vertex (comme indiqué dans le [dessin des graphiques 3D avec Sommets dans MonoGame](~/graphics-games/monogame/3d/part2.md) guide). Malgré sa simplicité, CocosSharp n’élimine pas le coût du paramètre *États de rendu*, qui sont le nombre de fois que le code de rendu doit passer des textures ou autres États de rendu.

Le code interne de CocosSharp restitue chaque élément visuel de manière séquentielle, en parcourant le début de l’arborescence d’éléments visuels avec actuel `CCScene`. Par exemple, considérez la séquence suivante :

![](ccspritesheet-images/image2.png "Envisagez cette séquence.")

CocosSharp rendrait les quatre étoiles dans l’ordre :

![](ccspritesheet-images/image3.png "CocosSharp rendrait les quatre étoiles dans l’ordre")

Étant donné que chaque `CCSprite` utilise la même texture CocosSharp capable de regrouper toutes les quatre étoiles. Ce code nécessite une attribution rendu qu’un seul état (affectation de la texture en étoile) par frame. Ce scénario est très efficace.

Bien entendu, très peu de jeux utilisent uniquement une image. La séquence suivante présente un graphique de la planète :

![](ccspritesheet-images/image4.png "Cette séquence présente un graphique de la planète")

Dans l’idéal, CocosSharp doit être dessiné de tous les sprites à l’aide d’une image tout d’abord (par exemple, les étoiles), puis le reste des sprites à l’aide de l’autre image (la planète) :

![](ccspritesheet-images/image5.png "Dans l’idéal, CocosSharp doit être dessiné de tous les sprites à l’aide d’une image tout d’abord telles que les étoiles, puis le reste à l’aide de l’autre image de la planète sprites")

Le tri requiert plus de deux États de rendu : une sur le premier en étoile, sur la planète en premier.

Si tous les `CCSprite` instances sont des enfants de la même `CCNode`, puis CocosSharp permettra d’optimiser l’ordre de dessin afin de réduire les changements d’état de rendu. If, d’autre part, la `CCSprite` instances sont organisées telles que CocosSharp ne parvient pas à optimiser le rendu (par exemple si elles font partie d’une autre entité `CCNode` instances), puis la commande n’est peut-être pas optimale. Le code suivant illustre l’ordre de dessin possible pire, résultant dans cinq États de rendu :

![](ccspritesheet-images/image6.png "Cela indique l’ordre de dessin possible pire, résultant dans cinq États de rendu")

États de rendu peuvent être difficiles à optimiser, car l’ordre de dessin doit obéir à l’arborescence d’éléments visuels de `CCNode` instances. Cette arborescence est souvent structurée pour être facile à utiliser (par exemple, les entités qui contient les enfants visual), ou organisée en raison de la présentation visuelle de votre choix, tel que défini par un artiste.

Bien sûr, l’idéal est de présenter un état unique de rendu, en dépit de plusieurs images. Jeux de CocosSharp pour ce faire, en combinant toutes les images dans un seul fichier, puis le chargement de fichier (ainsi que ses accompagnant **.plist** fichier) dans un `CCSpriteSheet`. À l’aide de la `CCSpriteSheet` classe devient encore plus important pour les jeux qui ont un grand nombre d’images, ou qui ont très compliqué de dispositions. 

## <a name="load-times"></a>Temps de chargement.

Combinaison de plusieurs images dans un seul fichier améliore également les temps de chargement d’un jeu pour plusieurs raisons :

 - Combinaison de plusieurs images dans un fichier unique peut réduire le nombre total de pixels utilisés par le biais compresser efficacement
 - Le chargement des fichiers moins signifie moins de surcharge par fichier, telles que l’analyse des en-têtes de .png
 - Le chargement des fichiers moins requiert inférieur temps, ce qui est important pour basée sur disque de supports tels que les DVD et les disques durs traditionnels de recherche

# <a name="using-ccspritesheet-in-code"></a>À l’aide de CCSpriteSheet dans le Code

Pour créer un `CCSpriteSheet` instance, le code doit fournir une image et un fichier qui définit les régions de l’image à utiliser pour chaque frame. L’image peut être chargée comme un **.png** ou **.xnb** fichier (si vous utilisez la [contenu pipeline](~/graphics-games/cocossharp/content-pipeline/index.md)). Le fichier de définition des cadres est un **.plist** fichier qui peut être créé manuellement ou *TexturePacker* (dont nous parlerons ci-dessous).

L’exemple d’application, qui [peut être téléchargé ici](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), crée le `CCSpriteSheet` d’un **.png** et **.plist** fichier en utilisant le code suivant :

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Une fois chargé, le `CCSpriteSheet` contient un `List` de `CCSpriteFrame` instances – chaque instance correspondant à une des images source utilisés pour créer l’intégralité de la feuille. Dans le cas de la **SpriteSheetDemo** projet, le `CCSpriteSheet` contient trois images. Le **.plist** fichier peut être inspecté dans Visual Studio pour Mac ou dans n’importe quel éditeur de texte pour afficher des images qui sont disponibles. Si nous permet d’afficher le **.plist** fichier dans un éditeur de texte que nous pouvons voir trois cadres (sections omises pour mettre en évidence les noms de clé) :


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

Nous pouvons utiliser le `Find` pour rechercher des images par nom, comme suit (méthode) (code omis pour mettre en évidence `CCSpriteFrame` utilisation) :


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Étant donné que la `CCSprite` constructeur peut prendre un `CCSpriteFrame` paramètre, le code ne doit jamais analyser les détails de la `CCSpriteFrame`, tels que les textures, il utilise, ou la région de l’image dans la feuille sprite maître.


#  <a name="creating-a-sprite-sheet-plist"></a>Création d’un composant de la feuille .plist

Le fichier .plist est un fichier xml, qui peut être créé et modifié manuellement. De même, les programmes de modification d’image peut servir pour combiner plusieurs fichiers dans un fichier plus volumineux. Depuis la création et la gestion de feuilles de sprite peuvent prendre beaucoup de temps, nous allons examiner le programme TexturePacker qui peut exporter des fichiers au format CocosSharp. TexturePacker offre gratuite et une version « Pro » et est disponible pour Windows et Mac OS. Le reste de ce guide peut être suivi à l’aide de la version gratuite. 

TexturePacker peut être [téléchargé depuis le site Web TexturePacker](https://www.codeandweb.com/texturepacker). L’ouverture TexturePacker n’a pas de projet chargé. L’écran de démarrage permet d’ajouter sprites, ouvrir des projets récents (si les autres projets ont été créés) et sélectionnez le format à utiliser pour la feuille sprite. CocosSharp utilise le Format de données Cocos2D :

![](ccspritesheet-images/image7.png "CocosSharp utilise le Format de données Cocos2D")

Fichiers image (tel que **.png**) peut être ajouté à TexturePacker par glisser-déplacer les à partir de l’Explorateur Windows sur Windows ou Finder sur Mac. TexturePacker met automatiquement à jour l’aperçu de la feuille sprite chaque fois qu’un fichier est ajouté :

![](ccspritesheet-images/image8.png "TexturePacker met automatiquement à jour l’aperçu de la feuille sprite chaque fois qu’un fichier est ajouté.")

Pour exporter une feuille de sprite, cliquez sur le **feuille sprite de publication** bouton et sélectionnez un emplacement pour la feuille sprite. TexturePacker enregistre un fichier .plist et un fichier image.

Pour utiliser les fichiers résultants, ajoutez le .png et .plist à un projet CocosSharp. Pour plus d’informations sur l’ajout de fichiers aux projets de CocosSharp, consultez la [implémentant le guide BouncingGame](~/graphics-games/cocossharp/first-game/part2.md). Une fois que les fichiers sont ajoutés, ils peuvent être chargés dans un `CCSpriteSheet` comme illustré précédemment dans le code ci-dessus :

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

## <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>Remarques sur la gestion d’une feuille de TexturePacker Sprite

Lors du développement de jeux, artistes peuvent ajouter, supprimer ou modifier l’illustration. Toute modification requiert une feuille sprite mis à jour. Les considérations suivantes peuvent faciliter la maintenance des feuille sprite :

 - Conserver les fichiers d’origine (les fichiers utilisés pour créer les feuilles sprite) dans un dossier dans votre projet et assurez-vous qu’ils sont ajoutés au contrôle de version. Ces fichiers sont nécessaires pour recréer la feuille sprite chaque fois qu’une modification est apportée.
 - Ne pas ajouter les fichiers d’origine à Visual Studio pour Mac/Visual Studio ou s’ils sont ajoutés, définissez la **Action de génération** à **aucun**. Si les fichiers sont ajoutés et spécifique à la plateforme **Action de génération**, puis ils vont augmenter inutilement de taille de l’application résultante.
 - Envisagez d’utiliser *actives dossiers* dans TexturePacker. Dossiers intelligents ajoutent automatiquement des images contenues à la feuille de sprite. Cette fonctionnalité peut enregistrer beaucoup de temps lorsque développement de jeux avec un grand nombre d’images. 

    ![](ccspritesheet-images/image9.png "Cette fonctionnalité permet d’économiser beaucoup de temps lorsque développement de jeux avec un grand nombre d’images")
 - Garder un œil sur les tailles de texture sprite feuille. Certains anciens matériels phone ne prend pas en charge la taille de texture est supérieure à 2 048 x 2 048. En outre, une image 32 bits de 2048 x 2048 utilise presque 17 Mo de RAM – une quantité importante de mémoire.
 - TexturePacker n’inclut pas les dossiers dans les noms de sprite par défaut, les conflits de nom sont possibles. Il est préférable de choisir d’inclure le dossier des noms ou pas au début du développement. Jeux plus grandes envisagez d’utiliser des noms de dossier pour éviter les conflits. Pour inclure des chemins d’accès de dossier, cliquez sur **affichage avancé** dans les **données** section et vérifiez **nom du dossier ajoutez**. 

    ![](ccspritesheet-images/image10.png "Pour inclure des chemins d’accès de dossier, cliquez sur Afficher l’option Avancé dans la section de données et vérifiez le nom du dossier ajoutez")

# <a name="summary"></a>Récapitulatif

Ce guide explique comment créer et utiliser le `CCSpriteSheet` classe. Il explique également comment construire des fichiers qui peuvent être chargées dans `CCSpriteSheet` instances à l’aide du programme TexturePacker.

## <a name="related-links"></a>Liens associés

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Démonstration complète (exemple)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
