---
title: "Détails d’implémentation de temps de prise en charge totale"
description: "Ce guide décrit les détails d’implémentation dans la partie heure de la prise en charge totale, y compris l’utilisation des mappages de vignette, créer des entités, animation sprites et l’implémentation de collision efficace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 2302f5f9cc24ec3eab6c84c20a37a93d687d692d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="coin-time-implementation-details"></a>Détails d’implémentation de temps de prise en charge totale

_Ce guide décrit les détails d’implémentation dans la partie heure de la prise en charge totale, y compris l’utilisation des mappages de vignette, créer des entités, animation sprites et l’implémentation de collision efficace._

Heure de la prise en charge totale est un Platforming complète jeu pour iOS et Android. L’objectif du jeu consiste à collecter toutes les pièces d’un niveau et puis atteindre le capot de sortie tout en évitant les ennemis et éventuels obstacles.

![](cointime-images/image1.png "L’objectif du jeu est de collecter toutes les pièces d’un niveau et puis atteindre le capot de sortie tout en évitant les obstacles et les ennemis")

Ce guide décrit les détails d’implémentation dans le temps de monnaie, couvre les rubriques suivantes :

- [Utilisation des fichiers de TMX](#Working_with_TMX_Files)
- [Niveau de chargement](#Level_Loading)
- [Ajout de nouvelles entités](#Adding_New_Entities)
- [Entités animées](#Animated_Entities)


# <a name="content-in-coin-time"></a>Contenu dans le temps de prise en charge totale

Heure de la prise en charge totale est un exemple de projet qui représente la façon dont un projet CocosSharp complet peut être organisé. L’heure de monnaie vise à simplifier l’ajout et la gestion de contenu de la structure. Il utilise **.tmx** fichiers créés par [mosaïque](http://www.mapeditor.org) pour les niveaux et les fichiers XML pour définir des animations. Modification ou ajout de nouveau contenu peut être obtenue avec un minimum d’effort. 

Même si cette approche monnaie fois un projet efficace pour l’apprentissage automatique et d’expérimentation, il reflète également le professional jeux sont effectuées. Ce guide décrit certaines des approches adoptées pour simplifier l’ajout et modification de contenu.


# <a name="working-with-tmx-files"></a>Utilisation des fichiers de TMX

Niveaux de prise en charge totale de temps sont définies en utilisant le format de fichier .tmx, qui est généré par le [mosaïque](http://www.mapeditor.org) éditeur de carte de vignette. Pour obtenir une présentation détaillée de l’utilisation de la mosaïque, consultez la [à l’aide de mosaïque avec guide Cocos aigu](~/graphics-games/cocossharp/tiled.md). 

Chaque niveau est défini dans son propre fichier .tmx contenu dans le **CoinTime/actifs/contenu/niveaux** dossier. Tous les niveaux de prise en charge totale temps partagent un fichier tileset, qui est défini dans le **mastersheet.tsx** fichier. Ce fichier définit les propriétés personnalisées pour chaque vignette, telles que si la vignette a collision solide ou si la vignette doit être remplacée par une instance d’entité. Le fichier mastersheet.tsx permet de propriétés défini une seule fois et l’utilisation sur tous les niveaux. 


## <a name="editing-a-tile-map"></a>Modification d’un mappage de vignette

Pour modifier un mappage de vignette, ouvrez le fichier de .tmx dans la mosaïque en double-cliquant sur le fichier .tmx ou en ouvrant il via le menu fichier dans la mosaïque. Heure de la monnaie vignette niveau cartes contiennent trois couches : 

- **Entités** – cette couche contient des vignettes qui seront remplacés par les instances des entités lors de l’exécution. Le lecteur, pièces, ennemis et la porte de niveau de fin sont des exemples.
- **Terrain** – cette couche contient des vignettes qui ont généralement une collision solide. Collision solide permet du guider tout au long de ces vignettes sans passer le lecteur. Vignettes avec collision solide peuvent également agir en tant que murs et des plafonds.
- **Arrière-plan** – la couche d’arrière-plan contient des vignettes qui sont utilisés comme arrière-plan statique. Cette couche ne défile pas lorsque l’appareil photo se déplace dans l’ensemble du niveau, en donnant l’impression de profondeur via parallaxe.

Comme nous allons examiner ultérieurement, le code de chargement de niveau attend que ces trois couches de tous les niveaux de prise en charge totale temps.

### <a name="editing-terrain"></a>Modification de Terrain
Vignettes peuvent être placés en cliquant dans le **mastersheet** tileset et puis en cliquant sur la vignette mappent. Par exemple, pour peindre terrain nouveau dans un niveau :

1. Sélectionnez la couche de Terrain
1. Cliquez sur la vignette pour dessiner
1. Cliquez sur ou push, faites glisser sur la carte pour peindre la vignette


    ![](cointime-images/image2.png "Cliquez sur la vignette pour dessiner 1")

 

Le coin supérieur gauche de la tileset contient tous les de terrain dans le temps de prise en charge totale. Terrain, qui est pleine, inclut la **SolidCollision** propriété, comme indiqué dans les propriétés de la vignette sur la gauche de l’écran :

![](cointime-images/image3.png "Terrain, qui est pleine, inclut la propriété SolidCollision, comme indiqué dans les propriétés de la vignette sur la gauche de l’écran")

### <a name="editing-entities"></a>Modification des entités
Entités peuvent être ajoutées ou supprimées d’un niveau – comme terrain. Le **mastersheet** tileset a toutes les entités placées sur la moitié horizontalement, afin qu’ils ne sont pas forcément visibles sans défilement vers la droite :

![](cointime-images/image4.png "Le tileset mastersheet a toutes les entités placées sur la moitié horizontalement, afin qu’ils ne sont pas forcément visibles sans défilement vers la droite")

Nouvelles entités doivent être placées sur le **entités** couche.

![](cointime-images/image5.png "Nouvelles entités doivent être placées sur la couche d’entités")

Code de CoinTime recherche le **EntityType** lorsqu’un niveau est chargé pour identifier des mosaïques qui doivent être remplacés par les entités : 

![](cointime-images/image6.png "Code de CoinTime recherche l’EntityType lors du chargée d’un niveau pour identifier des mosaïques qui doivent être remplacés par des entités")

Une fois que le fichier a été modifié et enregistré, les modifications seront affichent automatiquement si le projet est généré et exécuté :

![](cointime-images/image7.png "Une fois que le fichier a été modifié et enregistré, les modifications seront affichent automatiquement si le projet est généré et exécuté")


## <a name="adding-new-levels"></a>Ajout de nouveaux niveaux

Le processus d’ajout des niveaux au moment de la prise en charge totale nécessite aucune modification de code et seuls quelques petites modifications au projet. Pour ajouter un nouveau niveau :

1. Ouvrez le dossier de niveau qui se trouve à <**CoinTime Root > \CoinTime\Assets\Content\levels**
1. Copiez et collez un des niveaux, tel que **level0.tmx**
1. Renommez le nouveau fichier .tmx afin qu’il continue de la séquence du numéro de niveau avec les niveaux existants, tels que **level8.tmx**
1. Dans Visual Studio ou Visual Studio pour Mac, ajoutez le nouveau fichier .tmx dans le dossier de niveaux Android. Vérifiez que le fichier utilise le **AndroidAsset** action de génération.


    ![](cointime-images/image8.png "Vérifiez que le fichier utilise l’action de génération AndroidAsset")


1. Ajouter le nouveau fichier .tmx dans le dossier de niveaux iOS. Veillez à lier le fichier à partir de son emplacement d’origine et vérifiez qu’il utilise le **BundleResource** action de génération.


    ![](cointime-images/image9.png "Veillez à lier le fichier à partir de son emplacement d’origine et vérifiez qu’il utilise l’action de génération BundleResource")


Le nouveau niveau doit apparaître dans l’écran Sélectionnez niveau en tant que niveau 9 (au niveau des noms de fichiers commencent à 0, mais les boutons niveau commencent par le numéro 1) :

![](cointime-images/image10.png "Le nouveau niveau doit apparaître dans l’écran de niveau sélectionnez comme début de noms de fichiers au niveau niveau 9 à 0, mais les boutons niveau commencent par le numéro 1")


# <a name="level-loading"></a>Niveau de chargement

Comme indiqué précédemment, les nouveaux niveaux ne nécessitent aucune modification dans le code : le jeu détecte automatiquement les niveaux s’ils sont nommés correctement et ajoutés à la **niveaux** dossier avec l’action de génération correcte (**BundleResource**ou **AndroidAsset**).

La logique permettant de déterminer le nombre de niveaux est contenue dans la `LevelManager` classe. Lorsqu’une instance de la `LevelManager` est généré (en utilisant le modèle de singleton), la `DetermineAvailbleLevels` méthode est appelée :


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

CocosSharp ne fournit pas une approche multiplateforme pour détecter si les fichiers sont présents, afin que nous s’appuyer sur la `TitleContainer` classe tente d’ouvrir un flux de données. Si le code de l’ouverture d’un flux de données lève une exception, puis le fichier inexistant et la boucle while sauts. Une fois que la boucle se termine, le `NumberOfLevels` propriété indique le nombre de niveaux valid font partie du projet.

Le `LevelSelectScene` classe utilise le `LevelManager.NumberOfLevels` pour déterminer le nombre de boutons pour créer dans le `CreateLevelButtons` méthode :


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

Le `NumberOflevels` propriété est utilisée pour déterminer les boutons qui doivent être créés. Ce code considère que la page de l’utilisateur est affichée et permet uniquement de créer un maximum de six boutons par page. Lorsque vous cliquez dessus, le bouton instances appel le `HandleButtonClicked` méthode :


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Cette méthode attribue le `CurrentLevel` propriété qui est utilisée par le `GameScene` lors du chargement d’un niveau. Après avoir défini la `CurrentLevel`, le `GoToGameScene` méthode est déclenchée, la scène à partir de commutation `LevelSelectScene` à `GameScene`.

Le `GameScene` les appels de constructeur `GoToLevel`, qui exécute la logique au niveau du chargement :


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


## <a name="loadlevel"></a>LoadLevel

Le `LoadLevel` méthode est responsable du chargement du fichier .tmx et en l’ajoutant à la `GameScene`. Cette méthode ne crée pas de tous les objets interactifs telles que la collision ou entités : il crée simplement les éléments visuels pour le niveau, également appelé le *environnement*.


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

Le `CCTileMap` constructeur accepte un nom de fichier est créé en utilisant le numéro de niveau passé à `LoadLevel`. Pour plus d’informations sur la création et l’utilisation de `CCTileMap` instances, consultez le [à l’aide de mosaïque avec CocosSharp guide](~/graphics-games/cocossharp/tiled.md).

Actuellement, CocosSharp n’autorise pas la réorganisation des couches sans supprimer puis ajouter de nouveau à leur parent `CCScene` (qui est le `GameScene` dans ce cas), de sorte que les deux dernières lignes de la méthode sont requis pour réorganiser les couches.


## <a name="createcollision"></a>CreateCollision

Le `CreateCollision` méthode constructions un `LevelCollision` instance qui est utilisée pour effectuer *collision solide* entre le lecteur et l’environnement.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Sans cette collision, le lecteur est comprises par le niveau et le jeu serait lu. Collision solide permet le joueur Parcourir sur le sol et empêche le lecteur parcourant des parois ou le passage des plafonds par le biais.

Collision dans le temps de prise en charge totale peut être ajoutée sans code supplémentaire : seules des modifications aux fichiers en mosaïque. 


## <a name="processtileproperties"></a>ProcessTileProperties

Une fois qu’un niveau est chargé et la collision est créée, `ProcessTileProperties` est appelée pour appliquer une logique basée sur les propriétés des mosaïques. Heure de la prise en charge totale inclut un `PropertyLocation` struct pour définir des propriétés et les coordonnées de la vignette avec ces propriétés :


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

Ce struct est utilisé pour construire des instances d’entité de créer et supprimer des vignettes inutiles dans le `ProcessTileProperties` (méthode) :


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

Si une propriété avec le `EntityType` clé est trouvée, `TryCreateEntity` est appelée, qui tente de créer une entité à l’aide de la propriété mise en correspondance le `EntityType` clé :


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


# <a name="adding-new-entities"></a>Ajout de nouvelles entités

Heure de la prise en charge totale utilise le modèle d’entité pour les objets du jeu (qui est décrite dans le [guident des entités dans CocosSharp](~/graphics-games/cocossharp/entities.md)). Héritent de toutes les entités `CCNode`, ce qui signifie qu’ils peuvent être ajoutés en tant qu’enfants de le `gameplayLayer`.

Chaque type d’entité est également référencé directement par le biais d’une liste ou d’une instance unique. Par exemple, le `Player` est référencé par le `player` champ et tous les `Coin` instances sont référencés dans un `coins` liste. Conservation des références directes aux entités (au lieu d’y faire référence via la `gameLayer.Children` liste) rend le code qui accède à ces entités plus faciles à lire et d’éliminer le cast de type potentiellement coûteuse.

Le code existant fournit plusieurs types d’entités en tant qu’exemples montrant comment créer des entités. Les étapes suivantes peuvent être utilisés pour créer une nouvelle entité :


## <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - définir une nouvelle classe à l’aide du modèle d’entité

La seule condition requise pour la création d’une entité consiste à créer une classe qui hérite de `CCNode`. La plupart des entités ont certaines visuel, comme un `CCSprite`, qui doit être ajouté en tant qu’enfant de l’entité dans son constructeur.

CoinTime fournit la `AnimatedSpriteEntity` classe qui simplifie la création d’entités animées. Animations seront abordées plus en détail dans les [section animées des entités](#Animated_Entities).


## <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – ajouter une nouvelle entrée à l’instruction switch TryCreateEntity

Les instances de la nouvelle entité doivent être instanciés dans le `TryCreateEntity`. Si l’entité exige une logique de chaque image comme collision, AI ou lit l’entrée, puis le `GameScene` doit conserver une référence à l’objet. Si plusieurs instances sont nécessaires (comme `Coin` ou `Enemy` instances), puis une nouvelle `List` doivent être ajoutés à la `GameScene` classe.


## <a name="3--modify-tile-properties-for-the-new-entity"></a>3 : modifier les propriétés de vignette pour la nouvelle entité

Une fois que le code prend en charge la création de la nouvelle entité, la nouvelle entité doit être ajouté à la tileset. Le tileset peut être modifiée en ouvrant n’importe quel niveau `.tmx` fichier. 

Le tileset est stockée distincte de la .tmx dans le **mastersheet.tsx** de fichiers, de sorte qu’elle doit être importée avant de pouvoir être modifié :

![](cointime-images/image11.png "Le tileset est stocké séparément du fichier .tsx, afin qu’il doit être importé avant de pouvoir être modifié")

Une fois importés, les propriétés sur les vignettes sélectionnées sont modifiables et l’EntityType peut être ajouté :

![](cointime-images/image12.png "Une fois importés, les propriétés sur les vignettes sélectionnées sont modifiables, et l’EntityType peut être ajouté.")

Une fois que la propriété est créée, sa valeur peut être définie pour correspondre à la nouvelle `case` dans `TryCreateEntity`:

![](cointime-images/image13.png "Une fois que la propriété est créée, sa valeur peut être définie pour faire correspondre le nouveau cas dans TryCreateEntity")

Une fois le tileset a été modifié, il doit être exporté, ce qui rend les modifications disponible pour tous les autres niveaux :

![](cointime-images/image14.png "Une fois le tileset a été modifié, il doit être exporté")

Le tileset doit remplacer existants **mastersheet.tsx** tileset :

![](cointime-images/image15.png "HE tileset doit remplacer le tileset mastersheet.tsx existant")


# <a name="entity-tile-removal"></a>Suppression de mosaïque d’entité

Lorsqu’une carte de la vignette est chargée dans un jeu, les vignettes individuels sont des objets statiques. Étant donné que les entités nécessitent un comportement personnalisé comme le déplacement des, code au moment de la prise en charge totale supprime les vignettes lorsque les entités sont créées.

`ProcessTileProperties` inclut une logique pour supprimer de vignettes qui créent des entités à l’aide de la `RemoveTile` méthode :


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

Cette suppression automatique des vignettes est suffisante pour les entités qui n'occupent qu’une seule vignette dans le tileset, tels que les pièces et ennemis. Entités plus volumineuses nécessitent des propriétés et une logique supplémentaire.

La porte nécessite deux vignettes doit être dessiné entièrement :

![](cointime-images/image16.png "La porte requiert deux vignettes à dessiner complètement")

La vignette en bas de la porte contient les propriétés pour la création d’une entité (**EntityType** la valeur **porte**) :

![](cointime-images/image17.png "La vignette en bas de la porte contient les propriétés pour la création d’une entité EntityType de la porte")

Uniquement la vignette en bas de la porte est supprimée lorsque l’instance de la porte est créée, une logique supplémentaire est nécessaire pour supprimer la vignette supérieure lors de l’exécution. La vignette supérieure a un **RemoveMe** propriété **true**:

![](cointime-images/image18.png "La vignette supérieure a une propriété RemoveMe définie à true")



Cette propriété est utilisée pour supprimer de vignettes dans `ProcessTileProperties`:


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


# <a name="entity-offsets"></a>Décalages d’entité

Entités créées à partir de vignettes sont placées en alignant le centre de l’entité avec le centre de la vignette. Entités plus volumineuses, telles que `Door`, utilisez des propriétés supplémentaires et la logique doit être placé correctement. 

La vignette porte bas, qui définit le `Door` la sélection élective de l’entité, spécifie un **décalage y** valeur 4. Sans cette propriété, le `Door` instance est placée au centre de la vignette :

![](cointime-images/image19.png "Sans cette propriété, l’instance de la porte est placé au centre de la vignette")

 

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


# <a name="animated-entities"></a>Entités animées

Heure de la prise en charge totale inclut plusieurs entités animées. Le `Player` et `Enemy` entités lire les animations de parcours et `Door` entité lit une animation d’ouverture, une fois que toutes les pièces ont été collectés.


## <a name="achx-files"></a>fichiers .achx

Prise en charge totale des animations heure sont définies dans les fichiers de .achx. Chaque animation est définie entre `AnimationChain` balises, comme indiqué dans l’animation suivante définie dans **propanimations.achx**:


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

Cette animation contient uniquement une seule image, ce qui entraîne l’entité et PIC affiche une image statique. Entités peuvent utiliser des fichiers de .achx si elles s’affichent les animations uniques ou plusieurs frames. Frames supplémentaires peuvent être ajoutés aux fichiers de .achx sans nécessiter une modification dans le code. 

Frames de définissent l’image à afficher dans le `TextureName` paramètre et les coordonnées de l’affichage dans le `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, et `BottomCoordinate` balises. Elles représentent les coordonnées en pixels de l’image d’animation dans l’image qui est utilisée : **mastersheet.png** dans ce cas.

![](cointime-images/image20.png "Elles représentent les coordonnées en pixels de l’image d’animation dans l’image")

Le `FrameLength` propriété définit le nombre de secondes pendant lesquelles une image de l’animation doit être affichée. Animations d’image unique ignorent cette valeur.

Toutes les autres propriétés dans le fichier .achx AnimationChain sont ignorées par heure de la prise en charge totale.


## <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Logique d’animation est contenue dans le `AnimatedSpriteEntity` (classe), qui sert de classe de base pour la plupart des entités utilisées dans le `GameScene`. Il fournit les fonctionnalités suivantes :

 - Lors du chargement de `.achx` fichiers
 - Cache d’animation des animations chargées
 - Instance CCSprite pour l’affichage de l’animation
 - Logique permettant de modifier le frame actuel

Le constructeur de pics fournit un exemple simple illustrant comment charger et utiliser des animations :


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

Le **propAnimations.achx** contient uniquement une animation, le constructeur accède à cette animation par index. Si un fichier .achx contient plusieurs animations, alors que les animations peuvent être référencées par son nom, comme indiqué dans le `Enemy` constructeur :


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


# <a name="summary"></a>Récapitulatif

Ce guide couvre les détails d’implémentation de l’heure de la prise en charge totale. Heure de la prise en charge totale est créée pour un jeu complet, mais est également un projet qui peut être modifié et développé. Lecteurs sont encouragés à consacrer du temps apporté des modifications à des niveaux, ajout de nouveaux niveaux et la création de nouvelles entités pour mieux comprendre comment l’heure de la prise en charge totale est implémentée.

## <a name="related-links"></a>Liens associés

- [Projet de jeu (exemple)](https://developer.xamarin.com/samples/mobile/CoinTime/)
