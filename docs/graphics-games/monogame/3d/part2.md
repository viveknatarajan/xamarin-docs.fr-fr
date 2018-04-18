---
title: Dessin des graphiques 3D de sommets dans MonoGame
description: MonoGame prend en charge l’utilisation des tableaux de sommets pour définir la façon dont un objet 3D est rendu sur une base par point. Les utilisateurs peuvent tirer parti des tableaux de sommets pour créer une géométrie dynamique, implémenter des effets spéciaux et améliorer l’efficacité de leur rendu à l’élimination.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4736bedd413663af098bbad522cc56f432e36ea0
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Dessin des graphiques 3D de sommets dans MonoGame

_MonoGame prend en charge l’utilisation des tableaux de sommets pour définir la façon dont un objet 3D est rendu sur une base par point. Les utilisateurs peuvent tirer parti des tableaux de sommets pour créer une géométrie dynamique, implémenter des effets spéciaux et améliorer l’efficacité de leur rendu à l’élimination._

Les utilisateurs qui ont lu le [guide sur les modèles de rendu](~/graphics-games/monogame/3d/part1.md) serez familiarisé avec le rendu d’un modèle 3D dans MonoGame. La `Model` classe est un moyen efficace pour afficher des graphiques 3D lorsque vous travaillez avec les données définies dans un fichier (par exemple, .fbx) et lorsque vous traitez des données statiques. Certains jeux nécessite la géométrie 3D pour être définie ou manipulées dynamiquement lors de l’exécution. Dans ce cas, nous pouvons utiliser des tableaux de *sommets* pour définir et afficher la géométrie. Un sommet est un terme général qui désigne un point dans l’espace 3D, qui fait partie d’une liste triée utilisée pour définir la géométrie. En général les sommets sont ordonnés de manière à définir un ensemble de triangles.

Pour mieux visualiser comment les sommets sont utilisés pour créer des objets 3D, examinons la sphère suivante :

![](part2-images/image1.png "Pour mieux visualiser comment les sommets sont utilisés pour créer des objets 3D, envisagez de ce domaine")

Comme indiqué ci-dessus, la sphère est clairement composée de plusieurs triangles. Nous pouvons afficher filaire de la sphère pour voir comment les sommets se connectent à triangles du formulaire :

![](part2-images/image2.png "Afficher le filaire de la sphère pour voir comment les sommets se connectent à des triangles de formulaire")

Cette procédure pas à pas couvre les rubriques suivantes :

- Création d'un projet
- Créer les sommets
- Ajout de code de dessin
- Rendu d’une texture
- Modification des coordonnées de texture
- Vertex avec les modèles

Le projet terminé contiendra un étage à damier qui sera dessiné à l’aide d’un tableau de vertex :

![](part2-images/image3.png "Le projet terminé contiendra un étage à damier qui sera dessiné à l’aide d’un tableau de sommets")

## <a name="creating-a-project"></a>Création d'un projet

Tout d’abord, nous allons télécharger un projet qui servira de point de départ. Nous allons utiliser le projet de modèle [qui se trouvent ici](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

Une fois téléchargé et décompressé, ouvrez et exécutez le projet. Nous prévoyons de voir les six modèles robot dessinées à l’écran :

![](part2-images/image4.png "Six modèles robot dessinées à l’écran")

À la fin de ce projet nous allons combinant notre propre rendu vertex personnalisée avec le robot `Model`, de sorte que nous n’allons pas supprimer le code de rendu de robot. Au lieu de cela, nous allons juste effacer la `Game1.Draw` appel à supprimer le dessin des 6 robots pour le moment. Pour ce faire, ouvrez le **Game1.cs** de fichiers et recherchez le `Draw` (méthode). Modifiez-le de sorte qu’il contienne le code suivant :

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Il en résulte dans notre jeu afficher un écran bleu vide :

![](part2-images/image5.png "Ainsi, le jeu d’afficher un écran bleu vide")

## <a name="creating-the-vertices"></a>Créer les sommets

Nous allons créer un tableau de vertex à définir la géométrie. Dans cette procédure pas à pas, nous allons créer un plan 3D (un carré dans un espace 3D, pas un avion). Bien que notre plan possède quatre côtés et les quatre coins, il est composé de deux triangles, chacun d’eux requiert trois sommets. Par conséquent, vous n’allez définir un total de six points.

Jusqu'à présent nous avons parlé sommets dans un sens général, mais MonoGame fournit certaines structures standards qui peuvent être utilisés pour les sommets :

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Nom de chaque type indique les composants qu’il contient. Par exemple, `VertexPositionColor` contient des valeurs de position et la couleur. Nous allons nous intéresser à chacun des composants :

- Position – tous les types de sommets incluent un `Position` composant. Le `Position` valeurs définissent où le sommet se trouve dans un espace 3D (X, Y et Z).
- Couleur – sommets peuvent éventuellement spécifier un `Color` valeur pour effectuer des teintes personnalisées.
- Normal – normales définissent de quelle façon la surface de l’objet fait l’objet. Normales sont nécessaires que si le rendu d’un objet avec éclairage depuis la direction une surface fait l’objet d’impacts combien clair qu’il reçoit. Normales sont généralement spécifiées comme un *vecteur unitaire* – un vecteur 3D d’une longueur de 1.
- Texture – Texture fait référence à des coordonnées de texture : autrement dit, la partie d’une texture doit apparaître à un sommet spécifique. Les valeurs de texture sont nécessaires si le rendu d’un objet 3D avec une texture. Les coordonnées de texture sont des coordonnées normalisées, ce qui signifie que les valeurs seront situent entre 0 et 1. Nous aborderons les coordonnées de texture plus en détail plus loin dans ce guide.

Notre plan servira d’un étage, et nous vous souhaitez appliquer une texture lors de l’exécution de notre rendu, et nous utiliserons le `VertexPositionTexture` type pour définir ses sommets.

Tout d’abord, nous allons ajouter un membre à la classe Game1 :

```csharp
VertexPositionTexture[] floorVerts; 
```

Ensuite, définissez ses sommets dans `Game1.Initialize`. Notez que le modèle fourni plus haut dans cet article ne contient pas un `Game1.Initialize` méthode, nous devons ajouter la totalité de la méthode à `Game1`:

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

Pour mieux visualiser l’aspect de notre sommets, considérez le diagramme suivant :

![](part2-images/image6.png "Pour mieux visualiser l’aspect de sommets, examinons ce diagramme")

Nous devons s’appuient sur notre diagramme pour visualiser les sommets jusqu'à ce que nous aurons terminé la mise en œuvre de notre code de rendu.

## <a name="adding-drawing-code"></a>Ajout de Code de dessin

Maintenant que nous avons les positions pour notre geometry définie, nous pouvons écrire notre code de rendu.

Tout d’abord, nous aurons besoin afin de définir un `BasicEffect` instance qui contiendra les paramètres de rendu, tels que de la position et l’éclairage. Pour ce faire, ajoutez un `BasicEffect` membre à la `Game1` classe ci-dessous où la `floorVerts` champ est défini :


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Ensuite, modifiez le `Initialize` méthode pour définir l’effet :

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
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Maintenant nous pouvons ajouter le code pour effectuer le dessin :

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
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

Examinons certains détails dans le code ci-dessus.

### <a name="view-and-projection-properties"></a>Afficher et les propriétés de Projection

Le `View` et `Projection` propriétés contrôlent la façon dont nous permet d’afficher la scène. Nous allons modifier ce code ultérieurement lorsque nous ajoutez de nouveau le code de rendu du modèle. Plus précisément, `View` contrôle l’emplacement et l’orientation de la caméra, et `Projection` contrôles le *champ de vue* (qui peut être utilisé pour effectuer un zoom de l’appareil photo).

### <a name="techniques-and-passes"></a>Techniques et passe

Une fois nous avons Propriétés attribuées sur notre effet, que nous pouvons effectuer le rendu réel. 

Nous ne modifierez la `CurrentTechnique` propriété dans cette procédure pas à pas, mais les jeux plus avancés peut avoir un effet unique qui peut effectuer le dessin de différentes façons (par exemple, la façon dont la valeur de couleur est appliquée). Chacun de ces modes de rendu peut être représentée comme une technique qui peut être assignée avant le rendu. En outre, chaque technique peut nécessiter plusieurs passages pour restituer correctement. Effets peut-être plusieurs passages si le rendu des éléments visuels complexes telle qu’une surface brillant ou un qui.

La chose importante à garder à l’esprit est que le `foreach` boucle permet au même code c# afficher n’importe quel effet, quelle que soit la complexité de l’objet sous-jacent `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` est où les sommets sont rendus. Le premier paramètre indique à la méthode comment nous avons organisée notre sommets. Nous avons les structuré de sorte que chaque triangle est défini par trois sommets ordonnées, donc nous utilisons le `PrimitiveType.TriangleList` valeur.

Le deuxième paramètre est un tableau de sommets qui nous définis précédemment.

Le troisième paramètre spécifie le premier index à dessiner. Étant donné que nous voulons que notre tableau entier vertex à restituer, nous allons passer une valeur de 0.

Enfin, nous spécifions le nombre de triangles à restituer. Notre tableau vertex contient deux triangles, par conséquent, passez la valeur 2.

## <a name="rendering-with-a-texture"></a>Rendu d’une Texture

À ce stade, notre application restitue un plan blanc (dans la perspective). Nous allons ensuite ajouter une texture à notre projet à utiliser lors du rendu de notre plan. 

Pour simplifier les choses, nous allons ajouter le .png directement à notre projet au lieu d’utiliser l’outil MonoGame Pipeline. Pour ce faire, téléchargez [ce fichier .png](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) sur votre ordinateur. Une fois téléchargé, avec le bouton droit sur le **contenu** dossier dans le remplissage de la Solution et sélectionnez **Ajouter > ajouter des fichiers...**  . Si vous travaillez sur Android, ce dossier sera créé sous le **actifs** dossier dans le projet Android spécifiques. Sur iOS, puis ce dossier sera à la racine du projet iOS. Accédez à l’emplacement où **checkerboard.png** est enregistré et sélectionnez ce fichier. Sélectionnez cette option pour copier le fichier dans le répertoire.

Ensuite, nous allons ajouter le code pour créer notre `Texture2D` instance. Ajoutez d’abord le `Texture2D` en tant que membre de `Game1` sous le `BasicEffect` instance :

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Modifier `Game1.LoadContent` comme suit :


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Ensuite, modifiez le `DrawGround` (méthode). La seule modification nécessaire est d’assigner `effect.TextureEnabled` à `true` et définir le `effect.Texture` à `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
}
```

Enfin, nous devons modifier le `Game1.Initialize` méthode pour attribuer une texture également coordonnées sur notre sommets :


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

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Si nous exécutons le code, nous pouvons voir que notre plan affiche maintenant un modèle de damier :

![](part2-images/image8.png "Le plan affiche maintenant un modèle de damier")

## <a name="modifying-texture-coordinates"></a>Coordonnées de Texture de modification

MonoGame utilise de normaliser les coordonnées de texture, qui sont des coordonnées comprise entre 0 et 1, et non entre 0 et de largeur de la texture ou de hauteur. Le diagramme suivant peut aider à visualiser des coordonnées normalisées :

![](part2-images/image9.png "Ce diagramme peut aider à visualiser des coordonnées normalisées")

Les coordonnées de texture normalisé autoriser le redimensionnement de texture sans avoir à réécrire le code ou de recréer des modèles (tels que les fichiers de .fbx). Cela est possible, car les coordonnées normalisées représentent un ratio plutôt que de pixels spécifiques. Par exemple, est (1,1) représentent toujours l’angle inférieur droit, quelle que soit la taille de texture.

Nous pouvons modifier l’attribution de coordonnées de texture pour utiliser une variable pour le nombre de répétitions :


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

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Cela entraîne la texture extensible 20 fois :

![](part2-images/image10.png "Cela entraîne la répétition de 20 fois de texture")


## <a name="rendering-vertices-with-models"></a>Vertex avec les modèles

Maintenant que notre plan est rendu correctement, nous pouvons rajouter les modèles pour afficher tous les éléments ensemble. Tout d’abord, nous allons ajouter nouveau le code de modèle à notre `Game1.Draw` (méthode) (avec les positions modifiées) :

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Nous allons également créer un `Vector3` dans `Game1` pour représenter la position de la caméra. Nous allons ajouter un champ sous notre `checkerboardTexture` déclaration :

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

Ensuite, supprimez l’ordinateur local `cameraPosition` variable à partir de la `DrawModel` méthode :

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

De même supprimer local `cameraPosition` variable à partir de la `DrawGround` méthode :

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Maintenant si nous exécutons le code nous pouvons voir les modèles et toutes pièces en même temps :

![](part2-images/image11.png "Les modèles et toutes pièces sont affichées en même temps")

Si nous modifions la position de la caméra (comme en augmentant sa valeur X, qui dans ce cas déplace la caméra vers la gauche), nous pouvons voir que la valeur a un impact sur le sol et les modèles :

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Ce code génère les éléments suivants :

![](part2-images/image3.png "Ce code génère cette vue")

## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas vous a montré comment utiliser un tableau de vertex pour effectuer le rendu personnalisé. Dans ce cas, nous avons créé un étage à damier en combinant notre rendu basés sur les sommets d’une texture et `BasicEffect`, mais le code présenté ici sert comme base pour un rendu 3D. Nous a également montré que le rendu en fonction des sommets pouvant être combiné des modèles dans la même séquence.

## <a name="related-links"></a>Liens associés

- [Fichier Damier (exemple)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Projet terminé (exemple)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
