---
title: Core Animation dans Xamarin.iOS
description: Cet article examine l’infrastructure Core Animation, en montrant comment elle permet de hautes performances, des animations fluides dans UIKit, et la manière de le pour utiliser directement pour le contrôle de l’animation de niveau inférieur.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a40d0911b7dabc900a4c6e50c692e4f091f22be9
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233624"
---
# <a name="core-animation-in-xamarinios"></a>Core Animation dans Xamarin.iOS

_Cet article examine l’infrastructure Core Animation, en montrant comment elle permet de hautes performances, des animations fluides dans UIKit, et la manière de le pour utiliser directement pour le contrôle de l’animation de niveau inférieur._

iOS inclut [ *Core Animation* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) vues dans votre application pour prendre en charge de l’animation.
Toutes les animations et régulier dans iOS telles que le défilement des tables et le glissement entre différentes vues exécutent aussi bien qu’ils le font, car elles s’appuient sur Core Animation en interne.

Les frameworks Core Animation et Core Graphics peuvent collaborer pour créer de belles, des graphiques 2D animés. En fait Core Animation peut même transformer des graphismes 2D dans l’espace 3D, créer des expériences exceptionnelles, cinématiques. Toutefois, pour créer des graphiques 3D trues, vous devez utiliser quelque chose comme OpenGL ES, ou pour activer les jeux pour une API telle que MonoGame, bien que 3D est abordée dans cet article.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Core Animation

iOS utilise l’infrastructure Core Animation pour créer des effets d’animation tels que transition entre les vues, glissante menus et le défilement des effets pour citer que quelques-uns. Il existe deux façons de travailler avec l’animation :

- [Par le biais de UIKit](#Using_UIKit_Animation), qui inclut des animations basées sur la vue, ainsi que les transitions animées entre les contrôleurs.
- [Via les Core Animation](#Using_Core_Animation), quelles couches directement, ce qui permet de contrôle plus précis.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>À l’aide de l’Animation UIKit

UIKit fournit plusieurs fonctionnalités qui facilitent l’ajouter une animation à une application. Bien qu’il utilise en interne Core Animation, elle effectue l’abstraction il pour travailler uniquement avec les vues et contrôleurs.

Cette section traite des fonctionnalités d’animation de UIKit, y compris :

-  Transitions entre les contrôleurs
-  Transitions entre les vues
-  Afficher des animations de propriété


### <a name="view-controller-transitions"></a>Transitions du contrôleur d’affichage

 `UIViewController` Fournit la prise en charge intégrée pour la transition entre les contrôleurs d’affichage via le `PresentViewController` (méthode). Lorsque vous utilisez `PresentViewController`, la transition vers le deuxième contrôleur peut éventuellement être animée.

Par exemple, considérez une application avec les deux contrôleurs, où toucher un bouton dans le premier contrôleur appelle `PresentViewController` pour afficher un second contrôleur. Pour contrôler les animations de transition sont utilisée pour afficher le deuxième contrôleur, il suffit de définir son [ `ModalTransitionStyle` ](xref:UIKit.UIModalTransitionStyle) propriété comme indiqué ci-dessous :

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

Dans ce cas un `PartialCurl` animation est utilisée, bien que plusieurs autres sont disponibles, notamment :

-  `CoverVertical` – Diapositives haut en bas de l’écran
-  `CrossDissolve` – L’ancienne vue fondu & la nouvelle vue de fondu
-  `FlipHorizontal` -Retournement de horizontale droite à gauche. Sur licenciement la transition renverse gauche à droite.


Pour animer la transition, passer `true` comme deuxième argument à `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

La capture d’écran suivante montre à quoi ressemble la transition pour le `PartialCurl` cas :

 ![](core-animation-images/06-view-transitions.png "Cette capture d’écran montre la transition PartialCurl")

### <a name="view-transitions"></a>Afficher des Transitions

En plus des transitions entre les contrôleurs, UIKit prend également en charge les transitions d’animation entre les vues pour le remplacement d’une vue d’une autre.

Par exemple, imaginons que vous ayez un contrôleur avec `UIImageView`, où appuyant sur l’image doit afficher une seconde `UIImageView`. Pour animer l’image superview de la vue à passer à la deuxième vue image est aussi simple que l’appel `UIView.Transition`, en lui passant le `toView` et `fromView` comme indiqué ci-dessous :

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` prend également un `duration` paramètre qui contrôle la durée pendant laquelle l’animation s’exécute, ainsi que [ `options` ](xref:UIKit.UIViewAnimationOptions) pour spécifier des éléments tels que de l’animation à utiliser et la fonction d’accélération. En outre, vous pouvez spécifier un gestionnaire d’achèvement qui sera appelé lorsque l’animation est terminée.

La capture d’écran ci-dessous montrent la transition entre l’image animée vues lorsque `TransitionFlipFromTop` est utilisé :

 ![](core-animation-images/07-animated-transition.png "Cette capture d’écran montre la transition animée entre les vues de l’image lorsque TransitionFlipFromTop est utilisé.")

### <a name="view-property-animations"></a>Animations de propriété de vue

UIKit prend en charge l’animation des propriétés sur la `UIView` classe gratuitement, y compris :

-  Frame
-  Limites
-  Center
-  Alpha
-  Transformer
-  Color


Ces animations se produisent implicitement en spécifiant les modifications de propriété dans un `NSAction` délégué transmis à la méthode statique `UIView.Animate` (méthode). Par exemple, le code suivant anime le point central d’un `UIImageView`:

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

Il en résulte dans une image animée dans les deux sens dans la partie supérieure de l’écran, comme indiqué ci-dessous :

 ![](core-animation-images/08-animate-center.png "Une image animée dans les deux sens dans la partie supérieure de l’écran en tant que la sortie")

Comme avec la `Transition` (méthode), `Animate` permet la durée à définir, ainsi que la fonction d’accélération. Cet exemple, également utilisé le `UIViewAnimationOptions.Autoreverse` option, ce qui entraîne l’animation animer à partir de la valeur de retour au premier. Toutefois, le code affecte également le `Center` à sa valeur initiale dans un gestionnaire d’achèvement. Pendant une animation est interpolation des valeurs de propriété au fil du temps, la valeur de modèle réel de la propriété est toujours la valeur finale qui a été définie. Dans cet exemple, la valeur est un point situé près de la partie droite de la super-vue. Sans paramètre la `Center` au point initial, c'est-à-dire où l’animation est terminée en raison du `Autoreverse` qui est définie, l’image reviendrait au côté droit une fois l’animation terminée, comme indiqué ci-dessous :

 ![](core-animation-images/09-animation-complete.png "Sans définir le centre sur le point de départ, l’image reviendrait au côté droit une fois l’animation terminée")

## <a name="using-core-animation"></a>À l’aide de Core Animation

 `UIView` les animations permettent une grande capacité et doivent être utilisées si possible en raison de la facilité d’implémentation. Comme mentionné précédemment, les animations UIView utilisent le framework Core Animation. Toutefois, certaines choses ne peut pas être effectuées avec `UIView` animation, tels que l’animation de propriétés supplémentaires qui ne peut pas être animées qu’avec une vue ou interpolation le long d’un chemin d’accès non linéaire. Dans ce cas où vous avez besoin d’un contrôle plus précis, Core Animation permet aussi directement.

### <a name="layers"></a>Couches

Lorsque vous travaillez avec Core Animation, l’animation se fait via *couches*, qui sont de type `CALayer`. Une couche est conceptuellement semblable à une vue dans la mesure où il existe beaucoup une hiérarchie de la couche, comme il existe une hiérarchie d’affichage. En fait, les couches sauvegarder vues, avec la vue de l’ajout de prise en charge pour l’interaction utilisateur. Vous pouvez accéder à la couche de n’importe quelle vue par le biais de la vue `Layer` propriété. En fait, le contexte utilisé dans `Draw` méthode de `UIView` réellement créé à partir de la couche. En interne, la couche de stockage un `UIView` a son délégué défini sur la vue proprement dite, qui est ce qu’appelle `Draw`. C’est le cas lors du dessin à un `UIView`, que vous dessinez réellement à sa couche.

Animations de couche peuvent être implicite ou explicite. Les animations implicites sont déclaratives. Vous déclarez simplement ce que doivent modifier les propriétés de la couche et l’animation fonctionne tout simplement. Animations explicites, quant à eux, sont créées via une classe d’animation qui est ajoutée à une couche. Animations explicites permettent un contrôle plus sur la création d’une animation. Les sections suivantes se plonger dans les animations implicites et explicites de manière plus approfondie.

### <a name="implicit-animations"></a>Animations implicites

La première pour animer les propriétés d’une couche consiste via une animation implicite. `UIView` animations de créer des animations implicites. Toutefois, vous pouvez créer des animations implicites directement par rapport à une couche ainsi.

Par exemple, le code suivant définit la couche `Contents` à partir d’une image, définit la largeur de bordure et la couleur, et l’ajoute en tant que la sous-couche de couche de la vue :

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

Pour ajouter une animation implicite pour la couche, encapsulez simplement les modifications de propriété dans un `CATransaction`. Cela permet d’animer des propriétés qui ne serait pas animables avec une animation de la vue, telles que la `BorderWidth` et `BorderColor` comme indiqué ci-dessous :

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

Ce code anime également de la couche `Position`, qui est l’emplacement du point d’ancrage de la couche mesurée à partir de la partie supérieure gauche des coordonnées de la superlayer. Le point d’ancrage d’une couche est un point normalisé au sein du système de coordonnées de la couche.

La figure suivante illustre le point d’ancrage et de position :

 ![](core-animation-images/10-postion-anchorpt.png "Cette illustration montre le point d’ancrage et de position")

Lorsque l’exemple est exécuté, le `Position`, `BorderWidth` et `BorderColor` animer comme indiqué dans les captures d’écran suivante :

 ![](core-animation-images/11-implicit-animation.png "Lorsque l’exemple est exécuté, la Position, BorderWidth et BorderColor animer comme indiqué")

### <a name="explicit-animations"></a>Animations explicites

En plus des animations implicites, Core Animation inclut une variété de classes qui héritent de `CAAnimation` qui vous permettent d’encapsuler des animations qui sont ensuite explicitement ajoutées à une couche. Elles permettent de contrôle plus fin sur les animations, telles que la modification de la valeur de début d’une animation, en regroupant les animations et en spécifiant des images clés pour autoriser les chemins d’accès non linéaire.

Le code suivant montre un exemple d’une animation explicite avec un `CAKeyframeAnimation` pour la couche indiquée plus haut (dans la section Animation implicite) :

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

Ce code modifie la `Position` de la couche en créant un chemin d’accès qui est ensuite utilisé pour définir une animation d’image clé. Notez que la couche `Position` est défini sur la valeur finale de la `Position` à partir de l’animation. Sans cela, la couche brusquement retourne à son `Position` avant l’animation, car l’animation modifie uniquement la valeur de la présentation et non la valeur de modèle réel. En définissant la valeur de modèle à la valeur finale à partir de l’animation, la couche de demeurer en place à la fin de l’animation.

Les captures d’écran suivantes montrent la couche contenant l’animation d’image via le chemin d’accès spécifié :

 ![](core-animation-images/12-explicit-animation.png "Cette capture d’écran montre la couche contenant l’animation d’image via le chemin d’accès spécifié")
 
## <a name="summary"></a>Récapitulatif

Dans cet article nous avons étudié les fonctionnalités d’animation fournies le *Core Animation* frameworks. Nous avons examiné Core Animation, affichant comment il alimente les animations dans UIKit, et comment il peut être utilisé directement pour le contrôle de l’animation de niveau inférieur.

## <a name="related-links"></a>Liens associés

- [Exemple d’Animation Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Graphismes de base](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Graphismes et Animation procédure pas à pas](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Animation de base](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
