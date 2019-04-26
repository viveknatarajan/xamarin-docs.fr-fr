---
title: Détails de BouncingGame
description: Ce document fournit une procédure pas à pas pour la création de BouncingGame, un jeu de boule de rebondissement simple créé avec CocosSharp.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: 9fd6c9108695f58bd69a1c4aa307ca2e4be6dede
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357489"
---
# <a name="bouncinggame-details"></a>Détails de BouncingGame

> [!TIP]
> Vous trouverez le code de BouncingGame dans le [exemples Xamarin](https://developer.xamarin.com/samples/mobile/BouncingGame/). Pour mieux suivre ce guide, télécharger et Explorer le code comme le guide de référence.

Cette procédure pas à pas montre comment implémenter un jeu de balle rebondit simple à l’aide de CocosSharp. 

 - Contenu de jeu décompression
 - Éléments visuels CocosSharp courantes
 - Ajout d’éléments visuels pour `GameLayer`
 - Implémentation de la logique de chaque frame

Notre jeu terminé doit ressembler à ceci :

![BouncingGame, completed](bouncing-game-images/image1.png "BouncingGame, completed")

## <a name="unzipping-game-content"></a>Contenu de jeu décompression

Les développeurs de jeux utilisent souvent le terme *contenu* pour faire référence à des fichiers de code non qui sont généralement créés par visual artistes, des concepteurs de jeux ou des concepteurs audio. Types de contenu courants incluent les fichiers utilisés pour afficher des éléments visuels, émettre un signal sonore ou contrôler le comportement de l’intelligence artificielle (IA). Point de vue d’une équipe de développement de jeux, de contenu est généralement créé par non programmeurs. Notre jeu comprend deux types de contenu :

 - fichiers PNG pour définir l’apparaissent de la balle et une raquette.
 - Un fichier unique xnb pour définir la police utilisée par l’affichage de score (décrit plus en détail lorsque nous ajoutons l’affichage de score ci-dessous)

Vous trouverez ce contenu utilisé ici dans [contenu zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Nous avons besoin de ces fichiers téléchargé et décompressé dans un emplacement auquel nous accéderons plus loin dans cette procédure pas à pas.

## <a name="common-cocossharp-visual-elements"></a>Éléments visuels CocosSharp courantes

CocosSharp fournit un nombre de classes utilisées pour afficher des éléments visuels. Certains éléments sont directement visibles, tandis que d’autres sont utilisés pour l’organisation. Nous allons utiliser ce qui suit dans le jeu :

 - `CCNode` – Classe base pour tous les objets visuels dans CocosSharp. Le `CCNode` classe contient un `AddChild` fonction qui peut être utilisée pour construire une hiérarchie parent/enfant, également appelé un *arborescence d’éléments visuels* ou *graphique de scène*. Toutes les classes mentionnées ci-dessous héritent `CCNode`.
 - `CCScene` – Racine de l’arborescence visuelle pour tous les jeux de CocosSharp. Tous les éléments visuels doivent faire partie d’une arborescence visuelle avec un `CCScene` à la racine, ou ils ne seront pas visibles.
 - `CCLayer` – Conteneur visuel des objets, tels que `CCSprite`. Comme son nom l’indique, la `CCLayer` classe est utilisée pour contrôler comment visual couche d’éléments sur eux.
 - `CCSprite` : Affiche une image ou une partie d’une image. `CCSprite` instances peuvent être positionnés, redimensionné et fournissant un nombre d’effets visuels.
 - `CCLabel` : Affiche une chaîne à l’écran. La police utilisée par `CCLabel` est défini dans un fichier xnb. Nous aborderons xnbs plus en détail ci-dessous.

Pour comprendre comment les différents types sont utilisés nous allons considérer comme un seul `CCSprite`. Éléments visuels doivent être ajoutés à un `CCLayer`, et l’arborescence visuelle doit avoir un `CCScene` à sa racine. Par conséquent, la relation parent/enfant pour un seul `CCSprite` serait `CCScene`  >  `CCLayer`  >  `CCSprite`.

## <a name="adding-visual-elements-to-gamelayer"></a>Ajout d’éléments visuels à GameLayer

### <a name="adding-the-paddle-sprite"></a>Ajout du sprite raquette

Initialement, nous allons définir d’arrière-plan du jeu à noir et également ajouter un seul `CCSprite` rendu sur l’écran. Modifier le `GameLayer` classe pour ajouter un `CCSprite` comme suit :

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
            // Use the bounds to layout the positioning of the drawable assets
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

Le code ci-dessus crée une valeur unique `CCSprite` et l’ajoute en tant qu’enfant de le `GameLayer`. Le `CCSprite` constructeur permet de définir le fichier image à utiliser en tant que chaîne. Notre code indique à CocosSharp pour rechercher un fichier appelé `paddle` (l’extension est omise, ce que nous aborderons plus loin dans ce guide). CocosSharp recherchera des noms de fichiers `paddle` dans le dossier de contenu racine (c'est-à-dire **contenu**), ainsi que tous les dossiers ajoutés à la `gameView.ContentManager.SearchPaths` (présenté dans la section précédente).

Nous allons ajouter les fichiers directement à la racine **contenu** dossier pour iOS et Android. Pour ce faire, clic droit ou en cliquant sur le **contenu** dossier dans le projet iOS, puis sélectionnez **ajouter** > **ajouter des fichiers...** Accédez à où nous avons décompressé le contenu précédemment et sélectionnez **paddle.png**. Si vous êtes invité sur la façon d’ajouter le fichier au dossier, nous devons sélectionner le **copie** option :

![Le fichier à ajouter à la boîte de dialogue dossier](bouncing-game-images/image2.png "le fichier à ajouter à la boîte de dialogue dossier")

Ensuite, nous allons ajouter le fichier au projet Android. Clic droit ou cliquez sur le dossier de contenu (qui se trouve dans le **actifs** dossier sur les projets Android) et sélectionnez **ajouter** > **ajouter des fichiers...** . Cette fois, accédez au projet iOS **contenu** dossier. Lorsque vous êtes invité sur l’ajout du fichier, sélectionnez le **ajouter un lien** option :

![Le fichier à ajouter à la boîte de dialogue dossier](bouncing-game-images/addalink.png "ajouter le fichier à la boîte de dialogue dossiers")

Nous aborderons pourquoi devaient être ajoutés pour les deux projets ci-dessous. De chaque projet **contenu** dossier contiennent désormais la **paddle.png** fichier :

![Le contenu du dossier contenu](bouncing-game-images/image3.png "le contenu du dossier de contenu")

Si nous exécutons le jeu, nous allons voir le `CCSprite` dessiné :

![La raquette, dessinée à l’écran](bouncing-game-images/image4.png "la raquette, dessinée à l’écran")

### <a name="file-details"></a>Détails du fichier

Jusqu'à présent, nous avons ajouté un seul fichier au projet, et le processus était assez simple. Nous avons simplement ajouté le **paddle.png** de fichiers à la **contenu** dossiers et il référencé dans le code. Prenons un moment pour examiner certaines considérations lorsque vous travaillez avec des fichiers dans CocosSharp.

#### <a name="capitalization"></a>Mise en majuscules

Notez que le nom de fichier et la chaîne que nous avons utilisé dans le code d’accès au fichier sont en minuscules. Il s’agit, car certaines plateformes (par exemple, le simulateur Windows de bureau et iOS) respectent la casse, tandis que les autres plateformes (telles que les appareils Android et iOS) respectent la casse. Nous allons utiliser tous les fichiers en minuscules pour le reste de ce didacticiel afin que les fichiers et le code restent en tant qu’inter-plateformes que possible.

#### <a name="extensions"></a>Extensions

Le constructeur pour la création du Sprite n’inclut pas l’extension « « .png » lorsque vous référencez le fichier raquette. L’extension des fichiers est généralement omise lorsque travaillant sur des projets de CocosSharp en tant qu’extensions de fichier du même type d’élément peut-être différer entre plateformes. Par exemple, les fichiers audio peuvent être des formats de fichier .aiff, .mp3 ou .wma selon la plateforme. Si vous omettez l’extension permet le même code fonctionne sur toutes les plateformes, quel que soit l’extension de fichier.

#### <a name="content-in-platform-specific-projects"></a>Le contenu dans les projets propres à la plateforme

Contrairement à la plupart des fichiers de code qui peuvent être dans une bibliothèque de classes portable, les fichiers de contenu doivent être ajoutés à chaque projet spécifique à la plateforme. CocosSharp cela nécessite pour deux raisons :

1. Chaque plateforme dispose de différents **actions de génération**. Le contenu ajouté à des projets iOS doit utiliser le **BundleResource** action de génération. Le contenu ajouté aux projets Android doit utiliser le **AndroidAsset** action de génération.
2. Certaines plateformes requièrent des formats de fichier spécifique à la plateforme. Par exemple, fichiers de polices .xnb diffèrent entre iOS et Android, comme nous le verrons plus loin dans cette procédure pas à pas.

Si un format de fichier ne diffère pas entre les plateformes (par exemple, .png), chaque plateforme peut utiliser le même fichier, où une ou plusieurs plateformes peuvent lier le fichier à partir du même emplacement.

## <a name="orientation"></a>Orientation

Comme pour n’importe quel autre application, les applications de CocosSharp peuvent s’exécuter dans les orientations portrait ou paysage. Nous modifions le jeu à s’exécuter en mode portrait uniquement. Tout d’abord, nous allons modifier le code de résolution dans le jeu pour gérer un rapport hauteur / largeur portrait. Pour ce faire, modifiez le `width` et `height` des valeurs dans le `LoadGame` méthode dans le `ViewController` classe sur iOS et `MainActivity` classe sur Android :

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
    // ...
```

Ensuite, nous allons devoir modifier chaque projet spécifique à la plateforme doit être exécuté en mode portrait.

### <a name="ios-orientation"></a>orientation d’iOS

Pour modifier l’orientation du projet iOS, sélectionnez le **Info.plist** de fichiers dans le **BouncingGame.iOS** le projet, puis modifier **iPhone/iPod des informations de déploiement** et le **iPad déploiement Info** pour inclure uniquement les orientations Portrait :

![Options d’orientation](bouncing-game-images/image5.png "options d’Orientation")

### <a name="android-orientation"></a>Orientation Android

Pour modifier l’orientation du projet Android, ouvrez le fichier MainActivity.cs dans le projet BouncingGame.Android. Modifier la définition d’attribut activité afin qu’il spécifie uniquement une orientation portrait, comme suit :

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>Système de coordonnées par défaut

Notre code qui instancie un `CCSprite`, définit le `PositionX` et `PositionY` valeurs à 100. Par défaut, cela signifie que le `CCSprite` center sera positionné à 100 pixels de haut et vers la droite dans la coin inférieur gauche de l’écran. Le système de coordonnées peut être modifié en attachant un `CCCamera` à la `CCLayer`. Nous ne sont pas travailler avec `CCCamera` dans ce projet, mais plus d’informations sur `CCCamera` se trouve dans le [documentation sur les API de CocosSharp](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

100 pixels mentionnés ci-dessus pixels « jeu » par opposition aux pixels sur le matériel. Cela signifie que le même jeu en cours d’exécution sur un appareil d’une résolution différente (par exemple, un iPad et un iPhone) entraîne des objets en cours positionné et correctement dimensionnée par rapport à l’écran physique. Plus précisément, zone visible du jeu seront toujours 1 024 pixels en hauteur et 768 pixels de large, car il s’agit la résolution nous avons spécifié précédemment dans le `LoadGame` (méthode).

## <a name="adding-the-ball-sprite"></a>Ajout du sprite boule

Maintenant que nous connaissons les principes fondamentaux de l’utilisation de `CCSprite`, nous allons ajouter le second `CCSprite` – une balle. Nous allons suivant les étapes qui sont très similaires à la façon dont nous avons créé la raquette `CCSprite`. 

Tout d’abord, nous allons ajouter le **ball.png** image à partir du dossier décompressé dans le projet iOS **contenu** dossier. Sélectionnez cette option pour **copie** le fichier à la **contenu** directory. Suivez les mêmes étapes que ci-dessus pour ajouter un lien vers le **ball.png** fichier dans le projet Android.

Ensuite, créez le `CCSprite` pour cette bille en ajoutant un membre appelé `ballSprite` au `GameScene` classe, ainsi que le code d’instanciation pour le `ballSprite`. Une fois la `GameLayer` classe se présente comme suit :

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

## <a name="adding-the-score-label"></a>Ajout de l’étiquette de score

Le dernier élément visuel, nous allons ajouter au jeu est un `CCLabel` pour afficher le nombre de fois où l’utilisateur a retourné avec succès de la balle. Le `CCLabel` utilise une police spécifiée dans le constructeur pour afficher des chaînes à l’écran.

Ajoutez le code suivant pour créer un `CCLabel` dans l’instance `GameLayer`. Une fois terminé, le code doit ressembler à ceci :

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
    // ...
```

Notez qu’à l’aide de la scoreLabel un `AnchorPoint` de `CCPoint.AnchorUpperLeft`, ce qui signifie que le `PositionX` et `PositionY` valeurs définissent sa position de l’angle supérieur gauche. Cela nous permet de position la `scoreLabel` par rapport à haut à gauche de l’écran sans avoir à prendre en compte ses dimensions.

## <a name="implementing-every-frame-logic"></a>Implémentation de la logique de chaque frame

Jusqu’ici, le jeu présente une scène statique. Nous allons ajouter une logique pour contrôler le déplacement des objets dans la scène en ajoutant du code qui met à jour la position des objets à une fréquence élevée. Dans ce cas, le code s’exécutera soixante fois par seconde - également appelé soixante *frames* par seconde (sauf si le matériel ne peut pas gérer cette mise à jour fréquemment). Plus précisément, nous allons ajouter une logique pour rendre la balle appartiennent et rebondissent contre la raquette, pour déplacer la raquette en fonction de l’entrée et mettre à jour le score du joueur chaque fois que la balle frappe la raquette.

Le `Schedule` (méthode), qui est fourni par le `CCNode` de classe, nous permet d’ajouter une logique de chaque image dans le jeu. Nous allons ajouter le code après `// New code` au constructeur GameLayer :

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

À présent créer un `RunGameLogic` méthode dans le `GameLayer` (classe), ce qui va héberger toute la logique de chaque image :

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Le paramètre de type float définit la durée pendant laquelle le frame est en secondes. Nous allons utiliser cette valeur lors de l’implémentation de déplacement du ballon.

### <a name="making-the-ball-fall"></a>Rendre la balle appartiennent

Nous pouvons faire la balle se répartissent en un code de gravité implémentation manuellement ou à l’aide de la fonctionnalité intégrée de Box2D dans CocosSharp. Le moteur de simulation Box2D physique est disponible pour les jeux de CocosSharp. Il est très puissant et efficace, mais nécessite l’écriture de code de programme d’installation. Étant donné que la simulation physique est relativement simple, ici il sera implémenté manuellement.

Pour implémenter la gravité, nous devons magasin X actuel et vitesse Y de la boule. Nous allons ajouter deux membres pour la `GameLayer` classe :

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Ensuite, nous pouvons implémenter la logique en baisse dans `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>Déplacer la raquette avec entrée tactile

Maintenant que la balle diminue, nous allons ajouter déplacement horizontal à la raquette à l’aide de la `CCEventListenerTouchAllAtOnce` objet. Cet objet fournit un nombre d’événements d’entrée associées. Dans ce cas, nous voulons être averti si les points tactiles déplacent afin de nous ne pouvons ajuster la position de la raquette. Le `GameLayer` déjà instancie un `CCEventListenerTouchAllAtOnce`, de sorte que nous devons simplement affecter la `OnTouchesMoved` déléguer. Pour ce faire, modifiez la méthode AddedToScene comme suit :

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
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

### <a name="implementing-ball-collision"></a>Implémentation de collision de boule

Si nous exécutons le jeu maintenant nous constatons que la balle traverse la raquette. Nous allons implémenter *collision* (logique de réagir à des objets de jeux superposés) dans le code de chaque image. Étant donné que la modification des objets de déplacement positionne chaque trame, vérification de collision est généralement effectuée également chaque trame. Nous allons également ajouter velocity sur l’axe des X lorsque la balle atteigne la raquette pour ajouter un défi pour le jeu, mais cela signifie que nous devons garder la balle de faire défiler les bords de l’écran. Nous allons le faire dans le `RunGameLogic` code après avoir appliqué la rapidité à la `ballSprite` après `// New Code`:

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

## <a name="adding-scoring"></a>Ajout de notation

Maintenant que le jeu est lisible, la dernière étape consiste à ajouter la logique de calcul de score. Tout d’abord, nous allons ajouter un membre de score à la classe GameLayer nommée `score`:

```csharp
int score;
```

Nous allons utiliser cette variable pour suivre le score du joueur et pour l’afficher à l’aide de la `scoreLabel`. Pour effectuer cette ajoute le code suivant à l’intérieur de l’instruction if dans `RunGameLogic` quand la balle et une raquette se chevauchent :

```csharp
// ...
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
// ...
```

Maintenant, nous pouvons exécuter le jeu et observez que le jeu affiche un score qui incrémente comme la balle rebondit sur la raquette :

![Le jeu terminé, avec un score incrémentation](bouncing-game-images/image1.png "le jeu terminé, avec un score d’incrémentation")

## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas présentées création d’un jeu multiplateforme avec entrée à l’aide de CocosSharp, physique et du graphisme. Il est la première étape de mise en route du développement de jeux de CocosSharp. Nous avons vu certaines des classes plus courantes dans CocosSharp, comment construire une arborescence d’éléments visuels pour les objets affichés correctement et comment implémenter la logique de jeu de chaque image.

Cette procédure pas à pas couvert uniquement une petite partie de ce que propose le moteur de jeu de CocosSharp. Pour plus d’informations et procédures pas à pas sur les autres rubriques de CocosSharp, consultez [le reste des guides de CocosSharp](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Liens connexes

- [Jeu terminé (exemple)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Contenu de jeu (exemple)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
