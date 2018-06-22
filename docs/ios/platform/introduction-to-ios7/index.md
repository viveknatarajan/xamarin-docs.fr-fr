---
title: Introduction à iOS 7
description: Cet article décrit les nouvelles API principales introduites dans iOS 7, y compris les transitions de contrôleur d’affichage, les améliorations apportées aux animations UIView, UIKit Dynamics et Kit de texte. Elle traite également les modifications apportées à l’interface utilisateur et les nouvelles fonctionnalités de multitâche amélioré.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9ae82eba78f099f675d21bf53a250923630a0ff6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30780011"
---
# <a name="introduction-to-ios-7"></a>Introduction à iOS 7

_Cet article décrit les nouvelles API principales introduites dans iOS 7, y compris les transitions de contrôleur d’affichage, les améliorations apportées aux animations UIView, UIKit Dynamics et Kit de texte. Elle traite également les modifications apportées à l’interface utilisateur et les nouvelles fonctionnalités de multitâche amélioré._

iOS 7 est une mise à jour majeure pour iOS. Il présente une toute nouvelle conception d’interface utilisateur qui met le focus sur le contenu au lieu d’application chrome. En même temps que l’élément visuel change, iOS 7 ajoute une multitude de nouvelles API pour créer des expériences et des interactions plus riches. Ce document d’enquêtes les nouvelles technologies introduites avec iOS 7 et sert de point de départ pour une exploration plus approfondie.

## <a name="uiview-animation-enhancements"></a>Améliorations d’Animation UIView

iOS 7 étend la prise en charge de l’animation dans UIKit, permettant aux applications d’effectuer les opérations qui nécessitaient auparavant des suppression directement dans le cadre de l’Animation de base. Par exemple, `UIView` pouvez à présent effectuer animations à effet ressort ainsi que les animations d’image clé, qui auparavant un `CAKeyframeAnimation` appliqué à un `CALayer`.

### <a name="spring-animations"></a>Animations à effet ressort

 `UIView` prend désormais en charge les modifications de propriété animation avec un effet ressort. Pour ce faire, appelez le `AnimateNotify` ou `AnimateNotifyAsync` méthode, en passant les valeurs pour le facteur d’amortissement ressort et la rapidité initiale ressort, comme décrit ci-dessous :

-  `springWithDampingRatio` – Une valeur comprise entre 0 et 1, où oscillation augmente plus petite valeur.
-  `initialSpringVelocity` – La rapidité initiale ressort sous forme de pourcentage de la distance de total de l’animation par seconde.


Le code suivant produit un effet ressort lorsque change de centre de la vue de l’image :

```csharp
void AnimateWithSpring ()
{ 
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;
    
    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {
    
        imageView.Center = new CGPoint (imageView.Center.X, 400);   
            
    }, null);
}
```

Cet effet ressort provoque l’affichage de l’image semble rebondir quand il termine son animation vers un nouvel emplacement de centre, comme illustré ci-dessous :

 ![](images/spring-animation.png "Cet effet ressort entraîne l’affichage de l’image semble rebondir quand il termine son animation vers un nouvel emplacement du centre")

### <a name="keyframe-animations"></a>Animations d’image clé

Le `UIView` classe inclut désormais la `AnimateWithKeyframes` méthode pour créer des animations d’image clé sur un `UIView`. Cette méthode est similaire à d’autres `UIView` méthodes d’animation, sauf que supplémentaire `NSAction` est passé en tant que paramètre pour inclure les images clés. Dans le `NSAction`, des images clés sont ajoutés en appelant `UIView.AddKeyframeWithRelativeStartTime`.

Par exemple, l’extrait de code suivant crée une animation d’image clé pour animer centre d’un affichage, ainsi que pour faire pivoter la vue :

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => { 
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => { 
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

Les deux premiers paramètres pour le `AddKeyframeWithRelativeStartTime` méthode spécifier l’heure de début et la durée de l’image clé, respectivement, sous forme de pourcentage de la longueur totale de l’animation. L’exemple ci-dessus entraîne l’image vue animation à son nouveau centre sur la seconde en premier, suivie de rotation de 90 degrés sur la prochaine seconde. Étant donné que l’animation spécifie `UIViewKeyframeAnimationOptions.Autoreverse` en option, les deux images clés animer dans l’ordre inverse également. Enfin, les valeurs finales sont définies à l’état initial dans le Gestionnaire d’achèvement.

Les captures d’écran ci-dessous illustre l’animation combinée via les images clés :

 ![](images/keyframes.png "Cette captures d’écran illustre l’animation combinée via les images clés")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics est un nouvel ensemble d’API UIKit qui permettent aux applications de créer des interactions animées selon physique. UIKit Dynamics encapsule un moteur 2D physique pour que cela soit possible.

L’API est déclaratif par nature. Vous déclarez le comportement des interactions physique en créant des objets - appelés *comportements* - concepts d’express physique telles que la gravité, collisions, springs, etc. Vous joindre le behavior(s) à un autre objet, appelé un *animation dynamique*, qui encapsule une vue. L’animation dynamique prend soucis d’appliquer les comportements physique déclaré à *éléments dynamiques* -qui implémentent les éléments `IUIDynamicItem`, comme un `UIView`.

Il existe plusieurs primitives des comportements différents disponibles pour déclencher des interactions complexes, notamment :

-  `UIAttachmentBehavior` – Joint deux éléments dynamiques tels qu’ils se déplacent ensemble, ou attache un élément dynamique à un point de la pièce jointe.
-  `UICollisionBehavior` – Permet d’éléments dynamiques à participer à des collisions.
-  `UIDynamicItemBehavior` : Indique un ensemble général de propriétés à appliquer à des éléments dynamiques tels que l’élasticité, densité et friction.
-  `UIGravityBehavior` -Applique gravité à un élément dynamique, à l’origine des éléments accélérer les dans la direction gravitationnelle.
-  `UIPushBehavior` – Applique les force à un élément dynamique.
-  `UISnapBehavior` : Permet à un élément dynamique de s’aligner sur une position avec un effet ressort.


Bien qu’il existe plusieurs primitives, le processus général pour l’ajout des interactions basées sur physique à une vue à l’aide de UIKit Dynamics est cohérent entre les comportements :

1.  Créer une animation dynamique.
1.  Créer behavior(s).
1.  Ajouter des comportements à l’animateur dynamique.


### <a name="dynamics-example"></a>Exemple de Dynamics

Examinons un exemple qui ajoute la gravité et une limite de collision pour un `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Ajout de gravité à un affichage de l’image suit les 3 étapes décrites ci-dessus.

Nous allons utiliser le `ViewDidLoad` méthode pour cet exemple. Tout d’abord, ajoutez un `UIImageView` de l’instance comme suit :

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Cela crée une vue de l’image centrée sur le bord supérieur de l’écran. Pour créer une image « automne » avec la gravité, créer une instance d’un `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

Le `UIDynamicAnimator` prend une instance d’une référence `UIView` ou `UICollectionViewLayout`, qui contient les éléments qui seront animées par le behavior(s) attaché.

Ensuite, créez un `UIGravityBehavior` instance. Vous pouvez passer un ou plusieurs objets implémentant le `IUIDynamicItem`, comme un `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Le comportement est passée à un tableau de `IUIDynamicItem`, lequel contient dans ce cas le seul `UIImageView` instance nous examinons l’animation.

Enfin, ajoutez le comportement à l’animateur dynamique :

```csharp
dynAnimator.AddBehavior (gravity);
```

Cela entraîne l’image d’animation vers le bas avec la gravité, comme illustré ci-dessous :

![](images/gravity2.png "L’emplacement de départ image") 
![](images/gravity3.png "l’emplacement d’image fin")

Étant donné que rien en limitant les limites de l’écran, l’affichage de l’image se situe simplement la partie inférieure. Pour limiter l’affichage afin que l’image est en conflit avec les bords de l’écran, nous pouvons ajouter un `UICollisionBehavior`. Nous aborderons cela dans la section suivante.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Nous allons commencer en créant un `UICollisionBehavior` et son ajout à la dynamique d’animation, comme nous l’avons fait le `UIGravityBehavior`.

Modifiez le code pour inclure le `UICollisionBehavior`:

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

Le `UICollisionBehavior` a une propriété appelée `TranslatesReferenceBoundsIntoBoundry`. Définir cette valeur sur `true` provoque des limites de l’affichage à utiliser comme limite de collision de la référence.

Maintenant, lorsque l’image réalise une animation vers le bas avec la gravité, elle rebondit légèrement le bas de l’écran avant de s’installer pour rest il.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Nous pouvons contrôler davantage le comportement de l’affichage d’images chute des comportements supplémentaires. Par exemple, nous pouvons ajouter un `UIDynamicItemBehavior` pour augmenter l’élasticité, à l’origine de la vue de l’image à plusieurs rebondir lorsqu’elle est en conflit avec le bas de l’écran.

Ajout d’un `UIDynamicItemBehavior` suit les mêmes étapes comme avec les autres comportements. Tout d’abord créer le comportement :

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Ensuite, ajoutez le comportement à l’animation dynamique :

 `dynAnimator.AddBehavior (dynBehavior);`

Avec ce comportement en place, l’affichage de l’image rebondit plus lorsqu’il est en conflit avec la limite.

## <a name="general-user-interface-changes"></a>Modifications généraux de l’Interface utilisateur

Outre les nouvelles APIs UIKit tels que UIKit Dynamics, les transitions de contrôleur et les animations de UIView améliorées décrites ci-dessus, iOS 7 introduit des modifications visuelles à l’interface utilisateur et modifications connexes de l’API pour les différentes vues et contrôles. Pour plus d’informations, consultez la [iOS 7 présentation de l’Interface utilisateur](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Kit de texte

Kit de texte est une nouvelle API qui offre des fonctionnalités de mise en page et le rendu de texte puissant. Il repose sur le framework du texte de la base de bas niveau, mais il est beaucoup plus facile à utiliser que le texte de base.

Pour plus d’informations, consultez notre [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Multitâche

iOS 7 change quand et comment le travail d’arrière-plan est effectuée. Fin de la tâche dans iOS 7 ne sont plus les applications restent éveillés lorsque les tâches sont en cours d’exécution en arrière-plan, et les applications sont réveillées pour le traitement d’une manière non contiguës en arrière-plan. iOS 7 ajoute également trois nouvelles API de mise à jour des applications avec le nouveau contenu en arrière-plan :

-  Extraction de l’arrière-plan – autorise les applications à mettre à jour le contenu en arrière-plan à intervalles réguliers.
-  Notifications à distance : permet aux applications de mettre à jour de contenu lors de la réception d’une notification push. Les notifications peuvent être en mode silencieux ou peut afficher une bannière dans l’écran de verrouillage.
-  Service de transfert en arrière-plan – permet de chargement et téléchargement de données, telles que des fichiers volumineux, sans une limite de temps fixe.


Pour plus d’informations sur les nouvelles fonctionnalités multitâches, consultez les sections d’iOS de le Xamarin [Backgrounding guide](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Récapitulatif

Cet article couvre plusieurs ajouts majeures pour iOS. Tout d’abord, il montre comment ajouter des transitions personnalisées pour les contrôleurs de la vue. Ensuite, il montre comment utiliser les transitions dans les vues de collection, à la fois à partir d’un contrôleur de navigation, ainsi qu’interactivement entre les vues de collection. Ensuite, il présente plusieurs améliorations apportées aux animations UIView, montrant comment les applications utilisent UIKit pour les éléments qui nécessitaient auparavant directement sur l’Animation de base. Enfin, nouvelle API de Dynamics UIKit, qui offre un moteur physique à UIKit, est introduite en même temps que la prise en charge du texte enrichi désormais disponible dans le cadre du Kit de texte.

## <a name="related-links"></a>Liens associés

- [Introduction à iOS 7 (exemple)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Vue d'ensemble de l'interface utilisateur iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
