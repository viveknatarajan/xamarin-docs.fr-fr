---
title: SceneKit dans Xamarin.iOS
description: Ce document décrit SceneKit, un graphique de scène 3D API qui simplifie l’utilisation des graphiques 3D en faisant disparaître la complexité de OpenGL.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 0944978b34c8e164acd6e829db177bf4fd72dea9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61376289"
---
# <a name="scenekit-in-xamarinios"></a>SceneKit dans Xamarin.iOS

SceneKit est un graphique de scène 3D API qui simplifie l’utilisation des graphiques 3D. Il a été introduite dans OS X 10.8 et est maintenant venue à iOS 8. Avec SceneKit la création de visualisations 3D IMMERSIFS et des jeux en 3D informels ne nécessite pas l’expertise dans OpenGL. S’appuyant sur les concepts communs de graphique de scène, SceneKit élimine la complexité de OpenGL et OpenGL ES, rendant très facile d’ajouter 3D contenu pour une application. Toutefois, si vous êtes un expert OpenGL, SceneKit a une aide appréciable pour lier des directement avec OpenGL également. Il inclut de nombreuses fonctionnalités qui complètent les graphiques 3D, tels que physique et s’intègre parfaitement avec plusieurs autres infrastructures de Apple, telles que Core Animation, l’Image de base et Spritekit.

SceneKit est extrêmement facile à utiliser. C’est une API déclarative qui prend en charge du rendu. Simplement, vous configurez une scène, ajouter des propriétés et les handles de SceneKit le rendu de la scène.

Pour travailler avec SceneKit, vous créez un graphique de scène à l’aide de la `SCNScene` classe. Une scène contient une hiérarchie de nœuds, représentés par des instances de `SCNNode`, définition des emplacements dans l’espace 3D. Chaque nœud possède des propriétés telles que geometry, éclairage et les matériaux qui affectent son apparence, comme l’illustre la figure suivante :

![](scenekit-images/image7.png "La hiérarchie SceneKit") 

## <a name="create-a-scene"></a>Créer une scène

Pour afficher une scène à l’écran, ajoutez-le à un `SCNView` en l’assignant à la propriété de scène de la vue. En outre, si vous apportez des modifications à la scène, `SCNView` se mettra à jour pour afficher les modifications.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Arrière-plan peut être rempli à partir des fichiers exportés via l’outil de modélisation 3d, ou par programme à partir de primitives géométriques. Par exemple, voici comment créer une sphère et l’ajouter à la scène :

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Ajout de lumière

À ce stade la sphère n’affichera rien, car il n’existe aucune lumière dans la scène. Attachement `SCNLight` instances aux nœuds crée des lumières dans SceneKit. Il existe plusieurs types de lumières allant de différentes formes de la lumière directionnelle à éclairage ambiant. Par exemple, le code suivant crée une lumière omnidirectionnelle sur le côté de la sphère :

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

Éclairage omnidirectionnel produit une réflexion diffuse, ce qui entraîne un éclairage de même, de tels que projection d’une torche. Création de la lumière ambiante ressemble, bien qu’il n’ait aucune direction comme il se distingue le plus équitablement dans toutes les directions. Considérez-le comme humeur éclairage  :)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Avec l’éclairage en place, la sphère est désormais visible dans la scène.

![](scenekit-images/image8.png "La sphère est visible dans la scène lorsqu’il est allumé")
 
## <a name="adding-a-camera"></a>Ajout d’une caméra

Ajout d’une caméra (SCNCamera) à la scène change du point de vue. Le modèle à ajouter l’appareil photo est similaire. Créer l’appareil photo, attacher à un nœud et ajouter le nœud à la scène.

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

Comme vous pouvez le voir à partir du code ci-dessus, SceneKit objets peuvent être créés à l’aide de constructeurs ou à partir de la méthode de fabrique de créer. La première permet à l’aide de C# syntaxe d’initialiseur, mais celle à utiliser est en grande partie une question de préférence.

Avec l’appareil photo en place, l’intégralité de la sphère est visible par l’utilisateur :

![](scenekit-images/image9.png "L’intégralité de la sphère est visible par l’utilisateur")
 
Vous pouvez ajouter des lumières supplémentaires à la scène également. Voici à quoi elle ressemble avec quelques lumières omnidirectionnels plus :

![](scenekit-images/image10.png "La sphère avec quelques lumières omnidirectionnels plus")
 
En outre, en définissant `sceneView.AllowsCameraControl = true`, l’utilisateur peut modifier le point de vue avec un mouvement tactile.

### <a name="materials"></a>Matériaux

Matériaux est créés avec la classe SCNMaterial. Par exemple pour ajouter une image sur la surface de la sphère, la valeur est l’image du matériau *diffuse* contenu.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Cela couches de l’image sur le nœud comme indiqué ci-dessous :

![](scenekit-images/image11.png "Superposition de l’image sur la sphère")
 
Un matériau peut être défini pour répondre à d’autres types de trop d’éclairage. Par exemple, l’objet peut être rendu brillant et son contenu spéculaire avoir pour afficher une réflexion spéculaire, ce qui entraîne un point lumineux sur la surface, comme indiqué ci-dessous :

![](scenekit-images/image12.png "L’objet effectué brillant avec la réflexion spéculaire, ce qui entraîne un point lumineux sur l’aire de")
 
Des documents sont très flexibles, ce qui vous permet d’obtenir beaucoup avec très peu de code. Par exemple, au lieu de définir l’image au contenu diffus, affectez-lui le contenu réfléchissant à la place.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Le monkey apparaît désormais rester assis visuellement au sein de la sphère, indépendamment du point de vue.

### <a name="animation"></a>Animation

SceneKit est conçu pour fonctionner avec l’animation. Vous pouvez créer des animations implicites ou explicites et même de rendre une scène à partir d’une arborescence de la couche Core Animation. Lorsque vous créez une animation implicite, SceneKit fournit sa propre classe de transition, `SCNTransaction`.

Voici un exemple qui fait pivoter la sphère :

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Vous pouvez animer beaucoup plus que rotation cependant. De nombreuses propriétés de SceneKit peuvent être animées. Par exemple, le code suivant anime le matériau `Shininess` pour augmenter la réflexion spéculaire.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit est très simple à utiliser. Il offre une multitude de fonctionnalités supplémentaires, notamment les contraintes physique, actions déclaratives, texte 3D, la profondeur de la prise en charge de champ, intégration du Sprite Kit et l’intégration d’Image Core à nommer que quelques-uns.