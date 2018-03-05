---
title: Suivi de tactiles doigt
description: "Cette rubrique montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts"
ms.topic: article
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 53e972211ce506b6bf32ee4785c853982528d92e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="multi-touch-finger-tracking"></a>Suivi de tactiles doigt

_Cette rubrique montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts_

Il existe des cas lorsqu’une application tactile doit effectuer le suivi des doigts individuels lors de leur déplacement simultanément sur l’écran. Une application classique est un programme finger-paint. Vous souhaitez que l’utilisateur pour être en mesure de dessiner avec un seul doigt, mais également pour dessiner avec plusieurs doigts à la fois. Lorsque votre programme traite plusieurs événements tactiles, il a besoin distinguer les événements qui correspondent à chaque doigt. Android fournit un code d’ID à cette fin, mais obtention et ce code de gestion peuvent être un peu difficile.

Pour tous les événements associés à un doigt particulière, le code de l’ID reste la même. Le code de l’ID est attribué à un doigt touche l’écran tout d’abord et devient non valide après que le doigt lève à partir de l’écran.
Ces codes sont généralement très petits entiers et Android les réutilise pour les événements tactiles ultérieure.

Presque toujours, un programme qui effectue le suivi des doigts maintient un dictionnaire pour les fonctions tactiles de suivi. La clé de dictionnaire est le code d’ID qui identifie un doigt particulier. La valeur de dictionnaire dépend de l’application. Dans le [peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programme, chaque tracé doigt (à partir de tactile à libérer) est associé à un objet qui contient toutes les informations nécessaires pour restituer la ligne dessinée avec ce doigt. Le programme définit un petit `FingerPaintPolyline` classe à cet effet :

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Chaque polyligne a une couleur, une épaisseur de trait et un graphique Android [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) objet à s’accumuler et à rendre plusieurs points de la ligne comme elle est dessinée.

Le reste du code ci-dessous est contenu dans un `View` dérivé nommé `FingerPaintCanvasView`. Que classe maintenance un dictionnaire d’objets de type `FingerPaintPolyline` pendant l’heure à laquelle ils sont dessinés activement par un ou plusieurs doigts :

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Ce dictionnaire permet l’affichage pour obtenir rapidement la `FingerPaintPolyline` informations associées à un doigt particulier.

Le `FingerPaintCanvasView` classe gère également une `List` objet pour les polylignes qui ont été effectuées :

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Les objets dans ce `List` se trouvent dans le même ordre qu’ils ont été dessinées.

`FingerPaintCanvasView` deux méthodes définies par les remplacements `View`: [ `OnDraw` ](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/) et [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
Dans son `OnDraw` override, la vue dessine les polylignes terminés et puis dessine les polylignes en cours d’exécution.

La substitution de la `OnTouchEvent` méthode commence par obtenir un `pointerIndex` à partir du `ActionIndex` propriété. Cela `ActionIndex` valeur fait la distinction entre plusieurs doigts, mais il n’est pas cohérente entre plusieurs événements. Pour cette raison, vous utilisez la `pointerIndex` pour obtenir le pointeur `id` à partir du `GetPointerId` (méthode). Cet ID *est* cohérente sur plusieurs événements :

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Notez que le remplacement utilise le `ActionMasked` propriété dans le `switch` instruction plutôt que `Action` propriété. En voici les raisons :

Lorsque vous avez affaire à tactiles, le `Action` propriété a la valeur `MotionEventsAction.Down` pour le premier doigt toucher l’écran, puis les valeurs de `Pointer2Down` et `Pointer3Down` comme les deuxième et troisième doigts appuyez également sur l’écran. Assurez-vous des quatrième et cinquième doigts contact, le `Action` propriété a des valeurs numériques qui ne correspondent pas aux membres du même le `MotionEventsAction` énumération ! Vous devez examiner les valeurs des indicateurs de bits des valeurs à interpréter leur signification.

De même, comme les doigts laisser le contact avec l’écran, la `Action` propriété a les valeurs `Pointer2Up` et `Pointer3Up` pour les deuxième et troisième doigts, et `Up` pour le premier doigt.

Le `ActionMasked` propriété prend moins nombre de valeurs, car elle est destinée à être utilisée conjointement avec la `ActionIndex` propriété faire la distinction entre plusieurs doigts. Lorsque les doigts touchent l’écran, la propriété peut uniquement être égale à `MotionEventActions.Down` pour le premier doigt et `PointerDown` pour doigts suivantes. Comme les doigts quittez l’écran, `ActionMasked` a des valeurs de `Pointer1Up` pour les doigts suivantes et `Up` pour le premier doigt.

Lorsque vous utilisez `ActionMasked`, le `ActionIndex` fait la distinction entre les doigts ultérieures tactile et laissez l’écran, mais en général, vous n’avez pas besoin d’utiliser cette valeur à l’exception en tant qu’argument à d’autres méthodes dans le `MotionEvent` objet. Pour tactiles, un des plus importants de ces méthodes est `GetPointerId` appelée dans le code ci-dessus. Que méthode retourne une valeur qui vous pouvez utiliser une clé de dictionnaire pour associer des événements spécifiques à doigts.

Le `OnTouchEvent` remplacer dans les [peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) processus de programme le `MotionEventActions.Down` et `PointerDown` événements identique en créant un nouveau `FingerPaintPolyline` objet et en l’ajoutant au dictionnaire :

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Notez que le `pointerIndex` est également utilisé pour obtenir la position du doigt dans la vue. Toutes les informations de contact sont associées le `pointerIndex` valeur. Le `id` identifie de façon unique des doigts à plusieurs messages, afin que de l’utiliser pour créer l’entrée de dictionnaire.

De même, la `OnTouchEvent` remplacer également handles le `MotionEventActions.Up` et `Pointer1Up` identique par le transfert de la polyligne terminée pour le `completedPolylines` collection afin qu’ils peuvent être dessinées pendant la `OnDraw` remplacer. Le code supprime également la `id` entrée du dictionnaire :

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Pour la partie délicate.

Entre le bas et les événements, en général il existe de nombreux `MotionEventActions.Move` événements. Ceux-ci sont regroupés dans un seul appel à `OnTouchEvent`, et elles doivent être traitées différemment à partir de la `Down` et `Up` les événements. Le `pointerIndex` valeur obtenue précédemment dans le `ActionIndex` propriété doit être ignorée. Au lieu de cela, la méthode doit obtenir plusieurs `pointerIndex` valeurs en effectuant une boucle compris entre 0 et la `PointerCount` propriété, puis d’obtenir un `id` de chacun d’eux `pointerIndex` valeurs :

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

Ce type de traitement permet la [peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programme pour effectuer le suivi des doigts individuels et dessiner les résultats à l’écran :

[![Capture d’écran de peinture avec les doigts exemple](touch-tracking-images/image01.png)](touch-tracking-images/image01.png)

Vous avez maintenant vu comment vous pouvez effectuer le suivi des doigts sur l’écran et faire la distinction entre eux.


## <a name="related-links"></a>Liens associés

- [Guide de l’équivalent Xamarin iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
