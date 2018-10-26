---
title: SpriteKit dans Xamarin.iOS
description: Ce document décrit SpriteKit, framework de graphismes 2D d’Apple qui s’intègre à SceneKit, incorpore des graphismes et animation, inclut la prise en charge de l’éclairage et l’ombrage et bien plus encore. SpriteKit peut être utilisé pour créer des jeux 2D.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: ef1e9a98b76166f4ee5638d1ab9762896d1e3bc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121644"
---
# <a name="spritekit-in-xamarinios"></a>SpriteKit dans Xamarin.iOS

SpriteKit, le framework de graphismes 2D à partir d’Apple, a de nouvelles fonctionnalités intéressantes dans iOS 8 et OS X Yosemite. Ceux-ci incluent l’intégration avec SceneKit, prise en charge du nuanceur, éclairage, ombres, contraintes, génération d’une carte de normale et améliorations de physique. En particulier, les nouvelles fonctionnalités de moteur physique rendent très facile d’ajouter des effets réalistes à un jeu.

## <a name="physics-bodies"></a>Corps de physique

SpriteKit inclut un 2D, physique corps rigide API. Chaque sprite a un corps physique associé (`SKPhysicsBody`) qui définit les propriétés de physique telles que masse et friction, ainsi que la géométrie du corps dans le monde physique.

## <a name="creating-a-physics-body-from-a-texture"></a>Création d’un corps de physique à partir d’une Texture
SpriteKit prend désormais en charge le corps de la physique d’un sprite dérivant de sa texture. Cela rend facile à implémenter des collisions aspect plus naturelles.

Par exemple, notez que dans la collision suivante comment le banana et monkey sont en conflit presque à la surface de chaque image :
 
![](spritekit-images/image13.png "Le banana et monkey sont en conflit presque à la surface de chaque image")

SpriteKit facilite la création d’un tel organisme physique possible avec une seule ligne de code. Il vous suffit d’appeler `SKPhysicsBody.Create` avec la taille et de texture : sprite. PhysicsBody = SKPhysicsBody.Create (sprite. Texture, sprite. Taille) ;

## <a name="alpha-threshold"></a>Seuil alpha

Outre la définition de simplement le `PhysicsBody` propriété directement à la géométrie dérivée de la texture, les applications peuvent définir et seuil alpha pour contrôler la manière dont la géométrie est déduite. 

Le seuil alpha définit la valeur alpha minimale qu'un pixel doit avoir pour être inclus dans le corps de la physique qui en résulte. Par exemple, le code suivant génère un corps physique légèrement différente :

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

L’effet du peaufinage le seuil alpha ainsi affine la collision précédente, telle que le monkey lors de la collision avec le banana :

![](spritekit-images/image14.png "Le monkey se situe lors de la collision avec le banana")
 
## <a name="physics-fields"></a>Champs de physique

Un autre excellent complément pour SpriteKit est le nouveau champ physique prend en charge. Pour ajouter des éléments tels que les champs tourbillon, qui vous permettent de champs de gravité radiale et spring pour nommer que quelques-uns.

Champs de physique sont créés à l’aide de la classe SKFieldNode, qui est ajoutée à une scène comme n’importe quel autre `SKNode`. Il existe une variété de méthodes de fabrique sur `SKFieldNode` pour créer des champs physique différent. Vous pouvez créer un champ spring en appelant `SKFieldNode.CreateSpringField()`, un champ de gravité radiale en appelant `SKFieldNode.CreateRadialGravityField()`, et ainsi de suite.

`SKFieldNode` possède également des propriétés pour contrôler les attributs de champ tels que l’intensité de champ, la champ région et l’atténuation des forces de champ.

## <a name="spring-field"></a>Champ de printemps

Par exemple, le code suivant crée un champ de printemps et l’ajoute à la scène :

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Vous pouvez ensuite ajouter des sprites et définissez leurs `PhysicsBody` propriétés afin que le champ physique affectera les sprites, comme avec le code suivant lorsque l’utilisateur touche l’écran :

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

Ceci provoque bananes osciller comme un ressort autour du nœud de champ :

![](spritekit-images/image15.png "Bananes osciller comme un ressort autour du nœud de champ")
 
## <a name="radial-gravity-field"></a>Champ de gravité radiale

Ajout d’un autre champ est similaire. Par exemple, le code suivant crée un champ de gravité radiale :

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Il en résulte dans un autre champ de force, où les bananes sont extraits radiale sur le champ :

![](spritekit-images/image16.png "Bananes sont extraits radiale autour du champ")
