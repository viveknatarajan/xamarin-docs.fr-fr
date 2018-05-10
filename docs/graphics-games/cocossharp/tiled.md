---
title: À l’aide de mosaïque avec CocosSharp
description: Mosaïque est un outil puissant, flexible et une application déjà rodée pour la création de vignette orthogonal et isométrique est mappé pour les jeux. CocosSharp fournit une intégration intégrée pour le format de fichier natif de mosaïque.
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 8a7782097324829b8150b968c5658a864d1fab4a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="using-tiled-with-cocossharp"></a>À l’aide de mosaïque avec CocosSharp

_Mosaïque est un outil puissant, flexible et une application déjà rodée pour la création de vignette orthogonal et isométrique est mappé pour les jeux. CocosSharp fournit une intégration intégrée pour le format de fichier natif de mosaïque._

Le *mosaïque* application est une norme pour la création de *vignette maps* pour une utilisation dans le développement de jeux. Ce guide vous guide dans comment prendre un fichier .tmx existant (fichier créé en mosaïque) et l’utiliser dans un jeu de CocosSharp. Plus précisément, ce guide couvre :

 - L’objectif de mappages de vignette
 - Utilisation des fichiers de .tmx
 - Considérations relatives à la pointe de pixel de rendu
 - À l’aide des propriétés des mosaïques lors de l’exécution

Lors de la fin, il nous faudra la démonstration suivante :

![](tiled-images/image1.png "L’application de démonstration créée en suivant les étapes décrites dans ce guide")


## <a name="the-purpose-of-tile-maps"></a>L’objectif de mappages de vignette

Mappages de vignette aurait dans le développement de jeux des dizaines d’années, mais sont toujours utilisés dans les jeux 2D de l’esthétique et l’efficacité. Mappages de vignette sont en mesure d’atteindre un niveau très élevé de l’efficacité grâce à l’utilisation de jeux de mosaïque : l’image source utilisée par vignette correspond. Un ensemble de la vignette est une collection d’images combinés en un seul fichier. Bien que les jeux de vignette font référence à des images utilisées dans les mappages de mosaïque, des fichiers qui contiennent plusieurs images plus petites sont également appelées des feuilles de sprite ou sprite est mappé dans le développement de jeux. Nous pouvons visualiser l’utilisation des jeux de vignette en ajoutant une grille à l’ensemble de la vignette que nous utilisons dans notre démonstration :

![](tiled-images/image2.png "Une vue visualisée de l’utilisation des jeux de vignette en ajoutant une grille à l’ensemble de mosaïque qui sera utilisé dans la démonstration")

Mappages de vignette réorganiser les vignettes individuels à partir de jeux de vignette. Nous devons Notez que chaque mappage de la vignette n’a pas besoin stocker sa propre copie de la vignette définir – au lieu de cela, plusieurs mappages de vignette peuvent référencer le même jeu de vignette. Cela signifie que, à l’exception de l’ensemble de la vignette, mappages de vignette nécessitent très peu de mémoire. Cela permet la création d’un grand nombre de cartes de mosaïque, même lorsqu’ils sont utilisés pour créer une zone de jeu de grande taille, comme un [défilement Platforming](http://en.wikipedia.org/wiki/Platform_game) environnement. L’exemple suivant montre des dispositions possibles, utilisant le même jeu de mosaïque :

![](tiled-images/image3.png "Cette image montre des dispositions possibles, utilisant le même jeu de vignette")

![](tiled-images/image4.png "Cette image montre des dispositions possibles, utilisant le même jeu de vignette")


## <a name="working-with-tmx-files"></a>Utilisation des fichiers de .tmx

Le format de fichier .tmx est un fichier XML créé par l’application de mosaïque, ce qui peut être [téléchargé gratuitement sur le site Web de mosaïque](http://www.mapeditor.org/). Le format de fichier .tmx stocke les informations pour les mappages de vignette. Un jeu ont généralement un fichier .tmx pour chaque zone niveau ou distinct.

Ce guide se concentre sur l’utilisation des fichiers .tmx existants dans CocosSharp ; Toutefois, d’autres didacticiels sont accessibles en ligne, notamment [cette introduction à l’éditeur de mappage de mosaïque](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838).

Vous devez décompresser le [fichier zip contenu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) pour l’utiliser dans le jeu. La première chose à noter est que vignette maps utilisent le fichier .tmx (dungeon.tmx), ainsi qu’un ou plusieurs fichiers d’image qui définissent la mosaïque de définir des données (dungeon_1.png). Le jeu doit inclure ces deux fichiers à charger le .tmx lors de l’exécution, donc ajouter à la fois dans le fichier **contenu** dossier (qui est contenu dans le **actifs** dossier dans les projets Android). En particulier, ajoutez les fichiers dans un dossier appelé **tilemaps** à l’intérieur de la **contenu** dossier :

![](tiled-images/image5.png "Ajouter les fichiers à un dossier appelé tilemaps dans le dossier de contenu")

Le **dungeon.tmx** fichier peut maintenant être chargé lors de l’exécution dans un `CCTileMap` objet. Ensuite, modifiez le `GameLayer` (ou un objet conteneur racine équivalent) pour contenir un `CCTileMap` instance et de l’ajouter en tant qu’enfant :


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

Si nous exécutons la partie que nous allons voir la vignette s’affiche dans le coin inférieur gauche de l’écran :

![](tiled-images/image6.png "Si le jeu est exécuté, la vignette s’affiche dans le coin inférieur gauche de l’écran")


## <a name="considerations-for-rendering-pixel-art"></a>Considérations relatives à la pointe de pixel de rendu

Illustration du pixel, dans le cadre du développement de jeux vidéo, fait référence à 2D art visual qui est généralement créé par disponible et est souvent basse résolution. Illustration du pixel peut être de manière restrictive intensif création pixel art vignette jeux inclure souvent des vignettes basse résolution, par exemple 16 ou 32 largeur en pixels et la hauteur du temps. Si ne pas à l’échelle lors de l’exécution, art de pixel est souvent trop petite pour les tablettes et téléphones les plus récents.

Nous pouvons ajuster les dimensions affichées dans fichier de GameAppDelegate.cs de notre jeu, où nous allons ajouter un appel à `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

Pour plus d’informations sur `CCSceneResolutionPolicy`, consultez notre guide sur [la gestion des solutions dans CocosSharp](~/graphics-games/cocossharp/resolutions.md).

Si nous exécutons le jeu maintenant, nous verrons le jeu occupe le plein écran de votre appareil :

![](tiled-images/image7.png "Le jeu occupe le plein écran de l’appareil")

Enfin, nous vous souhaitez désactivation de l’anticrénelage sur notre carte de vignette. Le `Antialiased` propriété applique un effet de flou lors du rendu des objets qui sont un zoom avant. L’anticrénelage peut être utile pour réduire l’aspect de la granularité des objets graphiques, mais peut également introduire des artefacts de rendu. Plus précisément, l’anticrénelage flou sur le contenu de chaque vignette. Toutefois, les bords de chaque vignette ne sont pas floues, ce qui rend les vignettes individuels ressortir plutôt que de fusion avec des mosaïques adjacentes. Notez également que les jeux d’art pixel conservent souvent leur apparence régularité pour maintenir un *rétro* sentir.

Définissez `Antialiased` à `false` après la construction du `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

Maintenant notre carte vignette s’affiche pas flou :

![](tiled-images/image8.png "Maintenant le mappage de la vignette n’apparaîtront pas flou")


## <a name="using-tile-properties-at-runtime"></a>À l’aide des propriétés des mosaïques lors de l’exécution

Jusqu'à présent, nous avons un `CCTileMap` chargement d’un fichier .tmx et de les afficher, mais nous ne pouvons pas interagir avec elle. Plus précisément, certaines vignettes (par exemple, notre coffre Trésor) doivent disposer d’une logique personnalisée. Nous allons parcourir comment détecter les propriétés de vignette personnalisée et les différentes façons de réagir à ces propriétés une fois identifiées lors de l’exécution.

Avant d’écrire du code, nous nous devrons ajouter des propriétés à notre carte vignette via la mosaïque. Pour ce faire, ouvrez le fichier dungeon.tmx dans le programme de mosaïque. Veillez à ouvrir le fichier qui est utilisé dans le projet de jeu.

Une fois qu’ouvert, sélectionnez le coffre trésor dans la vignette définie pour afficher ses propriétés :

![](tiled-images/image9.png "Une fois qu’ouvert, sélectionnez le coffre trésor dans la vignette définie pour afficher ses propriétés")

Si les propriétés de coffres Trésor n’apparaissent pas, avec le bouton droit sur le coffre Trésor et sélectionnez **propriétés des mosaïques**:

![](tiled-images/image10.png "Si les propriétés de coffres Trésor n’apparaissent pas, avec le bouton droit sur le coffre Trésor et sélectionnez les propriétés des mosaïques")

Les propriétés en mosaïque sont implémentées avec un nom et une valeur. Pour ajouter une propriété, cliquez sur le **+** bouton, entrez le nom **IsTreasure**, cliquez sur **OK**, puis entrez la valeur **true**: 

![](tiled-images/image11.png "Pour ajouter une propriété, cliquez sur le bouton, entrez le nom IsTreasure, cliquez sur OK, puis entrez la valeur true")

N’oubliez pas d’enregistrer le fichier .tmx après la modification des propriétés.

Enfin, nous allons ajouter le code pour rechercher la propriété qui vient d’être ajouté. Nous parcourt chaque `CCTileMapLayer` (notre plan possède 2 couches), puis à chaque ligne et colonne à rechercher toutes les mosaïques qui ont le `IsTreasure` propriété :


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

La plupart du code est explicite, mais nous devons traitent de la gestion des vignettes du Trésor. Dans ce cas, nous allons supprimer toutes les mosaïques qui sont identifiés comme coffre au Trésor. Il s’agit, car les coffres Trésor aurez probablement besoin de code personnalisé lors de l’exécution d’une collision effet et passer le lecteur le contenu de la Trésor ouverture. En outre, le Trésor devrez réagir à un état ouvert (modifier son apparence visuelle) et peut avoir une logique pour uniquement lorsque apparaissant à l’écran toutes les ennemis ont été invalidées.

En d’autres termes, le coffre Trésor bénéficieront en cours d’une entité plutôt qu’une vignette simple dans le `CCTileMap`. Pour plus d’informations sur les entités de jeu, consultez le [guident des entités dans CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas explique comment charger des fichiers de .tmx créés en mosaïque dans une application CocosSharp. Il montre comment modifier la résolution de l’application pour prendre en compte l’art de faible résolution pixels et comment rechercher des vignettes par leurs propriétés pour exécuter une logique personnalisée, comme la création d’instances d’entité.

## <a name="related-links"></a>Liens connexes

- [Site Web en mosaïque](http://www.mapeditor.org/)
- [Contenu zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
