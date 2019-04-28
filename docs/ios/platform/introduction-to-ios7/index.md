---
title: Introduction à iOS 7
description: Cet article aborde les nouvelles API principales introduits dans iOS 7, y compris les transitions du contrôleur d’affichage, améliorations apportées aux animations UIView, UIKit Dynamics et le Kit de texte. Il couvre également les modifications apportées à l’interface utilisateur et les nouvelles fonctionnalités de multitâche amélioré.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: db2ce779962947e2121ff03280544a080e193e2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037291"
---
# <a name="introduction-to-ios-7"></a>Introduction à iOS 7

_Cet article aborde les nouvelles API principales introduits dans iOS 7, y compris les transitions du contrôleur d’affichage, améliorations apportées aux animations UIView, UIKit Dynamics et le Kit de texte. Il couvre également les modifications apportées à l’interface utilisateur et les nouvelles fonctionnalités de multitâche amélioré._

iOS 7 est une mise à jour majeure à iOS. Il présente une conception d’interface utilisateur entièrement nouvelle qui met le focus sur le contenu plutôt qu’application chrome. En même temps que des modifications visuelles, iOS 7 ajoute une multitude de nouvelles API pour créer des expériences et des interactions plus riches. Ce document d’enquêtes les nouvelles technologies introduites avec iOS 7 et sert de point de départ pour une exploration plus approfondie.

## <a name="uiview-animation-enhancements"></a>Améliorations d’Animation UIView

iOS 7 étend la prise en charge de l’animation dans UIKit, qui permet aux applications d’effectuer des opérations qui nécessitaient auparavant des suppression directement dans le framework Core Animation. Par exemple, `UIView` peuvent désormais effectuer des animations à effet ressort, ainsi que des animations d’image clé, qui précédemment un `CAKeyframeAnimation` appliqué à un `CALayer`.

### <a name="spring-animations"></a>Animations à effet ressort

 `UIView` prend désormais en charge les modifications de propriété animation avec un effet ressort. Pour ce faire, appelez le `AnimateNotify` ou `AnimateNotifyAsync` méthode, en passant les valeurs pour le facteur d’amortissement spring et la rapidité initiale spring, comme décrit ci-dessous :

-  `springWithDampingRatio` – Une valeur comprise entre 0 et 1, où l’oscillation augmente de plus petite valeur.
-  `initialSpringVelocity` – La rapidité initiale spring sous forme de pourcentage de la distance totale animation par seconde.


Le code suivant produit un effet ressort lorsque le centre de la vue de l’image change :

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

 ![](images/spring-animation.png "Cet effet ressort provoque l’affichage de l’image semble rebondir quand il termine son animation vers un nouvel emplacement de centre")

### <a name="keyframe-animations"></a>Animations de trame clé

Le `UIView` classe inclut désormais la `AnimateWithKeyframes` méthode pour créer des animations de trame clé sur un `UIView`. Cette méthode est similaire à d’autres `UIView` méthodes d’animation, sauf que supplémentaires `NSAction` est passé en tant que paramètre pour inclure les images clés. Dans le `NSAction`, des images clés sont ajoutés en appelant `UIView.AddKeyframeWithRelativeStartTime`.

Par exemple, l’extrait de code suivant crée une animation d’image clé pour animer center de l’affichage, ainsi que pour faire pivoter la vue :

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

Les deux premiers paramètres pour le `AddKeyframeWithRelativeStartTime` méthode spécifier l’heure de début et la durée de l’image clé, respectivement, sous forme de pourcentage de la longueur totale de l’animation. L’exemple ci-dessus entraîne l’image vue animer à son nouveau centre sur la première deuxième, suivie de la rotation de 90 degrés sur la prochaine seconde. Dans la mesure où l’animation spécifie `UIViewKeyframeAnimationOptions.Autoreverse` en tant qu’option, les deux images clés animer dans l’ordre inverse également. Enfin, les valeurs finales sont définies à l’état initial dans le Gestionnaire d’achèvement.

Les captures d’écran ci-dessous illustre l’animation combinée via les images clés :

 ![](images/keyframes.png "Cette captures d’écran illustre l’animation combinée via les images clés")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics est un nouvel ensemble d’API qui permettent aux applications de créer des interactions animées selon physique dans UIKit. UIKit Dynamics encapsule un moteur physique 2D pour rendre cela possible.

L’API est déclaratif par nature. Vous déclarez comment les interactions physique se comportent en créant des objets - appelés *comportements* - concepts de physique express telles que la gravité, collisions, springs, etc. Vous joindre le ses propres comportements à un autre objet, appelé un *animation dynamique*, qui encapsule une vue. L’animation dynamique prend des soucis de l’application les comportements physique déclaré à *éléments dynamiques* -qui implémentent les éléments `IUIDynamicItem`, comme un `UIView`.

Il existe plusieurs primitives des comportements différents disponibles pour déclencher des interactions complexes, y compris :

-  `UIAttachmentBehavior` – Joint deux éléments dynamiques tels qu’ils se déplacent ensemble, ou attache un élément dynamique à un point de la pièce jointe.
-  `UICollisionBehavior` – Permet d’éléments dynamiques à participer à des collisions.
-  `UIDynamicItemBehavior` : Spécifie un ensemble de propriétés à appliquer à des éléments dynamiques, telles que l’élasticité, la densité et friction général.
-  `UIGravityBehavior` -S’applique gravité à un élément dynamique, à l’origine des éléments accélérer dans la direction gravitationnelle.
-  `UIPushBehavior` – Applique les force à un élément dynamique.
-  `UISnapBehavior` – Permet d’un élément dynamique de s’aligner sur une position avec un effet ressort.


Bien qu’il existe plusieurs primitives, le processus général pour l’ajout des interactions basées sur le moteur physique à une vue à l’aide de UIKit Dynamics est cohérent entre les comportements :

1.  Créer une animation dynamique.
1.  Créer ses propres comportements.
1.  Ajouter des comportements à l’animation dynamique.


### <a name="dynamics-example"></a>Exemple de Dynamics

Examinons un exemple qui ajoute la gravité et une limite de collision pour un `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Ajout de gravité à un affichage d’image suit les 3 étapes présentées ci-dessus.

Nous allons utiliser le `ViewDidLoad` méthode pour cet exemple. Tout d’abord, ajoutez un `UIImageView` instance comme suit :

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Cette opération crée une vue de l’image centrée sur le bord supérieur de l’écran. Pour rendre une image « automne » avec la gravité, créer une instance d’un `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

Le `UIDynamicAnimator` prend une instance d’une référence `UIView` ou un `UICollectionViewLayout`, qui contient les éléments qui seront animées par le ses propres comportements attaché.

Ensuite, créez un `UIGravityBehavior` instance. Vous pouvez passer un ou plusieurs objets implémentant le `IUIDynamicItem`, comme un `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Le comportement est passée à un tableau de `IUIDynamicItem`, lequel contient dans ce cas le seul `UIImageView` instance nous sommes animer.

Enfin, ajoutez le comportement de l’animation dynamique :

```csharp
dynAnimator.AddBehavior (gravity);
```

Il en résulte dans l’image d’animation vers le bas avec la gravité, comme illustré ci-dessous :

![](images/gravity2.png "L’emplacement de départ image") 
![](images/gravity3.png "l’emplacement de l’image fin")

Dans la mesure où il n’a rien contraindre les limites de l’écran, l’affichage de l’image se situe simplement la partie inférieure. Pour limiter la vue afin que l’image est en conflit avec les bords de l’écran, nous pouvons ajouter un `UICollisionBehavior`. Nous le traiterons dans la section suivante.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Nous allons commencer en créant un `UICollisionBehavior` et en l’ajoutant à la dynamique d’animation, comme nous l’avons fait le `UIGravityBehavior`.

Modifier le code pour inclure le `UICollisionBehavior`:

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

Le `UICollisionBehavior` possède une propriété appelée `TranslatesReferenceBoundsIntoBoundry`. Définir cette valeur sur `true` provoque des limites de l’affichage à utiliser comme limite de collision de la référence.

Maintenant, lorsque l’image s’anime vers le bas avec la gravité, qu’elle ne rebondisse légèrement la partie inférieure de l’écran avant de choisir pour rest il.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Nous pouvons contrôler davantage le comportement de l’affichage d’image en baisse des comportements supplémentaires. Par exemple, nous pourrions ajouter un `UIDynamicItemBehavior` pour augmenter l’élasticité, à l’origine de la vue de l’image à plusieurs rebondir lorsqu’elle est en conflit avec le bas de l’écran.

Ajout d’un `UIDynamicItemBehavior` suit la même procédure comme avec les autres comportements. Commencez par créer le comportement :

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Ensuite, ajoutez le comportement de l’animation dynamique :

 `dynAnimator.AddBehavior (dynBehavior);`

Avec ce comportement en place, la vue de l’image plus rebondit lorsqu’il est en conflit avec la limite.

## <a name="general-user-interface-changes"></a>Modifications de généraux de l’Interface utilisateur

Outre les nouvelles APIs UIKit comme UIKit Dynamics, transitions du contrôleur et des animations UIView améliorées décrites ci-dessus, iOS 7 introduit des modifications visuelles à l’interface utilisateur et des modifications d’API associées pour différentes vues et les contrôles. Pour plus d’informations, consultez le [iOS 7 présentation de l’Interface utilisateur](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Kit de texte

Kit de texte est une nouvelle API qui offre des fonctionnalités de mise en page et rendu de texte puissant. Il s’appuie sur le framework de texte de Core de bas niveau, mais il est beaucoup plus facile à utiliser que le texte de base.

Pour plus d’informations, consultez notre [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Multitâche

iOS 7 change quand et comment le travail en arrière-plan est effectué. Fin de la tâche dans iOS 7 ne sont plus les applications restent éveillés lorsque les tâches s’exécutent en arrière-plan et applications sont réveillées pour le traitement de manière non contiguës en arrière-plan. iOS 7 ajoute également trois nouvelles API pour la mise à jour des applications avec le nouveau contenu en arrière-plan :

-  Récupération en arrière-plan – applications permet de mettre à jour le contenu en arrière-plan à intervalles réguliers.
-  Notifications à distance - permet aux applications de mettre à jour de contenu lors de la réception d’une notification push. Les notifications peuvent être soit en mode silencieux ou vous pouvez afficher une bannière sur l’écran de verrouillage.
-  Service de transfert en arrière-plan – permet de chargement et téléchargement de données, telles que des fichiers volumineux, sans une limite de temps fixe.


Pour plus d’informations sur les nouvelles fonctionnalités de multitâche, consultez les sections d’iOS de Xamarin [Backgrounding guide](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Récapitulatif

Cet article couvre plusieurs nouveaux ajouts majeures pour iOS. Tout d’abord, il montre comment ajouter des transitions personnalisées aux contrôleurs d’affichage. Ensuite, il montre comment utiliser des transitions dans les vues de collection, à la fois à partir d’un contrôleur de navigation, ainsi qu’interactivement entre les vues de collection. Ensuite, il présente plusieurs améliorations apportées aux animations UIView, montrant comment les applications utilisent UIKit pour les opérations qui nécessitaient auparavant des programmation directe sur Core Animation. Enfin, nouvelle API de Dynamics UIKit, qui offre un moteur physique à UIKit, est introduite en même temps que la prise en charge du texte enrichi désormais disponible dans le cadre du Kit de texte.

## <a name="related-links"></a>Liens associés

- [Introduction à iOS 7 (exemple)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Vue d'ensemble de l'interface utilisateur iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
