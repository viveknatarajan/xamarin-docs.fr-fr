---
title: 'Procédure pas à pas : Utilisation tactile dans Xamarin.iOS'
description: Ce document décrit comment gérer tactile dans les applications Xamarin.iOS, traitant des interactions tactiles d’exemple reconnaissances de mouvement et modules de reconnaissance de mouvement personnalisée.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fff49599d3843bb09d407316d6964ca54b6a1004
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784788"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>Procédure pas à pas : Utilisation tactile dans Xamarin.iOS

Cette procédure pas à pas montre comment écrire du code qui répond à différents types d’événements tactiles. Chaque exemple est contenue dans une autre fenêtre :

- [Touch exemples](#Touch_Samples) – comment répondre aux événements tactiles.
- [Exemples de module de reconnaissance de mouvement](#Gesture_Recognizer_Samples) : comment utiliser des modules de reconnaissance de mouvement intégré.
- [Exemple personnalisé de reconnaissance de mouvement](#Custom_Gesture_Recognizer) : la création d’un module de reconnaissance de mouvement personnalisée.

Chaque section contient des instructions pour écrire le code à partir de zéro.
Le [exemple de code de démarrage](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) inclut déjà un écran de plan conceptuel et menu terminé :

 [![](ios-touch-walkthrough-images/image3.png "L’exemple inclut l’écran de menu")](ios-touch-walkthrough-images/image3.png#lightbox)

Suivez les instructions ci-dessous pour ajouter du code à la table de montage séquentiel et en savoir plus sur les différents types d’événements tactiles disponibles dans iOS. Vous pouvez également ouvrir le [exemple terminé](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) pour voir tout ce travail.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Exemples de fonctions tactiles

Dans cet exemple, nous illustrerons certaines des fonctions tactiles API. Suivez ces étapes pour ajouter le code requis pour implémenter des événements tactiles :


1. Ouvrez le projet **Touch_Start**. Premier exécuter le projet pour vous assurer que tout est OK, ainsi que touch le **se touchent les exemples** bouton. Vous devez voir un écran semblable à la suivante (même si aucun des boutons fonctionne) :
    
    [![](ios-touch-walkthrough-images/image4.png "Exemple d’application de s’exécuter avec des boutons qui ne fonctionnent pas")](ios-touch-walkthrough-images/image4.png#lightbox)


1. Modifiez le fichier **TouchViewController.cs** et ajoutez les variables de deux instance suivantes à la classe `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. Implémentez la `TouchesBegan` méthode, comme indiqué dans le code ci-dessous :

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    
    Cette méthode fonctionne en recherchant un `UITouch` de l’objet et si elle existe effectuer une action selon où la fonction tactile s’est produite :

    * _À l’intérieur de TouchImage_ – afficher le texte `Touches Began` dans une étiquette et la modification de l’image.
    * _À l’intérieur de DoubleTouchImage_ : modifier l’image affichée si le mouvement a un double-clic.
    * _À l’intérieur de DragImage_ : définir un indicateur indiquant que les fonctions tactiles a démarré. La méthode `TouchesMoved` utilise cet indicateur pour déterminer si `DragImage` doit être déplacé sur l’écran ou non, comme nous le verrons dans l’étape suivante.

    Le code ci-dessus porte uniquement sur des touches finales, il n’est toujours aucun comportement si l’utilisateur déplace son doigt sur l’écran. Pour répondre à la circulation, implémentez `TouchesMoved` comme indiqué dans le code ci-dessous :

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Cette méthode obtient un `UITouch` objet, puis vérifie pour voir où la fonction tactile s’est produite. Si la fonction tactile s’est produite dans `TouchImage`, puis le texte des touches finales déplacé s’affiche sur l’écran. 

    Si `touchStartedInside` a la valeur true, nous savons que l’utilisateur a leur doigt `DragImage` et est déplacer. Le code déplacera `DragImage` lorsque l’utilisateur déplace son doigt sur l’écran.

1. Nous devons gérer le cas lorsque l’utilisateur lève le doigt son désactivé l’écran ou iOS annule l’événement tactile. Pour ce faire, nous implémenterons `TouchesEnded` et `TouchesCancelled` comme indiqué ci-dessous :

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```
    
    Ces deux méthodes réinitialisera le `touchStartedInside` indicateur sur false. `TouchesEnded` afficheront également `TouchesEnded` sur l’écran.

1. À ce stade, l’écran tactile des échantillons est terminé. Notez comment l’écran change à mesure que vous interagissez avec chacune des images, comme indiqué dans la capture d’écran suivante :
        
    [![](ios-touch-walkthrough-images/image4.png "Écran de l’application de départ")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "L’écran une fois que l’utilisateur fait glisser un bouton")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>Exemples de module de reconnaissance de mouvement

Le [section précédente](#Touch_Samples) vous a montré comment faire glisser un objet autour de l’écran à l’aide des événements tactiles.
Dans cette section, nous permet de supprimer les événements tactiles et montrent comment utiliser les modules de reconnaissance de mouvement suivantes :

-  Le `UIPanGestureRecognizer` pour faire glisser une image autour de l’écran.
-  Le `UITapGestureRecognizer` pour répondre aux clics double à l’écran.

Si vous exécutez le [exemple de code de démarrage](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) , puis cliquez sur le **exemples de module de reconnaissance de mouvement** bouton, vous devez voir l’écran suivant :

 [![](ios-touch-walkthrough-images/image6.png "Cliquez sur le bouton d’exemples de module de reconnaissance de mouvement affiche cet écran")](ios-touch-walkthrough-images/image6.png#lightbox)

Suivez ces étapes pour implémenter les reconnaissances de mouvement :


1. Modifiez le fichier **GestureViewController.cs** et ajoutez la variable d’instance suivante :

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Nous avons besoin de cette variable d’instance pour effectuer le suivi de l’emplacement précédent de l’image.
Le module de reconnaissance de mouvement panoramique utilisera le `originalImageFrame` valeur pour calculer le décalage nécessaire pour redessiner l’image sur l’écran.

1. Ajoutez la méthode suivante au contrôleur :

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Ce code instancie un `UIPanGestureRecognizer` de l’instance et l’ajoute à une vue.
Notez que nous affectons une cible au mouvement sous la forme de la méthode `HandleDrag` – cette méthode est fournie à l’étape suivante.

1. Pour implémenter HandleDrag, ajoutez le code suivant pour le contrôleur :

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Le code ci-dessus sera tout d’abord vérifier l’état de la reconnaissance de mouvement, puis déplacez l’image autour de l’écran. Avec ce code en place, le contrôleur peut prennent désormais en charge la faire glisser l’un image autour de l’écran.


1. Ajouter un `UITapGestureRecognizer` qui modifie l’image qui est affichée dans DoubleTouchImage. Ajoutez la méthode suivante à la `GestureViewController` contrôleur :

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Ce code est très similaire au code pour le `UIPanGestureRecognizer` , mais au lieu d’utiliser un délégué pour une cible, nous utilisons un `Action`. 

1. La dernière chose que nous devons faire est de modifier `ViewDidLoad` afin qu’il appelle les méthodes que nous venons d’ajouter. Modifiez ViewDidLoad afin qu’il ressemble au code suivant :

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Notez également que nous initialiser la valeur de `originalImageFrame`.


1. Exécutez l’application et interagir avec les deux images.
La capture d’écran suivante est un exemple de ces interactions :
    
    [![](ios-touch-walkthrough-images/image7.png "Cette capture d’écran montre une interaction glisser")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Reconnaissance de mouvement personnalisée

Dans cette section, nous allons appliquer les concepts des sections précédentes pour créer un module de reconnaissance de mouvement personnalisée. Le module de reconnaissance de mouvement personnalisé sera sous-classes `UIGestureRecognizer`et sera reconnaît lorsque l’utilisateur dessine un « V » sur l’écran, puis basculer vers une image bitmap. La capture d’écran suivante est un exemple de l’écran :

 [![](ios-touch-walkthrough-images/image8.png "L’application reconnaît lorsque l’utilisateur dessine un « V » sur l’écran")](ios-touch-walkthrough-images/image8.png#lightbox)

Suivez ces étapes pour créer un module de reconnaissance de mouvement personnalisé :


1. Ajoutez une nouvelle classe au projet nommé `CheckmarkGestureRecognizer`et le rendre ressemble au code suivant :

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    La méthode de réinitialisation est appelée lorsque le `State` prend le propriété `Recognized` ou `Ended`. Il s’agit de l’heure pour réinitialiser tout état interne défini dans le module de reconnaissance de mouvement personnalisée.
La classe peut maintenant démarrer prochaine fois que l’utilisateur interagit avec l’application et prêt à retenter la reconnaissance du mouvement.



1. Maintenant que nous avons défini un module de reconnaissance de mouvement personnalisé (`CheckmarkGestureRecognizer`) modifier les **CustomGestureViewController.cs** et ajoutez les variables de deux instance suivantes :

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Pour instancier et configurer la reconnaissance de mouvement, ajoutez la méthode suivante au contrôleur :

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Modifier `ViewDidLoad` afin qu’il appelle `WireUpCheckmarkGestureRecognizer`, comme illustré dans l’extrait de code suivant :

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Exécutez l’application et essayez de dessiner un « V » sur l’écran. Vous devez voir l’image en cours affiche les modifications, comme indiqué dans les captures d’écran suivants :
    
    [![](ios-touch-walkthrough-images/image9.png "Le bouton activé")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "Le bouton désactivé")](ios-touch-walkthrough-images/image10.png#lightbox)



Ces trois sections présentent les différentes méthodes pour répondre aux événements dans iOS tactiles : à l’aide des événements tactiles, les modules de reconnaissance de mouvement intégrés, ou avec un module de reconnaissance de mouvement personnalisée.



## <a name="related-links"></a>Liens associés

- [iOS Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch finale (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
