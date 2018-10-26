---
title: API de jeux dans Xamarin.iOS iOS
description: Cet article aborde les nouvelles améliorations de jeux fournies par iOS 9 qui peut être utilisé pour améliorer les graphiques et les fonctionnalités audio de votre jeu de Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: d8a531e495a19be7437d4a600e758028594248ab
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116002"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>API de jeux dans Xamarin.iOS iOS

_Cet article aborde les nouvelles améliorations de jeux fournies par iOS 9 qui peut être utilisé pour améliorer les graphiques et les fonctionnalités audio de votre jeu de Xamarin.iOS._

Apple a apporté plusieurs améliorations technologiques à l’API de jeux dans iOS 9 qui le rendent plus facile à implémenter les graphismes de jeu et de l’audio dans une application Xamarin.iOS.
Ceux-ci incluent les deux facilité de développement par le biais de frameworks de haut niveau et de maîtriser la puissance du GPU de l’appareil iOS pour l’amélioration de la vitesse et des capacités graphiques.

[![](images/flocking01.png "Un exemple d’une application en cours d’exécution flocage")](images/flocking01.png#lightbox)

Cela inclut GameplayKit, ReplayKit, e/s du modèle, MetalKit et complète les nuanceurs de performances, ainsi que des fonctionnalités nouvelles et améliorées de métal, SceneKit et SpriteKit.

Cet article vous présente toutes les méthodes pour améliorer votre jeu Xamarin.iOS grâce aux nouvelles améliorations de jeux iOS 9 :

## <a name="introducing-gameplaykit"></a>Présentation de GameplayKit

Le framework d’Apple nouvelle GameplayKit fournit un ensemble de technologies qui permet de facilement créer des jeux pour les appareils iOS en réduisant la quantité de code répétitive, courants requis pour l’implémentation. GameplayKit fournit des outils de développement rapidement de la mécanique de jeu qui peut ensuite être facilement combinée avec un moteur graphique (par exemple, SceneKit ou SpriteKit) pour fournir un jeu terminé.

GameplayKit inclut plusieurs, courants, de jeu algorithmes tels que :

- Un comportement en fonction, la simulation de l’agent qui vous permet de définir des mouvements et des objectifs qui se chargera automatiquement de l’intelligence artificielle.
- Une intelligence artificielle minmax aux jeux en alternance.
- Un système de règle pour la logique de jeu piloté par les données avec le raisonnement floue pour fournir un comportement émergent.

En outre, GameplayKit prend une approche modulaire au développement de jeux à l’aide d’une architecture modulaire qui fournit les fonctionnalités suivantes :

- Ordinateur d’état pour la gestion du code complex, de procédure en fonction des systèmes dans le jeu.
- Outils permettant l’aléatoires du jeu et imprévisibilité sans provoquer de problèmes de débogage.
- Une architecture basés sur les entités réutilisables et modulaire.

Pour en savoir plus sur GameplayKit, consultez le site d’Apple [Guide de programmation Gameplaykit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) et [GameplayKit Framework référence](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Exemples de GameplayKit

Jetons un œil rapide à mettre en oeuvre des mécanismes de jeu simple dans une application Xamarin.iOS à l’aide du kit de jeu.

### <a name="pathfinding"></a>Pathfinding

Pathfinding est la possibilité pour un élément de l’intelligence artificielle d’un jeu de son chemin autour du plateau de jeu.
Par exemple, un ennemi 2D recherche son parcours dans un labyrinthe ou un caractère 3D via un terrain-premier-TIR world.

Prenez en compte le plan suivant :

[![](images/gkpathfindpath.png "Un exemple de mappage pathfinding")](images/gkpathfindpath.png#lightbox)

À l’aide de pathfinding cela C# code peut trouver un moyen via la carte :

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>Système d’Expert classique

L’extrait suivant de C# code illustre l’utilisation des GameplayKit pour implémenter un système expert classique :

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

Basé sur un ensemble de règles donné (`GKRule`) et un ensemble connu d’entrées, le système expert (`GKRuleSystem`) crée une sortie prévisible (`fizzbuzz` dans notre exemple ci-dessus).

### <a name="flocking"></a>Flocage

Flocage permet à qu'un groupe de l’intelligence artificielle contrôlé des entités de jeu se comporte comme un troupeau, où le groupe répond aux mouvements et les actions d’une entité responsable comme un vol d’oiseaux en vol ou d’un établissement d’un poisson.

L’extrait suivant de C# code implémente simulant un comportement à l’aide de GameplayKit et SpriteKit pour affichent les graphiques :

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

Ensuite, implémentez cette scène dans un contrôleur d’affichage :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

Exécution, peu animé _« Boids »_ sera troupeau autour des drainages de notre doigt :

[![](images/flocking01.png "Le Boids peu animée sera troupeau autour les pressions doigt")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Autres exemples d’Apple

Outre les exemples présentés, Apple a fourni les exemples d’applications suivants qui peuvent être transcodé à C# et Xamarin.iOS :

- [FourInARow : À l’aide de la stratège GameplayKit Minmax pour l’intelligence artificielle adversaire](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog : À l’aide du système d’Agents dans GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots : Création d’un jeu d’inter-plateformes avec SpriteKit et GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

Dans iOS 9, Apple a apporté plusieurs modifications et ajouts à nu pour fournir un accès de faible charge vers le GPU. À l’aide de Metal vous pouvez optimiser les graphiques et le potentiel informatique de vos applications iOS.

L’infrastructure complète inclut les nouvelles fonctionnalités suivantes :

- Nouveau privé et profondeur de textures de gabarit pour OS X.
- Qualité de clichés instantanés améliorée avec profondeur serrage et distincte avant et arrière du stencil valeurs.
- Améliorations de langage d’ombrage et de bibliothèque Standard nus complètes.
- Les nuanceurs de calculs prend en charge un éventail plus large de formats de pixel.

### <a name="the-metalkit-framework"></a>Le Framework MetalKit

Le framework MetalKit fournit un ensemble de classes d’utilitaires et fonctionnalités qui réduisent la quantité de travail nécessaire pour utiliser complète dans une application iOS. MetalKit prend en charge dans trois domaines clés :

1. Texture asynchrone du chargement à partir de diverses sources, y compris les formats courants tels que PNG, JPEG, KTX et magnétoscope numérique.
2. Facilité d’accès aux e/s du modèle en fonction des ressources pour la gestion de modèle spécifique complète. Ces fonctionnalités ont été hautement optimisées pour fournir le transfert de données efficace entre les mailles de modèle d’e/s et les mémoires tampons complètes.
3. Des affichages prédéfinis métalliques et gestion de l’affichage qui réduisent considérablement la quantité de code nécessaire pour afficher les rendus de graphique dans une application iOS.

Pour en savoir plus sur MetalKit, consultez le site d’Apple [MetalKit Framework référence](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [Guide de programmation complète](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [référence de l’infrastructure complète](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) et [complète Guide du langage d’ombrage](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Framework de nuanceurs performances complète

Le framework de nuanceur de performances complète fournit un ensemble hautement optimisées de graphiques et en fonction de calcul nuanceurs pour une utilisation dans votre complète en fonction des applications iOS. Chaque nuanceur dans le nuanceur de performances nus framework a été conçu pour fournir des performances élevées sur métal pris en charge iOS GPU.

À l’aide de classes de nuanceur de performances complète, vous pouvez obtenir des performances maximales sur chaque iOS spécifique GPU sans avoir à cibler et gérer des bases de code individuels. METAL nuanciers de performances peuvent être utilisés avec n’importe quelle ressource complète tels que les textures et les mémoires tampons.

Le framework de nuanceur de performances complète fournit un ensemble de nuanceurs courantes telles que :

- **Flou gaussien** (`MPSImageGaussianBlur`)
- **Détection des bords de Sobel** (`MPSImageSobel`)
- **Histogramme de l’image** (`MPSImageHistogram`)

Pour plus d’informations, consultez le site d’Apple [Guide du langage d’ombrage nus](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Présentation du modèle d’e/s

Framework de modèle d’e/s d’Apple fournit une compréhension approfondie des composants 3D (par exemple, les modèles et leurs ressources associées). E/s du modèle fournit à vos jeux iOS avec les documents de type physique, modèles et éclairage qui peut être utilisé avec GameplayKit, complète et SceneKit.

Avec le modèle d’e/s, vous pouvez prennent en charge les types de tâches suivants :

- Importation d’éclairage, matériaux, données, paramètres de la caméra et autres informations sur scène à partir d’un éventail de formats de moteur de jeu et de logiciels populaires de maillage.
- Traiter ou générer des informations scène telles que create procédurale texturé ciel dômes ou bake d’éclairage dans une maille.
- Fonctionne avec MetalKit, SceneKit et GLKit pour charger efficacement des ressources de jeu dans des mémoires tampons GPU pour le rendu.
- Exporter des informations basées sur la scène à un éventail de formats de moteur de jeu et de logiciels les plus courants.

Pour en savoir plus sur le modèle d’e/s, consultez le site d’Apple [Framework de référence du modèle d’e/s](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>Présentation de ReplayKit

Nouvelle infrastructure de ReplayKit d’Apple vous permet de facilement ajouter un enregistrement de jeu de jeu iOS et autoriser l’utilisateur à modifier et partager cette vidéo à partir de l’application rapidement et facilement.

Pour plus d’informations, consultez le site d’Apple [va Social avec vidéo ReplayKit et Game Center](https://developer.apple.com/videos/wwdc/2015/?id=605) et leur [DemoBots : création d’un jeu de plateforme Cross avec SpriteKit et GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) exemple d’application.

## <a name="scenekit"></a>SceneKit

Kit de la scène est un graphique de scène 3D API qui simplifie l’utilisation des graphiques 3D. Il a été introduite dans OS X 10.8 et est maintenant venue à iOS 8. Avec le Kit de scène création de visualisations 3D IMMERSIFS et des jeux en 3D occasionnels ne nécessite pas l’expertise dans OpenGL. S’appuyant sur les concepts communs de graphique de scène, Kit de scène élimine la complexité de OpenGL et OpenGL ES, rendant très facile d’ajouter 3D contenu pour une application. Toutefois, si vous êtes un expert OpenGL, Kit de scène a une aide appréciable pour lier des directement avec OpenGL également. Il inclut de nombreuses fonctionnalités qui complètent les graphiques 3D, tels que physique et s’intègre parfaitement avec plusieurs autres infrastructures de Apple, telles que Core Animation, l’Image de base et Spritekit.

Pour plus d’informations, consultez notre [SceneKit](~/ios/platform/gaming/scenekit.md) documentation.

### <a name="scenekit-changes"></a>Modifications de SceneKit

Apple a ajouté les nouvelles fonctionnalités suivantes à SceneKit pour iOS 9 :

- Xcode fournit désormais un éditeur de scène qui vous permet de créer rapidement des jeux et les applications 3D interactives en modifiant les scènes directement dans Xcode.
- Le `SCNView` et `SCNSceneRenderer` classes peuvent être utilisées pour activer le rendu complète (sur les appareils iOS pris en charge).
- Le `SCNAudioPlayer` et `SCNNode` classes peuvent être utilisées pour ajouter des effets audio spatiales qui effectuent le suivi automatiquement d’une position de l’acteur à une application iOS.

Pour plus d’informations, consultez notre [SceneKit Documentation](~/ios/platform/introduction-to-ios8.md#scenekit) et d’Apple [SceneKit Framework référence](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) et [Fox : création d’un jeu SceneKit avec l’éditeur de scène Xcode](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)exemple de projet.

## <a name="spritekit"></a>SpriteKit

Kit du sprite, le framework de jeux 2D à partir d’Apple, a des nouvelles fonctionnalités intéressantes dans iOS 8 et OS X Yosemite. Ceux-ci incluent l’intégration avec Kit de scène, prise en charge de nuanceur, éclairage, ombres, contraintes, génération d’une carte de normale et physique améliorations. En particulier, les nouvelles fonctionnalités de moteur physique rendent très facile d’ajouter des effets réalistes à un jeu.

Pour plus d’informations, consultez notre [SpriteKit](~/ios/platform/gaming/spritekit.md) documentation.

### <a name="spritekit-changes"></a>Modifications de SpriteKit

Apple a ajouté les nouvelles fonctionnalités suivantes pour SpriteKit pour iOS 9 :

- Effet audio spatial automatiquement le suivi du joueur avec la `SKAudioNode` classe.
- Xcode comporte désormais un éditeur de la scène et d’un éditeur d’Action pour la création facile de jeu et application 2D.
- Défilement facile jeu prise en charge avec les nouveaux nœuds de l’appareil photo (`SKCameraNode`) objets.
- Sur des appareils iOS qui prennent en charge complète SpriteKit allez automatiquement l’utiliser pour le rendu, même si vous utilisiez déjà des nuanceurs OpenGL ES personnalisés.

Pour plus d’informations, consultez notre [SpriteKit Documentation](~/ios/platform/introduction-to-ios8.md#spritekit) Apple [SpriteKit Framework référence](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) et leur [DemoBots : création d’un jeu de plateforme Cross avec SpriteKit et GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) exemple d’application.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les nouvelles fonctionnalités de jeu qu’iOS 9 fournit pour vos applications Xamarin.iOS.
Il introduit GameplayKit et e/s du modèle ; les améliorations majeures apportées aux complète ; et les nouvelles fonctionnalités de SceneKit et SpriteKit.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
