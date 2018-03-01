---
title: "Afficher des Transitions de contrôleur"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: f85867f08b21a525937e1c938c7c8fd224554abc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="view-controller-transitions"></a>Afficher des Transitions de contrôleur

UIKit ajoute la prise en charge pour la personnalisation de la transition animée qui se produit lors de la présentation des contrôleurs de la vue. Cette prise en charge est inclus avec les contrôleurs intégrés, ainsi que tous les contrôleurs personnalisés qui héritent directement de `UIViewController`. En outre, `UICollectionViewController` tire parti de la personnalisation de transition de contrôleur pour tirer parti des transitions animées dans les dispositions de vue de collection.

## <a name="custom-transitions"></a>Transitions personnalisées

La transition animée entre les contrôleurs de la vue dans iOS 7 est entièrement personnalisable. `UIViewController` inclut désormais un `TransitioningDelegate` propriété qui fournit une classe d’animation personnalisée dans le système lorsqu’une transition se produit.

Pour utiliser une transition personnalisée avec `PresentViewController`:

1.  Définir le `ModalPresentationStyle` à `UIModalPresentationStyle.Custom` sur le contrôleur pour être présentées.
2.  Implémentez `UIViewControllerTransitioningDelegate` pour créer une classe d’animation, qui est une instance de `UIViewControllerAnimatedTransitioning` .
3.  Définir le `TransitioningDelegate` propriété à une instance de `UIViewControllerTransitioningDelegate` , également sur le contrôleur à présenter.
4.  Présenter le contrôleur de la vue.


Par exemple, le code suivant présente un contrôleur de vue de type `ControllerTwo` - un `UIViewController` sous-classe :

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Exécution de l’application et en appuyant sur le bouton entraîne l’animation de la valeur par défaut de vue du deuxième contrôleur pour animer dans à partir du bas, comme indiqué ci-dessous :

 ![](transitions-images/no-custom-transition.png "Exécution de l’application et en appuyant sur le bouton provoquent l’animation de la valeur par défaut de la deuxième vue de contrôleurs à animer dans à partir du bas")

Toutefois, la définition du `ModalPresentationStyle` et `TransitioningDelegate` entraîne une animation personnalisée pour la transition :

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

Le `TransitioningDelegate` est responsable de la création d’une instance de la `UIViewControllerAnimatedTransitioning` sous-classe - appelée `CustomAnimator` dans l’exemple ci-dessous :

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning PresentingController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Lorsque la transition a lieu, le système crée une instance de `IUIViewControllerContextTransitioning`, réussi pour les méthodes de l’animation. `IUIViewControllerContextTransitioning` contient le `ContainerView` où l’animation se produit, ainsi que le contrôleur de vue lancement de la transition et le contrôleur de la vue en cours de transition.

La `UIViewControllerAnimatedTransitioning` classe gère l’animation proprement dite. Deux méthodes doivent être implémentées :

1.  `TransitionDuration` : retourne la durée de l’animation en secondes.
1.  `AnimateTransition` : exécute l’animation réelle.


Par exemple, la classe suivante implémente `UIViewControllerAnimatedTransitioning` pour animer le frame de vue du contrôleur :

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
        {
            var inView = transitionContext.ContainerView;
            var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
            var toView = toVC.View;

            inView.AddSubview (toView);

            var frame = toView.Frame;
            toView.Frame = CGRect.Empty;

            UIView.Animate (TransitionDuration (transitionContext), () => {
                toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
            }, () => {
                transitionContext.CompleteTransition (true);
            });
        }
}
```

Maintenant, lorsque l’utilisateur clique sur le bouton, l’animation est implémenté dans le `UIViewControllerAnimatedTransitioning` classe est utilisée :

 ![](transitions-images/custom-transition.png "Un exemple de zoom en vigueur en cours d’exécution.")

## <a name="collection-view-transitions"></a>Transitions entre les vues de collection

Vues de collection ont une prise en charge intégrée pour la création de transitions animées :

-  **Contrôleurs de navigation** – l’anime la transition entre deux `UICollectionViewController` instances peuvent éventuellement être gérées automatiquement lorsque un `UINavigationController` les gère.
-  **Disposition de transition** – un nouveau `UICollectionViewTransitionLayout` classe permet interactive en cours de transition entre les dispositions.


### <a name="navigation-controller-transitions"></a>Transitions de contrôleur de navigation

Lorsqu’il est utilisé au sein d’un contrôleur de navigation, un `UICollectionViewController` prend en charge les transitions animées entre les contrôleurs. Cette prise en charge est intégrée et nécessite seulement quelques étapes simples à implémenter :

1.  Définissez `UseLayoutToLayoutNavigationTransitions` à `false` sur un `UICollectionViewController` .
1.  Ajouter une instance de la `UICollectionViewController` à la racine de la pile du contrôleur de navigation.
1.  Créer un second `UICollectionViewController` et définir son `UseLayoutToLayoutNavigtionTransitions` propriété `true` .
1.  Push de la seconde `UICollectionViewController` sur la pile du contrôleur de navigation.


Le code suivant ajoute un `UICollectionViewController` sous-classe nommée `ImagesCollectionViewController` à la racine de la pile d’un contrôleur de navigation avec le `UseLayoutToLayoutNavigationTransitions` propriété `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Lorsqu’un élément est sélectionné, une deuxième instance de la `ImagesController` est créé, cette fois à l’aide d’une classe de mise en page différente. Pour ce contrôleur, `UseLayoutToLayoutNavigtionTransitions` a la valeur `true`, comme indiqué ci-dessous :

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
        circleLayout = new CircleLayout (Monkeys.Instance.Count){
                ItemSize = new CGSize (100, 100)
            };
            
    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

Le `UseLayoutToLayoutNavigationTransitions` propriété doit être définie avant l’ajout du contrôleur à la pile de navigation. Avec cette propriété est définie, la transition glissante horizontale normale est remplacée par une transition animée entre les dispositions des deux contrôleurs, comme illustré ci-dessous :

![](transitions-images/nav2.png "Une transition animée entre les dispositions des deux contrôleurs")

### <a name="transition-layout"></a>Disposition de transition

En plus de la transition de la disposition prise en charge dans les contrôleurs de navigation, une nouvelle disposition appelée `UICollectionViewTransitionLayout` est désormais disponible. Cette classe de disposition permet un contrôle interactif pendant le processus de transition de disposition, en permettant la `TransitionProgress` à définir à partir de code. `UICollectionViewTransitionLayout` est différent de - et non pas le remplacement de - le `SetCollectionViewLayout` méthode à partir d’iOS 6 qui a provoqué une transition animée disposition se produise. Cette méthode n’a pas fourni la prise en charge intégrée pour contrôler la progression de la transition animée.

 `UICollectionViewTransitionLayout` permet, par exemple, un module de reconnaissance de mouvement à être configuré pour contrôler la transition entre les dispositions en réponse à une interaction utilisateur, en gérant la disposition d’origine, ainsi que la mise en page prévue pour la transition vers.

Les étapes pour implémenter une transition interactive au sein d’un module de reconnaissance de mouvement à l’aide de `UICollectionViewTransitionLayout` sont les suivantes :

1.  Créer un module de reconnaissance de mouvement.
1.  Appelez le `StartInteractiveTransition` méthode de la `UICollectionView` , en lui passant la disposition de la cible et un gestionnaire d’achèvement.
1.  Définir le `TransitionProgress` propriété de la `UICollectionViewTransitionLayout` instance retournée à partir de la `StartInteractiveTransition` (méthode).
1.  Invalide la disposition.
1.  Appeler le `FinishInteractiveTransition` méthode de la `UICollectionView` pour effectuer la transition ou `CancelInteractiveTransition` méthode pour l’annuler.  `FinishInteractiveTransition` entraîne l’animation effectuer sa transition vers la disposition de la cible, tandis que `CancelInteractiveTransition` entraîne l’animation de renvoi à la disposition d’origine.
1.  Gérer l’achèvement de transition dans le Gestionnaire d’achèvement de la `StartInteractiveTransition` (méthode).
1.  Ajouter le module de reconnaissance de mouvement à la vue de collection.


Le code suivant implémente une transition de présentation interactive au sein d’un module de reconnaissance de mouvement de pincement :

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {   
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Comme l’utilisateur pinches la vue de collection, le `TransitionProgress` est définie par rapport à l’échelle de la pincement. Dans cette implémentation, si l’utilisateur termine le pincement avant la transition est 50 % terminé, la transition est annulée. Sinon, la transition est terminée.




## <a name="related-links"></a>Liens associés

- [Introduction à iOS 7 (exemple)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 présentation de l’Interface utilisateur](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
