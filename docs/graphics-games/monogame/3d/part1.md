---
title: "À l’aide de la classe de modèle"
description: "La classe de modèle simplifie considérablement le rendu d’objets 3D complexes par rapport à la méthode traditionnelle de rendu des graphiques 3D. Objets de modèle sont créés à partir de fichiers de contenu, ce qui permet une intégration facile du contenu sans code personnalisé."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: dd0f97f450d131bcbddff43ffc778b91bff6b12e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="using-the-model-class"></a>À l’aide de la classe de modèle

_La classe de modèle simplifie considérablement le rendu d’objets 3D complexes par rapport à la méthode traditionnelle de rendu des graphiques 3D. Objets de modèle sont créés à partir de fichiers de contenu, ce qui permet une intégration facile du contenu sans code personnalisé._

L’API MonoGame inclut un `Model` classe qui peut être utilisé pour stocker les données chargées à partir d’un fichier de contenu et pour effectuer le rendu. Fichiers de modèle peuvent être très simples (par exemple, un triangle de couleur Uni) ou peuvent inclure des informations pour le rendu complexe, y compris la texture et l’éclairage.

Cette procédure pas à pas utilise [un modèle 3D d’un robot](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) et couvre les éléments suivants :

 - Démarrage d’un nouveau projet de jeu
 - Création de XNBs pour le modèle et sa texture
 - Y compris les XNBs dans le projet de jeu
 - Dessin d’un modèle 3D
 - Plusieurs modèles de dessin

Lorsque vous avez terminé, votre projet apparaît comme suit :

![](part1-images/image1.png "Lorsque vous avez terminé, le projet s’affiche comme suit")


# <a name="creating-an-empty-game-project"></a>Créez un projet de jeu vide

Nous aurons besoin afin de configurer un projet de jeu d’abord appelé MonoGame3D. Pour plus d’informations sur la création d’un nouveau projet MonoGame, consultez [cette procédure pas à pas sur la création d’un projet de Monogame Cross-Platform](~/graphics-games/monogame/introduction/part1.md).

Avant de continuer, nous devons vérifier que le projet s’ouvre et déploie correctement. Une fois déployée, nous devons voir un écran bleu vide :

![](part1-images/image2.png "Une fois déployé, le développeur doit voir un écran bleu vide")


# <a name="including-the-xnbs-in-the-game-project"></a>Y compris les XNBs dans le projet de jeu

Le format de fichier .xnb est une extension standard pour le contenu généré (le contenu qui a été créé par le [MonoGame Pipeline outil](http://www.monogame.net/documentation/?page=Pipeline)). Tout le contenu généré est un fichier source (qui est un fichier .fbx dans le cas de notre modèle) et un fichier de destination (fichier .xnb). Le format .fbx est un format de modèle 3D courantes qui peut être créé par les applications telles que [Maya](http://www.autodesk.com/products/maya/overview) et [Blender](http://www.blender.org/). 

La `Model` classe peut être construit en chargeant un fichier .xnb à partir d’un disque qui contient les données de géométrie 3D.   Ce fichier .xnb est créé via un projet de contenu. Modèles de Monogame incluent automatiquement un projet de contenu (avec l’extension .mgcp) dans notre dossier de contenu. Pour plus d’informations sur l’outil MonoGame Pipeline, consultez la [guide du Pipeline de contenu](~/graphics-games/cocossharp/content-pipeline/index.md).

Pour ce guide, nous allons ignorer l’aide du MonoGame Pipeline outil et utilisera le. Fichiers XNB inclus ici. Notez que le. Les fichiers XNB diffèrent par la plateforme, par conséquent, veillez à utiliser l’ensemble correct de fichiers XNB pour quelle que soit la plateforme que vous travaillez avec.

Nous allons décompressez le [Content.zip fichier](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) afin que nous pouvons utiliser les fichiers .xnb contenus dans le jeu. Si vous travaillez sur un projet Android, avec le bouton droit sur le **actifs** dossier dans le **WalkingGame.Android** projet. Si vous travaillez sur un projet iOS, avec le bouton droit sur le **WalkingGame.iOS** projet. Sélectionnez **Ajouter -> ajouter des fichiers...**  et sélectionnez les deux fichiers .xnb dans le dossier pour la plateforme que vous utilisez.

Les deux fichiers doivent être dans notre projet maintenant :

![](part1-images/xnbsinxs.png "Les deux fichiers doivent faire partie du projet maintenant")

Visual Studio pour Mac ne peut-être pas définir automatiquement l’action de génération pour XNBs de qui vient d’être ajouté. Pour iOS, cliquez sur chacun des fichiers et sélectionnez **BundleResource -> l’Action de génération**. Pour Android, cliquez sur chacun des fichiers et sélectionnez **AndroidAsset -> l’Action de génération**.

# <a name="rendering-a-3d-model"></a>Rendu d’un modèle 3D

La dernière étape nécessaire pour consulter le modèle à l’écran consiste à ajouter le chargement et le code de dessin. Plus précisément, il convient d’exécuter les éléments suivants :

 - Définition d’un `Model` d’instance dans notre `Game1` classe
 - Le chargement du `Model` dans l’instance `Game1.LoadContent`
 - Dessiner la `Model` dans l’instance `Game1.Draw`

Remplacez le `Game1.cs` fichier de code (qui se trouve dans le **WalkingGame** PCL) avec les éléments suivants :


```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;
                    
        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
                                                                                                                 
```

Si nous exécutons ce code, que nous allons voir le modèle à l’écran :

![](part1-images/image8.png "Si ce code est exécuté, le modèle s’affiche à l’écran")

Examinons certaines parties du code ci-dessus plus importants.


## <a name="model-class"></a>Classe de modèle

La `Model` classe est la classe de base pour effectuer le rendu 3D à partir de fichiers de contenu (par exemple, les fichiers de .fbx). Il contient toutes les informations nécessaires pour le rendu, y compris la géométrie 3D, les références de texture, et `BasicEffect` instances qui contrôlent les valeurs de positionnement d’éclairage et appareil photo.

La `Model` classe proprement dite n’a pas directement les variables pour le positionnement dans la mesure où une instance de modèle unique peut être rendue dans plusieurs emplacements, comme nous allons montrer plus loin dans ce guide.

Chaque `Model` est composé d’un ou plusieurs `ModelMesh` instances qui sont exposées via la `Meshes` propriété. Bien que nous pouvons prendre en compte un `Model` comme un jeu unique de l’objet (par exemple, un robot ou une voiture), chacun `ModelMesh` peuvent être dessinées avec différents `BasicEffect` valeurs. Par exemple, les parties de maille peuvent représenter les éléments d’un robot ou les roues sur une voiture, et nous pouvons affecter le `BasicEffect` déplacent des valeurs pour effectuer la rotation des roues ou les pieds. 


## <a name="basiceffect-class"></a>Classe de BasicEffect

La `BasicEffect` classe fournit des propriétés pour contrôler les options de rendu. La première modification nous apporter à la `BasicEffect` consiste à appeler le `EnableDefaultLighting` (méthode). Comme son nom l’indique, cela permet d’éclairage par défaut, ce qui est très pratique pour vérifier qu’un `Model` s’affiche comme prévu dans le jeu. Si nous Commentez la `EnableDefaultLighting` appeler, puis nous allons voir le modèle de rendu simplement sa texture, mais avec aucun ombrage ou spéculaire de Halo :


```csharp
//effect.EnableDefaultLighting (); 
```

![](part1-images/image9.png "Le modèle de rendu simplement sa texture, mais avec aucun ombrage ou spéculaire de Halo")

Le `World` propriété peut être utilisée pour ajuster la position, de rotation et l’échelle du modèle. Le code ci-dessus utilise la `Matrix.Identity` valeur, ce qui signifie que le `Model` sera rendue dans le jeu exactement comme spécifié dans le fichier .fbx. Nous allons être couvrant les matrices et les coordonnées 3D plus en détail dans [partie 3](~/graphics-games/monogame/3d/part3.md), mais par exemple, nous pouvons modifier la position de la `Model` en modifiant le `World` propriété comme suit :


```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Ce code génère l’objet déplacé vers le haut par 3 unités universelles :

![](part1-images/image10.png "Ce code génère l’objet déplacé vers le haut par 3 unités universelles")

Les deux propriétés finales attribuées sur le `BasicEffect` sont `View` et `Projection`. Nous allons couvrant les caméras 3D dans [partie 3](~/graphics-games/monogame/3d/part3.md), par exemple, nous pouvons modifier la position de la caméra en modifiant l’ordinateur local, mais `cameraPosition` variable :


```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Nous pouvons voir l’appareil photo a été déplacé plus en avant, ce qui entraîne la `Model` qui apparaissent plus petits en raison d’une perspective :

![](part1-images/image11.png "L’appareil photo a déplacé, résultant dans le modèle qui apparaissent plus petits en raison d’une perspective")


# <a name="rendering-multiple-models"></a>Plusieurs modèles de rendu

Comme indiqué ci-dessus, un seul `Model` peut être dessinée à plusieurs reprises. Pour faciliter cette opération sera mobiles le `Model` code de dessin dans sa propre méthode qui accepte les `Model` position en tant que paramètre. Une fois terminé, notre `Draw` et `DrawModel` méthodes doit ressembler à :


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;
            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;
            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

Cela entraîne le robot modèle dessinée six fois :

![](part1-images/image1.png "Cela entraîne le robot modèle dessinée six fois")


# <a name="summary"></a>Récapitulatif

Cette procédure pas à pas introduit de MonoGame `Model` classe. Il couvre la conversion d’un fichier de .fbx dans un .xnb, qui peut à leur tour être chargée dans un `Model` classe. Il montre également comment les modifications apportées à un `BasicEffect` instance peut avoir un impact sur `Model` dessin.

## <a name="related-links"></a>Liens associés

- [Référence du modèle de MonoGame](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
