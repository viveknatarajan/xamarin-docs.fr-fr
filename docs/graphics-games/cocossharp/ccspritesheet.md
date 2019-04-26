---
title: Amélioration de la fréquence d’images avec CCSpriteSheet
description: CCSpriteSheet fournit des fonctionnalités permettant de combiner et à l’aide de fichiers image dans une texture. En réduisant le nombre de texture peut améliorer les temps de chargement d’un jeu et de la fréquence d’images.
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: eab6153653a8c8df2068aaaf879d84d35473c541
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61245693"
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>Amélioration de la fréquence d’images avec CCSpriteSheet

_CCSpriteSheet fournit des fonctionnalités permettant de combiner et à l’aide de fichiers image dans une texture. En réduisant le nombre de texture peut améliorer les temps de chargement d’un jeu et de la fréquence d’images._

De nombreux jeux requièrent leurs efforts d’optimisation pour fonctionner correctement et de charger rapidement sur le matériel mobile. Le `CCSpriteSheet` classe peut aider à résoudre de nombreux problèmes de performances courants rencontrés par les jeux de CocosSharp. Ce guide couvre les problèmes de performances courants et comment les résoudre à l’aide de la `CCSpriteSheet` classe.


## <a name="what-is-a-sprite-sheet"></a>Qu’est une feuille de sprite ?

Un *feuille du sprite*, ce qui peut également être appelé une *atlas de textures*, est une image qui combine plusieurs images en un seul fichier. Cela peut améliorer les performances d’exécution, ainsi que des temps de chargement de contenu.

Par exemple, l’image suivante est une feuille de sprite simple créée par trois images distinctes. Les images individuelles peuvent être n’importe quelle taille, et la feuille sprite qui en résulte n’est pas nécessaire d’être complètement remplie :

![](ccspritesheet-images/image1.png "Les images individuelles peuvent être n’importe quelle taille et la feuille sprite qui en résulte n’est pas nécessaire d’être complètement remplie")


### <a name="render-states"></a>États de rendu

Objets visuels de CocosSharp (tel que `CCSprite`) simplifient le code de rendu sur le code de rendu de API graphique traditionnel comme MonoGame ou OpenGL, qui nécessitent la création de mémoires tampons de vertex (comme indiqué dans le [dessin de graphiques 3D avec Vertex dans MonoGame](~/graphics-games/monogame/3d/part2.md) guide). Malgré sa simplicité, CocosSharp n’élimine pas le coût du paramètre *États de rendu*, qui sont le nombre de fois que le code de rendu doit passer des textures ou des autres États de rendu.

Code interne de CocosSharp restitue chaque élément visuel de façon séquentielle, en parcourant le début de l’arborescence d’éléments visuels avec actuel `CCScene`. Par exemple, considérez la scène suivante :

![](ccspritesheet-images/image2.png "Prendre en compte cette scène")

CocosSharp rendrait les quatre étoiles dans l’ordre :

![](ccspritesheet-images/image3.png "CocosSharp rendrait les quatre étoiles dans une séquence")

Étant donné que chaque `CCSprite` utilise la même texture, CocosSharp pouvez regrouper toutes les quatre étoiles. Ce code nécessite une attribution rendu qu’un seul état (affectation de la texture en étoile) par trame. Ce scénario est très efficace.

Bien sûr, des jeux très peu utilisent une seule image. La scène suivante présente un graphique de la planète :

![](ccspritesheet-images/image4.png "Cette scène présente un graphique de la planète")

Dans l’idéal, tous les sprites à l’aide d’une image tout d’abord (par exemple, les étoiles), puis le reste de sprites à l’aide de l’autre image (la planète) doit être dessiné de CocosSharp :

![](ccspritesheet-images/image5.png "Dans l’idéal, CocosSharp doit être dessiné de tous les sprites à l’aide d’une image tout d’abord telles que les étoiles, puis le reste des sprites à l’aide de l’autre image de la planète")

Le classement nécessite au-dessus de deux États de rendu : un sur le premier en étoile, sur la planète en premier.

Si tous les `CCSprite` instances sont des enfants du même `CCNode`, puis CocosSharp permettra d’optimiser l’ordre de dessin afin de réduire les changements d’état de rendu. If, d’autre part, le `CCSprite` instances sont organisés de sorte que CocosSharp ne peut pas optimiser le rendu (par exemple si elles font partie d’une autre entité `CCNode` instances), puis l’ordre ne peut pas être optimal. Voici l’ordre de dessin possible pire, résultant dans cinq États de rendu :

![](ccspritesheet-images/image6.png "Cela indique l’ordre de dessin possible pire, résultant dans cinq États de rendu")

États de rendu peuvent être difficiles à optimiser, car l’ordre de dessin doit respecter l’arborescence visuelle de `CCNode` instances. Cette arborescence est souvent structurée pour être facile à utiliser (par exemple, les entités qui contient leurs enfants visuels), ou organisée en raison de la disposition visuelle de votre choix, tel que défini par un artiste.

Bien sûr, l’idéal est de présenter un état de rendu, en dépit de plusieurs images. Jeux de CocosSharp pour ce faire, en combinant toutes les images dans un seul fichier, puis charger qu’un fichier (accompagnée de sa **.plist** fichier) dans un `CCSpriteSheet`. À l’aide de la `CCSpriteSheet` classe devient encore plus important pour les jeux qui ont un grand nombre d’images, ou qui ont très compliqué de dispositions. 

### <a name="load-times"></a>Temps de chargement

Combinaison de plusieurs images en un seul fichier améliore également les temps de chargement d’un jeu pour plusieurs raisons :

 - Combinaison de plusieurs images dans un seul fichier peut réduire le nombre total de pixels utilisés par le biais livraison efficace
 - Le chargement des fichiers moins signifie moins de surcharge par fichier, telles que l’analyse des en-têtes de .png
 - Le chargement des fichiers moins nécessite moins temps, ce qui est important pour un média basé sur le disque telles que les DVD et les disques durs des ordinateurs traditionnelles de recherche

## <a name="using-ccspritesheet-in-code"></a>Dans le code à l’aide de CCSpriteSheet

Pour créer un `CCSpriteSheet` instance, le code doit fournir une image et un fichier qui définit les régions de l’image à utiliser pour chaque trame. L’image peut être chargé comme un **.png** ou **.xnb** fichier (si vous utilisez le [Pipeline de contenu](~/graphics-games/cocossharp/content-pipeline/index.md)). Le fichier définissant les frames est un **.plist** fichier qui peut être créé manuellement ou *TexturePacker* (dont nous parlerons ci-dessous).

L’exemple d’application, ce qui [peut être téléchargé ici](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), crée le `CCSpriteSheet` à partir d’un **.png** et **.plist** de fichiers utilisant le code suivant :

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Une fois chargé, le `CCSpriteSheet` contient un `List` de `CCSpriteFrame` instances – chaque instance correspondant à une des images source utilisés pour créer la feuille entière. Dans le cas de la **SpriteSheetDemo** projet, le `CCSpriteSheet` contient trois images. Le **.plist** fichier peut être inspecté dans Visual Studio pour Mac ou dans n’importe quel éditeur de texte pour afficher les images sont disponibles. Si nous permet d’afficher le **.plist** fichier dans un éditeur de texte que nous pouvons voir trois cadres (sections omises pour mettre en évidence les noms de clé) :


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

Nous pouvons utiliser le `Find` pour trouver des images par nom, comme suit (méthode) (code omis pour mettre en évidence `CCSpriteFrame` utilisation) :


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Dans la mesure où le `CCSprite` constructeur peut prendre un `CCSpriteFrame` paramètre, le code n’a jamais examiner les détails de la `CCSpriteFrame`, tels que les textures, il utilise, ou la région de l’image dans la feuille principale sprite.


## <a name="creating-a-sprite-sheet-plist"></a>Création d’un .plist de feuille du sprite

Le fichier .plist est un fichier basé sur xml, qui peut être créé et modifié manuellement. De même, programmes de retouche d’image peut être utilisé pour combiner plusieurs fichiers dans un fichier plus volumineux. Depuis la création et la gestion des feuilles de sprite peuvent prendre beaucoup de temps, nous allons examiner le programme TexturePacker qui peut exporter des fichiers au format CocosSharp. TexturePacker offre un gratuit et une version « Pro » et est disponible pour Windows et Mac OS. Le reste de ce guide peut être suivi à l’aide de la version gratuite. 

TexturePacker peut être [téléchargé depuis le site Web TexturePacker](https://www.codeandweb.com/texturepacker). L’ouverture TexturePacker n’a pas d’un projet est chargé. L’écran de démarrage permet d’ajouter des sprites, ouvrez des projets récents (si les autres projets ont été créés) et sélectionnez le format à utiliser pour la feuille de sprite. CocosSharp utilise le Format de données Cocos2D :

![](ccspritesheet-images/image7.png "CocosSharp utilise le Format de données Cocos2D")

Fichiers image (tel que **.png**) peut être ajouté à TexturePacker par glisser-déplacer les à partir de l’Explorateur Windows sur Windows ou Finder sur Mac. TexturePacker met automatiquement à jour l’aperçu de la feuille sprite chaque fois qu’un fichier est ajouté :

![](ccspritesheet-images/image8.png "TexturePacker met automatiquement à jour l’aperçu de la feuille sprite chaque fois qu’un fichier est ajouté.")

Pour exporter une feuille de sprite, cliquez sur le **feuille du sprite publier** bouton et sélectionnez un emplacement pour la feuille sprite. TexturePacker enregistre un fichier .plist et un fichier image.

Pour utiliser les fichiers résultants, ajoutez le .png et .plist à un projet CocosSharp. Pour plus d’informations sur l’ajout de fichiers aux projets de CocosSharp, consultez le [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md). Une fois que les fichiers sont ajoutés, ils peuvent être chargés dans un `CCSpriteSheet` comme indiqué précédemment dans le code ci-dessus :

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>Remarques sur la gestion d’une feuille de sprite TexturePacker

Lors du développement de jeux, les artistes peuvent ajouter, supprimer ou modifier art. Toute modification requiert une feuille sprite mis à jour. Les considérations suivantes peuvent faciliter la maintenance de feuille sprite :

 - Conserver les fichiers d’origine (les fichiers utilisés pour créer les feuilles de sprite) dans un dossier dans votre projet et assurez-vous qu’ils sont ajoutés au contrôle de version. Ces fichiers sont nécessaires pour recréer la feuille sprite chaque fois qu’une modification est apportée.
 - Ne pas ajouter les fichiers d’origine dans Visual Studio pour Mac/Visual Studio ou s’ils sont ajoutés, définissez le **Action de génération** à **aucun**. Si les fichiers sont ajoutés et ont le spécifiques à la plateforme **Action de génération**, puis ils augmentent inutilement la taille du fichier de l’application résultante.
 - Envisagez d’utiliser *intelligente dossiers* dans TexturePacker. Dossiers intelligents ajoutent automatiquement des images de relation contenant-contenus à la feuille de sprite. Cette fonctionnalité peut enregistrer beaucoup de temps lorsque développement de jeux avec un grand nombre d’images. 

    ![](ccspritesheet-images/image9.png "Cette fonctionnalité peut gagner beaucoup de temps lorsque développement de jeux avec un grand nombre d’images")
 - Gardez un œil sur les tailles de texture feuille sprite. Certains matériels anciens de téléphone ne prend pas en charge les tailles de texture supérieures à 2048 x 2048. En outre, une image 32 bits de 2048 x 2048 utilise presque 17 Mo de RAM – une quantité importante de mémoire.
 - TexturePacker n’inclut pas les dossiers dans les noms de sprite par défaut, les conflits de nom sont possibles. Il est préférable de choisir pour inclure le dossier des noms ou pas au début du développement. Jeux plus volumineux doivent envisager d’utiliser des noms de dossier pour éviter les conflits. Pour inclure les chemins d’accès de dossier, cliquez sur **afficher les options avancées** dans le **données** section et vérifier **nom du dossier Prepend**. 

    ![](ccspritesheet-images/image10.png "Pour inclure les chemins d’accès de dossier, cliquez sur Afficher les options avancée dans la section de données et vérifiez le nom du dossier Prepend")

## <a name="summary"></a>Récapitulatif

Ce guide explique comment créer et utiliser le `CCSpriteSheet` classe. Il décrit également comment construire des fichiers qui peuvent être chargées dans `CCSpriteSheet` instances à l’aide du programme TexturePacker.

## <a name="related-links"></a>Liens connexes

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Démonstration complète (exemple)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
