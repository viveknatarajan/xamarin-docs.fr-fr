---
title: Dessin de graphiques 3D avec des sommets dans MonoGame
description: MonoGame prend en charge les tableaux de sommets pour définir la façon dont un objet 3D est rendu sur une base par point. Les utilisateurs peuvent tirer parti des tableaux de sommets pour créer une géométrie dynamique, implémenter des effets spéciaux et améliorer l’efficacité de leur rendu via l’élimination.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: df36c149e98e8c0cbb16de4c2cf52def5713ec13
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121436"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Dessin de graphiques 3D avec des sommets dans MonoGame

_MonoGame prend en charge les tableaux de sommets pour définir la façon dont un objet 3D est rendu sur une base par point. Les utilisateurs peuvent tirer parti des tableaux de sommets pour créer une géométrie dynamique, implémenter des effets spéciaux et améliorer l’efficacité de leur rendu via l’élimination._

Les utilisateurs qui ont déjà lu via le [guide sur les modèles de rendu](~/graphics-games/monogame/3d/part1.md) sera familier avec le rendu d’un modèle 3D dans MonoGame. Le `Model` classe est un moyen efficace pour rendre des graphiques 3D quand vous travaillez avec les données définies dans un fichier (par exemple, .fbx) et lorsque vous traitez des données statiques. Certains jeux nécessite la géométrie 3D pour être définie ou manipulées dynamiquement lors de l’exécution. Dans ce cas, nous pouvons utiliser des tableaux de *vertex* pour définir et afficher la géométrie. Un sommet est un terme général qui désigne un point dans l’espace 3D, qui fait partie d’une liste triée, utilisée pour définir la géométrie. En général, les sommets sont classés de manière à définir une série de triangles.

Pour mieux visualiser comment les sommets sont utilisés pour créer des objets 3D, prenons la sphère suivante :

![](part2-images/image1.png "Pour mieux visualiser comment les sommets sont utilisés pour créer des objets 3D, envisagez cette sphère")

Comme indiqué ci-dessus, la sphère est clairement composée de plusieurs triangles. Nous pouvons examiner la structure de base de la sphère pour voir comment les vertex se connectent à triangles du formulaire :

![](part2-images/image2.png "Afficher une structure de base de la sphère pour voir comment les vertex se connectent à des triangles de formulaire")

Cette procédure pas à pas couvre les rubriques suivantes :

- Création d'un projet
- Création du vertex
- Ajout de code de dessin
- Rendu avec une texture
- Modification des coordonnées de texture
- Vertex avec des modèles

Le projet terminé contiendra un étage à damier qui sera dessiné à l’aide d’un tableau de vertex :

![](part2-images/image3.png "Le projet terminé contiendra un étage à damier qui sera dessiné à l’aide d’un tableau de vertex")

## <a name="creating-a-project"></a>Création d'un projet

Nous allons tout d’abord, téléchargez un projet qui servira de notre point de départ. Nous allons utiliser le projet de modèle [qui se trouve ici](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

Une fois téléchargé et décompressé, ouvrez et exécutez le projet. Nous devrions voir six modèles de robot qui est dessinés à l’écran :

![](part2-images/image4.png "Six modèles de robot qui est dessinés à l’écran")

À la fin de ce projet nous allons combinant notre propre rendu vertex personnalisée avec le robot `Model`, de sorte que nous n’allons pas supprimer le code de rendu de robot. Au lieu de cela, nous allons juste effacer la `Game1.Draw` appel à supprimer le dessin des 6 robots pour l’instant. Pour ce faire, ouvrez le **Game1.cs** de fichiers et recherchez le `Draw` (méthode). Modifiez-le afin qu’il contient le code suivant :

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Il en résulte dans notre jeu affichant un écran bleu vide :

![](part2-images/image5.png "Ainsi, le jeu d’afficher un écran bleu vide")

## <a name="creating-the-vertices"></a>Création du vertex

Nous allons créer un tableau des sommets pour définir notre geometry. Dans cette procédure pas à pas, nous allons créer un plan 3D (un carré dans l’espace 3D, pas un avion). Bien que notre plan possède quatre côtés et les quatre coins, elle est composée de deux triangles, chacun d’eux requiert trois sommets. Par conséquent, vous n’allez définir un total de six points.

Jusqu'à présent, nous avons parlé sommets dans un sens général, mais MonoGame fournit certaines structures standards, ce qui peuvent être utilisées pour les vertex :

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Nom de chaque type indique les composants qu’il contient. Par exemple, `VertexPositionColor` contient des valeurs pour la position et la couleur. Examinons chacun des composants :

- Position – tous les types de vertex incluent un `Position` composant. Le `Position` valeurs définissent où le vertex se trouve dans l’espace 3D (X, Y et Z).
- Couleur – vertex peuvent éventuellement spécifier un `Color` valeur pour effectuer des teintes personnalisées.
- Normal – normales de définissent de quelle façon la surface de l’objet est accessible sur. Les normales sont nécessaires si le rendu d’un objet avec éclairage depuis la direction une surface est accessible sur les impacts lumière qu’il reçoit. Les normales sont généralement spécifiées en tant qu’un *vecteur unitaire* – un vecteur 3D qui a une longueur de 1.
- Texture – Texture fait référence aux coordonnées de texture : autrement dit, la partie d’une texture doit apparaître à un sommet donné. Les valeurs de texture sont nécessaires si le rendu d’un objet avec une texture 3D. Coordonnées de texture sont coordonnées normalisées, ce qui signifie que les valeurs seront situent entre 0 et 1. Nous aborderons plus en détail plus loin dans ce guide, les coordonnées de texture.

Notre plan servira un étage, et il nous faut appliquer une texture lors de l’exécution de notre rendu, donc nous utiliserons le `VertexPositionTexture` type pour définir notre vertex.

Tout d’abord, nous allons ajouter un membre à la classe Game1 :

```csharp
VertexPositionTexture[] floorVerts; 
```

Ensuite, définissez notre vertex dans `Game1.Initialize`. Notez que le modèle fourni référencé plus haut dans cet article ne contient-elle pas un `Game1.Initialize` (méthode), nous devons ajouter la totalité de la méthode à `Game1`:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

Pour mieux visualiser l’aspect de notre vertex, considérez le diagramme suivant :

![](part2-images/image6.png "Pour mieux visualiser l’aspect de vertex, considérez ce diagramme")

Nous devons s’appuient sur notre diagramme pour visualiser les sommets jusqu'à ce que nous aurons terminé l’implémentation de notre code de rendu.

## <a name="adding-drawing-code"></a>Ajout de Code de dessin

Maintenant que nous avons les positions pour notre géométrie définis, nous pouvons écrire notre code de rendu.

Tout d’abord, nous devons définir un `BasicEffect` instance qui contiendra les paramètres de rendu telles que de la position et l’éclairage. Pour ce faire, ajoutez un `BasicEffect` membre à la `Game1` classe ci-dessous où la `floorVerts` champ est défini :


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Ensuite, modifiez le `Initialize` méthode pour définir l’effet :

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Maintenant, nous pouvons ajouter le code pour effectuer le dessin :

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start 
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

Nous allons devoir appeler `DrawGround` dans notre `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

L’application affiche les éléments suivants lors de l’exécution :

![](part2-images/image7.png "L’application s’affiche lors de l’exécution")

Examinons quelques-unes des détails dans le code ci-dessus.

### <a name="view-and-projection-properties"></a>Afficher et les propriétés de Projection

Le `View` et `Projection` propriétés contrôlent la façon de considérer la scène. Nous allons modifier ce code plus tard lorsque nous ajoutons de nouveau le code de rendu de modèle. Plus précisément, `View` contrôle l’emplacement et l’orientation de la caméra, et `Projection` contrôles le *champ de vision* (qui peut être utilisé pour effectuer un zoom de l’appareil photo).

### <a name="techniques-and-passes"></a>Techniques et passe

Une fois que nous avons Propriétés attribuées sur notre effet, nous pouvons effectuer le rendu réel. 

Nous ne modifiera pas le `CurrentTechnique` propriété dans cette procédure pas à pas, mais les jeux plus avancés peut-être avoir un effet unique qui peut effectuer le dessin de manières différentes (par exemple, comment la valeur de couleur est appliquée). Chacun de ces modes de rendu peut être représentée comme une technique qui peut être assignée avant le rendu. En outre, chaque technique peut nécessiter plusieurs passages pour restituer correctement. Effets peut-être plusieurs passes si le rendu des visuels complexes tel qu’une surface lumineux ou la fourrure.

La chose importante à garder à l’esprit est que le `foreach` boucle permet les mêmes C# pour effectuer le rendu d’effet, quel que soit la complexité de sous-jacent `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` est où les sommets sont affichés. Le premier paramètre indique à la méthode de façon dont nous avons organisé nos vertex. Nous avons les structuré de sorte que chaque triangle est défini par trois sommets ordonnées, donc nous utilisons le `PrimitiveType.TriangleList` valeur.

Le deuxième paramètre est le tableau de vertex qui nous définis précédemment.

Le troisième paramètre spécifie le premier index à dessiner. Dans la mesure où nous voulons que notre tableau de vertex entière doit être restitué, nous passons une valeur égale à 0.

Enfin, nous spécifions combien de triangles à restituer. Notre tableau de vertex contient deux triangles, donc pas passer une valeur de 2.

## <a name="rendering-with-a-texture"></a>Rendu avec une Texture

À ce stade, notre application restitue un plan blanc (dans la perspective). Ensuite, nous allons ajouter une texture à notre projet à utiliser lors du rendu de notre plan. 

Pour simplifier les choses, nous allons ajouter le .png directement à notre projet, plutôt que d’utiliser l’outil de Pipeline de MonoGame. Pour ce faire, téléchargez [ce fichier .png](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) sur votre ordinateur. Une fois téléchargé, avec le bouton droit sur le **contenu** dossier dans le panneau solutions, puis sélectionnez **Ajouter > ajouter des fichiers...**  . Si vous travailliez sur Android, puis ce dossier se trouve sous le **actifs** dossier dans le projet spécifique à Android. Si vous utilisez iOS, puis ce dossier sera à la racine du projet iOS. Accédez à l’emplacement où **checkerboard.png** est enregistré et sélectionnez ce fichier. Sélectionnez cette option pour copier le fichier dans le répertoire.

Ensuite, nous allons ajouter le code pour créer notre `Texture2D` instance. Tout d’abord, ajoutez le `Texture2D` en tant que membre de `Game1` sous le `BasicEffect` instance :

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Modifier `Game1.LoadContent` comme suit :


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Ensuite, modifiez le `DrawGround` (méthode). La seule modification nécessaire consiste à attribuer `effect.TextureEnabled` à `true` et pour définir le `effect.Texture` à `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    // new code:
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
```

Enfin, nous devons modifier la `Game1.Initialize` coordonne la méthode pour attribuer une texture également sur notre vertex :


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Si nous exécutons le code, nous pouvons voir que notre plan affiche maintenant un modèle de damier :

![](part2-images/image8.png "Le plan affiche maintenant un modèle de damier")

## <a name="modifying-texture-coordinates"></a>Coordonnées de Texture de modification

MonoGame utilise normalisées des coordonnées de texture, qui sont des coordonnées comprise entre 0 et 1, et non entre 0 et la largeur de la texture ou la hauteur. Le diagramme suivant peut vous aider à visualiser des coordonnées normalisées :

![](part2-images/image9.png "Ce diagramme peut aider à visualiser des coordonnées normalisées")

Coordonnées de texture normalisé autorise le redimensionnement de texture sans avoir à réécrire le code ou de recréer des modèles (tels que les fichiers de .fbx). Cela est possible, car les coordonnées normalisées représentent un ratio plutôt que de pixels spécifiques. Par exemple, est (1,1) représentent toujours l’angle inférieur droit, quel que soit la taille de texture.

Nous pouvons modifier l’attribution de coordonnées de texture pour utiliser une variable pour le nombre de répétitions :


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

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

    base.Initialize ();
}
```

Il en résulte dans la texture extensible 20 fois :

![](part2-images/image10.png "Cela entraîne la répétition de 20 fois de texture")


## <a name="rendering-vertices-with-models"></a>Vertex avec des modèles

Maintenant que notre plan est rendu correctement, nous pouvons rajouter les modèles pour afficher tous les éléments ensemble. Tout d’abord, nous allons ajouter nouveau le code de modèle à notre `Game1.Draw` (méthode) (avec les positions modifiées) :

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Nous allons également créer un `Vector3` dans `Game1` pour représenter la position de notre la caméra. Nous allons ajouter un champ sous notre `checkerboardTexture` déclaration :

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

Ensuite, supprimez l’ordinateur local `cameraPosition` variable à partir de la `DrawModel` méthode :

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

De même supprimer local `cameraPosition` variable à partir de la `DrawGround` méthode :

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Maintenant si nous exécutons le code nous pouvons voir les modèles et bout en bout en même temps :

![](part2-images/image11.png "Les modèles et bout en bout sont affichées en même temps")

Si nous modifions la position de la caméra (comme en augmentant sa valeur X, qui dans ce cas déplace la caméra vers la gauche), nous pouvons voir que la valeur a un impact sur les modèles et bout en bout :

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Ce code génère les éléments suivants :

![](part2-images/image3.png "Ce code génère cette vue")

## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas vous a montré comment utiliser un tableau de vertex pour effectuer un rendu personnalisé. Dans ce cas, nous avons créé un étage à damier en combinant notre rendu basé sur les vertex avec une texture et `BasicEffect`, mais le code présenté ici sert comme base pour un rendu 3D. Nous vous a également montré que le rendu du vertex en fonction peut être combiné avec les modèles dans la scène même.

## <a name="related-links"></a>Liens associés

- [Fichier Damier (exemple)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
