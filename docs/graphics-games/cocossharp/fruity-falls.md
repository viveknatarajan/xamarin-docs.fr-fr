---
title: Détails de jeu se situe, agréables
description: Ce guide passe en revue le jeu Fruity revient, couvrant CocosSharp courants et les concepts de développement de jeux telles que physique, la gestion de contenu, l’état du jeu et la conception de jeu.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2138e27c1097e014f302cc0b9de0fa411bed89fc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="fruity-falls-game-details"></a>Détails de jeu se situe, agréables

_Ce guide passe en revue le jeu Fruity revient, couvrant CocosSharp courants et les concepts de développement de jeux telles que physique, la gestion de contenu, l’état du jeu et la conception de jeu._

Se situe, agréables est un jeu de simple basée sur le physique, dans lequel le joueur trie les fruits rouges et jaunes dans des compartiments de couleur pour gagner des points. L’objectif du jeu est gagner des points autant que possible sans en avertir un dépôt de fruits dans le mauvais emplacement, le jeu de fin.

![](fruity-falls-images/image1.png "L’objectif du jeu est de gagner des points autant que possible sans en avertir un dépôt de fruits dans le mauvais emplacement, le jeu de fin")

Se situe, agréables étend les concepts présentés dans le [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md) en ajoutant le code suivant :

 - Contenu sous la forme de PNG
 - Physique avancé
 - État du jeu (transition entre les scènes)
 - Possibilité de paramétrer les coefficients jeu via des variables contenues dans une seule classe
 - Prise en charge débogage visual intégrée
 - Organisation de code à l’aide des entités de jeu
 - Conception du jeu le focus sur la valeur fun et la relecture

Alors que le [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md) le focus sur la présentation de certains concepts CocosSharp, concernent la plage Fruity montre comment mettre tous les éléments en un produit fini de jeu. Étant donné que le BouncingGame fait référence à ce guide, lecteurs doivent tout d’abord qu’ils se familiarisent avec les [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md) avant la lecture de ce guide.

Ce guide couvre l’implémentation et la conception de Fruity recommence pour fournir des informations pour vous aider à rendre votre propre jeu. Elle couvre les rubriques suivantes :


- [Classe de GameController](#gamecontroller-class)
- [Entités de jeu](#game-entities)
- [Graphique de fruits](#fruit-graphics)
- [Physique](#physics)
- [Jeu de contenu](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>Classe de GameController

Le projet, agréables PCL se situe inclut un `GameController` classe qui est responsable de l’instanciation du jeu et le déplacement entre les séquences. Cette classe est utilisée par les projets Android et iOS d’éliminer le code en double.

Le code contenu dans le `Initialize` méthode est similaire au code dans le`Initialize` méthode dans un modèle CocosSharp inchangé, mais il contient un nombre de modifications.

Par défaut, le `GameView.ContentManager.SearchPaths` dépendent de la résolution du périphérique. Comme expliqué ci-dessous plus en détail, concernent la plage Fruity utilise le même contenu, quelle que soit la résolution de l’appareil, donc la `Initialize` méthode ajoute la `Images` chemin d’accès (avec aucun des sous-dossiers) à la `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Nouveaux modèles CocosSharp incluent une classe qui hérite de `CCLayer`, ce qui implique que les éléments visuels de jeu et logique doivent être ajoutés à cette classe. Au lieu de cela, les chutes Fruity utilise plusieurs `CCLayer` instances au contrôle de dessin ordre. Ces `CCLayer` instances sont contenues dans le `GameView` (classe), qui hérite de `CCScene`. Se situe, agréables inclut également plusieurs scènes, le premier étant le `TitleScene`. Par conséquent, le `Initialize` méthode instancie un `TitleScene` instance qui est passé à `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Le contenu pour Fruity est art de faible résolution pixel a été créé pour des raisons esthétiques. Le `GameView.DesignResolution` est définie pour que le jeu est uniquement une largeur de 384 unités et 512 taller :


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Enfin, le `GameController` classe fournit une méthode statique qui peut être appelée par les `CCGameScene` pour effectuer la transition vers un autre `CCScene`. Cette méthode est utilisée pour vous déplacer entre les `TitleScene` et `GameScene`.


## <a name="game-entities"></a>Entités de jeu

Se situe, agréables utilise le modèle d’entité pour la plupart des objets de jeu. Vous trouverez une explication détaillée de ce modèle dans le [guident des entités dans CocosSharp](~/graphics-games/cocossharp/entities.md).

Les objets de jeu de mise en application d’entité sont accessibles dans le dossier d’entités dans le **FruityFalls.Common** projet :

![](fruity-falls-images/image2.png "Le dossier d’entités dans le projet FruityFalls.Common")

Entités sont des objets qui héritent de `CCNode`et peut avoir des éléments visuels, collision et le comportement de chaque image.

Le `Fruit` objet représente une entité CocosSharp classique : il contient un objet visuel, collision et la logique de chaque image. Son constructeur est chargé d’initialiser le Fruit :


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>Graphique de fruits

Le `CreateFruitGraphic` méthode crée un `CCSprite` de l’instance et l’ajoute à la `Fruit`. Le `IsAntialiased` propriété est définie sur false pour donner le jeu un aspect de granularité. Cette valeur est définie sur false sur tous les `CCSprite` et `CCLabel` instances dans le jeu :


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Chaque fois que le joueur touche un `Fruit` de l’instance avec la `Paddle`, la valeur du point de ce fruits augmente d’un. Cela fournit une stimulation supplémentaires à des lecteurs expérimentés de jongler avec fruits pour des points supplémentaires. La valeur de point de fruits est affichée à l’aide de la `extraPointsLabel` instance.

Le `CreateExtraPointsLabel` méthode crée un `CCLabel` de l’instance et l’ajoute à la `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Se situe, agréables comprend deux types différents de fruits – cerises et citrons. Le `CreateFruitGraphic` affecte un élément visuel par défaut, mais les éléments visuels de fruits peuvent être modifié via le `FruitColor` propriété, qui à son tour appelle `UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

La première instruction if dans `UpdateGraphics` met à jour les graphiques de débogage, qui sont utilisés pour visualiser les zones de collision. Les éléments visuels sont mis hors tension avant une version finale du jeu est effectuée, mais peut être conservée sur pendant le développement pour le débogage physique. La deuxième partie modifie le `graphic.Texture` propriété en appelant `CCTextureCache.SharedTextureCache.AddImage`. Cette méthode permet d’accéder à une texture par nom de fichier. Vous trouverez plus d’informations sur cette méthode dans le [guide Mise en cache de Texture](~/graphics-games/cocossharp/texture-cache.md).

Le `extraPointsLabel` couleur est ajustée pour conserver le contraste avec l’image de fruits et son `PositionY` valeur est ajustée au centre de la `CCLabel` sur des fruits `CCSprite`:

![](fruity-falls-images/image3.png "La couleur extraPointsLabel est ajustée pour conserver le contraste avec l’image de fruits, et sa valeur PositionY est ajustée pour centrer le CCLabel sur les fruits CCSprite")

![](fruity-falls-images/image4.png "La couleur extraPointsLabel est ajustée pour conserver le contraste avec l’image de fruits, et sa valeur PositionY est ajustée pour centrer le CCLabel sur les fruits CCSprite")


### <a name="collision"></a>Collision

Se situe, agréables implémente une solution de collision personnalisé à l’aide des objets dans le dossier de la géométrie :

![](fruity-falls-images/image5.png "Se situe, agréables implémente une solution de collision personnalisé à l’aide des objets dans le dossier de la géométrie")

Collision dans Fruity concernent la plage est implémentée à l’aide du `Circle` ou `Polygon` objet, généralement avec l’un de ces deux types ajoutés en tant qu’enfant d’une entité. Par exemple, le `Fruit` objet a un `Circle` appelé `Collision` dont il initialise dans son `CreateCollision` (méthode) :


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

Notez que la `Circle` classe hérite de la `CCNode` classe, donc elle peut être ajoutée en tant qu’enfant à des entités du jeu :


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` la création est similaire à `Circle` la création, comme indiqué dans la `Paddle` classe :


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

Logique de collision est couvert [plus loin dans ce guide](#collision).


## <a name="physics"></a>Physique

La physique dans Fruity concernent la plage peut être divisée en deux catégories : le déplacement et la collision. 


### <a name="movement-using-velocity-and-acceleration"></a>Déplacement à l’aide de la vitesse et l’accélération

Se situe, agréables utilise `Velocity` et `Acceleration` valeurs pour contrôler le déplacement de ses entités, similaire à la [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Entités implémentent la logique de déplacement dans une méthode nommée `Activity`, qui est appelé une fois par frame. Par exemple, nous pouvons voir l’implémentation de déplacement dans la `Fruit` classe `Activity` méthode :

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
Le `Fruit` objet est unique dans son implémentation de glissement : une valeur qui ralentit la vitesse par rapport à la rapidité Fruit est mobile. Cette implémentation de glisser fournit un *rapidité terminal*, qui est la vitesse maximale à une instance de fruits pouvant être. Faites glisser ralentit également le mouvement horizontal de fruits, ce qui rend le jeu légèrement plus facile à lire.

Le `Paddle` objet s’applique également `Velocity` dans son `Activity` (méthode), mais son `Velocity` est calculée à l’aide un `desiredLocation` valeur :


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

En règle générale, les jeux qui utilisent `Velocity` pour contrôler la position de leurs objets de ne pas directement définie positions de l’entité, au moins pas après l’initialisation. Au lieu de définir directement la `Paddle` position, la `Paddle.HandleInput` méthode attribue le `desiredLocation` valeur :

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Collision

Se situe, agréables implémente semi réaliste collision entre les fruits et d’autres objets collidable, comme le `Paddle` et `GameScene.Splitter`. Pour vous aider à déboguer collision, zones de collision Fruity concernent la plage peuvent être rendus visibles en modifiant le `GameCoefficients.ShowDebugInfo` dans le `GameCoefficients.cs` fichier :


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

La valeur `true` Active le rendu des zones de conflit :  

![](fruity-falls-images/image6.png "La valeur true active le rendu des zones de collision")

Logique de collision commence dans le `GameScene.Activity` méthode :


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` gère tous les collision `Fruit` instances avec d’autres objets : 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` collision effectue sa propre logique pour collision plutôt que de compter sur la logique contenue dans une autre classe. Cette différence existe, car la collision entre les fruits et les bordures de l’écran n’est pas parfaitement solide : il est possible pour les fruits d’envoyer aux mouvements de la manette de jeu prudent hors des limites de l’écran. Sur l’écran lorsqu’il est atteint avec la manette de jeu rebondit fruits, mais si le joueur lentement exécute un push de fruits déplacera au-delà du bord et en dehors de l’écran :


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

Le `FruitVsBins` (méthode) est chargée de vérifier si les fruits se trouve dans un des deux emplacements. Dans ce cas, puis le lecteur est attribué à points (si la correspondance de couleurs le fruits/bin) ou la partie se termine (si les couleurs ne correspondent pas) :


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle et FruitPolygonCollision

Fruits et manette de jeu et de fruits et fractionnées (la zone séparant les deux emplacements) collision les deux s’appuient sur la `FruitPolygonCollision` (méthode). Cette méthode comprend trois parties :

1. Vérifier si les objets sont en conflit
1. Déplacer les objets (uniquement les fruits dans Fruity revient) afin qu’ils ne se chevauchent plus
1. Ajuster la rapidité des objets (uniquement les fruits dans Fruity revient) pour simuler un rebond que le code suivant illustre les points faites ci-dessus :


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Réponse de collision se situe, agréables est unilatérale – d’uniquement les fruits rapidité et position sont ajustées. Il est important de noter que les autres jeux peut-être avoir des collisions qui a un impact sur les deux objets impliqués, comme un caractère envoyant un boulder ou deux voitures en panne dans l’autre.

Les mathématiques qui sous-tendent la logique de collision contenue dans le `Polygon` et `CollisionResponse` classes est dépasse le cadre de ce guide, mais comme écrite, ces méthodes peuvent être utilisées pour de nombreux types de jeux. Le polygone et `CollisionResponse` classes prennent en charge même polygones non rectangulaires et convexes, ce code peut être utilisée pour de nombreux types de jeux.

 


## <a name="game-content"></a>Jeu de contenu

L’illustration du Fruity revient immédiatement distingue le jeu le BouncingGame. Alors que les conceptions de jeu sont similaires, lecteurs verront immédiatement un changement des deux jeux. Joueurs souvent décident essayez un jeu par ses éléments visuels. Par conséquent, il est essentiel que les développeurs investissent des ressources lors de l’élaboration visuellement attrayante jeu.

L’illustration du Fruity concernent la plage a été créée avec les objectifs suivants :

 - Thème cohérent
 - Mettant l’accent sur un seul caractère – le singe Xamarin
 - Expérience de couleurs vives à créer une relaxation agréable
 - Créer un contraste entre le premier plan et d’arrière-plan afin de faciliter suivre visuellement les objets de jeu
 - Possibilité de créer des effets visuels simples sans les animations de ressources


### <a name="content-location"></a>Emplacement du contenu

Se situe, agréables inclut tout son contenu dans le dossier Images dans le projet Android :

![](fruity-falls-images/image7.png "Se situe, agréables inclut tout son contenu dans le dossier Images dans le projet Android")

Ces mêmes fichiers sont liés dans le projet iOS pour éviter la duplication, et c’est le cas apportées aux fichiers avoir un impact sur les deux projets :

![](fruity-falls-images/image8.png "Ces mêmes fichiers sont liés dans le projet iOS pour éviter la duplication, et c’est le cas apportées aux fichiers avoir un impact sur les deux projets")

Il est important de noter que le contenu n’est pas contenu dans le **%ld** ou **Hd** dossiers, qui font partie du modèle CocosSharp par défaut. Le **%ld** et **Hd** dossiers sont destinés à être utilisés pour les jeux qui fournissent les deux ensembles de contenu : un pour les appareils de faible résolution, tels que des téléphones et pour les appareils haute résolution, tels que les tablettes. L’art Fruity concernent la plage est intentionnellement créé avec une granularité esthétique, et il est inutile de fournir du contenu pour différentes tailles d’écran. Par conséquent, le **%ld** et **Hd** dossiers ont été complètement supprimés à partir du projet.


### <a name="gamescene-layering"></a>GameScene superposition

Comme mentionné précédemment dans ce guide, le `GameScene` est responsable de toutes les instanciation de l’objet de jeu, le positionnement et la logique entre les objets (tels que les collisions). Tous les objets sont ajoutés à un des quatre `CCLayer` instances :


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

Ces quatre couches sont créés dans le `CreateLayers` (méthode). Notez qu’ils sont ajoutés à la `GameScene` dans l’ordre de l’arrière vers l’avant :


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

Une fois que les couches sont créés, tous les objets visuels sont ajoutés aux couches à l’aide de la `AddChild` méthode, comme indiqué dans le `CreateBackground` méthode :


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>Entité de vigne

Le `Vine` entité est utilisée uniquement pour le contenu, il n’a aucun impact sur le jeu. Il est composé de vingt `CCSprite` instances, un nombre sélectionné par essais et erreurs pour vous assurer que la vigne atteint toujours en haut de l’écran :


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

Le premier `CCSprite` est positionné de sorte que son bord inférieur corresponde à la position de la vigne. Cela s’effectue en affectant le AnchorPoint `new CCPoint(.5f, 0)`. Le `AnchorPoint` propriété utilise *normalisées coordonnées* la plage comprise entre 0 et 1, quelle que soit la taille de la `CCSprite`:

![](fruity-falls-images/image9.png "La propriété AnchorPoint utilise des coordonnées normalisées la plage comprise entre 0 et 1, quelle que soit la taille de la CCSprite")

Sprites de vigne suivantes sont positionnées à l’aide de la `graphic.ContentSize.Height` valeur, qui retourne la hauteur de l’image en pixels. Le diagramme suivant montre comment le graphique vigne vignettes vers le haut :

![](fruity-falls-images/image10.png "Ce diagramme montre comment le graphique vigne vignettes vers le haut")

Depuis l’origine de la `Vine` entité figure au bas de la vigne, puis il le positionnement est simple, comme indiqué dans le `Paddle.CreateVines` méthode :


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

La vigne instances dans le `Paddle` entité ont également le comportement intéressant de rotation. Étant donné que la `Paddle` entité en tant qu’ensemble fait pivoter en fonction de l’entrée de lecteur (qui ce guide couvre plus en détail ci-dessous), le `Vine` instances sont également affectés par cette rotation. Toutefois, faire pivoter le `Vine` instances avec le `Paddle` produit un effet visuel indésirable, comme le montre l’animation suivante :

![](fruity-falls-images/image11.gif "Toutefois, faire pivoter les instances de vigne ainsi que de la manette de jeu produit un effet visuel indésirable comme indiqué dans l’animation suivante")

De contrer le `Paddle` rotation, le `leftVine` et `rightVine` instances pivotent la direction opposée de la manette de jeu, comme indiqué dans le `Paddle.Activity` méthode :


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

Notez qu’une petite quantité de rotation est ajoutée à la vigne via le `vineAngle` coefficient. Cette valeur peut être modifiée pour ajuster la quantité faire pivoter les vines.


## <a name="gamecoefficients"></a>GameCoefficients

Chaque bon jeu étant le produit de l’itération, concernent la plage Fruity inclut une classe appelée `GameCoefficients` qui contrôle la façon dont le jeu est lu. Cette classe contient des variables expressives, qui sont utilisés dans le jeu de contrôle physique, mise en page, la génération automatique et le score.

Par exemple, la physique fruits est contrôlée par les variables suivantes :


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

Comme le nom le laisse supposer, ces variables peuvent être utilisées pour ajuster la rapidité de fruits se situe, comment horizontal mouvement ralentit dans le temps et rebond de manette de jeu.

Coefficients jeu stockées dans les fichiers de code ou des données (par exemple, XML) peuvent être particulièrement utiles pour les équipes avec les concepteurs de jeu qui ne sont pas également aux programmeurs.

La `GameCoefficients` classe inclut également des valeurs pour les informations de débogage telles que la génération automatique d’informations ou collision de zones sous tension :


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>Conclusion

Ce guide exploré le jeu Fruity recommence. Il couvert les concepts, y compris le contenu, physique et la gestion de l’état du jeu.

## <a name="related-links"></a>Liens connexes

- [Documentation de l’API de CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
