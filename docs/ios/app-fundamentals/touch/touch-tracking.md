---
title: Doigt de l’interaction tactile multipoint suivi des modifications dans Xamarin.iOS
description: Ce document décrit comment effectuer le suivi des doigts individuels dans les entrées tactiles multipoints dans une application Xamarin.iOS. Il se concentre sur un exemple d’application de peinture.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a1ddcda84d51b5a8a9220558ddaf9476a2321ee8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105049"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Doigt de l’interaction tactile multipoint suivi des modifications dans Xamarin.iOS

_Ce document montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts_

Il est parfois quand une application tactile multipoint doit suivre chaque doigt lorsqu’ils passent simultanément sur l’écran. Une application classique est un programme finger-paint. Vous souhaitez que l’utilisateur pour être en mesure de dessiner avec un seul doigt, mais également à dessiner avec plusieurs doigts en une seule fois. Pendant que votre programme traite plusieurs événements tactiles, elle doit faire la distinction entre ces doigts.

Lorsqu’un doigt touche l’écran en premier, iOS crée un [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) objet pour ce doigt. Cet objet reste la même que le doigt se déplace sur l’écran et puis soulève à partir de l’écran, à quel point l’objet est supprimé. Pour suivre les doigts, un programme doit éviter de stocker cette `UITouch` directement l’objet. Au lieu de cela, il peut utiliser le [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) propriété de type `IntPtr` pour identifier ces `UITouch` objets.

Presque toujours, un programme qui effectue le suivi de chaque doigt gère un dictionnaire de suivi tactile. Pour un programme iOS, la clé de dictionnaire est le `Handle` valeur qui identifie un doigt particulier. La valeur de dictionnaire dépend de l’application. Dans le [peinture avec les doigts](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) programme, chaque trait du doigt (de capacités tactiles release) est associé à un objet qui contient toutes les informations nécessaires à l’affichage de la ligne dessinée avec ce doigt. Le programme définit un petit `FingerPaintPolyline` classe à cet effet :

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Chaque polyligne a une couleur, une épaisseur de trait et un graphique iOS [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) objet à s’accumuler et afficher plusieurs points de la ligne comme elle est dessinée.


Le reste du code indiqué ci-dessous est contenu dans un `UIView` dérivée nommée `FingerPaintCanvasView`. Que classe gère un dictionnaire d’objets de type `FingerPaintPolyline` pendant l’heure à laquelle ils sont dessinés activement par un ou plusieurs doigts :

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Ce dictionnaire permet l’affichage pour obtenir rapidement la `FingerPaintPolyline` informations associées à chaque doigt selon le `Handle` propriété de la `UITouch` objet.

Le `FingerPaintCanvasView` classe gère également un `List` objet pour les polylignes qui ont été effectuées :

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Les objets dans ce `List` se trouvent dans le même ordre qu’ils ont été dessinés.

`FingerPaintCanvasView` remplace les cinq méthodes définies par `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Les différents `Touches` remplacements s’accumulent les points qui composent les polylignes.

Le [`Draw`] remplacement dessine les polylignes terminées, puis les polylignes en cours d’exécution :

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Chacun de la `Touches` remplacements signale potentiellement les actions de plusieurs doigts, indiquées par un ou plusieurs `UITouch` objets stockés dans le `touches` argument à la méthode. Le `TouchesBegan` parcourir les remplacements de ces objets. Pour chaque `UITouch` de l’objet, la méthode crée et initialise un nouvel `FingerPaintPolyline` objet, y compris le stockage de l’emplacement initial du doigt obtenu à partir de la `LocationInView` (méthode). Cela `FingerPaintPolyline` objet est ajouté à la `InProgressPolylines` à l’aide du dictionnaire la `Handle` propriété de la `UITouch` objet sous la forme d’une clé de dictionnaire :

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

La méthode se termine en appelant `SetNeedsDisplay` pour générer un appel à la `Draw` remplacer et mettre à jour l’écran.

Comme le doigt ou les doigts se déplacement sur l’écran, le `View` Obtient plusieurs appels à ses `TouchesMoved` remplacer. Cette substitution de la même façon parcourt en boucle le `UITouch` objets stockés dans le `touches` argument et ajoute l’emplacement actuel du doigt pour le chemin d’accès de graphiques :

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

Le `touches` collection contient uniquement les `UITouch` objets pour les doigts qui ont été déplacées depuis le dernier appel à `TouchesBegan` ou `TouchesMoved`. Si vous avez besoin de `UITouch` correspondant à des objets *tous les* les doigts actuellement au contact de l’écran, ces informations sont disponibles via le `AllTouches` propriété de la `UIEvent` argument à la méthode.

Le `TouchesEnded` remplacement exécute deux tâches. Il doit ajouter le dernier point sur le tracé graphique et un transfert la `FingerPaintPolyline` de l’objet à partir de la `inProgressPolylines` dictionnaire à la `completedPolylines` liste :

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

Le `TouchesCancelled` override est gérée par simplement abandonner le `FingerPaintPolyline` objet dans le dictionnaire :

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

Complètement, ce traitement permet la [peinture avec les doigts](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) programme pour suivre chaque doigt et de dessiner les résultats à l’écran :

[![](touch-tracking-images/image01.png "Suivre chaque doigt et dessiner les résultats sur l’écran")](touch-tracking-images/image01.png#lightbox)

Vous avez maintenant vu comment vous pouvez suivre chaque doigt sur l’écran et faire la distinction entre eux.



## <a name="related-links"></a>Liens associés

- [Équivalent guide Xamarin Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
