---
title: "Procédure pas à pas - à l’aide de contrôles personnalisés avec le Concepteur de Xamarin pour iOS"
description: "Cet article fournit une procédure pas à pas montrant comment créer un contrôle personnalisé et l’utiliser dans le Concepteur de Xamarin pour iOS. Il montre comment créer un contrôle disponible dans la boîte à outils du concepteur afin qu’il peut être glisser/déposé sur une vue. En outre, il montre comment implémenter un contrôle afin qu’il restitue correctement au moment de la conception et d’exécution, ainsi que comment créer des propriétés qui peuvent être définies au moment du design."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 3e12e33b254e58759043ed838d5c34dd7bd024fd
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="walkthrough---using-custom-controls-with-the-xamarin-designer-for-ios"></a>Procédure pas à pas - à l’aide de contrôles personnalisés avec le Concepteur de Xamarin pour iOS

_Cet article fournit une procédure pas à pas montrant comment créer un contrôle personnalisé et l’utiliser dans le Concepteur de Xamarin pour iOS. Il montre comment créer un contrôle disponible dans la boîte à outils du concepteur afin qu’il peut être glisser/déposé sur une vue. En outre, il montre comment implémenter un contrôle afin qu’il restitue correctement au moment de la conception et d’exécution, ainsi que comment créer des propriétés qui peuvent être définies au moment du design._

## <a name="requirements"></a>Configuration requise

Le Concepteur de Xamarin pour iOS est disponible dans Visual Studio pour Mac et Visual Studio 2015 et 2017 sur Windows.

Ce guide suppose que vous connaissez le contenu traitées dans le [Getting Started Guide](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Procédure pas à pas

> [!IMPORTANT]
> À partir de Xamarin.Studio 5.5, la création des contrôles personnalisés de façon est légèrement différente pour les versions antérieures. Pour créer un contrôle personnalisé, soit le `IComponent` interface est requise (avec les méthodes d’implémentation associée) ou de la classe peut être annotée avec `[DesignTimeVisible(true)]`. La seconde méthode est utilisée dans l’exemple suivant de procédure pas à pas.


1. Créez une solution à partir de la **iOS > application > Application vue unique > C#** modèle, nommez-le `ScratchTicket`et suivez les instructions de l’Assistant Nouveau projet :


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


1. Glisser-déplacer un **affichage Image** à partir de la **boîte à outils** sur la vue dans le plan conceptuel.

    [![](ios-designable-controls-walkthrough-images/04new.png "Affichage d’une Image est ajoutée à la mise en page")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Sélectionnez le **affichage Image** et modifiez son **Image** propriété `Monkey.png`.

    [! [] (ios-possible-contrôles-walkthrough-images/05new.png « propriété d’Image d’affichage du paramètre Image à Monkey.png)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Nous utilisons des classes de taille nous devrons limiter cette vue de l’image. Cliquez sur l’image à deux reprises pour placer en mode de contrainte. Nous allons limiter au centre en cliquant sur le handle de l’épinglage de centre et l’aligner verticalement et horizontalement :

    [![](ios-designable-controls-walkthrough-images/06new.png "Centrer l’image")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Pour limiter la hauteur et la largeur, cliquez sur les poignées de l’épinglage de taille (les poignées 'segment' en forme), puis sélectionnez les largeur et hauteur respectivement :

    [![](ios-designable-controls-walkthrough-images/07new.png "Ajout de contraintes")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Mettre à jour le frame en fonction de contraintes en cliquant sur le bouton de mise à jour dans la barre d’outils :

    [![](ios-designable-controls-walkthrough-images/08new.png "La barre d’outils de contraintes")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Ensuite, générez le projet afin que les **vue de Ticket Scratch** s’affiche sous **des composants personnalisés** dans la boîte à outils :

    [![](ios-designable-controls-walkthrough-images/09new.png "La boîte à outils des composants personnalisés")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Faites glisser et déposez un **vue de Ticket Scratch** afin qu’il apparaisse sur l’image singe. Ajuster les poignées pour la vue de Ticket Scratch couvre le singe complètement, comme indiqué ci-dessous :

    [![](ios-designable-controls-walkthrough-images/10new.png "Une vue de Ticket de travail sur la vue de l’Image")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Limiter l’affichage de Ticket de travail à l’affichage de l’Image en dessinant un rectangle englobant pour sélectionner les deux vues. Sélectionnez les options pour limiter aux images largeur, hauteur, centre et au milieu et mise à jour en fonction de contraintes, comme indiqué ci-dessous :

    [![](ios-designable-controls-walkthrough-images/11new.png "Centrage et ajout de contraintes")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Exécutez l’application et « scratch off » l’image pour faire apparaître le singe.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Un exemple d’application de s’exécuter")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Ajout de propriétés au moment du Design

Le concepteur inclut également la prise en charge au moment du design pour les contrôles personnalisés de numérique de type de propriété, énumération, string, bool, CGSize, UIColor et UIImage. Pour illustrer, nous allons ajouter une propriété à la `ScratchTicketView` pour définir l’image est « rayé désactivé. »

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

Nous pouvons également ajouter un contrôle de valeur null pour le `Draw` (méthode), comme suit :

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

Y compris une `ExportAttribute` et un `BrowsableAttribute` avec l’argument défini `true` entraîne la propriété qui est affichée dans le concepteur **propriété** Panneau de configuration. Modification de la propriété à une autre image incluse avec le projet, tel que `FillTexture2.png`, entraîne la mise à jour du contrôle au moment du design, comme indiqué ci-dessous :

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Modification des propriétés au moment du Design")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Récapitulatif

Dans cet article nous décrit comment créer un contrôle personnalisé, ainsi que de les utiliser dans une application iOS à l’aide du concepteur iOS. Nous avons vu comment créer et générer le contrôle pour le rendre disponible pour une application dans le concepteur **boîte à outils**. En outre, nous avons examiné comment implémenter le contrôle telle qu’elle s’affiche correctement au moment de la conception et d’exécution, ainsi que comment exposer les propriétés de contrôle personnalisé dans le concepteur.



## <a name="related-links"></a>Liens associés

- [ScratchTicket (exemple)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [images requises (exemple)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
