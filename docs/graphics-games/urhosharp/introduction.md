---
title: "Introduction à UrhoSharp"
description: "Cela fournit une brève introduction aux concepts derrière UrhoSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 280ff20a6b45e4bfde984fc9c068e5c95276f9e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="an-introduction-to-urhosharp"></a>Introduction à UrhoSharp

_Cela fournit une brève introduction aux concepts derrière UrhoSharp_

![](introduction-images/urhosharp-icon.png "UrhoSharp est un puissant moteur de jeu 3D pour les développeurs Xamarin et .NET")

UrhoSharp est un puissant moteur de jeu 3D pour les développeurs Xamarin et .NET.  Il est même esprit d’Apple SceneKit et SpriteKit et inclure physique, la navigation, mise en réseau et une grande partie plus tout en étant toujours cross-platform.

Il s’agit d’une liaison de .NET pour les [Urho3D](http://urho3d.github.io/) moteur et permet aux développeurs d’écrire du code interplateformes ciblant Android, iOS, Windows et Mac avec la même base de code et peuvent rendre aux systèmes OpenGL et Direct3D.

UrhoSharp est un moteur de jeu avec de nombreuses fonctionnalités prêtes à l’emploi :

 - Rendu de graphiques 3D puissantes
- [Physique simulation](https://developer.xamarin.com/api/namespace/Urho.Physics/) (à l’aide de la bibliothèque)
- [Gestion de la scène](https://developer.xamarin.com/api/type/Urho.Scene/)
- Prise en charge await/Async
- [Actions conviviales API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [Intégration 2D dans les scènes 3D](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Rendu de police avec FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Client et serveur, fonctionnalités de réseau](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Importer un large éventail de ressources](https://developer.xamarin.com/api/namespace/Urho.Resources/) (avec ouvrir la bibliothèque de ressources)
- [Maillage de navigation et pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (à l’aide de la refonte/détournement)
- [Génération de la forme convexe pour la détection de collision](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (à l’aide de StanHull)
- [Lecture audio](https://developer.xamarin.com/api/namespace/Urho.Audio/) (avec **libvorbis**)

# <a name="getting-started"></a>Prise en main

UrhoSharp est distribué pour des raisons pratiques comme un [package NuGet](https://www.nuget.org/) et il peut être ajouté à vos projets c# ou F # qui ciblent Windows, Mac, Android ou iOS.  NuGet est fourni avec les bibliothèques nécessaires pour exécuter votre programme, ainsi que les composants de base (CoreData) utilisés par le moteur.

## <a name="urho-as-a-portable-class-library"></a>Urho comme une bibliothèque de classes Portable

Le package Urho peut être utilisé à partir d’un projet spécifique à la plateforme, ou à partir d’un projet de bibliothèque de classes portables, ce qui vous permet de réutiliser tout votre code sur toutes les plateformes.  Cela signifie que tous les vous devriez effectuer sur chaque plateforme consiste à écrire votre point d’entrée spécifique de plate-forme, puis transférer le contrôle à votre code de jeu partagé.

## <a name="samples"></a>Exemples

Vous pouvez obtenir un aperçu des capacités de Urho en l’ouvrant dans Visual Studio pour Mac ou Visual Studio à partir de l’exemple de solution :

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

La solution par défaut contient des projets pour Android, iOS, Windows et Mac.  Nous avons structuré cette solution afin que nous avons un lanceur de plateforme minuscule spécifique et tous les exemples de code et code de jeu réside dans une bibliothèque de classes portables illustrant comment optimiser la réutilisation du code sur toutes les plateformes.

Consultez le [Urho et votre plateforme](~/graphics-games/urhosharp/platform/index.md) pour plus d’informations sur la façon de créer vos propres solutions.

Étant donné que tous les exemples partagent un ensemble d’éléments d’interface utilisateur, les exemples ont extrait le paramétrage de base dans ce fichier :

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Cela fournit une classe de base d’exemple qui gère certaines combinaisons de touches de base et tactile paramétrages des événements, une caméra, fournit des éléments d’interface utilisateur de base et cela permet à chaque exemple pour vous concentrer sur la fonctionnalité spécifique qui est présentée.

L’exemple suivant montre ce que le moteur est capable de faire :

- [Jeu samply](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) un clone simple de ShootySkies.

Alors que les autres exemples montrent des propriétés individuelles de chaque exemple.

# <a name="basic-structure"></a>Structure de base

Votre jeu doit sous-classe le [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) (classe), il s’agit d’où vous va installer votre jeu (sur le [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) (méthode)) et démarrer votre jeu (dans le [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) méthode).  Ensuite, vous construisez votre interface utilisateur principal.  Nous allons permettant de parcourir un petit exemple qui montre les API pour configurer une scène 3D, certains éléments d’interface utilisateur et y attachant un comportement simple.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
    // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

Si vous exécutez cette application, vous découvrirez rapidement que le runtime se plaint vos éléments multimédias n’existent pas.  Ce que vous devez faire est de créer une hiérarchie dans votre projet qui commence par le nom de répertoire spécial « Données », et à l’intérieur de cette opération, vous pouvez placer les ressources que vous référencez dans votre programme.  Vous devez ensuite définir dans les propriétés d’élément pour chaque élément multimédia le « copie au répertoire de sortie » à « Copier si plus récent », qui garantit que vos données sont conservées.

Nous expliquent comment faire ici.

Pour lancer votre application, vous appelez la fonction de l’initialisation du moteur, puis en créant une nouvelle instance de la classe d’Application, comme suit :

    new MySample().Run();

Le runtime appelle le `Setup` et `Start` méthodes pour vous.  Si vous substituez `Setup` vous pouvez configurer les paramètres du moteur (non illustrée dans cet exemple).

Vous devez substituer `Start` que cette action lance votre jeu.  Dans cette méthode vous charger vos éléments multimédias, connectez les gestionnaires d’événements, le programme d’installation de votre scène et démarrer toutes les actions que vous le souhaitez.  Dans notre exemple, nous les deux créer un peu de l’interface utilisateur permettant à l’utilisateur, ainsi que la configuration d’une scène 3D.

Le fragment de code suivant utilise l’infrastructure d’interface utilisateur pour créer un élément de texte et l’ajouter à votre application :

        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from UrhoSharp",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

L’infrastructure d’interface utilisateur est là pour fournir une interface très simple de l’utilisateur dans le jeu, et il fonctionne en ajoutant de nouveaux nœuds à la [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) nœud.

La deuxième partie de nos exemples de configuration la scène principale.  Cela implique un nombre d’étapes, en créant une scène 3D, création d’une zone 3D dans l’écran, ajout d’une lumière, un appareil photo et une fenêtre d’affichage.  Elles sont abordées plus en détail dans la section «[scène, les nœuds, les composants et les appareils photo](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)»

La troisième partie de notre exemple déclenche deux actions.  Les actions sont les recettes décrivent un effet particulier, une fois créée qu’ils peuvent être exécutées par un nœud à la demande en appelant le [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) méthode sur un `Node`.

La première action met la zone avec un effet en mouvement à l’échelle et l’autre fait pivoter la zone indéfiniment :

    await boxNode.RunActionsAsync(
        new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));

Le tableau ci-dessus montre comment la première action que vous créez est un [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) action, il s’agit simplement une recette qui indique que vous souhaitez à l’échelle pour une seconde vers la valeur de la propriété de l’échelle d’un nœud.  Cette action est à son tour encapsulée autour d’une action d’accélération, le [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) action.  Les actions d’accélération déforment l’exécution linéaire d’une action et appliquent un effet, dans ce cas, il fournit l’effet ne rebondissent pas à la sortie.
Par conséquent, notre recette peut être écrite en tant que :

    var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));

Une fois la recette a été créée, nous exécutons la recette :

    await boxNode.RunActionsAsync (recipe)

L’instruction await indique que le souhaitez reprendre l’exécution après cette ligne lors de l’action se termine.  Une fois que l’action se termine, nous déclencher la deuxième animation.

Le [à l’aide de UrhoSharp](~/graphics-games/urhosharp/using.md) document explore en détail les concepts Urho et la structure de votre code pour générer un jeu.

# <a name="copyrights"></a>Copyrights

Cette documentation contient du contenu d’origine à partir de Xamarin, Inc, mais dessine largement à partir de la documentation open source pour le projet Urho3D et contient des captures d’écran à partir du projet Cocos2D.



## <a name="related-links"></a>Liens associés

- [Planète terre classeur](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Exploration des coordonnées de classeur](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
