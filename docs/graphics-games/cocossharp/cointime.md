---
title: Coin des détails de jeu de temps
description: Ce guide décrit les détails d’implémentation dans le jeu de temps de la médaille, y compris l’utilisation des mappages de vignette, créer des entités, animation de sprites et l’implémentation de collision efficace.
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: af914e10eb93aa0668743a113ffe647a939fea75
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112622"
---
# <a name="coin-time-game-details"></a>Coin des détails de jeu de temps

_Ce guide décrit les détails d’implémentation dans le jeu de temps de la médaille, y compris l’utilisation des mappages de vignette, créer des entités, animation de sprites et l’implémentation de collision efficace._

Heure de la médaille est un jeux de plateforme complète jeu pour iOS et Android. L’objectif du jeu consiste à collecter toutes les pièces d’un niveau et puis atteindre le capot de sortie tout en évitant les ennemis et obstacles.

![](cointime-images/image1.png "L’objectif du jeu consiste à collecter toutes les pièces d’un niveau et puis atteindre le capot de sortie tout en évitant les ennemis et obstacles")

Ce guide décrit les détails d’implémentation dans le temps de la médaille, couvrant les rubriques suivantes :

- [Utilisation des fichiers de tmx](#working-with-tmx-files)
- [Le chargement de niveau](#level-loading)
- [Ajout de nouvelles entités](#adding-new-entities)
- [Entités animées](#animated-entities)


## <a name="content-in-coin-time"></a>Contenu dans le temps de la médaille

Heure de la médaille est un exemple de projet qui représente la façon dont un projet CocosSharp complet peut être organisé. Coin Time structure vise à simplifier l’ajout et la maintenance du contenu. Il utilise **.tmx** fichiers créés par [mosaïque](http://www.mapeditor.org) pour les niveaux et les fichiers XML pour définir des animations. Modification ou ajout de nouveau contenu peut être obtenue avec un minimum d’effort. 

Bien que cette approche rend médaille temps un projet efficace pour learning et l’expérimentation, il reflète également les jeux professionnels comment sont effectuées. Ce guide explique certaines des approches prises pour simplifier l’ajout et modification de contenu.


## <a name="working-with-tmx-files"></a>Utilisation des fichiers de tmx

Niveaux de temps en monnaie sont définis en utilisant le format de fichier .tmx, qui est sorti par le [mosaïque](http://www.mapeditor.org) éditeur de carte de vignette. Pour obtenir une présentation détaillée de l’utilisation de la mosaïque, consultez le [utilisation de Tiled avec Cocos Sharp guide](~/graphics-games/cocossharp/tiled.md). 

Chaque niveau est défini dans son propre fichier .tmx contenu dans le **CoinTime/ressources/contenu/niveaux** dossier. Tous les niveaux de temps de la médaille partagent un seul fichier tileset, qui est défini dans le **mastersheet.tsx** fichier. Ce fichier définit les propriétés personnalisées pour chaque mosaïque, telles que si la vignette a solide collision ou si la vignette doit être remplacée par une instance d’entité. Le fichier mastersheet.tsx permet de propriétés défini une seule fois et utilisable sur tous les niveaux. 


### <a name="editing-a-tile-map"></a>Modification d’un mappage de vignette

Pour modifier un mappage de vignette, ouvrez le fichier de .tmx dans la mosaïque en double-cliquant sur le fichier .tmx ou en ouvrant il via le menu fichier dans la mosaïque. Coin Time vignette niveau cartes contiennent trois couches : 

- **Entités** – cette couche contient des vignettes qui seront remplacés avec des instances d’entités lors de l’exécution. Exemples incluent le joueur, pièces, ennemis et la porte de la fin de niveau.
- **Terrain** – cette couche contient des vignettes qui ont généralement une collision solide. Collision solide permet au joueur de montrent comment sur ces vignettes sans passer. Vignettes avec solide collision peuvent également agir comme murs et des plafonds.
- **Arrière-plan** – la couche d’arrière-plan contient des vignettes qui sont utilisés comme arrière-plan statique. Cette couche ne défile pas lorsque l’appareil photo se déplace dans le niveau, en donnant l’impression de profondeur via parallaxe.

Comme nous explorerons plus tard, le code de chargement de niveau attend ces trois couches dans tous les niveaux de temps de la monnaie.

#### <a name="editing-terrain"></a>Modification de terrain

Vignettes peuvent être placés en cliquant dans le **mastersheet** tileset et puis en cliquant sur la vignette de carte. Par exemple, pour peindre le nouveau terrain dans un niveau :

1. Sélectionnez la couche de Terrain
1. Cliquez sur la vignette pour dessiner
1. Cliquez sur ou push et faites glisser la carte pour peindre la vignette

    ![](cointime-images/image2.png "Cliquez sur la vignette pour dessiner 1")

L’angle supérieur gauche de la tileset contient tous les du terrain dans le temps de la monnaie. Inclut un terrain, ce qui est solid, la **SolidCollision** propriété, comme indiqué dans les propriétés de la vignette sur la gauche de l’écran :

![](cointime-images/image3.png "Terrain, ce qui est solid, inclut la propriété SolidCollision, comme indiqué dans les propriétés de la vignette sur la gauche de l’écran")

#### <a name="editing-entities"></a>Modification des entités

Les entités peuvent être ajoutées ou supprimées à partir d’un niveau – à l’instar du terrain. Le **mastersheet** tileset a toutes les entités sont placées sur à mi-chemin horizontalement, afin qu’ils ne sont pas forcément visibles sans faire défiler vers la droite :

![](cointime-images/image4.png "Le tileset mastersheet a toutes les entités sont placées sur à mi-chemin horizontalement, afin qu’ils ne sont pas forcément visibles sans faire défiler vers la droite")

Nouvelles entités doivent être placées sur le **entités** couche.

![](cointime-images/image5.png "Nouvelles entités doivent être placées sur la couche d’entités")

CoinTime code recherche le **EntityType** lorsqu’un niveau est chargé d’identifier les vignettes qui doivent être remplacés par les entités : 

![](cointime-images/image6.png "Code de CoinTime recherche l’EntityType quand un niveau est chargé d’identifier les vignettes qui doivent être remplacés par des entités")

Une fois que le fichier a été modifié et enregistré, les modifications apparaissent automatiquement si le projet est généré et exécuté :

![](cointime-images/image7.png "Une fois que le fichier a été modifié et enregistré, les modifications apparaissent automatiquement si le projet est généré et exécuter")


### <a name="adding-new-levels"></a>Ajout de nouveaux niveaux

Le processus d’ajout de niveaux à l’heure de la médaille nécessite aucune modification de code et uniquement quelques modifications mineures au projet. Pour ajouter un nouveau niveau :

1. Ouvrez le dossier de niveau qui se trouve à <**CoinTime Root > \CoinTime\Assets\Content\levels**
1. Copiez et collez un des niveaux, tel que **level0.tmx**
1. Renommez le nouveau fichier .tmx afin qu’il continue la séquence de nombres niveau avec les niveaux existants, tels que **level8.tmx**
1. Dans Visual Studio ou Visual Studio pour Mac, ajoutez le nouveau fichier .tmx dans le dossier des niveaux d’Android. Vérifiez que le fichier utilise le **AndroidAsset** action de génération.

    ![](cointime-images/image8.png "Vérifiez que le fichier utilise l’action de génération AndroidAsset")

1. Ajouter le nouveau fichier .tmx dans le dossier de niveaux d’iOS. Veillez à lier le fichier à partir de son emplacement d’origine et vérifiez qu’il utilise le **BundleResource** action de génération.

    ![](cointime-images/image9.png "Veillez à lier le fichier à partir de son emplacement d’origine et vérifiez qu’il utilise l’action de génération BundleResource")

Le nouveau niveau doit apparaître dans l’écran Sélectionnez niveau en tant que niveau 9 (niveau des noms de fichiers commencent à 0, mais les boutons niveau commencent par le numéro 1) :

![](cointime-images/image10.png "Le nouveau niveau doit apparaître dans l’écran Sélectionnez niveau comme commence au niveau du 9 au niveau fichier à 0, mais les boutons niveau commencent par le numéro 1")


## <a name="level-loading"></a>Le chargement de niveau

Comme indiqué précédemment, les nouveaux niveaux ne nécessitent aucune modification dans le code : le jeu détecte automatiquement les niveaux si elles sont correctement nommés et ajoutés à la **niveaux** dossier avec l’action de génération correcte (**BundleResource**ou **AndroidAsset**).

La logique permettant de déterminer le nombre de niveaux est contenue dans le `LevelManager` classe. Lorsqu’une instance de la `LevelManager` est généré (en utilisant le modèle singleton), le `DetermineAvailbleLevels` méthode est appelée :


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

CocosSharp ne fournit pas une approche multiplateforme permettant de détecter si les fichiers sont présents, donc nous devons nous appuyer sur la `TitleContainer` classe tente d’ouvrir un flux de données. Si le code pour l’ouverture d’un flux de données lève une exception, puis le fichier n'existe pas et la boucle while sauts. Une fois que la boucle se termine, le `NumberOfLevels` propriété indique le nombre de niveaux valid font partie du projet.

Le `LevelSelectScene` classe utilise le `LevelManager.NumberOfLevels` pour déterminer combien de boutons pour créer dans le `CreateLevelButtons` méthode :


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

Le `NumberOflevels` propriété est utilisée pour déterminer les boutons qui doivent être créées. Ce code considère que la page en cours d’affichage de l’utilisateur et crée uniquement un maximum de six boutons par page. Lorsque vous cliquez dessus, le bouton instances appel le `HandleButtonClicked` méthode :


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Cette méthode attribue le `CurrentLevel` propriété qui est utilisée par le `GameScene` lors du chargement d’un niveau. Après avoir défini le `CurrentLevel`, le `GoToGameScene` méthode est déclenchée, la scène à partir de commutation `LevelSelectScene` à `GameScene`.

Le `GameScene` constructeur appelle `GoToLevel`, qui effectue la logique au niveau du chargement :


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

Ensuite, nous allons examiner les méthodes appelées dans `GoToLevel`.


### <a name="loadlevel"></a>LoadLevel

Le `LoadLevel` méthode est responsable du chargement du fichier .tmx et en l’ajoutant à la `GameScene`. Cette méthode ne crée pas de tous les objets interactifs tels que la collision ou entités : elle crée simplement les éléments visuels pour le niveau, également appelé le *environnement*.


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

Le `CCTileMap` constructeur accepte un nom de fichier est créé en utilisant le numéro de niveau passé à `LoadLevel`. Pour plus d’informations sur la création et utilisation de `CCTileMap` instances, consultez le [utilisation de Tiled avec CocosSharp guide](~/graphics-games/cocossharp/tiled.md).

Actuellement, CocosSharp n’autorise pas la réorganisation des couches sans supprimer puis ajouter de nouveau à leur parent `CCScene` (c'est-à-dire le `GameScene` dans ce cas), de sorte que les dernières lignes de la méthode sont nécessaires pour réorganiser les couches.


### <a name="createcollision"></a>CreateCollision

Le `CreateCollision` méthode constructions un `LevelCollision` instance qui est utilisé pour effectuer *collision solide* entre le lecteur et l’environnement.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Sans cette collision, le joueur comprises par le biais du niveau et le jeu est lu. Le lecteur vous permet de remonter au sol de collision solide et empêche le lecteur parcourant les murs ou le passage de traverser les plafonds.

Collision dans le temps de la médaille peut être ajoutée sans code supplémentaire : seules les modifications apportées aux fichiers en mosaïque. 


### <a name="processtileproperties"></a>ProcessTileProperties

Une fois qu’un niveau est chargé et la collision est créée, `ProcessTileProperties` est appelée pour exécuter une logique basée sur les propriétés des mosaïques. Médaille temps inclut un `PropertyLocation` struct pour définir des propriétés et les coordonnées de la vignette avec ces propriétés :


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

Ce struct est utilisé pour construire des instances d’entité de créer et supprimer des vignettes inutiles dans le `ProcessTileProperties` méthode :


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

La boucle foreach évalue chaque propriété de la vignette, vérifier si la clé est soit `EntityType` ou `RemoveMe`. `EntityType` Indique qu’une instance d’entité doit être créée. `RemoveMe` Indique que la vignette doit être complètement supprimée lors de l’exécution.

Si une propriété avec le `EntityType` clé est trouvée, `TryCreateEntity` est appelée, les tentatives de création d’une entité à l’aide de la correspondance de propriété le `EntityType` clé :


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


## <a name="adding-new-entities"></a>Ajout de nouvelles entités

Heure de la médaille utilise le modèle d’entité pour ses objets de jeu (la démarche à suivre le [guident des entités dans CocosSharp](~/graphics-games/cocossharp/entities.md)). Héritent de toutes les entités `CCNode`, ce qui signifie qu’ils peuvent être ajoutés en tant qu’enfants de le `gameplayLayer`.

Chaque type d’entité est également référencé directement par le biais d’une liste ou d’une seule instance. Par exemple, le `Player` est référencé par le `player` champ et tous les `Coin` instances sont référencées dans un `coins` liste. Conservation des références directes à des entités (par opposition à y faire référence par le biais du `gameLayer.Children` liste) rend le code qui accède à ces entités plus faciles à lire et d’éliminer la conversion de type potentiellement coûteuse.

Le code existant fournit un certain nombre de types d’entités en tant qu’exemples montrant comment créer des entités. Les étapes suivantes peuvent servir à créer une nouvelle entité :


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - définir une nouvelle classe à l’aide du modèle d’entité

La seule exigence pour la création d’une entité consiste à créer une classe qui hérite de `CCNode`. La plupart des entités ont certains visuel, comme un `CCSprite`, qui doit être ajouté en tant qu’enfant de l’entité dans son constructeur.

CoinTime fournit la `AnimatedSpriteEntity` classe, ce qui simplifie la création d’entités animées. Animations seront abordées plus en détail dans le [section entités animée](#animated-entities).


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – ajouter une nouvelle entrée à l’instruction switch TryCreateEntity

Instances de la nouvelle entité doivent être instanciés dans le `TryCreateEntity`. Si l’entité requiert une logique de chaque image comme collision, intelligence artificielle ou lecture de l’entrée, puis le `GameScene` doit conserver une référence à l’objet. Si plusieurs instances sont nécessaires (telles que `Coin` ou `Enemy` instances), puis un nouveau `List` doivent être ajoutés à la `GameScene` classe.


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3 : modifier les propriétés des mosaïques pour la nouvelle entité

Une fois que le code prend en charge la création de la nouvelle entité, la nouvelle entité doit être ajouté à la tileset. Le tileset peut être modifiée en ouvrant n’importe quel niveau `.tmx` fichier. 

Le tileset est stockée distinct à partir de la .tmx dans le **mastersheet.tsx** de fichiers, de sorte qu’il doit être importé avant de pouvoir être modifié :

![](cointime-images/image11.png "Le tileset est stocké séparément du fichier .tsx, afin qu’il doit être importé avant de pouvoir être modifié")

Une fois importés, les propriétés sur les vignettes sélectionnées sont modifiables, et l’EntityType peut être ajouté :

![](cointime-images/image12.png "Une fois importés, les propriétés sur les vignettes sélectionnées sont modifiables, et l’EntityType peut être ajouté.")

Une fois que la propriété est créée, sa valeur peut être définie pour correspondre à la nouvelle `case` dans `TryCreateEntity`:

![](cointime-images/image13.png "Une fois que la propriété est créée, sa valeur peut être définie pour faire correspondre le nouveau cas dans TryCreateEntity")

Une fois que le tileset a été modifié, il doit être exporté, ce qui rend les modifications disponible pour tous les autres niveaux :

![](cointime-images/image14.png "Une fois que le tileset a été modifié, il doit être exporté")

Le tileset doit remplacer les existantes **mastersheet.tsx** tileset :

![](cointime-images/image15.png "HE tileset doit remplacer le tileset mastersheet.tsx existant")


## <a name="entity-tile-removal"></a>Suppression de vignette d’entité

Lorsqu’une carte de la vignette est chargée dans un jeu, les vignettes individuelles sont des objets statiques. Dans la mesure où les entités exigent un comportement personnalisé tel que le déplacement, code au moment de la médaille supprime les vignettes lorsque les entités sont créées.

`ProcessTileProperties` inclut une logique pour supprimer des vignettes qui créent des entités à l’aide de la `RemoveTile` méthode :


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

Cette suppression automatique des vignettes est suffisante pour les entités qui n'occupent qu’une seule vignette dans le tileset, telles que les pièces et ennemis. Entités plus volumineuses nécessitent des propriétés et une logique supplémentaire.

La porte nécessite deux vignettes à dessiner complètement :

![](cointime-images/image16.png "La porte nécessite deux vignettes à dessiner complètement")

La vignette en bas de la porte contient les propriétés pour la création d’une entité (**EntityType** définie sur **porte**) :

![](cointime-images/image17.png "La vignette en bas de la porte contient les propriétés pour la création d’une jeu d’entités EntityType à la porte")

Dans la mesure où uniquement la vignette en bas de la porte est supprimée lorsque l’instance de la porte est créée, une logique supplémentaire est nécessaire pour supprimer la vignette supérieure lors de l’exécution. La vignette supérieure a un **RemoveMe** propriété définie sur **true**:

![](cointime-images/image18.png "La vignette supérieure a une propriété RemoveMe définie sur true")



Cette propriété est utilisée pour supprimer des vignettes dans `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


## <a name="entity-offsets"></a>Décalages de l’entité

Entités créées à partir de vignettes sont positionnées en alignant le centre de l’entité avec le centre de la vignette. Entités plus volumineuses, telles que `Door`, utiliser des propriétés supplémentaires et la logique à placer correctement. 

La vignette de la porte de la partie inférieure, qui définit le `Door` placement de l’entité, spécifie un **décalage y** égale à 4. Sans cette propriété, le `Door` instance est placée au centre de la vignette :

![](cointime-images/image19.png "Sans cette propriété, l’instance de la porte est placé dans le centre de la vignette")

 

Ce problème est corrigé en appliquant la **décalage y** valeur dans `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


## <a name="animated-entities"></a>Entités animées

Durée de la médaille comprend plusieurs entités animées. Le `Player` et `Enemy` entités lire les animations de parcours et `Door` entité exécute une animation de l’ouverture une fois que toutes les pièces ont été collectés.


### <a name="achx-files"></a>fichiers .achx

Médaille temps animations sont définies dans les fichiers .achx. Chaque animation est définie entre `AnimationChain` balises, comme indiqué dans l’animation suivante définie dans **propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

Cette animation contient uniquement une seule image, ce qui entraîne l’entité de pic affichant une image statique. Les entités peuvent utiliser des fichiers de .achx si elles s’affichent les animations uniques ou plusieurs frames. Frames supplémentaires peuvent être ajoutés aux fichiers de .achx sans nécessiter de modification dans le code. 

Frames de définissent l’image à afficher dans le `TextureName` paramètre et les coordonnées de l’affichage dans le `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, et `BottomCoordinate` balises. Ils représentent les coordonnées en pixels de l’image d’animation dans l’image qui est utilisé : **mastersheet.png** dans ce cas.

![](cointime-images/image20.png "Ils représentent les coordonnées en pixels de l’image d’animation dans l’image")

Le `FrameLength` propriété définit le nombre de secondes pendant lesquelles une image de l’animation doit être affichée. Animations d’image unique ignorent cette valeur.

Toutes les autres propriétés dans le fichier .achx AnimationChain sont ignorées par heure de la monnaie.


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Logique de l’animation est contenue dans le `AnimatedSpriteEntity` (classe), qui sert de la classe de base pour la plupart des entités utilisées dans le `GameScene`. Il fournit les fonctionnalités suivantes :

 - Le chargement de `.achx` fichiers
 - Cache d’animation des animations chargées
 - Instance CCSprite pour l’affichage de l’animation
 - Logique permettant de modifier le frame actuel

Le constructeur de pics fournit un exemple montrant comment charger et utiliser des animations simple :


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

Le **propAnimations.achx** contient uniquement une animation, donc le constructeur accède à cette animation par index. Si un fichier .achx contient plusieurs animations, animations peuvent être référencées par nom, comme indiqué dans le `Enemy` constructeur :


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>Récapitulatif

Ce guide couvre les détails d’implémentation de temps de la monnaie. Heure de la médaille est créé pour être un jeu complet, mais est également un projet qui peut être facilement modifié et étendu. Lecteurs sont invités à dépenser temps apporté des modifications à des niveaux, ajout de nouveaux niveaux et la création de nouvelles entités pour mieux comprendre comment les temps de la médaille est implémentée.

## <a name="related-links"></a>Liens connexes

- [Projet de jeu (exemple)](https://developer.xamarin.com/samples/mobile/CoinTime/)
