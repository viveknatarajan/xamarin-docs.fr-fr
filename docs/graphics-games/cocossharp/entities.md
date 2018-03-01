---
title: "Entités dans CocosSharp"
description: "Le modèle d’entité est une méthode efficace pour organiser le code de jeu. Il améliore la lisibilité, rend le code plus facile à gérer et il exploite les fonctionnalités intégrées de parent/enfant."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: fe722ce75f0322ab60bb6fd967ff2c498b2e7b20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="entities-in-cocossharp"></a>Entités dans CocosSharp

_Le modèle d’entité est une méthode efficace pour organiser le code de jeu. Il améliore la lisibilité, rend le code plus facile à gérer et il exploite les fonctionnalités intégrées de parent/enfant._

Le modèle d’entité peut améliorer les efforts d’un développeur avec CocosSharp par une organisation de code améliorée. Cette procédure pas à pas est un exemple pratique montrant comment créer deux entités : une entité de livraison et une entité de puce. Ces entités seront objets autonomes, ce qui signifie qu’instancié une fois qu’ils seront automatiquement rendus et effectuera une logique de déplacement en fonction de leur type. 

Ce guide couvre les rubriques suivantes :

 - Introduction aux entités de jeu
 - Générale et les types d’entités spécifique
 - Programme d’installation de projet
 - Création des classes d’entité
 - Ajout d’instances d’entité de la `GameLayer`
 - Réagir à la logique de l’entité dans le `GameLayer`

Le jeu terminé doit ressembler à ceci :

![](entities-images/image1.png "Le jeu terminé doit ressembler à ceci")


# <a name="introduction-to-game-entities"></a>Introduction aux entités de jeu

Entités de jeu sont des classes qui définissent des objets nécessitant une logique de rendu, collision, physique ou intelligence artificielle. Heureusement, les entités présentes dans la base de code d’un jeu correspondent souvent aux objets conceptuels dans un jeu. Lorsque cela est vrai, identifier les entités nécessaires dans un jeu peut être plus facilement réalisée. 

Par exemple, un espace à thème [dépanner em de jeu](http://en.wikipedia.org/wiki/Shoot_%27em_up) peut inclure les entités suivantes :

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

Ces entités serait leurs propres classes dans le jeu, et chaque instance nécessite peu ou pas le programme d’installation au-delà de l’instanciation.


# <a name="general-vs-specific-entity-types"></a>Vs générales. Types d’entités spécifique

La première question rencontrés par les développeurs de jeux à l’aide d’un système de l’entité est la quantité afin de généraliser leurs entités. La plus spécifique des implémentations définirait classes pour chaque type d’entité, même si elles diffèrent par quelques caractéristiques. Systèmes plus générales vous associer des groupes d’entités dans une classe et les instances à personnaliser.

Par exemple, nous pouvons imaginer un jeu d’espace qui définit les classes suivantes :

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

Une approche plus généralisé serait de créer une seule classe de lecteur fourni et une seule classe adverse est fourni, ce qui peut être configuré pour prendre en charge les types de livraison différentes. Personnalisation peut inclure quelle image pour charger, le type des entités de puce à créer lors de la résolution de problèmes, les coefficients de mouvement, et la logique pour les menaces AI est fourni. Dans ce cas, la liste des entités peut être réduite à :

 - `PlayerShip`
 - `EnemyShip`

Naturellement, ces types d’entité peuvent être davantage généralisées en autorisant la personnalisation par instance pour contrôler le mouvement. Instances de livraison de lecteur lit à partir de l’entrée lors de la livraison adverse instances peuvent exécuter la logique de AI. Cela signifie que les entités pourraient être généralisées davantage en une seule classe :

 - `Ship`

La généralisation peut continuer même : un jeu peut utiliser une seule classe de base pour toutes les entités. Cette classe unique, qui peut être appelée `GameEntity`, serait la classe utilisée pour chaque instance d’entité dans le jeu entier, y compris les entités qui ne sont pas est fourni comme puces et les éléments pouvant être collectés (power-ups).

Cela général de la plupart des systèmes est communément appelée un *système du composant*. Dans un tel système, jeu entités peuvent être des composants individuels tels que physique, AI, ou composants de rendu pour personnaliser l’apparence et le comportement. Systèmes composant pures activer maximum de souplesse et peuvent éviter les problèmes causés par l’utilisation de l’héritage, telles que les chaînes complexes d’héritage. Comme avec d’autres généralisations, systèmes de composant effective peuvent être difficiles de configurer et de réduisent la simplicité du code.

Le niveau de généralisation utilisé dépend de plusieurs considérations, y compris :

 - Taille du jeu – jeux plus petits supportent créer des classes spécifiques, tandis que les jeux plus grands peuvent être difficiles de gérer avec un grand nombre de classes.
 - Développement – piloté par des données les jeux qui s’appuient sur des données (images, des modèles 3D et des fichiers de données tels que JSON ou XML) peuvent tirer parti d’avoir généralisé des types d’entité et les spécificités en fonction des données de configuration. Il s’agit notamment d’importation pour les jeux qui s’attendre à ajouter du contenu pendant le développement ou après que le jeu a été publié.
 - Modèles de moteur de jeu : certains moteurs de jeu encourageons fortement l’utilisation des composants système alors que d’autres permettent aux développeurs de décider comment organiser les entités. CocosSharp ne requiert pas l’utilisation d’un système de composant, les développeurs sont libres d’implémenter n’importe quel type d’entité. 

Par souci de simplicité, nous utiliserons une approche basée sur une classe spécifique avec une seule entité de livraison et de la puce pour ce didacticiel.


# <a name="project-setup"></a>Programme d’installation de projet

Avant de commencer la mise en œuvre de notre entités, nous devons créer un projet. Nous utiliserons les modèles de projet CocosSharp pour simplifier la création du projet. [Vérifiez ce billet](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) pour plus d’informations sur la création d’un projet CocosSharp à partir de Visual Studio pour les modèles de Mac. Le reste de ce guide utilise le nom du projet **EntityProject**.

Une fois notre projet est créé, nous mettrons la résolution de notre jeu pour s’exécuter à 480 x 320. Pour ce faire, appelez `CCScene.SetDefaultDesignResolution` dans le `GameAppDelegate.ApplicationDidFinishLaunching` méthode comme suit :


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

Pour plus d’informations sur le traitement des résolutions de CocosSharp, consultez notre [guide sur la gestion des plusieurs résolutions dans CocosSharp](~/graphics-games/cocossharp/resolutions.md).


# <a name="adding-content-to-the-project"></a>Ajout de contenu au projet

Une fois notre projet a été créé, nous allons ajouter les fichiers contenus dans [ce fichier zip contenu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true). Pour ce faire, téléchargez le fichier zip et décompressez-le. Ajouter à la fois **ship.png** et **bullet.png** à la **contenu** dossier. Le **contenu** dossier se trouve dans le **actifs** dossier sur Android et sera à la racine du projet sur iOS. Une fois ajoutée, nous devrions voir les deux fichiers dans le **contenu** dossier :

![](entities-images/image2.png "Une fois ajoutée, les deux fichiers doivent se trouver dans le dossier de contenu")


# <a name="creating-the-ship-entity"></a>Création de l’entité de livraison

La `Ship` classe sera la première entité de notre jeu. Pour ajouter un `Ship` de classe, commencez par créer un dossier appelé **entités** à la racine du projet. Ajoutez une nouvelle classe dans le **entités** dossier appelé `Ship`:

![](entities-images/image3.png "Ajoutez une nouvelle classe dans le dossier d’entités appelé expédition")

La première modification, nous allons effectuer notre `Ship` classe consiste à laisser héritent de la `CCNode` classe. `CCNode` sert de classe de base pour les classes CocosSharp communes telles que `CCSprite` et `CCLayer`et nous offre les fonctionnalités suivantes :

 - `Position` propriété pour le déplacement de la livraison autour de l’écran.
 - `Children` propriété pour l’ajout d’un `CCSprite.`
 - `Parent` propriété, qui peut être utilisée pour attacher `Ship` instances à d’autres `CCNodes`. Il ne faut utiliser cette fonctionnalité dans ce didacticiel ; jeux de plus grandes généralement profiter de l’attachement d’entités à d’autres `CCNodes`. 
 - `AddEventListener` méthode pour répondre à l’entrée pour le déplacement de la livraison.
 - `Schedule` méthode pour résoudre les puces.

Nous allons également ajouter un `CCSprite` afin que notre destinataire peut être affichée sur l’écran de l’instance :


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

Ensuite, nous allons ajouter le destinataire à notre `GameLayer` à faire apparaître dans le jeu :


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

Si nous exécutons notre jeu, que nous allons maintenant voir notre entité destinataire :

![](entities-images/image4.png "Lorsque vous exécutez le jeu, l’entité de livraison s’affichera")


## <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>Pourquoi hériter CCNode au lieu de CCSprite ?

À ce stade notre `Ship` classe est un simple wrapper pour un `CCSprite` instance. Étant donné que `CCSprite` hérite également de `CCNode`, nous pourrions avoir héritée directement à partir de `CCSprite`, ce qui serait ont réduit le code dans `Ship.cs`. En outre, héritant directement `CCSprite` réduit le nombre d’objets en mémoire et peut améliorer les performances en réduisant la taille de l’arborescence des dépendances.

En dépit de ces avantages, nous héritée `CCNode` pour masquer une partie de la `CCSprite` propriétés à partir de chaque instance. Par exemple, le `Texture` propriété ne doit pas être modifiée en dehors de la `Ship` classe et héritant de `CCNode` permet de masquer cette propriété. Les membres publics de nos entités sont particulièrement importantes, comme un jeu augmente et que les développeurs supplémentaires sont ajoutées à une équipe.


# <a name="adding-input-to-the-ship"></a>Ajout d’une entrée à la livraison

Maintenant que notre destinataire est visible sur l’écran, nous allons ajouter d’entrée. Notre approche sera similaire à l’approche adoptée dans le [Introduction au Guide de CocosSharp](~/graphics-games/cocossharp/first-game/part2.md), sauf que nous allez placer le code de déplacement dans la `Ship` classe plutôt que dans le conteneur `CCLayer` ou `CCScene`.

Ajoutez le code à `Ship` pour prendre en charge le déplacement vers, là où l’utilisateur est toucher l’écran :


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

Nombreux dépanner em des jeux implémentent une rapidité maximale, en simulant le contrôleur traditionnelles de déplacement. Ceci dit, nous allons implémenter simplement transfert immédiat pour conserver notre code plus court.


# <a name="creating-the-bullet-entity"></a>Création de l’entité de puce

La deuxième entité dans le jeu simple est une entité pour afficher les puces. Tout comme le `Ship` entité, le `Bullet` entité contiendra un `CCSprite` afin qu’il apparaisse sur l’écran. La logique de déplacement est différente dans la mesure où il ne dépend pas de l’entrée d’utilisateur pour le déplacement des ; au lieu de cela, `Bullet` déplacent des instances dans une ligne droite à l’aide des propriétés de rapidité.

Tout d’abord, nous allons ajouter un nouveau fichier de classe à notre **entités** dossier et appelez-le **puce**:

![](entities-images/image5.png "Ajouter un nouveau fichier de classe dans le dossier d’entités et appelez-le puce")

Une fois ajoutée, nous allons modifier le `Bullet.cs` de code comme suit :


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

À l’exception de la modification du fichier utilisé pour le `CCSprite` à `bullet.png`, le code dans `ApplyVelocity` inclut la logique de déplacement qui repose sur deux coefficients : `VelocityX` et `VelocityY`.

Le `Schedule` méthode permet d’ajouter des délégués à être appelée à chaque image. Dans ce cas, nous ajoutons la `ApplyVelocity` méthode afin que notre puce se déplace en fonction de ses valeurs de rapidité. Le `Schedule` méthode prend un `Action<float>`, où le paramètre de type float spécifie la quantité de temps (en secondes) depuis la dernière image, ce qui nous permet d’implémenter des temps de déplacement. Depuis la valeur est mesurée en secondes, puis ses valeurs de rapidités représentent le déplacement dans *pixels par seconde*.


# <a name="adding-bullets-to-gamelayer"></a>Ajout de puces à GameLayer

Avant d’ajouter les `Bullet` instances à notre jeu nous effectuerons un conteneur, en particulier un `List<Bullet>`. Modifier le `GameLayer` pour qu’elle comporte une liste à puces :


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

Ensuite, nous devrons à remplir la `Bullet` liste. La logique de création d’un `Bullet` doivent être contenus dans le `Ship` entité, mais la `GameLayer` est chargé de stocker la liste à puces. Nous allons utiliser le modèle de fabrique pour permettre la `Ship` entité pour laquelle créer `Bullet` instances. Cette fabrique expose un événement qui le `GameLayer` peut gérer. 

Pour ce faire tout d’abord, nous ajouterons un dossier à notre projet appelé **fabriques**, puis ajoutez une nouvelle classe nommée `BulletFactory`:

![](entities-images/image6.png "Ajouter un dossier au projet nommé fabriques et puis ajoutez une nouvelle classe nommée BulletFactory")

Ensuite, nous allons implémenter la `BulletFactory` classe singleton :


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

Le `Ship` entité gérera création `Bullet` instances – plus particulièrement, il gère la fréquence à laquelle `Bullet` instances doivent être créés (par exemple, la fréquence à laquelle la puce est déclenchée), leur position et leur vitesse.

Modifier la `Ship` constructeur de l’entité pour ajouter un nouveau `Schedule` appeler, puis implémentez cette méthode comme suit :


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

La dernière étape consiste à gérer la création de nouveaux `Bullet` instances dans le `GameLayer` code. Ajouter un gestionnaire d’événements pour le `BulletCreated` les événements qui ajoute nouvellement créé `Bullet` pour les listes appropriées :


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

Maintenant, nous pouvons exécuter le jeu et consultez le `Ship` dépannage `Bullet` instances :

![](entities-images/image1.png "Exécuter le jeu et le destinataire sera résoudre les instances de la puce")


# <a name="why-gamelayer-has-ship-and-bullets-members"></a>Pourquoi GameLayer possède des membres de puces et de livraison

Notre `GameLayer` classe définit deux champs pour contenir des références à des instances d’entité (`ship` et `bullets`), mais ne fait rien avec eux. En outre, les entités sont responsables de leur propre comportement telles que le déplacement et la résolution de problèmes. Pourquoi nous préférons ajouter `ship` et `bullets` champs `GameLayer`?

La raison pour laquelle nous avons ajouté à ces membres est, car une implémentation d’un jeu complet nécessiterait la logique dans le `GameLayer` pour l’interaction entre les différentes entités. Par exemple, ce jeu peut être développé pour inclure ennemis peuvent être détruits par le lecteur. Ces ennemis sont trouve dans un `List` dans le `GameLayer`et la logique pour tester si `Bullet` instances entrent en conflit avec les ennemis doivent être exécutées selon la `GameLayer` également. En d’autres termes, le `GameLayer` est la racine *propriétaire* d’entité de toutes les instances et il est responsable des interactions entre les instances d’entité.


# <a name="bullet-destruction-considerations"></a>Considérations relatives à la puce Destruction

Notre jeu ne dispose pas actuellement le code de la destruction de `Bullet` instances. Chaque `Bullet` instance contenant la logique de déplacement à l’écran, mais nous n’avons pas ajouté de code pour détruire les soudainement `Bullet` instances.

En outre, la destruction de `Bullet` instances ne peuvent pas appartenir dans `GameLayer`. Par exemple, plutôt que d’être détruite lorsque hors écran, la `Bullet` entité peut avoir la logique de suppression de lui-même après un certain temps. Dans ce cas, le `Bullet` doit pouvoir communiquer qu’il doit être détruit à la `GameLayer`, similaire à la `Ship` entité communiquées à la `GameLayer` qui un nouveau `Bullet` a été créé via la `BulletFactory`.

La solution la plus simple consiste à développer la responsabilité de la classe de fabrique pour prendre en charge de destruction. Ensuite, la fabrique peut être avertie d’une instance d’entité est détruite, qui peut être gérée par d’autres objets, tels que le `GameLayer` suppression de l’instance d’entité à partir de ses listes. 


# <a name="summary"></a>Récapitulatif

Ce guide montre comment créer des entités de CocosSharp en héritant de la `CCNode` classe. Ces entités sont des objets autonomes, gestion de la création de leurs propres éléments visuels et une logique personnalisée. Ce guide désigne le code qui appartient à l’intérieur d’une entité (déplacement et la création d’autres entités) à partir du code qui se trouve dans le conteneur d’entités racine (collision et toute autre logique d’interaction entité).

## <a name="related-links"></a>Liens associés

- [Documentation de l’API de CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Contenu zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
