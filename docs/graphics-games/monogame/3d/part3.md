---
title: Coordonnées 3D dans MonoGame
description: Compréhension du système de coordonnées 3D est une étape importante dans le développement de jeux en 3D. MonoGame fournit un nombre de classes pour le positionnement et l’orientation des mise à l’échelle des objets dans l’espace 3D.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: dc21228f1daa74a90ff8f0ea346bc01b109f0987
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107637"
---
# <a name="3d-coordinates-in-monogame"></a>Coordonnées 3D dans MonoGame

_Compréhension du système de coordonnées 3D est une étape importante dans le développement de jeux en 3D. MonoGame fournit un nombre de classes pour le positionnement et l’orientation des mise à l’échelle des objets dans l’espace 3D._

Développement de jeux 3D implique de comprendre comment manipuler des objets dans un système de coordonnées 3D. Cette procédure pas à pas décrit comment manipuler des objets visuels (en particulier un modèle). Nous allons nous baser sur les concepts de contrôle d’un modèle pour créer une classe d’appareil photo en 3D.

Les concepts présentés sont issus d’algèbre linéaire, mais nous allons adopter une approche pratique afin que n’importe quel utilisateur sans un arrière-plan mathématiques fort peut appliquer ces concepts dans leurs propres jeux.

Nous allons aborder les sujets les rubriques suivantes :

- Création d'un projet
- Création d’une entité de Robot
- Déplacement de l’entité de Robot
- Multiplication de matrice
- Création de l’entité de l’appareil photo
- Déplacement de la caméra avec une entrée

Une fois que vous avez terminé, nous aurons un projet avec un robot de déplacement dans un cercle et un appareil photo qui peut être contrôlé par l’entrée tactile :

![](part3-images/image1.gif "Une fois que vous avez terminé, l’application inclut un projet avec un robot de déplacement dans un cercle et un appareil photo qui peut être contrôlé par l’entrée tactile")


## <a name="creating-a-project"></a>Création d'un projet

Cette procédure pas à pas se concentre sur le déplacement d’objets dans l’espace 3D. Nous allons commencer avec le projet pour les modèles de rendu et de tableaux de sommet [qui se trouve ici](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/). Une fois téléchargé, décompressez et ouvrez le projet pour vous assurer qu’il s’exécute et nous devrions voir ce qui suit :

![](part3-images/image2.png "Une fois téléchargé, décompressez et ouvrez le projet pour vous assurer qu’il s’exécute et cette vue doit être affichée.")


## <a name="creating-a-robot-entity"></a>Création d’une entité de Robot

Avant de commencer notre robot autour mobile, nous allons créer un `Robot` classe pour contenir la logique de dessin et de déplacement. Les développeurs de jeux faire référence à cette encapsulation de la logique et les données comme un *entité*.

Ajouter un nouveau fichier de classe vide à la **MonoGame3D** bibliothèque de classes portables (pas le ModelAndVerts.Android spécifique à la plateforme). Nommez-le **Robot** et cliquez sur **New**:

![](part3-images/image3.png "Nommez-le Robot et cliquez sur Nouveau")

Modifier la `Robot` classe comme suit :

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

Le `Robot` code est essentiellement le même code dans `Game1` pour le dessin un `Model`. Pour un examen sur `Model` le chargement et le dessin, consultez [ce guide sur l’utilisation des modèles](~/graphics-games/monogame/3d/part1.md). Nous pouvons maintenant supprimer tous les `Model` le chargement et le code à partir de rendu `Game1`et remplacez-la par une `Robot` instance :

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Si nous exécutons le code maintenant nous aurons une scène avec qu’un seul robot qui est dessiné principalement sous le plancher :

![](part3-images/image4.png "Si le code est exécuté maintenant, l’application affiche une scène avec qu’un seul robot qui est dessiné principalement sous le plancher")

## <a name="moving-the-robot"></a>Déplacer le Robot

Maintenant que nous avons un `Robot` (classe), nous pouvons ajouter la logique de déplacement pour le robot. Dans ce cas, nous allons simplement rendre le robot déplacer dans un cercle en fonction de l’heure de jeu. Il s’agit d’une implémentation quelque peu difficile pour un jeu réel dans la mesure où un caractère peut généralement répondre aux entrées ou de l’intelligence artificielle, mais il fournit un environnement pour nous Explorer le positionnement 3D et la rotation.

La seule information que nous aurons besoin d’en dehors de la `Robot` classe est l’heure actuelle de jeu. Nous allons ajouter un `Update` méthode qui prendra un `GameTime` paramètre. Cela `GameTime` paramètre servira à incrémenter une variable d’angle que nous allons utiliser pour déterminer la position finale pour le robot.

Tout d’abord, nous allons ajouter le champ d’angle pour le `Robot` classe sous le `model` champ :

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Maintenant nous pouvons incrémenter cette valeur dans un `Update` (fonction) :

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Nous avons besoin pour vous assurer que le `Update` méthode est appelée à partir de `Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Bien sûr, à ce stade le champ angle ne fait rien : nous avons besoin d’écrire du code pour l’utiliser. Nous allons modifier le `Draw` méthode afin que nous pouvons calculer le monde `Matrix` dans une méthode dédiée : 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

Ensuite, nous allons implémenter le `GetWorldMatrix` méthode dans la `Robot` classe :

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Le résultat de l’exécution de ce code génère le robot déplacement dans un cercle :

![](part3-images/image5.gif "Les résultats de ce code en cours d’exécution dans le robot déplacement dans un cercle")

## <a name="matrix-multiplication"></a>Multiplication de matrice

Le code ci-dessus fait pivoter le robot en créant un `Matrix` dans le `GetWorldMatrix` (méthode). Le `Matrix` struct contient des valeurs float 16, ce qui peuvent être utilisées pour traduire (définir la position), faire pivoter et mettre à l’échelle (définir la taille). Lorsque nous attribuons le `effect.World` propriété, nous indiquons sous-jacent rendu système comment positionner et dimensionner orienter tout ce qui nous se produire à dessiner (un `Model` ou une géométrie à partir de vertex). 

Heureusement, le `Matrix` struct inclut un nombre de méthodes qui simplifient la création des types courants de matrices. La première utilisée dans le code ci-dessus est `Matrix.CreateTranslation`. Le terme mathématique *traduction* fait référence à une opération qui se traduit par un point (ou dans notre cas un modèle) déplacer d’un emplacement vers un autre sans aucune autre modification (par exemple, la rotation ou le redimensionnement). La fonction prend une valeur X, Y et Z pour la traduction. Si nous voir notre scène à partir de haut en bas, notre `CreateTranslation` (méthode) (en isolation) effectue les opérations suivantes :

![](part3-images/image6.png "La méthode CreateTranslation de manière isolée pour effectuer cette action")

Deuxième matrice que vous créez est une matrice de rotation à l’aide de la `CreateRotationZ` matrice. Il s’agit d’une des trois méthodes qui peuvent être utilisées pour créer la rotation :

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Chaque méthode crée une matrice de rotation en faisant pivoter autour d’un axe donné. Dans notre cas, nous allons faire pivoter autour de l’axe Z, qui pointe « up ». Les éléments suivants peuvent aider à visualiser comment basée sur un axe de rotation works :

![](part3-images/image7.png "Cela peut aider à visualiser comment basée sur un axe de rotation works")

Nous utilisons également le `CreateRotationZ` méthode avec le champ angle, qui s’incrémente au fil du temps en raison de notre `Update` méthode appelée. Le résultat est que le `CreateRotationZ` méthode se traduit par notre robot à orbite autour de l’origine en tant que délai s’écoule.

La dernière ligne de code combine deux matrices en une seule :

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Cela est appelé une multiplication de matrice, qui fonctionne légèrement différente de celle multiplication régulière. Le *propriété commutative de multiplication* indique que l’ordre des nombres dans une opération de multiplication ne change pas le résultat. Autrement dit, 3 * 4 équivaut à 4 * 3. Multiplication de matrice diffère car il n’est pas commutative. Autrement dit, la ligne ci-dessus peut être lu comme « Appliquer le translationMatrix pour déplacer le modèle, puis faire pivoter tout en appliquant la rotationMatrix ». Nous aurions pu visualiser la façon que la ligne ci-dessus affecte la position et la rotation comme suit :

![](part3-images/image8.png "Un dossier public de visualisation la façon que la ligne ci-dessus affecte la position et la rotation")

Pour aider à comprendre comment l’ordre de multiplication de matrice peut affecter le résultat, prenez en compte ce qui suit, où la multiplication de matrice est inversée :

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Le code ci-dessus est tout d’abord faire pivoter le modèle sur place, puis traduire :

![](part3-images/image9.png "Le code ci-dessus est tout d’abord faire pivoter le modèle sur place, puis traduire")

Si nous exécutons le code avec la multiplication inversée, nous remarquerez que dans la mesure où la rotation s’applique tout d’abord, il affecte uniquement l’orientation du modèle et la position du modèle reste le même. En d’autres termes, le modèle fait pivoter en place :

![](part3-images/image10.gif "Le modèle fait pivoter en place")

## <a name="creating-the-camera-entity"></a>Création de l’entité de l’appareil photo

Le `Camera` entité contient toute la logique nécessaire pour effectuer des déplacements d’entrée et pour fournir des propriétés pour l’attribution des propriétés sur la `BasicEffect` classe.

Tout d’abord nous allons implémenter une caméra statique (aucune entrée de déplacement) et l’intégrer dans notre projet existant. Ajoutez une nouvelle classe à la **MonoGame3D** bibliothèque de classes Portable (le même projet avec `Robot.cs`) et nommez-le **caméra**. Remplacez le contenu du fichier par le code suivant :

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Le code ci-dessus est très similaire au code à partir de `Game1` et `Robot` qui affectent les matrices sur `BasicEffect`. 

Maintenant nous pouvons intégrer le nouveau `Camera` classe dans nos projets existants. Tout d’abord, nous allons modifier le `Robot` classe pour prendre un `Camera` d’instance dans son `Draw` (méthode), ce qui élimine un grand nombre de doublons de code. Remplacez le `Robot.Draw` méthode par le code suivant :

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

Ensuite, modifiez le `Game1.cs` fichier :

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Les modifications apportées à la `Game1` à partir de la version précédente (qui sont identifié avec `// New camera code` ) sont :

- `Camera` Champ dans `Game1`
- `Camera` instanciation dans `Game1.Initialize`
- `Camera.Update` appeler dans `Game1.Update`
- `Robot.Draw` accepte désormais un `Camera` paramètre
- `Game1.Draw` utilise désormais `Camera.ViewMatrix` et `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Déplacement de la caméra avec une entrée

Jusqu’ici, nous avons ajouté un `Camera` entité mais n’avez rien fait avec lui pour modifier le comportement d’exécution. Nous allons ajouter le comportement qui autorise l’utilisateur à :

- Toucher le côté gauche de l’écran pour activer la caméra vers la gauche
- Toucher le côté droit de l’écran pour activer la caméra vers la droite
- Toucher le centre de l’écran d’avancer l’appareil photo

### <a name="making-lookat-relative"></a>Rendre regarder relatif

Nous allons tout d’abord mettre à jour le `Camera` classe pour inclure un `angle` champ qui sera utilisé pour définir la direction qui le `Camera` est face. Actuellement, notre `Camera` détermine la direction en face de via local `lookAtVector`, qui est assignée à `Vector3.Zero`. En d’autres termes, notre `Camera` ressemble toujours à l’origine. Si l’appareil photo est déplacé, puis l’angle de l’appareil photo est face modifiera également :

![](part3-images/image11.gif "Si l’appareil photo est déplacé, puis l’angle de l’appareil photo est face modifiera également")

Nous voulons le `Camera` pour être accessible sur la même direction, quel que soit sa position : au moins jusqu'à ce que nous implémentons la logique de rotation du `Camera` à l’aide d’entrée. La première modification sera pour ajuster le `lookAtVector` variable d’être basé sur notre emplacement actuel, plutôt que par examen à une position absolue :

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Il en résulte le `Camera` affichant le monde sur simple. Notez que la première `position` valeur a été modifiée pour `(0, 20, 10)` afin que la `Camera` est centrée sur l’axe X. Le jeu s’affiche en cours d’exécution :

![](part3-images/image12.png "Le jeu en cours d’exécution affiche cette vue")

### <a name="creating-an-angle-variable"></a>Création d’un angle Variable

Le `lookAtVector` variable de contrôle de l’angle de l’affichage de notre appareil photo. Actuellement il est fixé à afficher en bas de l’axe des Y négatif et légèrement inclinée vers le bas (à partir de la `-.5f` valeur Z). Nous allons créer un `angle` variable qui sera utilisé pour ajuster le `lookAtVector` propriété. 

Dans les sections précédentes de cette procédure pas à pas, nous avons montré que les matrices peuvent être utilisées pour faire pivoter la manière dont les objets sont dessinées. Nous pouvons également utiliser les matrices pour faire pivoter des vecteurs, comme le `lookAtVector` à l’aide de la `Vector3.Transform` (méthode). 

Ajouter un `angle` champ et modifier le `ViewMatrix` propriété comme suit :

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Lecture de l’entrée

Notre `Camera` entité peut désormais être entièrement contrôlée via sa position et les variables de l’angle : il suffit donc de les modifier en fonction de l’entrée.

Tout d’abord, nous obtenons le `TouchPanel` état pour trouver où l’utilisateur touche l’écran. Pour plus d’informations sur l’utilisation de la `TouchPanel` de classe, consultez [la référence d’API de l’écran tactile](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Si l’utilisateur touche le troisième gauche, puis nous modifions le `angle` valeur donc la `Camera` fait pivoter à gauche, et si l’utilisateur touche le troisième droite, nous allons faire pivoter l’inverse. Si l’utilisateur touche dans le tiers central de l’écran, nous allons déplacer les `Camera` vers l’avant.

Tout d’abord, ajoutez une instruction pour qualifier le `TouchPanel` et `TouchCollection` classes dans `Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Ensuite, modifiez le `Update` méthode pour lire l’écran tactile et ajuster la `angle` et `position` variables en conséquence :

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Maintenant le `Camera` répondra à l’entrée tactile :

![](part3-images/image1.gif "Maintenant que l’appareil photo répondra à l’entrée tactile")

La méthode de mise à jour commence en appelant `TouchPanel.GetState`, qui retourne une collection de contacts. Bien que `TouchPanel.GetState` peut retourner plusieurs points tactiles, nous allons uniquement soucier le par souci de simplicité.

Si l’utilisateur touche l’écran, puis le code vérifie si le premier contact tactile se trouve dans la gauche, du milieu ou de droite tiers de l’écran. Les tiers de gauche et droite faire pivoter la caméra en augmentant ou diminuant le `angle` variable en fonction de la `TotalSeconds` (de sorte que le jeu comporte, quel que soit la fréquence d’images).

Si l’utilisateur touche troisième le centre de l’écran, puis l’appareil photo déplacera vers l’avant. Cela s’effectue tout d’abord en obtenant le vecteur vers l’avant, ce qui est initialement défini comme pointant vers l’axe des Y négatif, pivoté par une matrice créée à l’aide `Matrix.CreateRotationZ` et `angle` valeur. Enfin le `forwardVector` est appliquée à `position` à l’aide de la `unitsPerSecond` coefficient.

## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas explique comment déplacer et faire pivoter `Models` en 3D de l’espace à l’aide de `Matrices` et `BasicEffect.World` propriété. Cette forme de mouvement fournit la base de déplacement d’objets dans des jeux en 3D. Cette procédure pas à pas explique également comment implémenter un `Camera` entité pour afficher le monde à partir de n’importe quel emplacement et l’angle.

## <a name="related-links"></a>Liens associés

- [Lien vers l’API MonoGame](http://www.monogame.net/documentation/?page=api)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
