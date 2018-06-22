---
title: 'Partie 2 : mise en œuvre de la WalkingGame'
description: Cette procédure pas à pas montre comment ajouter la logique de jeu et de contenu à un projet MonoGame vide pour créer une démonstration d’un sprite animée déplacement avec touch d’entrée.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 87678d9d77f75bccc68a667d3fb0f35b641b937c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921662"
---
# <a name="part-2--implementing-the-walkinggame"></a>Partie 2 : mise en œuvre de la WalkingGame

_Cette procédure pas à pas montre comment ajouter la logique de jeu et de contenu à un projet MonoGame vide pour créer une démonstration d’un sprite animée déplacement avec touch d’entrée._

Les sections précédentes de cette procédure pas à pas vous a montré comment créer des projets MonoGame vides. Nous allons créer sur ces parties précédentes en effectuant une jeu de démo simple. Cet article contient les sections suivantes :

- Décompresser le contenu de notre jeu
- Vue d’ensemble de la classe de MonoGame
- Notre première Sprite de rendu
- Création de la CharacterEntity
- Ajout de CharacterEntity au jeu
- Création de la classe d’Animation
- Ajout de la première Animation pour CharacterEntity
- Ajout de déplacement vers le caractère
- Correspondance de mouvement et animation


## <a name="unzipping-our-game-content"></a>Décompresser le contenu de notre jeu

Avant de commencer l’écriture de code, nous souhaitons décompresser notre jeu *contenu*. Chez les développeurs utilisent souvent le terme *contenu* pour faire référence à des fichiers de code non qui sont généralement créées par les artistes visual, les concepteurs de jeux ou des concepteurs audio. Types courants de contenu incluent les fichiers utilisés pour afficher des éléments visuels, sons ou contrôler le comportement de l’intelligence artificielle (AI). À partir d’un développement de jeux contenu du point de vue de l’équipe est généralement créé par les programmeurs non.

Le contenu utilisé ici, vous pouvez trouver [sur github](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Nous avons besoin de ces fichiers téléchargés vers un emplacement plus loin dans cette procédure pas à pas, nous devons accéder.

## <a name="monogame-class-overview"></a>Vue d’ensemble de la classe de MonoGame

Pour commencer, nous allons examiner les classes MonoGame utilisés dans le rendu de base :

- `SpriteBatch` : utilisé pour dessiner les graphiques 2D à l’écran. *Sprites* sont des éléments visuels 2D qui sont utilisés pour afficher des images sur l’écran. Le `SpriteBatch` objet peut dessiner un composant unique à la fois entre ses `Begin` et `End` méthodes ou plusieurs sprites peuvent être regroupées, ou *par lot*.
- `Texture2D` : représente un objet image lors de l’exécution. `Texture2D` instances sont souvent créées à partir des formats de fichier tels que .png ou .bmp, bien qu’ils peuvent également être créés dynamiquement lors de l’exécution. `Texture2D` les instances sont utilisées lors du rendu avec `SpriteBatch` instances.
- `Vector2` : représente une position dans un système de coordonnées 2D qui est souvent utilisé pour le positionnement des objets visuels. Inclut également des MonoGame `Vector3` et `Vector4` , mais nous n’utilisons `Vector2` dans cette procédure pas à pas.
- `Rectangle` – une zone rectangulaire avec la position, la largeur et hauteur. Nous utiliserons cette pour définir la partie de notre `Texture2D` à rendre lorsque nous commençons à travailler avec des animations.

Notez également que MonoGame n’est pas gérée par Microsoft – en dépit de son espace de noms. Le `Microsoft.Xna` espace de noms est utilisé dans MonoGame pour faciliter la migration des projets XNA existants vers MonoGame.

## <a name="rendering-our-first-sprite"></a>Notre première Sprite de rendu

Ensuite, nous attirerons un composant unique à l’écran pour montrer comment effectuer le rendu 2D dans MonoGame.

### <a name="creating-a-texture2d"></a>Création d’un Texture2D

Nous devons créer un `Texture2D` instance à utiliser lors du rendu de notre sprite. Partie du contenu est finalement contenu dans un dossier nommé **contenu,** situé dans le projet spécifique à la plateforme. Les projets MonoGame partagé ne peut pas contenir de contenu, comme le contenu doit utiliser des actions de génération spécifique à la plateforme. Les développeurs de CocosSharp trouverez le contenu du dossier un concept familier : ils se trouvent au même endroit dans les projets CocosSharp et MonoGame. Le contenu du dossier sont accessibles dans le projet iOS et dans le dossier de composants dans le projet Android.

Pour ajouter le contenu du jeu, cliquez sur le **contenu** et sélectionnez **Ajouter > ajouter des fichiers...** Naviguez jusqu'à l’emplacement où le fichier content.zip a été extrait et sélectionnez le **charactersheet.png** fichier. Si demandé sur la façon d’ajouter le fichier au dossier, nous devons sélectionner le **copie** option :

![](part2-images/image1.png "Forum sur la façon d’ajouter le fichier au dossier, sélectionnez l’option de copie")

Le contenu du dossier contient maintenant le fichier charactersheet.png :

![](part2-images/image2.png "Le contenu du dossier contient maintenant le fichier charactersheet.png")

Ensuite, nous allons ajouter le code pour charger le fichier charactersheet.png et créer un `Texture2D`. Pour cela, ouvrez le `Game1.cs` et ajoutez le champ suivant à la classe Game1.cs :

```csharp
Texture2D characterSheetTexture;
```

Ensuite, nous allons créer le `characterSheetTexture` dans le `LoadContent` (méthode). Avant toute modification `LoadContent` méthode ressemble à ceci :

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Nous devons Notez que le projet par défaut déjà instancie le `spriteBatch` instance pour nous. Nous utiliserons cette ultérieurement, mais nous ne sera pas ajouter de code supplémentaire pour préparer le `spriteBatch` à utiliser. En revanche, notre `spriteSheetTexture` requiert l’initialisation, nous initialisera après le `spriteBatch` est créé :

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Maintenant que nous avons un `SpriteBatch` instance et un `Texture2D` instance que nous pouvons ajouter notre code de rendu pour le `Game1.Draw` méthode :

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

En cours d’exécution du jeu montre un composant unique affichant la texture créée à partir de charactersheet.png :

![](part2-images/image3.png "Exécutez le jeu maintenant montre un composant unique affichant la texture créée à partir de charactersheet.png")

## <a name="creating-the-characterentity"></a>Création de la CharacterEntity

Jusqu'à présent, nous avons ajouté le code pour restituer un composant unique à l’écran ; Toutefois, si nous devions pour développer un jeu complet sans créer d’autres classes, nous serait rencontrer des problèmes d’organisation de code.

### <a name="what-is-an-entity"></a>Qu’est une entité ?

Pour organiser le code de jeu courant consiste à créer une classe pour chaque jeu *entité* type. Une entité dans le développement de jeux est un objet qui peut contenir des caractéristiques suivantes (pas toutes sont requises) :

- Un élément visuel comme un sprite, un texte ou un modèle 3D
- Chaque comportement de frame comme une unité patrouiller un chemin d’accès de l’ensemble ou un caractère de lecteur répondre à l’entrée ou de physique
- Peuvent être créés et détruits de façon dynamique, comme une tension apparaissant et collectées par le lecteur

Systèmes d’entreprise entité peuvent être complexes, et d’offrent des moteurs de jeu de classes pour aider à gérer des entités. Nous allons implémenter un système très simple d’entité, afin qu’il est important de noter que les jeux complètes nécessitent généralement plus d’organisation sur le développeur.


### <a name="defining-the-characterentity"></a>Définition de la CharacterEntity

Notre entité, que nous appellerons `CharacterEntity`, présente les caractéristiques suivantes :

- La capacité à charger son propre `Texture2D`
- La possibilité de s’afficher, y compris contenant l’appel de méthodes pour mettre à jour de l’animation de parcours
- `X` propriétés et Y pour contrôler la position du caractère.
- La possibilité de se mettre à jour, en particulier, pour lire les valeurs de la fonction tactile d’écran et ajustent la position de manière appropriée.

Pour ajouter le `CharacterEntity` à notre jeu, avec le bouton droit ou à en cliquant sur le **WalkingGame** de projet et sélectionnez **Ajouter > nouveau fichier...** . Sélectionnez le **classe vide** option et nommez le nouveau fichier **CharacterEntity**, puis cliquez sur **nouveau**.

Tout d’abord, nous allons ajouter la possibilité du `CharacterEntity` pour charger un `Texture2D` ainsi que pour dessiner lui-même. Nous allons modifier l’ajoutées récemment `CharacterEntity.cs` de fichiers comme suit :

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Le code ci-dessus ajoute la responsabilité de positionnement, le rendu et le chargement de contenu à la `CharacterEntity`. Prenons un moment pour souligner certaines considérations effectuées dans le code ci-dessus.

### <a name="why-are-x-and-y-floats"></a>Pourquoi les X et les valeurs en virgule flottante Y ?

Les développeurs qui débutent dans la programmation de jeux demandez peut-être pourquoi la `float` type utilisé par opposition à `int` ou `double`. La raison est qu’une valeur de 32 bits est plus courant pour le positionnement dans le code de bas niveau de rendu. Le type double occupe 64 bits pour la précision, ce qui est rarement nécessaire pour le positionnement des objets. Une différence de 32 bits peut-être sembler non significatif, de nombreux jeux modernes incluent graphiques qui nécessitent un traitement des dizaines de milliers de valeurs de position chaque frame (30 ou 60 fois par seconde). Couper la quantité de mémoire qui doit se déplacer dans les graphiques pipeline de moitié peut avoir un impact significatif sur les performances d’un jeu.

Le `int` type de données peut être une unité appropriée de mesure de positionnement, mais il peut placer des restrictions sur la manière dont les entités sont positionnées. Par exemple, à l’aide de valeurs entières rend plus difficile à implémenter un mouvement souple pour les jeux qui prennent en charge un zoom avant ou caméras 3D (qui sont autorisé par `SpriteBatch`).

Enfin, nous allons voir que la logique qui déplace notre caractère autour de l’écran sera le faire à l’aide des valeurs de minutage du jeu. Le résultat de la multiplication de rapidité en combien de temps écoulé dans un frame donné rarement entraîne un nombre entier, afin que nous devons utiliser `float` pour `X` et `Y`.

### <a name="why-is-charactersheettexture-static"></a>Pourquoi est-characterSheetTexture statique ?

Le `characterSheetTexture` `Texture2D` instance est une représentation d’exécution du fichier charactersheet.png. En d’autres termes, elle contient les valeurs de couleur pour chaque pixel, tel qu’extrait de la source de **charactersheet.png** fichier. Si notre jeu consistait à créer deux `CharacterEntity` instances, chacun d’eux est doivent accéder aux informations à partir de charactersheet.png. Dans ce cas, nous ne voudriez pas qu’encourir le coût de performances de chargement de charactersheet.png à deux reprises, ni nous voulons mémoire de texture en double stockées dans la mémoire ram. À l’aide un `static` champ permet de partager le même `Texture2D` pour tous les `CharacterEntity` instances.

À l’aide de `static` membres pour la représentation sous forme de runtime du contenu est une solution simpliste et ne traite pas les problèmes rencontrés dans les jeux supérieure tel que le contenu déchargement lors du déplacement d’un niveau vers un autre. Des solutions plus sophistiquées qui sortent du cadre de cette procédure pas à pas, incluent à l’aide du pipeline de contenu de MonoGame ou de création d’un système de gestion de contenu personnalisé.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Pourquoi est-il SpriteBatch passée comme un Argument Instead of instancié par l’entité

Le `Draw` la méthode implémentée ci-dessus prend un `SpriteBatch` argument, mais en revanche, le `characterSheetTexture` est instanciée par la `CharacterEntity`.

La raison en est, car le rendu plus efficace est possible lorsque le même `SpriteBatch` instance est utilisée pour tous les `Draw` des appels et à quel moment toutes les `Draw` font entre un ensemble d’appels `Begin` et `End` appels. Bien entendu, notre jeu inclut uniquement une seule instance d’entité, mais plus compliqués jeux bénéficieront de modèle qui permet à plusieurs entités d’utiliser le même `SpriteBatch` instance.


## <a name="adding-characterentity-to-the-game"></a>Ajout de CharacterEntity au jeu

Maintenant que nous avons ajouté notre `CharacterEntity` avec du code pour le rendu de lui-même, nous pouvons remplacer le code dans `Game1.cs` pour utiliser une instance de cette nouvelle entité. Pour cela, nous allons remplacer la `Texture2D` champ avec un `CharacterEntity` champ `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Ensuite, nous allons ajouter l’instanciation de cette entité dans `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Nous devons également supprimer la `Texture2D` création à partir de `LoadContent` depuis qui est désormais géré à l’intérieur de notre `CharacterEntity`. `Game1.LoadContent` doit ressembler à ceci :

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Il est important de noter que, en dépit de son nom le `LoadContent` méthode n’est pas le seul endroit où le contenu peut être chargé : implémentez de nombreux jeux contenu le chargement de leur méthode d’initialisation, ou même dans leur code de mise à jour en tant que contenu, n’est pas nécessaire jusqu'à ce que le joueur atteigne un certains points du jeu.

Enfin, nous pouvons modifier la méthode de dessin comme suit :


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Si nous exécutons le jeu, nous allons maintenant voir le caractère. Étant donné que X et Y par défaut sur 0, le caractère est positionné par rapport à l’angle supérieur gauche de l’écran :

![](part2-images/image4.png "Étant donné que X et Y par défaut sur 0, le caractère est positionné par rapport à l’angle supérieur gauche de l’écran")

## <a name="creating-the-animation-class"></a>Création de la classe d’Animation

Actuellement notre `CharacterEntity` affiche la version complète **charactersheet.png** fichier. Cette disposition de plusieurs images dans un seul fichier porte une *feuille de sprite*. En règle générale, un composant affichera uniquement une partie de la feuille sprite. Nous allons modifier le `CharacterEntity` pour restituer une partie de ce **charactersheet.png**, et cette partie change au fil du temps pour afficher une animation de parcours.

Nous allons créer le `Animation` classe pour contrôler la logique et l’état de l’animation CharacterEntity. La classe de l’Animation sera une classe générale qui pourrait être utilisée pour toutes les entités, pas seulement `CharacterEntity` animations. Édition intégrale la `Animation` classe fournit un `Rectangle` qui le `CharacterEntity` utilisera lors du dessin proprement dit. Nous allons également créer un `AnimationFrame` classe qui sera utilisé pour définir l’animation.


### <a name="defining-animationframe"></a>Définition AnimationFrame

`AnimationFrame` ne contient pas de toute logique relatives à l’animation. Nous il utiliserons uniquement pour stocker les données. Pour ajouter le `AnimationFrame` classe, avec le bouton droit ou en cliquant sur le **WalkingGame** partagé du projet et sélectionnez **Ajouter > nouveau fichier...** Entrez le nom **AnimationFrame** et cliquez sur le **nouveau** bouton. Nous allons modifier le `AnimationFrame.cs` de fichier pour qu’il contienne le code suivant :


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

La `AnimationFrame` classe contient deux informations :

- `SourceRectangle` : Définit la zone de la `Texture2D` qui sera affiché par le `AnimationFrame`. Cette valeur est mesurée en pixels, la supérieure gauche étant (0, 0).
- `Duration` : Définit la durée pendant laquelle un `AnimationFrame` s’affiche lorsqu’il est utilisé dans un `Animation`.

### <a name="defining-animation"></a>Définition d’Animation

Le `Animation` classe contiendra une `List<AnimationFrame>` , ainsi que la logique pour basculer l’image est affichée en fonction de la durée écoulée.

Pour ajouter le `Animation` classe, avec le bouton droit ou en cliquant sur le **WalkingGame** partagé du projet et sélectionnez **Ajouter > nouveau fichier...** . Entrez le nom **Animation** et cliquez sur le **nouveau** bouton. Nous allons modifier le `Animation.cs` fichier de sorte qu’il contienne le code suivant :


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Examinons certains détails de la `Animation` classe.

### <a name="frames-list"></a>Liste des frames

Le `frames` membre est ce qui stocke les données de notre animation. Le code qui instancie les animations ajoutera `AnimationFrame` instances à la `frames` liste via le `AddFrame` (méthode). Une implémentation plus complète peut offrir `public` méthodes ou propriétés de modification `frames`, mais nous allons nous limiter les fonctionnalités à l’ajout d’images pour cette procédure pas à pas.

### <a name="duration"></a>Duration

Durée affiche la durée totale de la `Animation,` qui est obtenu en ajoutant la durée de tous les `AnimationFrame` instances. Cette valeur peut être mis en cache si `AnimationFrame` est un objet immuable, mais nous avons implémenté AnimationFrame comme une classe qui peut être modifiée après avoir été ajouté à une Animation, nous devons calculer cette valeur chaque fois que la propriété est accessible.

### <a name="update"></a>Mise à jour

Le `Update` méthode doit être appelée à chaque frame (autrement dit, chaque mise à jour le jeu entier). Son objectif est d’augmenter la `timeIntoAnimation` membre qui est utilisée pour retourner l’image affichée. La logique de `Update` empêche le `timeIntoAnimation` de jamais être supérieure à la durée de la totalité de l’animation.

Étant donné que nous utiliserons la `Animation` classe pour afficher une animation marche, puis nous souhaitons faire répéter lorsqu’il a atteint la fin de cette animation. Pour ce faire, nous pouvons vérifier si le `timeIntoAnimation` est supérieure à la `Duration` valeur. Cas dans ce, il sera cycle jusqu’au début et conserver le reste, ce qui entraîne une animation en boucle.

### <a name="obtaining-the-current-frames-rectangle"></a>Obtention Rectangle du Frame actif

L’objectif de la `Animation` classe correspond finalement à fournir un `Rectangle` qui peut être utilisé lors du dessin d’un composant. Actuellement le `Animation` classe contient la logique permettant de modifier le `timeIntoAnimation` membre, que nous allons utiliser pour lequel obtenir `Rectangle` doit être affiché. Nous allons le faire en créant un `CurrentRectangle` propriété dans la `Animation` classe. Copiez cette propriété dans la `Animation` classe :

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>Ajout de la première Animation pour CharacterEntity

Le `CharacterEntity` contiendra les animations de parcours et permanent, ainsi qu’une référence à actuel `Animation` affiché.

Tout d’abord, nous allons ajouter notre premier `Animation,` que nous allons utiliser pour tester la fonctionnalité comme écrits jusqu'à présent. Vous allez ajouter les membres suivants à la classe CharacterEntity :

```csharp
Animation walkDown;
Animation currentAnimation;
```

Ensuite, nous allons définir le `walkDown` animation. Pour effectuer cette modification le `CharacterEntity` constructeur comme suit :

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Comme mentionné précédemment, nous avons besoin d’appeler `Animation.Update` pour les animations de temporels à lire. Nous devons également affecter la `currentAnimation`. Maintenant nous allons attribuer le `currentAnimation` à `walkDown`, mais nous allons remplacer ce code ultérieurement lorsque nous implémentons notre logique de déplacement. Nous allons ajouter le `Update` méthode `CharacterEntity` comme suit :


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Maintenant que nous avons le `currentAnimation` qui est attribué et mis à jour, nous pouvons l’utiliser pour exécuter notre dessin. Nous allons modifier `CharacterEntity.Draw` comme suit :

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

La dernière étape à l’obtention de la `CharacterEntity` animation consiste à appeler récemment ajouté `Update` méthode à partir de `Game1`. Modifier `Game1.Update` comme suit :

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Maintenant le `CharacterEntity` jouera son `walkDown` animation :

![](part2-images/image5.gif "Maintenant le CharacterEntity jouera son animation walkDown")

## <a name="adding-movement-to-the-character"></a>Ajout de déplacement vers le caractère

Ensuite, nous allons ajouter de mouvement à notre caractère à l’aide de contrôles de tactile. Lorsque l’utilisateur touche l’écran, le caractère se déplace vers le point où l’écran est affecté. Si aucune des touches finales ne sont détectées, le caractère ressort en place.

### <a name="defining-getdesiredvelocityfrominput"></a>Définition GetDesiredVelocityFromInput

Nous utiliserons de MonoGame `TouchPanel` (classe), qui fournit des informations sur l’état actuel de l’écran tactile. Vous allez ajouter une méthode qui vérifie la `TouchPanel` et la rapidité de votre choix de notre caractère de retour :


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

Le `TouchPanel.GetState` méthode retourne un `TouchCollection` qui contient plus d’informations sur l’endroit où l’utilisateur touche l’écran. Si l’utilisateur n’est pas toucher l’écran, puis le `TouchCollection` sera vide, auquel cas nous ne devrait pas être déplacer le caractère.

Si l’utilisateur touche l’écran, nous déplacera le caractère vers la première fonction tactile, en d’autres termes, le `TouchLocation` à l’index 0. Nous mettrons initialement la rapidité souhaitée pour être égale à la différence entre les emplacements du caractère et de la première tactile :

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Ce qui suit est un peu de mathématiques qui conserve le déplacement de caractère à la même vitesse. Pour expliquer pourquoi il est important, prenons l’exemple d’une situation où l’utilisateur touche le 500 pixels en s’éloignant d’où se trouve le caractère. La première ligne de `desiredVelocity.X` est ensemble feriez pour attribuer une valeur de 500. Toutefois, si l’utilisateur ont été toucher l’écran à une distance de 100 unités à partir du caractère, puis le `desiredVelocity.X `a la valeur 100. Le résultat est que vitesse de déplacement du caractère réponde à la manière dont loin que du point de contact est à partir du caractère. Étant donné que nous voulons que le caractère toujours déplacer à la même vitesse, nous devons modifier le desiredVelocity.

La `if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` instruction vérifie si la rapidité est non-zéro – en d’autres termes, elle est la vérification pour vous assurer que l’utilisateur n’est pas toucher au même endroit que la position du caractère en cours. Si pas, vous devez ensuite définir la vitesse du caractère comme une constante, quelle que soit la façon de loin la fonction tactile est. Pour cela, nous normalisation du vecteur de rapidité entraînant, qu’il soit d’une longueur de 1. Un vecteur de vitesse de 1 signifie que le caractère se déplace à 1 pixel par seconde. Nous allons accélérer ce processus en multipliant la valeur de la vitesse de votre choix de 200.


### <a name="applying-velocity-to-position"></a>Vélocité de l’application à la Position

La rapidité retournée à partir de `GetDesiredVelocityFromInput` doit être appliquée à du caractère `X` et `Y` valeurs en vigueur lors de l’exécution. Nous allons modifier le `Update` méthode comme suit :

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Ce que nous avons implémenté ici est appelé *temporels* mouvement (par opposition à *basée sur le frame* mouvement). Temps de déplacement multiplie une valeur de la vitesse (dans notre cas, les valeurs stockées dans le `velocity` variable) en combien de temps écoulé depuis la dernière mise à jour qui est stocké dans `gameTime.ElapsedGameTime.TotalSeconds`. Si le jeu s’exécute en moins d’images par seconde, le temps écoulé entre les frames remonte : le résultat final est que les objets à l’aide de déplacement temps seront déplace toujours à la même vitesse, quelle que soit la fréquence d’images.

Si nous exécutons notre jeu maintenant, nous verrons que le caractère se déplace vers l’emplacement tactile :

![](part2-images/image6.gif "Le caractère se tourne vers l’emplacement tactile")

## <a name="matching-movement-and-animation"></a>Correspondance de mouvement et Animation

Une fois notre caractère déplacement et la lecture d’une animation unique, nous pouvons définir le reste de notre animations, puis les utiliser pour refléter le déplacement du caractère. Lors de la fin, il nous faudra huit animations au total :

- Animations pour remontant vers le bas, gauche et droite
- Animations debout toujours et vers le haut, bas, gauche et droite

### <a name="defining-the-rest-of-the-animations"></a>Définir le reste des Animations

Tout d’abord, nous allons ajouter le `Animation` instances à la `CharacterEntity` classe pour toutes ses animations au même endroit où nous avons ajouté `walkDown`. Une fois que cela, nous, le `CharacterEntity` aura suit `Animation` membres :

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Maintenant nous allons définir les animations dans la `CharacterEntity` constructeur comme suit :

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Nous devons Notez que le code ci-dessus a été ajouté à la `CharacterEntity` constructeur à conserver cette procédure pas à pas plus court. Jeux seront généralement séparer la définition des animations de caractère dans leurs propres classes ou charger ces informations à partir d’un format de données telles que XML ou JSON.

Ensuite, nous allons ajuster la logique pour utiliser les animations en fonction de la direction dans laquelle le caractère correspond à un déplacement, ou en fonction de la dernière animation si le caractère a simplement été arrêtés. Pour ce faire, nous allons modifier le `Update` méthode :


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Le code pour basculer les animations est divisé en deux blocs. Le premier vérifie si la vitesse n’est pas égale à `Vector2.Zero` – nous indique si le caractère est mobile. Si le caractère est mobile, nous pouvons examiner la `velocity.X` et `velocity.Y` valeurs afin de déterminer l’animation parcours à lire.

Si le caractère n’est pas déplacer, puis nous souhaitons définir du caractère `currentAnimation` à une animation permanent – mais nous le faire uniquement si la `currentAnimation` est un parcours de l’animation ou si une animation n’a pas été définie. Si le `currentAnimation` n’est pas une des quatre animations marche, puis le caractère se trouve déjà afin de nous n’avez pas besoin de modifier `currentAnimation`.

Le résultat de ce code est que le caractère correctement animer lorsque vous parcourez et ensuite le sens dernier qu’il a été parcours lorsqu’il s’arrête :

![](part2-images/image7.gif "Le résultat de ce code est que le caractère correctement animer lorsque vous parcourez et ensuite le sens dernier qu’il a été parcours lorsqu’il s’arrête")

## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas vous a montré comment travailler avec MonoGame pour créer un inter-plateformes jeu avec sprites, déplacement d’objets, la détection d’entrée et l’animation. Elle traite de la création d’une classe d’animation à usage général. Il a également montré comment créer une entité de caractère pour l’organisation logique du code.

## <a name="related-links"></a>Liens associés

- [Ressource d’Image CharacterSheet (exemple)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Parcours de jeu complète (exemple)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
