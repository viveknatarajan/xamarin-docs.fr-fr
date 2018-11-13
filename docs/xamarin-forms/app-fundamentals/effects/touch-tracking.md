---
title: Appel d’événements à partir d’effets
description: Un effet peut définir et appeler un événement, signalant les modifications dans la vue native sous-jacente. Cet article explique comment implémenter les doigts de l’interaction tactile multipoint de bas niveau de suivi et comment générer des événements qui indiquent l’activité de tactile.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 0a5e2c1a7a7807da91fd98e617467ea251a25bc0
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527402"
---
# <a name="invoking-events-from-effects"></a>Appel d’événements à partir d’effets

_Un effet peut définir et appeler un événement, signalant les modifications dans la vue native sous-jacente. Cet article explique comment implémenter les doigts de l’interaction tactile multipoint de bas niveau de suivi et comment générer des événements qui indiquent l’activité de tactile._

L’effet décrite dans cet article fournit l’accès aux événements tactiles de bas niveau. Ces événements de bas niveau ne sont pas disponibles via existant `GestureRecognizer` classes, mais elles sont vitales à certains types d’applications. Par exemple, un finger-paint les besoins de l’application pour effectuer le suivi de chaque doigt lorsqu’ils passent à l’écran. Un clavier de musique doit détecter les clics et libère sur les clés individuelles, mais aussi un doigt élargissement variable à partir d’une clé à l’autre dans un glissando.

Un effet est idéal pour le suivi de l’interaction tactile multipoint doigt, car elle peut être jointe à n’importe quel élément Xamarin.Forms.

## <a name="platform-touch-events"></a>Événements tactiles de plateforme

L’iOS, Android et plateforme Windows universelle incluent toutes une API de bas niveau qui permet aux applications de détecter touch activité. Ces plateformes que tous faire la distinction entre les trois types de base des événements tactiles :

- *Enfoncé*, lorsqu’un doigt touche l’écran
- *Déplacé*, lorsqu’un doigt touche l’écran se déplace
- *Publié*, lorsque le doigt est libéré de l’écran

Dans un environnement de l’interaction tactile multipoint, plusieurs doigts peuvent se toucher l’écran en même temps. Les différentes plateformes incluent un numéro d’identification (ID) d’applications peuvent utiliser pour faire la distinction entre plusieurs doigts.

Dans iOS, le `UIView` classe définit trois méthodes substituables, `TouchesBegan`, `TouchesMoved`, et `TouchesEnded` correspondant à ces trois événements de base. L’article [suivi de l’interaction tactile multipoint doigt](~/ios/app-fundamentals/touch/touch-tracking.md) explique comment utiliser ces méthodes. Toutefois, un programme iOS sans devoir remplacer une classe qui dérive de `UIView` d’utiliser ces méthodes. IOS `UIGestureRecognizer` définit également ces mêmes trois méthodes et vous pouvez attacher une instance d’une classe qui dérive de `UIGestureRecognizer` aux `UIView` objet.

Dans Android, le `View` classe définit une méthode substituable nommée `OnTouchEvent` pour traiter toutes les activités tactile. Le type de l’activité tactile est défini par les membres de l’énumération `Down`, `PointerDown`, `Move`, `Up`, et `PointerUp` comme décrit dans l’article [suivi de l’interaction tactile multipoint doigt](~/android/app-fundamentals/touch/touch-tracking.md). Android `View` définit également un événement nommé `Touch` qui permet à un gestionnaire d’événements à attacher aux `View` objet.

Dans la plateforme de Windows universelle (UWP), le `UIElement` classe définit les événements nommés `PointerPressed`, `PointerMoved`, et `PointerReleased`. Ceux-ci sont décrits dans l’article [article gérer une entrée de pointeur sur MSDN](/windows/uwp/input-and-devices/handle-pointer-input/) et la documentation API pour le [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) classe.

Le `Pointer` API dans la plateforme Windows universelle est destinée à unifier la souris, tactile et entrée du stylet. Pour cette raison, le `PointerMoved` événement est appelé lorsque la souris se trouve sur un élément même lorsque le bouton de la souris n’est pas enfoncé. Le `PointerRoutedEventArgs` objet qui accompagne ces événements possède une propriété nommée `Pointer` qui a une propriété nommée `IsInContact` qui indique si un bouton de la souris est enfoncé ou un doigt est en contact avec l’écran.

En outre, la plateforme Windows universelle définit deux événements plus nommés `PointerEntered` et `PointerExited`. Ces erreurs indiquent que lorsque la souris ou du doigt se déplace d’un élément à un autre. Par exemple, considérez les deux éléments adjacents nommées A et b. Les deux éléments ont installé des gestionnaires pour les événements de pointeur. Lorsqu’un doigt appuie sur A, le `PointerPressed` événement est appelé. Comme le doigt se déplace, A appelle `PointerMoved` événements. Si le doigt se déplace de A à B, A appelle un `PointerExited` B et événements appelle un `PointerEntered` événement. Si le doigt est ensuite libéré, B appelle un `PointerReleased` événement.

Les plateformes iOS et Android sont différents à partir de la plateforme Windows universelle : la vue qui obtient d’abord l’appel à `TouchesBegan` ou `OnTouchEvent` lorsqu’un doigt touche l’affichage se poursuit obtenir toutes les activités touch, même si le doigt se déplace vers différentes vues. La plateforme Windows universelle peut se comporter de même si l’application capture le pointeur : dans le `PointerEntered` Gestionnaire d’événements, les appels de l’élément `CapturePointer` et obtient ensuite toutes les activités tactile à partir de ce doigt.

L’approche UWP s’avère très utile pour certains types d’applications, par exemple, un clavier de la musique. Chaque clé peut gérer les événements tactiles pour cette clé et détecter lorsqu’un doigt a glissé à partir d’une clé à un autre en utilisant le `PointerEntered` et `PointerExited` événements.

Pour cette raison, l’effet de suivi de tactile décrite dans cet article implémente l’approche UWP.

## <a name="the-touch-tracking-effect-api"></a>L’API d’effet-suivi tactile

Le [ **Touch des démonstrations de suivi effet** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) exemple contient les classes (et une énumération) qui implémentent le suivi tactile de bas niveau. Ces types appartiennent à l’espace de noms `TouchTracking` et commencent par le mot `Touch`. Le **TouchTrackingEffectDemos** projet de bibliothèque .NET Standard inclut la `TouchActionType` énumération pour le type d’événements tactiles :

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

Toutes les plateformes incluent également un événement qui indique que l’événement tactile a été annulée.

Le `TouchEffect` dérive de la classe dans la bibliothèque .NET Standard `RoutingEffect` et définit un événement nommé `TouchAction` et une méthode nommée `OnTouchAction` qui appelle le `TouchAction` événement :

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

Notez également le `Capture` propriété. Pour capturer les événements tactiles, une application doit définir cette propriété `true` avant un `Pressed` événement. Sinon, les événements tactiles se comportent comme celles de la plateforme Windows universelle.

Le `TouchActionEventArgs` classe dans la bibliothèque .NET Standard contient toutes les informations qui accompagne chaque événement :

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

Une application peut utiliser le `Id` propriété pour suivre chaque doigt. Notez que le `IsInContact` propriété. Cette propriété est toujours `true` pour `Pressed` événements et `false` pour `Released` événements. Il est également toujours `true` pour `Moved` événements sur iOS et Android. Le `IsInContact` propriété peut être `false` pour `Moved` événements sur la plateforme Windows universelle lorsque le programme est en cours d’exécution sur le bureau et le pointeur de la souris se déplace sans un bouton enfoncé.

Vous pouvez utiliser la `TouchEffect` classe dans vos propres applications, en incluant le fichier dans le projet de bibliothèque .NET Standard de la solution et en ajoutant une instance de la `Effects` collection de n’importe quel élément Xamarin.Forms. Attacher un gestionnaire pour le `TouchAction` événement pour obtenir les événements tactiles.

Pour utiliser `TouchEffect` dans votre propre application, vous devez également incluses dans les implémentations de plateforme **TouchTrackingEffectDemos** solution.

## <a name="the-touch-tracking-effect-implementations"></a>Les implémentations de l’effet de suivi de tactile

L’iOS, Android et UWP implémentations de la `TouchEffect` sont décrites ci-dessous à partir de l’implémentation la plus simple (UWP) et se terminant par l’implémentation d’iOS, car il n’est présente une structure plus complexe que les autres.

### <a name="the-uwp-implementation"></a>L’implémentation UWP

L’implémentation UWP de `TouchEffect` est la plus simple. Comme d’habitude, dérive de la classe `PlatformEffect` et inclut deux attributs d’assembly :

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

Le `OnAttached` remplacement enregistre des informations en tant que champs et attache les gestionnaires à tous les événements de pointeur :

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

Le `OnPointerPressed` gestionnaire appelle l’événement d’effet en appelant le `onTouchAction` champ dans le `CommonHandler` méthode :

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed` vérifie également la valeur de la `Capture` propriété dans la classe de l’effet dans la bibliothèque .NET Standard et les appels `CapturePointer` s’il s’agit `true`.

 Les autres gestionnaires d’événements UWP sont encore plus simples :

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>L’implémentation Android

Les implémentations iOS et Android sont nécessairement plus complexes, car ils doivent implémenter le `Exited` et `Entered` événements lorsqu’un doigt se déplace d’un élément à un autre. Les deux implémentations sont structurées de manière similaire.

Android `TouchEffect` classe installe un gestionnaire pour le `Touch` événement :

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

La classe définit également deux dictionnaires statiques :

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

Le `viewDictionary` Obtient une nouvelle entrée chaque fois que le `OnAttached` substitution est appelée :

```csharp
viewDictionary.Add(view, this);
```

L’entrée est supprimée à partir du dictionnaire dans `OnDetached`. Chaque instance de `TouchEffect` est associé à une vue associée à l’effet. Le dictionnaire statique permet les `TouchEffect` instance pour l’énumération de toutes les autres vues et leur correspondant `TouchEffect` instances. Cela est nécessaire pour autoriser pour transférer les événements à partir d’une vue à l’autre.

Android attribue un code d’ID à des événements tactiles qui permet à une application effectuer le suivi de chaque doigt. Le `idToEffectDictionary` associe ce code d’ID avec un `TouchEffect` instance. Un élément est ajouté à ce dictionnaire lorsque le `Touch` gestionnaire est appelé pour un press doigt :

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

L’élément est supprimé à partir de la `idToEffectDictionary` lorsque le doigt est libéré de l’écran. Le `FireEvent` méthode accumule simplement toutes les informations nécessaires pour appeler le `OnTouchAction` méthode :

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

Tous les autres types de fonctions tactiles sont traitées de deux manières différentes : si le `Capture` propriété est `true`, l’événement tactile est une traduction relativement simple à le `TouchEffect` plus d’informations. Il est plus compliqué lorsque `Capture` est `false` , car les événements tactiles doivent être déplacés à partir d’une vue à un autre. C’est la responsabilité de la `CheckForBoundaryHop` (méthode), qui est appelé pendant les événements de déplacement. Cette méthode utilise les deux dictionnaires statiques. Il énumère la `viewDictionary` pour déterminer la vue qui le doigt touche actuellement, et il utilise `idToEffectDictionary` pour stocker actuel `TouchEffect` instance (et par conséquent, la vue actuelle) associé à un ID spécifique :

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

S’il y a eu un changement dans le `idToEffectDictionary`, la méthode appelle potentiellement `FireEvent` pour `Exited` et `Entered` au transfert d’une vue à l’autre. Toutefois, le doigt peut-être avoir été déplacé vers une zone occupée par un affichage sans un attaché `TouchEffect`, ou à partir de cette zone à une vue avec l’effet attaché.

Notez que le `try` et `catch` bloquer lors de l’accès à la vue. Dans une page qui est accédée à qui, puis revient à la page d’accueil, le `OnDetached` méthode n’est pas appelée et les éléments restent dans le `viewDictionary` mais Android les considère comme supprimé.

### <a name="the-ios-implementation"></a>L’implémentation d’iOS

L’implémentation d’iOS est similaire à l’implémentation Android, sauf que l’iOS `TouchEffect` classe doit instancier un dérivé de `UIGestureRecognizer`. Il s’agit d’une classe dans le projet iOS nommé `TouchRecognizer`. Cette classe gère deux dictionnaires statiques qui stockent `TouchRecognizer` instances :

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Quantité de la structure de ce `TouchRecognizer` classe est semblable à la Android `TouchEffect` classe.

## <a name="putting-the-touch-effect-to-work"></a>Placez l’effet tactile travail

Le [ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) programme contienne cinq pages de tester l’effet de suivi des fonctions tactiles pour les tâches courantes.

Le **BoxView en faisant glisser** page vous permet d’ajouter `BoxView` éléments à un `AbsoluteLayout` et faites-les glisser autour de l’écran. Le [les fichiers XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) instancie deux `Button` vues pour l’ajout de `BoxView` éléments à la `AbsoluteLayout` et en désactivant le `AbsoluteLayout`.

La méthode dans le [fichier code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) qui ajoute un nouveau `BoxView` à la `AbsoluteLayout` ajoute également un `TouchEffect` de l’objet à le `BoxView` et attache un gestionnaire d’événements à l’effet :

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

Le `TouchAction` Gestionnaire d’événements traite tous les événements tactiles pour tous les le `BoxView` éléments, mais il a besoin pour une preuve de prudence : il ne peut pas autoriser des deux doigts sur un seul `BoxView` , car le programme implémente seulement la faisant glisser et seraient de deux doigts interférer entre eux. Pour cette raison, la page définit une classe incorporée pour chaque doigt actuellement suivi :

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

Le `dragDictionary` contient une entrée pour chaque `BoxView` actuellement glissé.

Le `Pressed` tactile action ajoute un élément à ce dictionnaire et le `Released` action le supprime. Le `Pressed` logique doit vérifier s’il existe déjà un élément dans le dictionnaire pour qui `BoxView`. Dans ce cas, le `BoxView` est déjà en cours d’un glisser le nouvel événement est un deuxième doigt sur ce même `BoxView`. Pour le `Moved` et `Released` actions, le Gestionnaire d’événements doit vérifier si le dictionnaire a une entrée pour ce `BoxView` et que la pression tactile `Id` propriété pour que l’utilisateur glisser `BoxView` correspond à celui de l’entrée de dictionnaire :

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

Le `Pressed` jeux de logique le `Capture` propriété de la `TouchEffect` objet `true`. Cela a pour effet de diffusion de tous les événements suivants pour ce doigt sur le même gestionnaire d’événements.

Le `Moved` logique déplace le `BoxView` en modifiant le `LayoutBounds` propriété jointe. Le `Location` propriété des arguments d’événement est toujours relatif le `BoxView` déplacé et si le `BoxView` est déplacé à une vitesse constante, le `Location` propriétés des événements consécutifs sera environ le même. Par exemple, si un doigt appuie sur le `BoxView` dans son centre, le `Pressed` action stocke un `PressPoint` propriété de (50, 50), qui reste le même pour les événements suivants. Si le `BoxView` est déplacé en diagonale à une vitesse constante, le `Location` propriétés durant la `Moved` action peut-être être des valeurs de (55, 55), auquel cas la `Moved` logique ajoute 5 à la position horizontale et verticale de la `BoxView`. Cette opération déplace le `BoxView` afin que son centre est à nouveau directement sous le doigt.

Vous pouvez déplacer plusieurs `BoxView` éléments simultanément à l’aide de doigts différents.

[![](touch-tracking-images/boxviewdragging-small.png "Triple capture d’écran de la page en faisant glisser des BoxView")](touch-tracking-images/boxviewdragging-large.png#lightbox "Triple capture d’écran de la page BoxView en faisant glisser")

### <a name="subclassing-the-view"></a>Sous-classement de la vue

Souvent, il est plus facile pour un élément de Xamarin.Forms gérer ses propres événements tactiles. Le **déplaçable en faisant glisser de BoxView** page fonctionne comme le **BoxView en faisant glisser** page, mais les éléments qui est des instances de l’utilisateur fait glisser un [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) classe qui dérive de `BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

Le constructeur crée et attache le `TouchEffect`et définit le `Capture` propriété lorsque cet objet est instancié. Aucun dictionnaire n’est nécessaire, car la classe elle-même stocke `isBeingDragged`, `pressPoint`, et `touchId` valeurs associées à chaque doigt. Le `Moved` gestion modifie le `TranslationX` et `TranslationY` propriétés pour la logique fonctionne même si le parent de la `DraggableBoxView` n’est pas un `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>L’intégration avec SkiaSharp

Les deux démonstrations nécessitent des graphiques, et utilisent SkiaSharp à cet effet. Vous souhaiterez peut-être en savoir plus sur [à l’aide de SkiaSharp dans Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) avant de vous examinerez ces exemples. Les deux premiers articles (« Principes de base de dessin SkiaSharp » et « SkiaSharp lignes et chemins d’accès ») couvrent tout ce dont vous avez besoin ici.

Le **Ellipse dessin** page vous permet de dessiner une ellipse en passant votre doigt sur l’écran. Selon la façon dont vous déplacez votre doigt, vous pouvez dessiner l’ellipse à partir de l’angle supérieur gauche de l’angle inférieur droit, ou à partir de n’importe quel autre coin vers le coin opposé. L’ellipse est dessinée avec une couleur aléatoire et une opacité.

[![](touch-tracking-images/ellipsedrawing-small.png "Capture d’écran triple de la page de dessin de l’Ellipse")](touch-tracking-images/ellipsedrawing-large.png#lightbox "Triple capture d’écran de la page de dessin de l’Ellipse")

Si vous touchez puis l’un des ellipses, vous pouvez faire glisser vers un autre emplacement. Cela nécessite une technique appelée « le test d’atteinte, » ce qui implique la recherche de l’objet graphique à un moment donné. Les points de suspension SkiaSharp ne sont pas des éléments de Xamarin.Forms, afin qu’ils ne peuvent pas effectuer leurs propres `TouchEffect` de traitement. Le `TouchEffect` doivent s’appliquer à l’ensemble du `SKCanvasView` objet.

Le [EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) fichier instancie le `SKCanvasView` dans une seule cellule `Grid`. Le `TouchEffect` objet est attaché au qui `Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Dans Android et la plateforme Windows universelle, le `TouchEffect` peut être attaché directement à la `SKCanvasView`, mais sur iOS qui ne fonctionne pas. Notez que le `Capture` propriété est définie sur `true`.

Chaque ellipse qui restitue SkiaSharp est représenté par un objet de type `EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

Le `StartPoint` et `EndPoint` propriétés sont utilisées lorsque le programme traite une entrée ; tactile le `Rectangle` propriété est utilisée pour dessiner l’ellipse. Le `LastFingerLocation` propriété intervient lors de l’ellipse est déplacée et le `IsInEllipse` aides à la méthode dans le test de positionnement. La méthode retourne `true` si le point est à l’intérieur de l’ellipse.

Le [fichier code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) gère trois collections :

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

Le `draggingFigure` dictionnaire contient un sous-ensemble de la `completedFigures` collection. Le SkiaSharp `PaintSurface` Gestionnaire d’événements affiche simplement les objets dans ces le `completedFigures` et `inProgressFigures` collections :

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

La partie la plus délicate du traitement tactile est le `Pressed` gestion. C’est là où le test de positionnement est effectué, mais si le code détecte une ellipse sous le doigt de l’utilisateur, cette ellipse peut uniquement être déplacé si elle n’est pas actuellement glissé par un autre doigt. S’il n’existe aucun ellipse sous le doigt de l’utilisateur, le code commence le processus de dessin d’une ellipse nouvelle :

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

L’exemple de SkiaSharp autres est le **peinture au doigt** page. Vous pouvez sélectionner une couleur de trait et l’épaisseur du trait à partir de deux `Picker` des vues et puis dessinez avec un ou plusieurs doigts :

[![](touch-tracking-images/fingerpaint-small.png "Capture d’écran triple de la page de peinture au doigt")](touch-tracking-images/fingerpaint-large.png#lightbox "Triple capture d’écran de la page de peinture au doigt")

Cet exemple nécessite également une classe distincte pour représenter chaque ligne peint à l’écran :

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

Un `SKPath` objet est utilisé pour afficher chaque ligne. Le [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) fichier gère deux collections de ces objets, une pour ces polylignes en cours de saisie et un autre pour les polylignes terminées :

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Le `Pressed` traitement crée un nouveau `FingerPaintPolyline`, appels `MoveTo` sur l’objet de chemin d’accès pour stocker le point initial et ajoute cet objet à le `inProgressPolylines` dictionnaire. Le `Moved` traitement des appels `LineTo` sur l’objet de chemin d’accès avec la nouvelle position du doigt et le `Released` traitement transfère la polyligne terminée à partir de `inProgressPolylines` à `completedPolylines`. Une fois encore, le code de dessin de SkiaSharp réelle est relativement simple :

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>Suivi-Affichage tactile

Tous les exemples précédents ont défini la `Capture` propriété de la `TouchEffect` à `true`, soit lorsque le `TouchEffect` a été créé ou lorsque le `Pressed` événement s’est produit. Cela garantit que le même élément reçoit tous les événements associés au doigt enfoncé tout d’abord la vue. L’exemple final *pas* définir `Capture` à `true`. Cela provoque un comportement différent quand un doigt en contact avec l’écran se déplace d’un élément vers un autre. L’élément qui le doigt se déplace du reçoit un événement avec un `Type` propriété définie sur `TouchActionType.Exited` et le deuxième élément reçoit un événement avec un `Type` paramètre `TouchActionType.Entered`.

Ce type de traitement de tactile est très utile pour un clavier de musique. Une clé doit être en mesure de détecter lorsqu’il est appuyé sur celui-ci, mais également lorsqu’un doigt diapositives à partir d’une clé à un autre.

Le **clavier en mode silencieux** page définit petit [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) et [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) classes qui dérivent [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs), qui dérive à son `BoxView`.

Le `Key` classe est prête à être utilisée dans un programme de musique réelle. Il définit les propriétés publiques nommées `IsPressed` et `KeyNumber`, qui est destiné à être définie sur le code de touche établi par la norme MIDI. Le `Key` classe définit également un événement nommé `StatusChanged`, qui est appelé lorsque le `IsPressed` les modifications de propriété.

Plusieurs doigts sont autorisées sur chaque clé. Pour cette raison, le `Key` classe maintient une `List` les tactile des numéros d’ID de tous les doigts touchant actuellement cette clé :

```csharp
List<long> ids = new List<long>();
```

Le `TouchAction` Gestionnaire d’événements ajoute un ID pour le `ids` liste pour les deux un `Pressed` type d’événement et un `Entered` type, mais uniquement lorsque le `IsInContact` propriété est `true` pour le `Entered` événement. L’ID est supprimé de la `List` pour un `Released` ou `Exited` événement :

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

Le `AddToList` et `RemoveFromList` méthodes à la fois vérifient si les `List` a changé entre vide et non vide et si tel est le cas, appelle le `StatusChanged` événement.

Les différents `WhiteKey` et `BlackKey` éléments sont organisés dans la page [les fichiers XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), ce qui convient le mieux lorsque le téléphone est maintenu dans un mode paysage :

[![](touch-tracking-images/silentkeyboard-small.png "Capture d’écran triple de la page de clavier en mode silencieux")](touch-tracking-images/silentkeyboard-large.png#lightbox "Triple capture d’écran de la page de clavier en mode silencieux")

Si vous balayage votre doigt sur les clés, vous verrez par les légères modifications de couleur que les événements tactiles sont transférés à partir d’une clé à un autre.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment appeler des événements dans un effet et comment écrire et utiliser un effet qui implémente le traitement de l’interaction tactile multipoint bas niveau.


## <a name="related-links"></a>Liens associés

- [Suivi d’un doigt de l’interaction tactile multipoint dans iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Doigt de l’interaction tactile multipoint suivi des modifications dans Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [Effet de suivi tactile (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
