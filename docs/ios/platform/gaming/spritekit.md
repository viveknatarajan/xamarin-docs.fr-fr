---
title: SpriteKit
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 967fd5b25213478c89d1ab849b6c0b7ac66d0e20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="spritekit"></a>SpriteKit

Kit Sprite, l’infrastructure de jeux 2D auprès d’Apple, a certaines nouvelles fonctionnalités intéressantes dans iOS 8 et OS X Yosemite. Ceux-ci incluent l’intégration avec la scène Kit, prise en charge du nuanceur, d’éclairage, ombres, contraintes, génération d’une carte de normale et améliorations de physique. En particulier, les nouvelles fonctionnalités physique rendent très facile d’ajouter des effets réalistes à un jeu.

## <a name="physics-bodies"></a>Corps de physique

Sprite Kit inclut un 2D, physique de corps rigide API. Chaque sprite a un corps physique associée (`SKPhysicsBody`) qui définit les propriétés de physique notamment masse et friction, ainsi que la géométrie du corps dans le monde physique.

## <a name="creating-a-physics-body-from-a-texture"></a>Création d’un corps de physique à partir d’une Texture
Sprite Kit prend désormais en charge le corps de physique d’un composant qui dérivent de la texture. Cela rend facile à implémenter des collisions aspect plus naturelles.

Par exemple, notez dans la collision suivante comment le banana et singe entrent en conflit quasiment à la surface de chaque image :
 
![](spritekit-images/image13.png "Entrer en conflit les banane singe quasiment à la surface de chaque image")

Sprite Kit facilite la création d’un tel organisme physique possible avec une seule ligne de code. Il vous suffit d’appeler `SKPhysicsBody.Create` avec la taille et la texture : sprite. PhysicsBody = SKPhysicsBody.Create (composant. Texture, sprite. Taille) ;

## <a name="alpha-threshold"></a>Seuil alpha

Outre la définition de simplement le `PhysicsBody` propriété directement à la géométrie dérivée de la texture, les applications peuvent définir et le seuil alpha pour contrôler la façon dont la géométrie est dérivée. 

Le seuil alpha définit la valeur alpha minimale qu'un pixel doit avoir pour être inclus dans le corps de physique qui en résulte. Par exemple, le code suivant renvoie un corps physique légèrement différentes :

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

L’effet de modifier le seuil alpha à ceci règle précisément la collision précédente, telle que le singe lorsque collision avec la banane :

![](spritekit-images/image14.png "Le singe se situe lorsque collision avec la banane")
 
## <a name="physics-fields"></a>Champs physique

Une autre nouveauté Sprite Kit est le nouveau champ physique prend en charge. Pour ajouter des éléments tels que les champs de vortex, qui vous permettent de champs de gravité radial et ressort pour nommer quelques.

Les champs physique sont créés à l’aide de la classe SKFieldNode, qui est ajoutée à une scène comme toute autre `SKNode`. Il existe diverses méthodes de fabrique sur `SKFieldNode` pour créer des champs physique différent. Vous pouvez créer un champ ressort en appelant `SKFieldNode.CreateSpringField()`, un champ de gravité radial en appelant `SKFieldNode.CreateRadialGravityField()`, et ainsi de suite.

`SKFieldNode` possède également des propriétés pour contrôler les attributs de champ tels que le niveau de champ, la région de champ et l’atténuation des forces de champ.

## <a name="spring-field"></a>Champ de ressort

Par exemple, le code suivant crée un champ ressort et l’ajoute à la scène :

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Vous pouvez ensuite ajouter sprites et définissez leurs `PhysicsBody` propriétés afin que le champ physique affectera les sprites, comme le code suivant n’est effectuée lorsque l’utilisateur touche l’écran :

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

Cela oblige la banane font osciller comme un ressort autour du nœud de champ suivantes :

![](spritekit-images/image15.png "La banane font osciller comme un ressort autour du nœud de champ")
 
## <a name="radial-gravity-field"></a>Champ de gravité radiale

Ajout d’un autre champ est similaire. Par exemple, le code suivant crée un champ de gravité radial :

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Il en résulte dans un autre champ de force, où la banane est extraits radiale sur le champ :

![](spritekit-images/image16.png "La banane est extraits radiale autour du champ")