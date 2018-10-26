---
title: Utilisation de contrôles personnalisés avec le concepteur iOS
description: Ce document décrit comment créer un contrôle personnalisé et l’utiliser avec le Concepteur de Xamarin pour iOS. Il montre comment rendre le contrôle disponible dans la boîte à outils du concepteur iOS, implémenter le contrôle afin qu’elle s’affiche correctement et concevoir le temps et bien plus encore.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: d24d62bf190a2cb078bf82d7a8676f302e740762
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115586"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Utilisation de contrôles personnalisés avec le concepteur iOS

## <a name="requirements"></a>Configuration requise

Le Concepteur de Xamarin pour iOS est disponible dans Visual Studio pour Mac et Visual Studio 2015 et 2017 sur Windows.

Ce guide suppose que vous connaissez le contenu couvert dans le [guides de mise en route](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Procédure pas à pas

> [!IMPORTANT]
> À compter de Xamarin.Studio 5.5, celle dans laquelle les contrôles personnalisés sont créés est légèrement différente à des versions antérieures. Pour créer un contrôle personnalisé, soit le `IComponent` interface est requise (avec les méthodes d’implémentation associée) ou de la classe peut être annotée avec `[DesignTimeVisible(true)]`. Cette dernière méthode est utilisée dans l’exemple suivant de procédure pas à pas.


1. Créer une solution à partir de la **iOS > application > Application avec affichage unique > C#** modèle, nommez-le `ScratchTicket`et suivez les instructions de l’Assistant Nouveau projet :

    [![](ios-designable-controls-walkthrough-images/01new.png "Créer une nouvelle solution")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Créer un nouveau fichier de classe vide nommé `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Créez une classe ScratchTicketView")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Ajoutez le code suivant pour `ScratchTicketView` classe :

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. Ajouter le `FillTexture.png`, `FillTexture2.png` et `Monkey.png` fichiers (disponible [à partir de GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) pour le **ressources** dossier.
    
1. Double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur :

    [![](ios-designable-controls-walkthrough-images/03new.png "Le concepteur iOS")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Glisser-déplacer un **affichage de l’Image** à partir de la **boîte à outils** sur la vue dans le storyboard.

    [![](ios-designable-controls-walkthrough-images/04new.png "Affichage d’une Image est ajoutée à la disposition")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Sélectionnez le **affichage de l’Image** et modifier son **Image** propriété `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Définition de propriété de l’Image d’affichage Image sur Monkey.png")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Comme nous utilisons des classes de taille, nous devrons limiter cette vue de l’image. Cliquez sur l’image à deux reprises pour placer en mode de contrainte. Nous allons limiter au centre en cliquant sur le handle de l’épinglage de centre et l’aligner verticalement et horizontalement :

    [![](ios-designable-controls-walkthrough-images/06new.png "Centrer l’image")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Pour limiter la hauteur et la largeur, cliquez sur les poignées de l’épinglage de taille (les poignées de « OS » en forme), puis sélectionnez la largeur et la hauteur de respectivement :

    [![](ios-designable-controls-walkthrough-images/07new.png "Ajout de contraintes")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Mettre à jour l’image en fonction de contraintes en cliquant sur le bouton de mise à jour dans la barre d’outils :

    [![](ios-designable-controls-walkthrough-images/08new.png "La barre d’outils de contraintes")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Ensuite, générez le projet afin que le **Scratch la vue de Ticket** apparaîtront sous **des composants personnalisés** dans la boîte à outils :

    [![](ios-designable-controls-walkthrough-images/09new.png "La boîte à outils des composants personnalisés")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Faites glisser et déposez un **Scratch la vue de Ticket** afin qu’il apparaisse sur l’image monkey. Ajuster les poignées de déplacement pour la vue de Ticket Scratch couvre le monkey complètement, comme indiqué ci-dessous :

    [![](ios-designable-controls-walkthrough-images/10new.png "Une vue de Ticket temporaire sur la vue de l’Image")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Limiter l’affichage de Ticket Scratch pour l’affichage de l’Image en dessinant un rectangle englobant pour sélectionner les deux vues. Sélectionnez les options pour limiter aux images de la largeur, hauteur, Center et intermédiaire et mise à jour en fonction de contraintes, comme indiqué ci-dessous :

    [![](ios-designable-controls-walkthrough-images/11new.png "Centrage et ajout de contraintes")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Exécutez l’application et « à gratter » l’image pour révéler le monkey.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Exécution d’une application exemple")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Ajout de propriétés au moment du Design

Le concepteur inclut également la prise en charge au moment du design pour les contrôles personnalisés de numérique de type de propriété, énumération, chaîne, bool, CGSize, UIColor, UIImage. Pour démontrer, nous allons ajouter une propriété à la `ScratchTicketView` pour définir l’image est « rayé désactivé. »

Ajoutez le code suivant à la `ScratchTicketView` classe pour la propriété :

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Nous pouvons également choisir d’ajouter une vérification de valeur null pour le `Draw` (méthode), comme suit :

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Y compris un `ExportAttribute` et un `BrowsableAttribute` avec l’argument défini `true` entraîne la propriété affichée dans le concepteur **propriété** Panneau de configuration. Modification de la propriété à une autre image incluse avec le projet, tel que `FillTexture2.png`, entraîne la mise à jour du contrôle au moment du design, comme indiqué ci-dessous :

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Modification des propriétés au moment du Design")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Récapitulatif

Dans cet article nous a expliqué comment créer un contrôle personnalisé, mais aussi les utiliser dans une application iOS à l’aide du concepteur iOS. Nous avons vu comment créer et générer le contrôle pour le rendre disponible pour une application dans le concepteur **boîte à outils**. En outre, nous avons vu comment implémenter le contrôle telle qu’elle s’affiche correctement au moment du design et de runtime, ainsi que comment exposer les propriétés de contrôle personnalisé dans le concepteur.



## <a name="related-links"></a>Liens associés

- [ScratchTicket (exemple)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [images nécessaires (exemple)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
