---
title: Implémentation du jeu rebondit
description: Cette procédure pas à pas montre comment ajouter la logique de jeu et de contenu à un modèle vide pour créer un jeu complet appelé BouncingGame.
ms.topic: article
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 584ae03a3a773ae7e16fa7a24b9dbfa6c5056342
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
# <a name="implementing-the-bouncing-game"></a>Implémentation du jeu rebondit

_Cette procédure pas à pas montre comment ajouter la logique de jeu et de contenu à un modèle vide pour créer un jeu complet appelé BouncingGame._

La partie précédente de cette procédure pas à pas vous a montré comment créer une solution CocosSharp avec des projets pour iOS et Android développement. Ce guide se poursuit en implémentant un jeu complet, couvrant les éléments suivants :

 - Décompresser le contenu de notre jeu
 - Éléments visuels CocosSharp courantes
 - Ajout d’éléments visuels pour `GameLayer`
 - Implémentation de la logique de chaque image

Notre jeu terminé doit ressembler à ceci :

![](part2-images/image1.png "Le jeu terminé doit ressembler à ceci")


# <a name="unzipping-our-game-content"></a>Décompresser le contenu de notre jeu

Chez les développeurs utilisent souvent le terme *contenu* pour faire référence à des fichiers de code non qui sont généralement créées par les artistes visual, les concepteurs de jeux ou des concepteurs audio. Types courants de contenu incluent les fichiers utilisés pour afficher des éléments visuels, sons ou contrôler le comportement de l’intelligence artificielle (AI). Du point de vue d’une équipe de développement de jeux, le contenu est généralement créé par les programmeurs non. Notre jeu comprend deux types de contenu :

 - Fichiers PNG à définir la façon dont notre boule et la manette de jeu s’affichent.
 - Un seul fichier XNB pour définir la police utilisée par notre affichage score (décrit plus en détail lorsque nous ajoutons notre affichage score ci-dessous)

Vous trouverez ce contenu utilisé ici dans [zip de contenu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Nous avons besoin de ces fichiers décompressé vers un emplacement plus loin dans cette procédure pas à pas, nous devons accéder.


# <a name="common-cocossharp-visual-elements"></a>Éléments communs CocosSharp Visual

CocosSharp fournit un nombre de classes utilisées pour afficher des éléments visuels. Certains éléments sont directement visibles, tandis que d’autres sont utilisés pour l’organisation. Nous allons utiliser les éléments suivants dans le jeu :

 - `CCNode` – Classe de base pour tous les objets visuels dans CocosSharp. Le `CCNode` classe contient un `AddChild` fonction qui peut être utilisée pour construire une hiérarchie parent/enfant, également appelé un *arborescence d’éléments visuels* ou *graphique de scène*. Héritent de toutes les classes mentionnées ci-dessous `CCNode`.
 - `CCScene` – Racine de l’arborescence d’éléments visuels pour tous les jeux de CocosSharp. Tous les éléments visuels doivent faire partie d’une arborescence d’éléments visuels avec un `CCScene` à la racine, ou ils ne seront pas visibles.
 - `CCLayer` – Conteneur pour visual objets, tels que `CCSprite`. Comme son nom l’indique, le `CCLayer` classe est utilisée pour contrôler la façon dont visual couche d’éléments sur eux.
 - `CCSprite` : Affiche une image ou une partie d’une image. `CCSprite` les instances peuvent être positionnées redimensionné et fournissant un nombre d’effets visuels.
 - `CCLabel` : Affiche une chaîne à l’écran. La police utilisée par `CCLabel` est défini dans un fichier XNB. Nous aborderons XNBs plus en détail ci-dessous.

Pour comprendre comment les différents types sont utilisés nous allons étudier une seule `CCSprite`. Éléments visuels doivent être ajoutés à un `CCLayer`, et l’arborescence visuelle doit avoir un `CCScene` à sa racine. Par conséquent, la relation parent/enfant pour un seul `CCSprite` serait `CCScene`  >  `CCLayer`  >  `CCSprite`.


# <a name="adding-visual-elements-to-gamelayer"></a>Ajout d’éléments visuels à GameLayer


## <a name="adding-the-paddlesprite"></a>Ajout de la paddleSprite

Initialement, nous mettrons noir et également ajouter un arrière-plan du jeu `CCSprite` rendu sur l’écran. Modifier la `GameLayer` classe pour ajouter un `CCSprite` comme suit :


```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of our drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

Le code ci-dessus crée un seul `CCSprite` et l’ajoute en tant qu’enfant de le `GameLayer`. Le `CCSprite` constructeur permet de définir le fichier image à utiliser en tant que chaîne. Notre code indique à CocosSharp pour rechercher un fichier appelé `paddle` (l’extension est omise, dont nous parlerons plus loin dans ce guide). CocosSharp recherchera des noms de fichiers `paddle` dans le dossier racine (qui est **contenu**), ainsi que tous les dossiers ajoutés à la `gameView.ContentManager.SearchPaths` (présenté dans la section précédente).

Nous allons ajouter nos fichiers directement à la racine **contenu** dossier pour iOS et Android. Pour ce faire, avec le bouton droit ou en cliquant sur le **contenu** dossier dans le projet iOS et sélectionnez **ajouter** > **ajouter des fichiers...** Accédez à où nous décompressé le contenu précédemment et sélectionnez **paddle.png**. Si demandé sur la façon d’ajouter le fichier au dossier, nous devons sélectionner le **copie** option :

![](part2-images/image2.png "Forum sur la façon d’ajouter le fichier au dossier, sélectionnez l’option de copie")

Ensuite, nous allons ajouter le fichier au projet Android. Avec le bouton droit ou en cliquant sur le dossier de contenu (qui se trouve dans le **actifs** dossier projets Android) et sélectionnez **ajouter** > **ajouter des fichiers...** . Cette fois-ci, accédez à du projet iOS **contenu** dossier. Lorsque vous êtes invité sur l’ajout du fichier, sélectionnez le **ajouter un lien** option :

![](part2-images/addalink.png "Lorsque vous êtes invité sur l’ajout du fichier, sélectionnez l’option Ajouter un lien")

Nous aborderons pourquoi les fichiers devaient être ajoutés pour les deux projets ci-dessous. De chaque projet **contenu** dossier contiennent désormais la **paddle.png** fichier :

![](part2-images/image3.png "Dossier de contenu de chaque projet contient maintenant le fichier paddle.png")

Si nous exécutons le jeu, nous allons voir notre `CCSprite` dessiné :

![](part2-images/image4.png "Si le jeu est exécuté, le CCSprite sera dessiné.")


## <a name="file-details"></a>Détails du fichier

Jusqu'à présent, nous avons ajouté un seul fichier au projet, et le processus est relativement simple. Nous avons simplement ajouté le **paddle.png** le fichier notre **contenu** dossiers et il référencé dans du code. Prenons un moment pour examiner certaines considérations lorsque vous travaillez avec des fichiers dans CocosSharp.

### <a name="capitalization"></a>Mise en majuscules

Notez que le nom de fichier et la chaîne que nous avons utilisé dans le code d’accès au fichier sont en minuscules. Il s’agit, car certaines plateformes (par exemple, le simulateur Windows desktop et iOS) respectent la casse, tandis que les autres plateformes (telles que les appareils Android et iOS) respectent la casse. Nous allons utiliser tous les fichiers en minuscules pour le reste de ce didacticiel pour que notre code et les fichiers restent en tant qu’inter-plateformes que possible.

### <a name="extensions"></a>Extensions

Le constructeur pour créer le Sprite n’inclut pas l’extension « .png » pour référencer le fichier de la manette de jeu. L’extension des fichiers est généralement omise lorsque vous travaillez sur des projets CocosSharp en tant qu’extensions de fichier du même type d’élément peut-être différer entre les plateformes. Par exemple, les fichiers audio peuvent être des formats de fichier .aiff, .mp3 ou .wma selon la plateforme. Si vous omettez l’extension permet le même code fonctionner sur toutes les plateformes, quelle que soit l’extension de fichier.

### <a name="content-in-platform-specific-projects"></a>Le contenu des projets spécifiques à la plateforme

Contrairement à la plupart de ses fichiers de code qui peuvent être dans un une bibliothèque PCL, contenue de fichiers doivent être ajoutés à chaque projet spécifique à la plateforme. CocosSharp nécessite ceci pour deux raisons :

1. Chaque plate-forme dispose de différents **des actions de génération**. Contenu ajouté aux projets d’iOS doit utiliser le **BundleResource** action de génération. Le contenu ajouté aux projets Android doit utiliser le **AndroidAsset** action de génération.
2. Certaines plateformes requièrent des formats de fichier spécifique à la plateforme. Par exemple, les fichiers de polices .xnb diffèrent iOS et Android, comme nous le verrons plus loin dans cette procédure pas à pas.

Si un format de fichier ne diffère pas entre les plateformes (par exemple, .png), chaque plateforme peut utiliser le même fichier, où une ou plusieurs plateformes peuvent lier le fichier dans le même emplacement.

# <a name="orientation"></a>Orientation

Comme toute autre application, les applications de la CocosSharp peuvent s’exécuter dans les orientations portrait ou paysage. Nous allons ajuster notre jeu à s’exécuter en mode portrait uniquement. Tout d’abord, nous allons modifier le code de résolution dans le jeu pour gérer les proportions de portrait. Pour ce faire, modifiez le `width` et `height` des valeurs dans le `LoadGame` méthode dans le `ViewController` classe sur iOS et `MainActivity` classe sur Android :


```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    ...
```

Ensuite, nous devrons modifier chaque projet spécifique à la plateforme pour s’exécuter en mode portrait.


## <a name="ios-orientation"></a>l’Orientation d’iOS

Pour modifier l’orientation du projet iOS, sélectionnez le **Info.plist** de fichiers dans le **BouncingGame.iOS** le projet, puis modifier **iPhone/iPod informations de déploiement** et le **iPad informations de déploiement** pour inclure uniquement les orientations de Portrait :

![](part2-images/image5.png "Modifier les iPhone/iPod informations de déploiement et l’informations de déploiement pour inclure uniquement les orientations de Portrait iPad")


## <a name="android-orientation"></a>Orientation Android

Pour modifier l’orientation du projet Android, ouvrez le fichier MainActivity.cs dans le projet BouncingGame.Android. Modifier la définition d’attribut activité afin qu’il spécifie uniquement une orientation portrait, comme suit :


```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```


# <a name="default-coordinate-system"></a>Système de coordonnées par défaut

Notre code qui instancie un `CCSprite`, définit le `PositionX` et `PositionY` sur 100. Par défaut, cela signifie que le `CCSprite` center est positionnée à 100 pixels de haut et vers la droite dans le coin inférieur gauche de l’écran. Le système de coordonnées peut être modifié en attachant un `CCCamera` à la `CCLayer`. Nous ne travaillons avec `CCCamera` dans ce projet, mais plus d’informations sur `CCCamera` se trouvent dans le [docs de l’API de CocosSharp](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

Les 100 pixels mentionnées ci-dessus « jeu » pixels par opposition aux pixels sur le matériel. Cela signifie que le même jeu en cours d’exécution sur un appareil d’une résolution différente (par exemple, un iPad et un iPhone) créent des objets en cours positionné et correctement dimensionnée par rapport à l’écran physique. Plus précisément, les zone visible de notre jeu seront toujours 1024 pixels de hauteur et largeur de 768 pixels, comme il s’agit de la résolution nous avons spécifié précédemment dans le `LoadGame` (méthode).


# <a name="adding-the-ball-ccsprite"></a>Ajout de la boule CCSprite

Maintenant que nous connaissons les principes fondamentaux de l’utilisation de `CCSprite`, nous allons ajouter nos seconde `CCSprite` – une boule. Nous allons après les étapes qui sont très semblables à la façon dont nous avons créé la manette de jeu `CCSprite`. 

Tout d’abord, nous allons ajouter le **ball.png** image à partir du dossier décompressé dans notre projet d’iOS **contenu** dossier. Sélectionnez cette option pour **copie** le fichier à notre **contenu** active. Suivez les mêmes étapes que ci-dessus pour ajouter un lien vers le **ball.png** fichier dans le projet Android.

Ensuite créer le `CCSprite` pour cette bille en ajoutant un membre appelé `ballSprite` à notre `GameScene` classe, ainsi que le code d’instanciation pour le `ballSprite`. Une fois notre `GameLayer` classe doit ressembler à ceci :


```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```


# <a name="adding-the-score-cclabel"></a>Ajout de la valeur de CCLabel Score

Le dernier élément visuel, nous allons ajouter à notre jeu est un `CCLabel` pour afficher le nombre de fois où l’utilisateur a retourné avec succès de la boule. Le `CCLabel` utilise une police spécifiée dans le constructeur pour afficher des chaînes à l’écran.

Ajoutez le code suivant pour créer un `CCLabel` d’instance dans `GameLayer`. Une fois terminé, votre code doit ressembler à ceci :


```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    ...
```

Notez qu’à l’aide de la scoreLabel un `AnchorPoint` de `CCPoint.AnchorUpperLeft`, ce qui signifie que la `PositionX` et `PositionY` valeurs définissent sa position de l’angle supérieur gauche. Cela nous permet de position du `scoreLabel` relatif au haut à gauche de l’écran sans avoir à prendre en compte ses dimensions.


# <a name="implementing-every-frame-logic"></a>Implémentation de la logique de chaque image

Jusqu'à présent, notre jeu présente une scène statique. Nous allons ajouter la logique pour contrôler le mouvement des objets dans la scène en ajoutant du code qui met à jour la position de nos objets à une fréquence élevée. Dans ce cas, notre code s’exécutera soixante fois par seconde - également appelé soixante *frames* par seconde (sauf si le matériel ne peut pas gérer cette mise à jour fréquemment). Plus précisément, nous allons ajouter la logique pour rendre la balle se produisant et rebondit sur la manette de jeu pour déplacer la manette de jeu en fonction de l’entrée et mettre à jour le score du joueur chaque fois que la balle établit la manette de jeu.

Le `Schedule` (méthode), qui est fourni par le `CCNode` de classes, nous permet d’ajouter une logique de chaque image à notre jeu. Nous allons ajouter le code après `// New code` au constructeur GameLayer :


```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

À présent créer un `RunGameLogic` méthode dans notre `GameLayer` (classe), qui hébergera l’ensemble de notre logique de chaque image :


```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Le paramètre de type float définit la durée pendant laquelle notre frame est exprimée en secondes. Nous utiliserons cette valeur lors de l’implémentation de déplacement du ballon.


## <a name="making-the-ball-fall"></a>Rendre la baisse de la boule

Vous pouvez rendre la balle se répartissent en soit notre propre code de gravité implémentant manuellement ou à l’aide de la fonctionnalité intégrée de Box2D dans CocosSharp. Le moteur de simulation Box2D physique est disponible pour les jeux de CocosSharp. Il est très puissant et plus efficace, mais requiert l’écriture de code d’installation. Étant donné que notre simulation physique est relativement simple, nous implémenterons il manuellement.

Pour implémenter la gravité, nous devrons au magasin X actuel et vitesse Y de notre balle. Nous allons ajouter deux membres de notre `GameLayer` classe :


```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Ensuite, nous pouvons implémenter la logique de baisse `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

## <a name="moving-the-paddle-with-touch-input"></a>Déplacement de la manette de jeu avec une entrée tactile

Maintenant que le curseur est en, nous allons ajouter déplacement horizontal à notre manette de jeu à l’aide de la `CCEventListenerTouchAllAtOnce` objet. Cet objet fournit un nombre d’événements d’entrée associés. Dans notre cas, vous souhaitez être averti si les points tactiles déplacent afin de nous pouvons modifier la position de la manette de jeu. Le `GameLayer` déjà instancie un `CCEventListenerTouchAllAtOnce`, donc nous suffit d’affecter la `OnTouchesMoved` déléguer. Pour ce faire, modifiez la méthode AddedToScene comme suit :


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of our drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

Ensuite, nous allons implémenter `HandleTouchesMoved`:


```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```


## <a name="implementing-ball-collision"></a>Mise en œuvre de la boule Collision

Si nous exécutons le jeu maintenant nous remarquerez que le curseur se trouve à la manette de jeu. Nous allons implémenter *collision* (logique pour réagir aux objets du jeu de qui se chevauchent) dans notre code de chaque image. Étant donné que la modification des objets de déplacement positionne chaque frame, la vérification de collision est généralement également effectuées chaque frame. Nous allons également ajouter de rapidité sur l’axe des X lorsque la balle la manette de jeu pour ajouter une demande d’accès au jeu, mais cela signifie que nous avons besoin de conserver la boule de faire défiler les bords de l’écran. Nous allons le faire dans le `RunGameLogic` code après avoir appliqué la rapidité de la `ballSprite` après `// New Code`:


```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```


## <a name="adding-scoring"></a>Ajout de calcul de score

Maintenant que notre jeu peut être lu, la dernière étape consiste à ajouter la logique de calcul de score. Tout d’abord, nous allons ajouter un membre de score à la classe GameLayer nommée `score`:


```csharp
int score;
```

Nous allons utiliser cette variable pour suivre les score du joueur et pour l’afficher à l’aide de notre `scoreLabel`. Pour faire cela ajoutez le code suivant à l’intérieur de l’instruction if dans `RunGameLogic` lorsque la balle et la manette de jeu se chevauchent :


```csharp
...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
...
```

Nous pouvons maintenant exécuter le jeu et que notre jeu affiche un score qui augmente la balle rebondit sur la manette de jeu :

![](part2-images/image1.png "Exécuter le jeu et qu’il affiche un score qui augmente la balle rebondit sur la manette de jeu")


# <a name="summary"></a>Récapitulatif

Cette procédure pas à pas affiche la création d’un jeu d’inter-plateformes avec des graphiques, physique et d’entrée à l’aide de CocosSharp. Il est la première étape de mise en route du développement de jeux CocosSharp. Nous avons examiné certaines des classes plus courantes dans CocosSharp, comment construire une arborescence d’éléments visuels pour les objets rendus correctement et comment implémenter la logique de jeu de chaque image.

Cette procédure pas à pas couverte uniquement une petite partie des fonctions proposées par le moteur de jeu CocosSharp. Pour plus d’informations et procédures pas à pas sur les autres rubriques CocosSharp, consultez [le reste de notre guides CocosSharp](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Liens associés

- [Jeu terminé (exemple)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Contenu de jeu (exemple)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
