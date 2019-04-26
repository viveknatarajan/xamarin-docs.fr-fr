---
title: Détails du jeu Fruity concernent la plage
description: Ce guide passe en revue le jeu Fruity concernent la plage, couvrant CocosSharp courants et les concepts de développement de jeux tels que physique, la gestion de contenu, état du jeu et conception de jeux.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 959f5eb149ad375d686b17a85eb3d3b8fbdf3659
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61160026"
---
# <a name="fruity-falls-game-details"></a>Détails du jeu Fruity concernent la plage

_Ce guide passe en revue le jeu Fruity concernent la plage, couvrant CocosSharp courants et les concepts de développement de jeux tels que physique, la gestion de contenu, état du jeu et conception de jeux._

Se situe Fruity est un jeu simple, basé sur le moteur physique dans lequel le joueur trie les fruits rouges et jaunes dans des compartiments de couleur pour gagner des points. L’objectif du jeu est de remporter des points autant que possible sans en avertir d’une baisse de fruits dans le mauvais emplacement, mettre fin au jeu.

![](fruity-falls-images/image1.png "L’objectif du jeu est de gagner des points autant que possible sans en avertir d’une baisse de fruits dans le mauvais emplacement, mettre fin au jeu")

Se situe Fruity étend les concepts présentés dans le [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md) en ajoutant le code suivant :

 - Contenu sous la forme de PNG
 - Physique avancé
 - État du jeu (transition entre les scènes)
 - Possibilité de paramétrer les coefficients de jeux via des variables contenues dans une classe unique
 - Prise en charge débogage visual intégrée
 - Organisation du code à l’aide d’entités de jeu
 - Conception de jeux axée sur la valeur de cadre amusant et une relecture

Bien que le [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md) axé sur la présentation des principaux concepts de CocosSharp, se situe Fruity montre comment tous les regrouper dans un produit fini de jeu. Étant donné que ce guide fait référence à du BouncingGame, lecteurs doivent tout d’abord se familiariser avec la [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md) avant de lire ce guide.

Ce guide couvre l’implémentation et la conception de Fruity concernent la plage pour fournir des informations pour vous aider à rendre votre propre jeu. Il couvre les rubriques suivantes :


- [Classe de GameController](#gamecontroller-class)
- [Entités de jeu](#game-entities)
- [Graphique de fruits](#fruit-graphics)
- [Physique](#physics)
- [Jeu de contenu](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>Classe de GameController

Le projet Fruity PCL se situe inclut un `GameController` classe qui est responsable de l’instanciation du jeu et le déplacement entre les scènes. Cette classe est utilisée par les projets Android et iOS pour éliminer les doublons de code.

Le code contenu dans le `Initialize` méthode est similaire au code dans le`Initialize` méthode dans un modèle de CocosSharp inchangé, mais il contient un nombre de modifications.

Par défaut, le `GameView.ContentManager.SearchPaths` dépendent de la résolution du périphérique. Comme expliqué ci-dessous plus en détail, se situe Fruity utilise le même contenu, quel que soit la résolution de l’appareil, par conséquent, le `Initialize` méthode ajoute la `Images` chemin d’accès (avec mais pas les sous-dossiers) à la `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Nouveaux modèles de CocosSharp incluent une classe héritant de `CCLayer`, ce qui implique que les éléments visuels de jeu et la logique doivent être ajoutés à cette classe. Au lieu de cela, se situe Fruity utilise plusieurs `CCLayer` instances au contrôle de dessiner l’ordre. Ces `CCLayer` instances sont contenues dans le `GameView` (classe), qui hérite de `CCScene`. Fruity concernent la plage inclut également plusieurs scènes, le premier étant le `TitleScene`. Par conséquent, le `Initialize` méthode instancie un `TitleScene` instance qui est transmise à `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Le contenu pour se situe Fruity art de basse résolution pixel a été créé pour des raisons esthétiques. Le `GameView.DesignResolution` est définie afin que le jeu est uniquement une largeur de 384 unités et 512 taller :


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Enfin, le `GameController` classe fournit une méthode statique qui peut être appelée par les `CCGameScene` pour effectuer la transition vers un autre `CCScene`. Cette méthode est utilisée pour vous déplacer entre les `TitleScene` et `GameScene`.


## <a name="game-entities"></a>Entités de jeu

Se situe Fruity utilise le modèle d’entité pour la plupart des objets de jeu. Vous trouverez une explication détaillée de ce modèle dans le [guident des entités dans CocosSharp](~/graphics-games/cocossharp/entities.md).

Vous trouverez les objets de jeu de mise en application d’entité dans le dossier d’entités dans le **FruityFalls.Common** projet :

![](fruity-falls-images/image2.png "Le dossier d’entités dans le projet FruityFalls.Common")

Entités sont des objets qui héritent de `CCNode`et peut avoir des éléments visuels, les collisions et comportement de chaque image.

Le `Fruit` objet représente une entité de CocosSharp typiques : il contient un objet visuel, les collisions et logique de chaque image. Son constructeur est responsable de l’initialisation du Fruit :


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

Le `CreateFruitGraphic` méthode crée un `CCSprite` de l’instance et l’ajoute à la `Fruit`. Le `IsAntialiased` propriété est définie sur false pour donner le jeu un aspect de l’aspect pixélisé. Cette valeur est définie sur false sur tous les `CCSprite` et `CCLabel` instances au cours du jeu :


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Chaque fois que le joueur touche un `Fruit` instance avec le `Paddle`, la valeur de point de ce fruits augmente d’une unité. Cela fournit un défi supplémentaire pour les joueurs expérimentés à jongler fruits pour des points supplémentaires. La valeur de point du fruit est affichée à l’aide de la `extraPointsLabel` instance.

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

Fruity concernent la plage comprend deux types différents de fruits – cerises et citrons. Le `CreateFruitGraphic` assigne un élément visuel par défaut, mais les visuels de fruits peuvent être modifié via le `FruitColor` propriété, qui à son tour appelle `UpdateGraphics`:


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

La première instruction if dans `UpdateGraphics` met à jour les graphiques de débogage, qui sont utilisés pour visualiser les zones de collision. Ces éléments visuels sont désactivés avant une version finale du jeu est effectuée, mais peut être conservée sur pendant le développement pour le débogage physique. La deuxième partie modifie le `graphic.Texture` propriété en appelant `CCTextureCache.SharedTextureCache.AddImage`. Cette méthode permet d’accéder à une texture par nom de fichier. Vous trouverez plus d’informations sur cette méthode dans le [guide Mise en cache de Texture](~/graphics-games/cocossharp/texture-cache.md).

Le `extraPointsLabel` couleur est ajustée pour conserver le contraste avec l’image de fruits et son `PositionY` valeur est ajustée au centre de la `CCLabel` sur les fruits `CCSprite`:

![](fruity-falls-images/image3.png "La couleur extraPointsLabel est ajustée pour conserver le contraste avec l’image de fruits, et sa valeur PositionY est ajustée pour centrer le CCLabel sur les fruits CCSprite")

![](fruity-falls-images/image4.png "La couleur extraPointsLabel est ajustée pour conserver le contraste avec l’image de fruits, et sa valeur PositionY est ajustée pour centrer le CCLabel sur les fruits CCSprite")


### <a name="collision"></a>Collision

Se situe Fruity met en œuvre une solution de collision personnalisé à l’aide des objets dans le dossier de la géométrie :

![](fruity-falls-images/image5.png "Se situe Fruity implémente une solution de collision personnalisé à l’aide des objets dans le dossier de géométrie")

Collision dans Fruity concernent la plage est implémentée à l’aide du `Circle` ou `Polygon` objet, généralement avec l’une de ces deux types ajoutés en tant qu’enfant d’une entité. Par exemple, le `Fruit` objet a un `Circle` appelé `Collision` qui il initialise dans son `CreateCollision` méthode :


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

Notez que le `Circle` classe hérite de la `CCNode` classe, et peut être ajouté en tant qu’enfant à des entités de notre jeu :


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

Utilise des chutes Fruity `Velocity` et `Acceleration` valeurs pour contrôler le déplacement de ses entités, similaire à la [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Entités implémentent leur logique de déplacement dans une méthode nommée `Activity`, qui est appelé une fois par trame. Par exemple, nous pouvons voir l’implémentation du mouvement dans le `Fruit` classe `Activity` méthode :

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
Le `Fruit` objet est unique dans son implémentation de glisser – une valeur qui ralentit la vitesse par rapport à la rapidité Fruit bouge. Cette implémentation du glisser fournit un *velocity terminal*, qui est la vitesse maximale qui une instance de fruits peut être classées. Faites glisser ralentit également le déplacement horizontal de fruits, ce qui rend le jeu légèrement plus facile à lire.

Le `Paddle` objet s’applique également `Velocity` dans son `Activity` (méthode), mais sa `Velocity` est calculée à l’aide un `desiredLocation` valeur :


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

En règle générale, les jeux qui utilisent `Velocity` pour contrôler la position de leur faire objets pas directement définie des positions de l’entité, au moins pas après l’initialisation. Au lieu de définir directement la `Paddle` position, la `Paddle.HandleInput` méthode attribue le `desiredLocation` valeur :

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Collision

Se situe Fruity implémente semi-structurées réaliste collision entre les fruits et d’autres objets collidable telles que le `Paddle` et `GameScene.Splitter`. Pour aider à déboguer collision, zones de collision se situe Fruity peuvent être rendus visibles en modifiant le `GameCoefficients.ShowDebugInfo` dans le `GameCoefficients.cs` fichier :


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

`PerformCollision` gère la collision tous `Fruit` instances avec d’autres objets : 


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

`FruitVsBorders` collision effectue sa propre logique de collision, plutôt que de compter sur la logique contenue dans une classe différente. Cette différence existe, car la collision entre les bordures de l’écran et les fruits n’est pas parfaitement solide : il est possible pour les fruits d’envoyer hors des limites de l’écran à un mouvement de la raquette prudent. Fruits rebondit sur l’écran lorsqu’il est atteint avec la raquette, mais si le joueur lentement exécute un push de fruits déplacera au-delà du bord et en dehors de l’écran :


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

Le `FruitVsBins` (méthode) est chargée de vérifier si les fruits sont passées dans un des deux emplacements. Dans ce cas, puis le joueur est attribué à points (si la correspondance des couleurs le fruits/bin) ou le jeu se termine (si les couleurs ne correspondent pas) :


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

Fruits ou la raquette et fruits et séparateur (la zone en séparant les deux emplacements) collision les deux s’appuient sur le `FruitPolygonCollision` (méthode). Cette méthode comporte trois parties :

1. Tester si les objets sont en conflit
1. Déplacer les objets (simplement les fruits d’un se situe Fruity) afin qu’ils ne se chevauchent plus
1. Ajustez la rapidité des objets (simplement les fruits d’un se situe Fruity) pour simuler un rebond que le code suivant illustre les points créées précédemment :


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

Réponse de collision Fruity concernent la plage est unilatérale – uniquement les fruits vélocité sont ajustées. Il est important de noter que les autres jeux peut-être avoir des collisions qui a un impact sur les deux objets impliqués, notamment un caractère de transmettre un boulder ou deux voitures se bloque à eux.

Les mathématiques qui sous-tendent la logique de collision contenue dans le `Polygon` et `CollisionResponse` classes n’entre pas dans le cadre de ce guide, mais en tant qu’écrit, ces méthodes peuvent être utilisées pour de nombreux types de jeux. Le polygone et `CollisionResponse` classes même en charge les polygones non rectangulaires et convexes, donc ce code peut être utilisé pour de nombreux types de jeux.

 


## <a name="game-content"></a>Jeu de contenu

L’art dans Fruity revient immédiatement distingue le jeu du BouncingGame. Si les conceptions de jeu sont similaires, les joueurs verront immédiatement un changement des deux jeux. Les joueurs décident souvent d’essayer un jeu par ses éléments visuels. Par conséquent, il est essentiel que les développeurs investissent des ressources dans la fabrication visuellement agréables jeu.

L’art pour Fruity concernent la plage a été créé avec les objectifs suivants :

 - Thème cohérents
 - Mettant l’accent sur un seul caractère – le monkey Xamarin
 - Expérience de couleurs vives pour créer un assouplissement agréable
 - Créer un contraste entre le premier plan et d’arrière-plan pour les objets de jeu sont faciles à suivre visuellement
 - Possibilité de créer des effets visuels simples sans les animations comportant beaucoup de ressources


### <a name="content-location"></a>Emplacement du contenu

Se situe Fruity inclut tout son contenu dans le dossier Images dans le projet Android :

![](fruity-falls-images/image7.png "Se situe Fruity inclut tout son contenu dans le dossier Images dans le projet Android")

Ces mêmes fichiers sont liés dans le projet iOS pour éviter la duplication, et c’est le cas apportées aux fichiers avoir un impact sur les deux projets :

![](fruity-falls-images/image8.png "Ces mêmes fichiers sont liés dans le projet iOS pour éviter la duplication, et c’est le cas apportées aux fichiers avoir un impact sur les deux projets")

Il est important de noter que le contenu n’est pas contenu dans le **%ld** ou **Hd** dossiers, qui font partie du modèle de CocosSharp par défaut. Le **%ld** et **Hd** dossiers sont destinées à être utilisées pour les jeux qui fournissent les deux ensembles de contenu : un pour les appareils de faible résolution, tels que des téléphones et l’autre pour les appareils de résolution plus élevée, comme les tablettes. L’art de revient de Fruity est intentionnellement créé avec un aspect pixélisé esthétique, et il est inutile de fournir du contenu pour différentes tailles d’écran. Par conséquent, le **%ld** et **Hd** dossiers ont été complètement supprimés à partir du projet.


### <a name="gamescene-layering"></a>Superposition de GameScene

Comme mentionné plus haut dans ce guide, le `GameScene` est responsable de tous les instanciation de l’objet de jeu, de positionnement et logique entre les objets (par exemple, des collisions). Tous les objets sont ajoutés à un des quatre `CCLayer` instances :


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

Le `Vine` entité est utilisée uniquement pour le contenu : il n’a aucun impact sur le jeu. Elle est composée de vingt `CCSprite` instances, un nombre sélectionné par essais et erreurs pour nous assurer la vigne atteint toujours en haut de l’écran :


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

Le premier `CCSprite` est positionné de sorte que son bord inférieur corresponde à la position de la vigne. Cela s’effectue en définissant le AnchorPoint `new CCPoint(.5f, 0)`. Le `AnchorPoint` propriété utilise *normalisées coordonnées* la plage comprise entre 0 et 1, quel que soit la taille de la `CCSprite`:

![](fruity-falls-images/image9.png "La propriété AnchorPoint utilise des coordonnées normalisées la plage comprise entre 0 et 1, quel que soit la taille de la CCSprite")

Les sprites vigne suivantes sont positionnées à l’aide de la `graphic.ContentSize.Height` valeur, qui retourne la hauteur de l’image en pixels. Le diagramme suivant montre comment le graphique vigne vignettes vers le haut :

![](fruity-falls-images/image10.png "Ce diagramme illustre comment le graphique vigne vignettes vers le haut")

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

La vigne instances dans le `Paddle` entité ont également intéressant de comportement de rotation. Dans la mesure où le `Paddle` entity dans son ensemble fait pivoter en fonction de l’entrée d’un joueur (ce qui ce guide aborde plus en détail ci-dessous), le `Vine` instances sont également affectés par cette rotation. Toutefois, faire pivoter le `Vine` instances avec le `Paddle` produit un effet indésirable visual comme illustré dans l’animation suivante :

![](fruity-falls-images/image11.gif "Toutefois, faire pivoter les instances de vigne ainsi que de la raquette produit un effet indésirable visual comme indiqué dans l’animation suivante")

Pour neutraliser le `Paddle` rotation, la `leftVine` et `rightVine` instances pivotent la direction opposée de la raquette, comme indiqué dans le `Paddle.Activity` méthode :


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

Notez qu’une petite quantité de rotation est ajoutée à la vigne via le `vineAngle` coefficient. Cette valeur peut être modifiée pour ajuster combien les vines faire pivoter.


## <a name="gamecoefficients"></a>GameCoefficients

Chaque bon jeu étant le produit de l’itération, se situe Fruity inclut une classe appelée `GameCoefficients` qui contrôle comment le jeu est lu. Cette classe contient des variables expressifs, qui sont utilisées dans le contrôle physique, disposition, lors de la génération et la notation du jeu.

Par exemple, la physique de fruits est contrôlée par les variables suivantes :


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

Comme les noms l’indiquent, ces variables peuvent être utilisées pour ajuster la rapidité fruits concernent la plage, comment horizontale ralentissement du mouvement vers le bas dans le temps et raquette rebond.

Jeu coefficients stockés dans des fichiers de code ou de données (par exemple, XML) peuvent être particulièrement utiles pour les équipes avec les concepteurs de jeux qui ne sont pas également des programmeurs.

Le `GameCoefficients` classe inclut également des valeurs pour les informations de débogage telle que la génération d’informations ou collision zones sous tension :


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

Ce guide a exploré le jeu Fruity concernent la plage. Il couvert les concepts, y compris le contenu, physique et la gestion de l’état du jeu.

## <a name="related-links"></a>Liens connexes

- [Documentation de l’API de CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
