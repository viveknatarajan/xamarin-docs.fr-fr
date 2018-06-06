---
title: SceneKit dans Xamarin.iOS
description: Ce document décrit SceneKit, un graphique de scène 3D API qui simplifie l’utilisation de graphiques 3D en faisant abstraction de la complexité d’OpenGL.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: fb72e194e14f903061e1bd2dc6d04ef88ab429d4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786783"
---
# <a name="scenekit-in-xamarinios"></a>SceneKit dans Xamarin.iOS

SceneKit est un graphique de scène 3D API qui simplifie l’utilisation de graphiques 3D. Il a été introduite dans OS X 10.8 et est venu iOS 8. Avec SceneKit la création de visualisations 3D réalistes et jeux 3D informels ne nécessite pas savoir OpenGL. S’appuyant sur les concepts de graphique de scène communs, SceneKit élimine les complexités des OpenGL et OpenGL ES, rendant très facile d’ajouter 3D contenu à une application. Toutefois, si vous êtes un expert OpenGL, SceneKit a réunissant directement avec OpenGL également très bien en charge. Il comprend de nombreuses fonctionnalités qui complètent les graphiques 3D, tels que physique et s’intègre parfaitement avec plusieurs autres infrastructures de Apple, telles que l’Animation de base, l’Image de base et Sprite Kit.

SceneKit est très facile à utiliser. Il s’agit d’une API déclarative qui prend en charge du rendu. Simplement, vous configurez une scène, ajouter des propriétés et les descripteurs de SceneKit le rendu de la scène.

Pour travailler avec SceneKit, vous créez un graphique de scène à l’aide de la `SCNScene` classe. Une séquence contient une hiérarchie de nœuds, représentés par des instances de `SCNNode`, définition des emplacements dans l’espace 3D. Chaque nœud possède des propriétés telles que la géométrie, d’éclairage et les documents qui affectent son apparence, comme illustré dans la figure ci-dessous :

![](scenekit-images/image7.png "La hiérarchie SceneKit") 

## <a name="create-a-scene"></a>Créer une scène

Pour rendre une scène apparaissent sur l’écran, vous l’ajouter à un `SCNView` en l’assignant à la propriété de séquence de la vue. En outre, si vous apportez des modifications à la scène, `SCNView` se mettra à jour pour afficher les modifications.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

En arrière-plan peut être remplis à partir des fichiers exportés via l’outil de modélisation 3d, ou par programme à partir de primitives géométriques. Par exemple, voici comment créer une sphère et l’ajouter à la scène :

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Ajout de lumière

À ce stade la sphère n’affichera rien, car il n’existe aucune lumière dans la scène. Attachement `SCNLight` instances aux nœuds crée les témoins lumineux dans SceneKit. Il existe plusieurs types de lumières allant de différentes formes d’éclairage directionnel à éclairage ambiant. Par exemple, le code suivant crée une lumière omnidirectionnelle sur le côté de la sphère :

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

Éclairage omnidirectionnel génère une réflexion diffuse résultant dans un même éclairage, de telles que la projection d’un Flash. Création de la lumière ambiante est similaire, bien qu’il n’ait aucun sens, car elle projette de manière égale dans toutes les directions. Considérez-le comme humeur éclairage  :)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Avec les voyants en place, la sphère est désormais visible dans la scène.

![](scenekit-images/image8.png "La sphère est visible dans la scène lorsqu’il est allumé")
 
## <a name="adding-a-camera"></a>Ajout d’un appareil photo

Ajout d’un appareil photo (SCNCamera) à la scène change du point de vue. Le modèle à ajouter l’appareil photo est similaire. Créer l’appareil photo, attacher à un nœud et ajouter le nœud à la scène.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Comme vous pouvez voir dans le code ci-dessus, SceneKit objets peuvent être créés à l’aide de constructeurs ou à partir de la méthode de fabrique de créer. La première permet à l’aide de la syntaxe d’initialiseur de c#, mais celui que vous souhaitez utiliser est essentiellement une question de préférence.

Avec l’appareil photo sur place, la totalité de la sphère est visible par l’utilisateur :

![](scenekit-images/image9.png "L’intégralité de la sphère est visible par l’utilisateur")
 
Vous pouvez ajouter des lumières supplémentaires à la scène ainsi. Voici son aspect avec quelques lumineux d’omnidirectionnel plus :

![](scenekit-images/image10.png "La sphère avec quelques plus lumineux d’omnidirectionnel")
 
En outre, en définissant `sceneView.AllowsCameraControl = true`, l’utilisateur peut modifier le point de vue avec un mouvement tactile.

### <a name="materials"></a>Matériaux

Matériaux est créés avec la classe SCNMaterial. Par exemple pour ajouter une image sur la surface de la sphère, définir une image pour le matériel *diffuse* contenu.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Cela couches de l’image sur le nœud comme indiqué ci-dessous :

![](scenekit-images/image11.png "Superposition de l’image sur la sphère")
 
Un matériel peut être défini pour répondre à d’autres types d’éclairage trop. Par exemple, l’objet peut être rendu brillant et son contenu spéculaire avoir pour afficher la réflexion spéculaire, ce qui entraîne un point lumineux sur la surface, comme indiqué ci-dessous :

![](scenekit-images/image12.png "L’objet brillant avec la réflexion spéculaire, ce qui entraîne un point lumineux sur l’aire de")
 
Des documents sont très souples, ce qui vous permet d’obtenir un lot avec très peu de code. Par exemple, au lieu de définir l’image pour le contenu de diffusion, définir le contenu de réflexion à la place.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Le singe apparaît désormais dans cet état visuellement au sein de la sphère, indépendamment du point de vue.

### <a name="animation"></a>Animation

SceneKit est conçu pour fonctionner avec l’animation. Vous pouvez créer des animations implicites ou explicites et même de rendre une scène à partir d’une arborescence de couche d’Animation de la base. Lorsque vous créez une animation implicite, SceneKit fournit sa propre classe de transition, `SCNTransaction`.

Voici un exemple qui fait pivoter la sphère :

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Vous pouvez animer beaucoup plus de rotation cependant. De nombreuses propriétés de SceneKit peuvent être animées. Par exemple, le code suivant réalise une animation de la matière `Shininess` pour augmenter la réflexion spéculaire.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit est très simple à utiliser. Il offre de nombreuses fonctionnalités supplémentaires, notamment les contraintes physique, actions déclaratives, texte 3D, la profondeur de la prise en charge de champ, l’intégration de Sprite Kit et intégration d’Image Core citer que quelques.