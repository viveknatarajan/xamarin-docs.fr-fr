---
title: Animation de base
description: Cet article examine le framework d’Animation de base, montrant comment elle permet de hautes performances, des animations fluides dans UIKit, et la manière de le pour utiliser directement pour le contrôle de l’animation de niveau inférieur.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b89b93a90e13072e36140b79081b90dcde1cdb05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="core-animation"></a>Animation de base

_Cet article examine le framework d’Animation de base, montrant comment elle permet de hautes performances, des animations fluides dans UIKit, et la manière de le pour utiliser directement pour le contrôle de l’animation de niveau inférieur._

e/s inclut [ *Core Animation* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) vues dans votre application pour prendre en charge de l’animation.
Toutes les animations et régulier dans iOS, tels que des tables de défilement et le glissement entre différentes vues effectuer également comme ils le font, car ils dépendent Core Animation en interne.

Les infrastructures Core Animation et les graphiques de base peuvent collaborer pour créer de belles, des graphiques 2D animés. En fait Core Animation peut même transformer graphiques 2D dans un espace 3D, créer des expériences incroyables, cinématiques. Toutefois, pour créer des graphiques 3D trues, vous devez utiliser quelque chose comme OpenGL ES, ou pour activer les jeux pour une API telle que MonoGame, bien que 3D dépasse le cadre de cet article.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Animation de base

iOS utilise le framework d’Animation de base pour créer des effets d’animation tels que transition entre les vues, glissante menus et le défilement effets citer que quelques-uns. Il existe deux façons de travailler avec animation :

- [Via UIKit](#Using_UIKit_Animation), qui inclut des animations basées sur la vue, ainsi que des transitions animées entre les contrôleurs.
- [Via l’Animation de noyaux](#Using_Core_Animation), les couches directement, permettant ainsi plus précise de contrôle.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>À l’aide de l’Animation UIKit

UIKit fournit plusieurs fonctionnalités qui facilitent l’ajouter une animation à une application. Bien qu’il utilise en interne une Animation de Core, elle élimine il afin de travailler uniquement avec les vues et contrôleurs.

Cette section présente les fonctionnalités d’animation UIKit, notamment :

-  Transitions entre les contrôleurs
-  Transitions entre les vues
-  Afficher des animations de propriété


### <a name="view-controller-transitions"></a>Afficher des Transitions de contrôleur

 `UIViewController` Fournit la prise en charge intégrée pour la transition entre les contrôleurs de vue via la `PresentViewController` (méthode). Lorsque vous utilisez `PresentViewController`, la transition vers le deuxième contrôleur peut éventuellement être animée.

Par exemple, considérez une application avec les deux contrôleurs, où la toucher un bouton dans le premier contrôleur appelle `PresentViewController` pour afficher un second contrôleur. Pour contrôler les animations de transition sont utilisée pour afficher le deuxième contrôleur, il suffit de définir son [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) propriété comme indiqué ci-dessous :

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

Dans ce cas un `PartialCurl` animation est utilisée, bien que plusieurs autres sont disponibles, notamment :

-  `CoverVertical` – Diapositives à partir du bas de l’écran
-  `CrossDissolve` – L’ancienne vue fondu & la nouvelle vue en fondu
-  `FlipHorizontal` -Retourner l’horizontal droite à gauche. Sur la charge la transition inverse de gauche à droite.


Pour animer la transition, passer `true` comme deuxième argument de `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

La capture d’écran suivante montre à quoi ressemble la transition pour le `PartialCurl` cas :

 ![](core-animation-images/06-view-transitions.png "Cette capture d’écran montre la transition PartialCurl")

### <a name="view-transitions"></a>Afficher des Transitions

En plus des transitions entre les contrôleurs, UIKit prend également en charge des transitions entre les vues pour le remplacement d’une vue pour une autre animation.

Par exemple, imaginons que vous ayez un contrôleur avec `UIImageView`, où appuyé sur l’image doit afficher une seconde `UIImageView`. Pour animer l’image super-vue de la vue pour effectuer la transition vers la seconde vue de l’image est aussi simple que d’appeler `UIView.Transition`, en lui passant le `toView` et `fromView` comme indiqué ci-dessous :

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` prend également un `duration` paramètre qui contrôle la durée pendant laquelle l’animation s’exécute, ainsi que [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) pour spécifier des éléments tels que l’animation à l’utilisation et la fonction d’accélération. En outre, vous pouvez spécifier un gestionnaire d’achèvement qui sera appelé lors de l’animation terminée.

La capture d’écran ci-dessous montrent la transition animée entre l’image de vues lorsque `TransitionFlipFromTop` est utilisé :

 ![](core-animation-images/07-animated-transition.png "Cette capture d’écran montre la transition animée entre les vues de l’image lorsque TransitionFlipFromTop est utilisé.")

### <a name="view-property-animations"></a>Afficher les Animations de propriété

UIKit prend en charge l’animation des propriétés sur la `UIView` classe gratuitement, y compris :

-  Frame
-  Limites
-  Center
-  Alpha
-  Transformer
-  Color


Ces animations se produisent implicitement en spécifiant les modifications de propriété dans un `NSAction` délégué transmis à la méthode statique `UIView.Animate` (méthode). Par exemple, le code suivant réalise une animation le point central d’un `UIImageView`:

```csharp
pt = imgView.Center;

UIView.Animate (
    duration: 2, 
    delay: 0, 
    options: UIViewAnimationOptions.CurveEaseInOut | 
        UIViewAnimationOptions.Autoreverse,
    animation: () => {
        imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
            - imgView.Frame.Width / 2, pt.Y);},
    completion: () => {
        imgView.Center = pt; }
);
```

Cela entraîne une image animation en arrière dans la partie supérieure de l’écran, comme indiqué ci-dessous :

 ![](core-animation-images/08-animate-center.png "Une image animation dans les deux sens entre le haut de l’écran en tant que la sortie")

Comme avec la `Transition` méthode `Animate` permet la durée à définir, ainsi que la fonction d’accélération. Cet exemple montre comment utilisé également la `UIViewAnimationOptions.Autoreverse` option, ce qui entraîne l’animation à laquelle commencer l’animation à partir de la valeur en premier. Toutefois, le code définit également la `Center` à sa valeur initiale dans un gestionnaire d’achèvement. Pendant une animation est interpolation des valeurs de propriété au fil du temps, la valeur de modèle réelle de la propriété est toujours la valeur finale qui a été définie. Dans cet exemple, la valeur est un point de près du côté droit de la super-vue. Sans paramètre la `Center` pour le point de départ, c'est-à-dire où l’animation terminée en raison du `Autoreverse` qui est définie, l’image reviendrait au côté droit une fois l’animation terminée, comme indiqué ci-dessous :

 ![](core-animation-images/09-animation-complete.png "Sans définir le centre de sur le point de départ, l’image reviendrait au côté droit une fois l’animation terminée")

## <a name="using-core-animation"></a>À l’aide de l’Animation Core

 `UIView` les animations autorise une grande quantité de capacité et doivent être utilisées si possible en raison de la facilité d’implémentation. Comme mentionné précédemment, les animations de UIView utilisent le framework d’Animation de base. Toutefois, certains éléments ne peuvent pas être faits avec `UIView` animations, telles que l’animation des propriétés supplémentaires qui ne peut pas être animées avec une vue, ou à l’interpolation le long d’un chemin d’accès non linéaire. Dans ce cas où vous avez besoin d’un contrôle plus précis, l’Animation Core peut servir directement.

### <a name="layers"></a>Couches

Lorsque vous travaillez avec Core Animation, animation se produit via *couches*, qui sont de type `CALayer`. Une couche est conceptuellement semblable à une vue, car il beaucoup est une hiérarchie de la couche, comme c’est une hiérarchie de la vue. En fait, couches sauvegarder des vues, avec la vue de la prise en charge pour l’interaction utilisateur. Vous pouvez accéder à la couche de n’importe quelle vue via la vue `Layer` propriété. En fait, le contexte utilisé dans `Draw` méthode `UIView` réellement créé à partir de la couche. En interne, la couche de stockage un `UIView` a son délégué défini sur la vue elle-même, ce qui est ce qu’appelle `Draw`. C’est le cas lors du dessin à un `UIView`, vous dessinez réellement à sa couche.

Les animations de couche peuvent être implicite ou explicite. Animations implicites sont déclaratives. Vous déclarez simplement ce que doivent modifier les propriétés de la couche et l’animation fonctionne tout simplement. Animations explicites, quant à eux, sont créées via une classe d’animation qui est ajoutée à une couche. Animations explicites permettent le contrôle d’ajout sur la création d’une animation. Les sections suivantes approfondir implicites et explicites des animations plus en détail.

### <a name="implicit-animations"></a>Animations implicites

Une façon pour animer les propriétés d’une couche est via une animation implicite. `UIView` animations créer des animations implicites. Toutefois, vous pouvez créer des animations implicites directement sur une couche.

Par exemple, le code suivant définit la couche `Contents` à partir d’une image, définit la largeur de la bordure et la couleur, et l’ajoute en tant qu’une sous-couche de couche de la vue :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    layer = new CALayer ();
    layer.Bounds = new CGRect (0, 0, 50, 50);
    layer.Position = new CGPoint (50, 50);
    layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
    layer.ContentsGravity = CALayer.GravityResize;
    layer.BorderWidth = 1.5f;
    layer.BorderColor = UIColor.Green.CGColor;

    View.Layer.AddSublayer (layer);
}
```

Pour ajouter une animation implicite de la couche, encapsulez simplement les modifications de propriété dans un `CATransaction`. Cela permet d’animation de propriétés qui ne serait pas animer avec une animation de la vue, telles que la `BorderWidth` et `BorderColor` comme indiqué ci-dessous :

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    CATransaction.Begin ();
    CATransaction.AnimationDuration = 10;
    layer.Position = new CGPoint (50, 400);
    layer.BorderWidth = 5.0f;
    layer.BorderColor = UIColor.Red.CGColor;
    CATransaction.Commit ();
}
```

Ce code anime également de la couche `Position`, qui est l’emplacement du point d’ancrage de la couche mesuré à partir de l’angle supérieur gauche des coordonnées de la superlayer. Le point d’ancrage d’une couche est un point normalisé dans le système de coordonnées de la couche.

La figure suivante illustre le point d’ancrage et de position :

 ![](core-animation-images/10-postion-anchorpt.png "Cette figure illustre le point d’ancrage et de position")

Lorsque l’exemple est exécuté, le `Position`, `BorderWidth` et `BorderColor` animer comme indiqué dans les captures d’écran suivants :

 ![](core-animation-images/11-implicit-animation.png "Lorsque l’exemple est exécuté, la Position, BorderWidth et BorderColor animer comme")

### <a name="explicit-animations"></a>Animations explicites

En plus des animations implicites, l’Animation Core inclut une variété de classes qui héritent de `CAAnimation` qui vous permettent d’encapsuler des animations qui sont ensuite ajoutées explicitement à une couche. Permettent de contrôle plus précise des animations, telles que la modification de la valeur de début d’une animation, en regroupant les animations et en spécifiant des images clés pour autoriser les chemins d’accès non linéaire.

Le code suivant montre un exemple d’une animation explicite avec un `CAKeyframeAnimation` de la couche indiquée plus haut (dans la section Animation implicite) :

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);
    
    // get the initial value to start the animation from
    CGPoint fromPt = layer.Position;
    
    /* set the position to coincide with the final animation value
    to prevent it from snapping back to the starting position
    after the animation completes*/
    layer.Position = new CGPoint (200, 300);
    
    // create a path for the animation to follow
    CGPath path = new CGPath ();
    path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
    
    // create a keyframe animation for the position using the path
    CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
    animPosition.Path = path;
    animPosition.Duration = 2;
    
    // add the animation to the layer.
    /* the "position" key is used to overwrite the implicit animation created
    when the layer positino is set above*/
    layer.AddAnimation (animPosition, "position");
}
```

Ce code modifie la `Position` de la couche en créant un chemin d’accès qui est ensuite utilisé pour définir une animation d’image clé. Notez que la couche `Position` est définie sur la valeur finale de la `Position` à partir de l’animation. Sans cela, la couche brusquement retourne à son `Position` avant que l’animation, car l’animation modifie uniquement la valeur de la présentation et non la valeur de modèle réels. En définissant la valeur de modèle à la valeur finale de l’animation, la couche restent en place à la fin de l’animation.

Les captures d’écran suivantes illustrent la couche contenant l’animation d’image via le chemin d’accès spécifié :

 ![](core-animation-images/12-explicit-animation.png "Cette capture d’écran montre la couche contenant l’animation d’image via le chemin d’accès spécifié")
 
## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons étudié les fonctionnalités d’animation fournies via la *Core Animation* infrastructures. Nous avons examiné Animation Core, montrant comment il alimente les animations dans UIKit, et comment elle peut être utilisée directement pour le contrôle de l’animation de niveau inférieur.

## <a name="related-links"></a>Liens associés

- [Exemple d’Animation Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Graphismes de base](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Procédure pas à pas de l’Animation et de graphique](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Animation de base](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
