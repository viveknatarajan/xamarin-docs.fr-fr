---
title: Doigt de l’interaction tactile multipoint suivi des modifications dans Xamarin.Android
description: Cette rubrique montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 34a9d2d9b8acb05a1b978a70e85038507032faaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61011785"
---
# <a name="multi-touch-finger-tracking"></a>Suivi de l’interaction tactile multipoint doigt

_Cette rubrique montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts_

Il est parfois quand une application tactile multipoint doit suivre chaque doigt lorsqu’ils passent simultanément sur l’écran. Une application classique est un programme finger-paint. Vous souhaitez que l’utilisateur pour être en mesure de dessiner avec un seul doigt, mais également à dessiner avec plusieurs doigts en une seule fois. Pendant que votre programme traite plusieurs événements tactiles, elle doit distinguer les événements qui correspondent à chaque doigt. Android fournit un code d’ID à cet effet, mais obtention et ce code de gestion peuvent être un peu difficile.

Pour tous les événements associés à un doigt particulier, le code de l’ID reste la même. Le code de l’ID est affecté lorsqu’un doigt touche l’écran tout d’abord et devient non valide, une fois que le doigt se soulève à partir de l’écran.
Ces codes d’ID sont généralement très petits entiers, et Android les réutilise pour les événements tactiles ultérieures.

Presque toujours, un programme qui effectue le suivi de chaque doigt gère un dictionnaire de suivi tactile. La clé de dictionnaire est le code d’ID qui identifie un doigt particulier. La valeur de dictionnaire dépend de l’application. Dans le [peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programme, chaque trait du doigt (de capacités tactiles release) est associé à un objet qui contient toutes les informations nécessaires à l’affichage de la ligne dessinée avec ce doigt. Le programme définit un petit `FingerPaintPolyline` classe à cet effet :

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

Chaque polyligne a une couleur, une épaisseur de trait et un graphique Android [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) objet à s’accumuler et afficher plusieurs points de la ligne comme elle est dessinée.

Le reste du code indiqué ci-dessous est contenu dans un `View` dérivée nommée `FingerPaintCanvasView`. Que classe gère un dictionnaire d’objets de type `FingerPaintPolyline` pendant l’heure à laquelle ils sont dessinés activement par un ou plusieurs doigts :

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Ce dictionnaire permet l’affichage pour obtenir rapidement la `FingerPaintPolyline` informations associées à un doigt particulier.

Le `FingerPaintCanvasView` classe gère également un `List` objet pour les polylignes qui ont été effectuées :

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Les objets dans ce `List` se trouvent dans le même ordre qu’ils ont été dessinés.

`FingerPaintCanvasView` remplace deux méthodes définies par `View`: [`OnDraw`](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/)
et [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
Dans son `OnDraw` override, la vue dessine les polylignes terminées, puis dessine les polylignes en cours d’exécution.

La substitution de la `OnTouchEvent` méthode commence en obtenant un `pointerIndex` valeur à partir de la `ActionIndex` propriété. Cela `ActionIndex` valeur établit une distinction entre plusieurs doigts, mais il n’est pas cohérente entre plusieurs événements. Pour cette raison, vous utilisez le `pointerIndex` pour obtenir le pointeur `id` valeur à partir de la `GetPointerId` (méthode). Cet ID *est* cohérente sur plusieurs événements :

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

Lorsque vous avez affaire à multipoint, les `Action` propriété a la valeur `MotionEventsAction.Down` pour le premier doigt toucher l’écran et les valeurs de `Pointer2Down` et `Pointer3Down` comme les deuxième et troisième doigts touchent également l’écran. Lorsque les doigts quatrième et cinquième prenez contact, le `Action` propriété a des valeurs numériques qui ne correspondent pas aux membres du même le `MotionEventsAction` énumération ! Vous devez examiner les valeurs des indicateurs de bits des valeurs à interpréter que leur signification.

De même, comme les doigts quittent le contact avec l’écran, le `Action` propriété a les valeurs `Pointer2Up` et `Pointer3Up` pour les doigts deuxième et troisième, et `Up` pour le premier doigt.

Le `ActionMasked` propriété prend un plus petit nombre de valeurs, car il est destiné à être utilisé conjointement avec le `ActionIndex` propriété faire la distinction entre plusieurs doigts. Lorsque les doigts touchent l’écran, la propriété peut uniquement être égale à `MotionEventActions.Down` pour le premier doigt et `PointerDown` pour les doigts suivantes. Comme les doigts quittent l’écran, `ActionMasked` a des valeurs de `Pointer1Up` pour les doigts suivantes et `Up` pour le premier doigt.

Lorsque vous utilisez `ActionMasked`, le `ActionIndex` fait la distinction entre les doigts ultérieures à tactiles et laissez l’écran, mais généralement, vous n’avez pas besoin d’utiliser cette valeur, sauf en tant qu’argument à d’autres méthodes dans le `MotionEvent` objet. Multipoint, une des plus importantes de ces méthodes est `GetPointerId` appelée dans le code ci-dessus. Que méthode retourne une valeur qui vous pouvez utiliser une clé de dictionnaire pour associer des événements spécifiques à des doigts.

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

Notez que le `pointerIndex` est également utilisé pour obtenir la position du doigt dans la vue. Toutes les informations de contact sont associées le `pointerIndex` valeur. Le `id` identifie doigts sur plusieurs messages, afin qu’utilisé pour créer l’entrée de dictionnaire.

De même, le `OnTouchEvent` remplacer également handles le `MotionEventActions.Up` et `Pointer1Up` identique en les transférant la polyligne terminée pour le `completedPolylines` collection afin qu’ils peuvent être dessinées pendant la `OnDraw` remplacer. Le code supprime également le `id` entrée à partir du dictionnaire :

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

Maintenant, pour la partie la plus difficile.

Entre le bas et les événements, en général il existe de nombreux `MotionEventActions.Move` événements. Ceux-ci sont regroupés dans un seul appel à `OnTouchEvent`, et elles doivent être traitées différemment à partir de la `Down` et `Up` événements. Le `pointerIndex` valeur obtenue précédemment dans le `ActionIndex` propriété doit être ignorée. Au lieu de cela, la méthode doit obtenir plusieurs `pointerIndex` valeurs en effectuant une boucle entre 0 et le `PointerCount` propriété, puis d’obtenir un `id` de chacun d’eux `pointerIndex` valeurs :

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

Ce type de traitement permet la [peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programme pour suivre chaque doigt et de dessiner les résultats à l’écran :

[![Capture d’écran de l’exemple à partir de l’exemple de la peinture avec les doigts](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Vous avez maintenant vu comment vous pouvez suivre chaque doigt sur l’écran et faire la distinction entre eux.


## <a name="related-links"></a>Liens associés

- [Guide d’équivalents iOS Xamarin](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
