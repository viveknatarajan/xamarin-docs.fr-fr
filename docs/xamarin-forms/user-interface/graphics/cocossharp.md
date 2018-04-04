---
title: À l’aide de CocosSharp dans Xamarin.Forms
description: CocosSharp peut servir à ajouter une forme précise, l’image et le rendu de texte à une application de visualisation avancée
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 5fcc3405780e0c5e8a0e8d32caf35abf59808c8e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>À l’aide de CocosSharp dans Xamarin.Forms

_CocosSharp peut servir à ajouter une forme précise, l’image et le rendu de texte à une application de visualisation avancée_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Faire évoluer 2016 : Cocos # dans Xamarin.Forms**

## <a name="overview"></a>Vue d'ensemble

CocosSharp est une technologie flexible et puissante pour l’affichage de graphiques, la lecture de la saisie tactile, lecture du contenu audio et la gestion des. Ce guide explique comment ajouter des CocosSharp à une application de Xamarin.Forms. Elle couvre les sujets suivants :

* [Qu’est CocosSharp ?](#what)
* [Ajouter les packages Nuget de CocosSharp](#nuget)
* [Procédure pas à pas : Ajout de CocosSharp à une application de Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Qu’est CocosSharp ?

[CocosSharp](~/graphics-games/cocossharp/index.md) est un moteur de jeu open source qui est disponible sur la plateforme de Xamarin.
CocosSharp est une bibliothèque runtime efficace qui inclut les fonctionnalités suivantes :

* Rendu d’image à l’aide du [CCSprite classe](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Rendu de la forme à l’aide du [CCDrawNode classe](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* À l’aide de la logique de chaque image le [CCNode.Schedule (méthode)](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* À l’aide de la gestion de contenu (chargement et déchargement des ressources telles que des fichiers .png) le [CCTextureCache classe](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Animations à l’aide de la [CCAction classe](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Principalement du CocosSharp consiste à simplifier la création de jeux 2D inter-plateformes Toutefois, il peut également être une nouveauté pour les applications Xamarin formulaire. Comme jeux nécessitent généralement rendu efficace et un contrôle précis sur les éléments visuels, CocosSharp peut servir à ajouter des effets et visualisation puissante aux applications de hors jeu.

Xamarin.Forms repose sur les systèmes de l’interface utilisateur natives, plate-forme spécifique. Par exemple, [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) s’affichent différemment sur iOS et Android et peut même diffèrent par la version du système d’exploitation. En revanche, CocosSharp n’utilise pas les objets visuels spécifiques à la plateforme, pour que tous les objets visuels apparaissent identiques sur toutes les plateformes. Bien entendu, résolution et les proportions diffèrent entre les appareils, et cela peut avoir un impact sur comment CocosSharp affiche ses éléments visuels. Ces détails sont présentés plus loin dans ce guide.

Vous trouverez des informations plus détaillées dans le [CocosSharp section](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Ajouter les packages Nuget de CocosSharp

Avant d’utiliser CocosSharp, les développeurs doivent effectuer quelques ajouts à leur projet Xamarin.Forms.
Ce guide suppose un projet de Xamarin.Forms avec un iOS, Android et PCL projet.
Tout le code sera écrit dans le projet de bibliothèque de classes portables. Toutefois, les bibliothèques doivent être ajoutés pour les projets iOS et Android.

Le package Nuget de CocosSharp contient tous les objets nécessaires pour créer des objets de CocosSharp.
Le package nuget CocosSharp.Forms inclut la `CocosSharpView` classe, qui est utilisé pour héberger le CocosSharp dans Xamarin.Forms.
Ajouter le **CocosSharp.Forms** NuGet et **CocosSharp** sont automatiquement ajoutés également.
Pour ce faire, cliquez sur la bibliothèque de classes portables <span class="UIItem">Packages</span> et sélectionnez <span class="UIItem">ajouter des Packages en cours... </span>. Entrez le terme de recherche <span class="UIItem">CocosSharp.Forms</span>, sélectionnez <span class="UIItem">CocosSharp pour Xamarin.Forms</span>, puis cliquez sur <span class="UIItem">ajouter un Package</span>.

![](cocossharp-images/image1.png "Ajouter des Packages de boîte de dialogue")

Les deux **CocosSharp** et **CocosSharp.Forms** les packages NuGet seront ajoutés au projet :

![](cocossharp-images/image2.png "Dossier packages")

Répétez les étapes ci-dessus pour les projets spécifiques à la plateforme (par exemple, iOS et Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Procédure pas à pas : Ajout de CocosSharp à une application de Xamarin.Forms

Suivez ces étapes pour ajouter une vue CocosSharp simple à une application de Xamarin.Forms :

1. [Création d’un Xamarin Forms Page](#1)
1. [Ajout d’un CocosSharpView](#2)
1. [Création de la GameScene](#3)
1. [Ajout d’un cercle](#4)
1. [Interaction avec CocosSharp](#5)

Une fois que vous avez ajouté une vue CocosSharp à une application de Xamarin.Forms, visitez le [CocosSharp documentation](~/graphics-games/cocossharp/index.md) pour en savoir plus sur la création de contenu avec CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Création d’un Xamarin Forms Page

CocosSharp peut être hébergé dans n’importe quel conteneur de Xamarin.Forms. Cet exemple de cette page utilise une page appelée `HomePage`. `HomePage` est divisé en deux par une `Grid` pour montrer comment Xamarin.Forms et CocosSharp peuvent être rendus simultanément sur la même page.

Définissez tout d’abord, la Page de sorte qu’elle contienne un `Grid` et deux `Button` instances :


```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

Sur iOS, le `HomePage` s’affiche comme illustré dans l’image suivante :

![](cocossharp-images/image3.png "Capture d’écran de la page d’accueil")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Ajout d’un CocosSharpView

La `CocosSharpView` classe est utilisée pour incorporer des CocosSharp dans une application de Xamarin.Forms. Étant donné que `CocosSharpView` hérite le [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) (classe), il fournit une interface familière pour la disposition, et il peut être utilisé dans des conteneurs de disposition tel que [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Ajouter un nouveau `CocosSharpView` au projet en effectuant le `CreateTopHalf` méthode :


```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

L’initialisation CocosSharp n’est pas immédiate, donc enregistrer un événement lorsque la `CocosSharpView` a terminé sa création. Cela dans le `HandleViewCreated` méthode :


```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

Le `HandleViewCreated` méthode comporte deux détails importants que nous examinerons. La première est la `GameScene` (classe), qui sera créé dans la section suivante. Il est important de noter que l’application ne se compilera pas jusqu'à ce que le `GameScene` est créé et le `gameScene` instance référence est résolue.

Le deuxième détail important est le `DesignResolution` propriété, qui définit la zone visible du jeu CocosSharp objets. Le `DesignResolution` propriété sera étudiée après la création de `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Création de la GameScene

Le `GameScene` classe hérite de CocosSharp `CCScene`. `GameScene` est le premier point où nous traiter entièrement CocosSharp. Code contenu dans `GameScene` fonctionnera dans n’importe quelle application CocosSharp, si elle est hébergée dans un projet Xamarin.Forms ou non.

La `CCScene` classe est la visual racine de l’ensemble du rendu CocosSharp. N’importe quel objet CocosSharp visible doit être contenue dans un `CCScene`. Plus spécifiquement, les objets visuels doivent être ajoutés à `CCLayer` instances et ceux `CCLayer` instances doivent être ajoutés à un `CCScene`.

Le graphique suivant peut aider à visualiser une hiérarchie CocosSharp classique :

![](cocossharp-images/image4.png "Hiérarchie CocosSharp typique")

Seul `CCScene` peut être active à la fois. La plupart des jeux utiliser plusieurs `CCLayer` instances pour trier le contenu, mais votre application utilise un seul. De même, la plupart des jeux utiliser plusieurs objets visuels, mais nous allons en avoir qu’une dans notre application. Plus détaillée la discussion à propos de la CocosSharp hiérarchie visuelle se trouvent dans le [BouncingGame procédure pas à pas](~/graphics-games/cocossharp/bouncing-game.md).

Initialement le `GameScene` classe sera presque vide, nous allons créer simplement pour répondre à la référence dans `HomePage`. Ajoutez une nouvelle classe à votre bibliothèque PCL nommée `GameScene`. Il doit hériter de la `CCScene` classe comme suit :


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Maintenant que `GameScene` est défini, nous pouvons retourner à `HomePage` et ajouter un champ :


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Nous pouvons maintenant compiler notre projet et exécutez-le pour voir CocosSharp en cours d’exécution. Nous n’avons pas ajouté à notre `GameScene,` afin que la moitié supérieure de notre page soit noir – la couleur par défaut d’une scène CocosSharp :

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Ajout d’un cercle

L’application n’a actuellement une instance en cours d’exécution du moteur CocosSharp, affichage vide `CCScene`. Ensuite, nous allons ajouter un objet visuel : un cercle. Le `CCDrawNode` classe peut être utilisée pour dessiner une variété de formes géométriques, comme indiqué dans le [Geometry de dessin avec le guide de CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

Ajouter un cercle à notre `GameScene` de classe et l’instancier dans le constructeur, comme indiqué dans le code suivant :


```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Exécution de l’application maintenant d’affiche un cercle sur le côté gauche de la zone d’affichage CocosSharp :

![](cocossharp-images/image6.png "Cercle dans GameScene")


#### <a name="understanding-designresolution"></a>Présentation des DesignResolution

Maintenant qu’un objet CocosSharp visuel s’affiche, nous pouvons examiner la `DesignResolution` propriété.

La `DesignResolution` représente la largeur et la hauteur de la zone CocosSharp de placement et le redimensionnement des objets. La résolution réelle de la zone est mesurée en *pixels* alors que le `DesignResolution` est mesurée dans le monde *unités*. Le diagramme suivant illustre la résolution des différentes parties de la vue tel qu’affiché sur un iPhone 5 avec une résolution d’écran de 640 x 1136 pixels :

![](cocossharp-images/image7.png "iPhone 5 s résolution de la conception")

Le diagramme ci-dessus affiche les dimensions en pixels à l’extérieur de l’écran en texte noir. Unités sont affichées à l’intérieur du diagramme dans un texte blanc. Voici quelques détails importants affichés au-dessus :

* L’origine de l’affichage CocosSharp est en bas à gauche. Déplacer vers la droite augmente la valeur de X, et déplacement vers le haut la valeur Y. Notez que la valeur Y est inversée par rapport à certains autres moteurs de disposition 2D, où (0,0) est le coin supérieur gauche de la zone de dessin.
* Le comportement par défaut de CocosSharp consiste à conserver les proportions de la vue. Étant donné que la première ligne dans la grille est plus large que verticales, CocosSharp ne remplit pas toute la largeur de la cellule, comme indiqué par le rectangle blanc en pointillés. Ce comportement peut être modifié, comme décrit dans la [guide de gestion de plusieurs résolutions dans CocosSharp](~/graphics-games/cocossharp/resolutions.md).
* Dans cet exemple, CocosSharp gère une zone d’affichage de 100 unités de larges et haute, quelle que soit la taille ou les proportions de son appareil. Cela signifie que le code peut assumer X = 100 représente lié de l’extrémité droite de la CocosSharp s’afficher, mise en page de la conservation cohérente sur tous les appareils.


#### <a name="ccdrawnode-details"></a>Détails de CCDrawNode

Notre utilise une application simple la `CCDrawNode` classe pour tracer un cercle. Cette classe peut être très utile pour les applications d’entreprise puisqu’il fournit le rendu vectoriel de géométrie – une fonctionnalité manquante dans Xamarin.Forms. En plus des cercles, la `CCDrawNode` classe peut être utilisée pour dessiner des rectangles, des splines, lignes et polygones personnalisés. `CCDrawNode` est également facile à utiliser, car il ne nécessite pas l’utilisation de fichiers image (.png). Vous trouverez une présentation plus détaillée des CCDrawNode dans les [Geometry de dessin avec le guide de CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Interaction avec CocosSharp

Les éléments visuels CocosSharp (tel que `CCDrawNode`) héritent de la `CCNode` classe. `CCNode` fournit deux propriétés qui peuvent être utilisées pour positionner un objet par rapport à son parent : `PositionX` et `PositionY`. Notre code utilise actuellement ces deux propriétés pour positionner le centre du cercle, comme indiqué dans cet extrait de code :


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Il est important de noter que les objets CocosSharp sont positionnés par les valeurs de position explicites, par opposition à la plupart des affichages de Xamarin.Forms, qui sont placées automatiquement en fonction du comportement de leurs contrôles de disposition du parent.

Nous allons ajouter le code pour autoriser l’utilisateur à cliquer sur un des deux boutons pour déplacer le cercle à gauche ou à droite de 10 unités (et non en pixels, étant donné que le cercle dessine dans l’espace d’unité CocosSharp world). Tout d’abord, nous allons créer deux méthodes publiques dans la `GameScene` classe :


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Ensuite, nous allons ajouter des gestionnaires pour les deux boutons de `HomePage` pour répondre aux clics. Lorsque vous avez terminé, notre `CreateBottomHalf` méthode contient le code suivant :


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Le cercle CocosSharp déplace maintenant en réponse aux clics. Nous pouvons également clairement voir les limites de la zone de dessin CocosSharp en déplaçant le cercle suffisamment vers la droite ou gauche :

![](cocossharp-images/image8.png "GameScene avec déplacement de cercle")

## <a name="summary"></a>Récapitulatif

Ce guide montre comment ajouter des CocosSharp à un Xamarin.Forms existant du projet, la création d’une interaction entre Xamarin.Forms et CocosSharp et décrit les différentes Considérations lors de la création des dispositions dans CocosSharp.

Le moteur de jeu CocosSharp offre un grand nombre de fonctionnalités et de profondeur, ce guide n’est pas la surface de CocosSharp réalisables. Les développeurs intéressés par la lecture plus CocosSharp peuvent rechercher de nombreux articles dans le [CocosSharp section](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Liens associés

- [API de CocosSharp](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (sample)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
