---
title: À l’aide de graphiques de base et l’Animation de la base de Xamarin.iOS
description: Cet article explique étape par étape comment créer une application qui utilise des graphiques de base et l’Animation de base. Il montre comment dessiner sur l’écran en réponse à une touche de l’utilisateur, ainsi que comment animer une image à parcourir le long d’un chemin d’accès.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7a4399a5d62e2000c2a15a65da8e0e427dc039e0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787053"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>À l’aide de graphiques de base et l’Animation de la base de Xamarin.iOS

Pour cette procédure pas à pas, nous allons dessiner un tracé à l’aide de graphiques de base en réponse à l’entrée tactile. Ensuite, nous allons ajouter un `CALayer` contenant une image qui nous s’anime le long du tracé.

La capture d’écran suivante montre l’application terminée :

![](graphics-animation-walkthrough-images/00-final-app.png "L’application terminée")

Avant de commencer à télécharger le *GraphicsDemo* exemple qui accompagne ce guide. Il peut être téléchargé [ici](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) et se trouve dans le **GraphicsWalkthrough** répertoire lancer le projet nommé **GraphicsDemo_starter** en double-cliquant dessus, et Ouvrez la `DemoView` classe.

## <a name="drawing-a-path"></a>Dessiner un tracé


1. Dans `DemoView` ajouter un `CGPath` variable à la classe et l’instancier dans le constructeur. Également déclarer deux `CGPoint` variables, `initialPoint` et `latestPoint`, que nous allons utiliser pour capturer le point de contact à partir de laquelle nous construire le chemin d’accès :
    
    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;
    
        public DemoView ()
        {
            BackgroundColor = UIColor.White;
    
            path = new CGPath ();
        }
    }
    ```

2. Ajoutez le code suivant à l’aide de directives :

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Ensuite, substituez `TouchesBegan` et `TouchesMoved,` et ajoutez les implémentations suivantes pour capturer les respectivement le point de contact initial et chaque point de contact suivantes :

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){
    
        base.TouchesBegan (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }
    
    public override void TouchesMoved (NSSet touches, UIEvent evt){
    
        base.TouchesMoved (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` est appelée chaque fois touches finales, le déplacement dans l’ordre pour `Draw` à appeler pour le test suivant de la boucle d’exécution.

4. Nous allons ajouter des lignes pour le chemin d’accès dans la `Draw` méthode et l’utilisation d’un trait rouge en pointillés avec. [Implémentez `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) avec le code ci-dessous :

    ```csharp
    public override void Draw (CGRect rect){
    
        base.Draw (rect);
    
        if (!initialPoint.IsEmpty) {
    
            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){
                    
                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();
    
                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }
            
                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });
                                
                //add geometry to graphics context and draw it
                g.AddPath (path);       
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Si nous exécutons l’application maintenant, nous pouvons toucher pour dessiner sur l’écran, comme illustré dans la capture d’écran suivante :

![](graphics-animation-walkthrough-images/01-path.png "Sur l’écran de dessin")

## <a name="animating-along-a-path"></a>Animer le long d’un chemin d’accès

Maintenant que nous avons implémenté le code pour permettre aux utilisateurs de dessiner le chemin d’accès, vous allez ajouter le code pour animer une couche le long du tracé.

1. Tout d’abord, ajoutez un [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) variable à la classe et la créer dans le constructeur :

    ```csharp
    public class DemoView : UIView
        {
            …
    
            CALayer layer;
    
            public DemoView (){
                …
    
                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. Ensuite, nous allons ajouter la couche comme une sous-couche de couche de la vue lorsque l’utilisateur retire des leur doigt de l’écran. Ensuite, nous allons créer une animation d’image clé à l’aide du chemin d’accès, l’animation de la couche `Position`.

    Pour cela nous devons remplacer le `TouchesEnded` et ajoutez le code suivant :

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Exécutez l’application maintenant et après le dessin, une couche avec une image est ajoutée et se déplace le long du tracé dessiné :

![](graphics-animation-walkthrough-images/00-final-app.png "Une couche avec une image est ajoutée et qu’il se déplace le long du tracé dessiné")

## <a name="summary"></a>Récapitulatif

Dans cet article, nous a présenté un exemple qui reliées entre elles des concepts de graphiques et l’animation. Tout d’abord, nous vous avons montré comment utiliser des graphiques de base pour dessiner un tracé dans un `UIView` en réponse à une touche de l’utilisateur. Puis nous a montré comment utiliser l’Animation de base pour créer une image de déplacement le long de ce chemin d’accès.


## <a name="related-links"></a>Liens associés

- [Animation de base](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Graphismes de base](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Recettes d’Animation de base](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
