---
title: Suivi de tactiles doigt
description: "Ce document montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts"
ms.topic: article
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: a9e3842611aab86d23a2b0c2a832efce18c22465
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="multi-touch-finger-tracking"></a>Suivi de tactiles doigt

_Ce document montre comment effectuer le suivi des événements tactiles à partir de plusieurs doigts_

Il existe des cas lorsqu’une application tactile doit effectuer le suivi des doigts individuels lors de leur déplacement simultanément sur l’écran. Une application classique est un programme finger-paint. Vous souhaitez que l’utilisateur pour être en mesure de dessiner avec un seul doigt, mais également pour dessiner avec plusieurs doigts à la fois. Comme votre programme traite plusieurs événements tactiles, il doit faire la distinction entre ces doigts.

Lorsqu’un doigt touche l’écran en premier, iOS crée un [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) objet pour ce doigt. Cet objet reste le même que le doigt déplace à l’écran et puis soulève à partir de l’écran, à partir de laquelle l’objet est supprimé. Pour effectuer le suivi des doigts, un programme doit éviter de stocker cette `UITouch` directement l’objet. Au lieu de cela, il peut utiliser le [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) propriété de type `IntPtr` pour identifier ces `UITouch` objets.

Presque toujours, un programme qui effectue le suivi des doigts maintient un dictionnaire pour les fonctions tactiles de suivi. Un programme iOS, la clé de dictionnaire est le `Handle` valeur qui identifie un doigt particulier. La valeur de dictionnaire dépend de l’application. Dans le [peinture avec les doigts](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) programme, chaque tracé doigt (à partir de tactile à libérer) est associé à un objet qui contient toutes les informations nécessaires pour restituer la ligne dessinée avec ce doigt. Le programme définit un petit `FingerPaintPolyline` classe à cet effet :

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

Chaque polyligne a une couleur, une épaisseur de trait et un graphique iOS [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) objet à s’accumuler et à rendre plusieurs points de la ligne comme elle est dessinée.


Le reste du code ci-dessous est contenu dans un `UIView` dérivé nommé `FingerPaintCanvasView`. Que classe maintenance un dictionnaire d’objets de type `FingerPaintPolyline` pendant l’heure à laquelle ils sont dessinés activement par un ou plusieurs doigts :

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Ce dictionnaire permet l’affichage pour obtenir rapidement la `FingerPaintPolyline` les informations associées à chaque doigt basées sur le `Handle` propriété de la `UITouch` objet.

Le `FingerPaintCanvasView` classe gère également une `List` objet pour les polylignes qui ont été effectuées :

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Les objets dans ce `List` se trouvent dans le même ordre qu’ils ont été dessinées.

`FingerPaintCanvasView` remplace les cinq méthodes définies par `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Les différentes `Touches` remplacements s’accumulent les points qui composent les polylignes.

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

Chacun de la `Touches` remplacements signale potentiellement les actions de plusieurs doigts, indiquées par un ou plusieurs `UITouch` objets stockés dans le `touches` argument à la méthode. Le `TouchesBegan` parcourir les remplacements de ces objets. Pour chaque `UITouch` de l’objet, la méthode crée et initialise un nouveau `FingerPaintPolyline` objet, y compris le stockage de l’emplacement d’origine du doigt obtenu à partir de la `LocationInView` (méthode). Cela `FingerPaintPolyline` objet est ajouté à la `InProgressPolylines` à l’aide du dictionnaire la `Handle` propriété de la `UITouch` objet comme clé de dictionnaire :

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

Comme le doigt ou doigts déplacement sur l’écran, la `View` Obtient plusieurs appels à ses `TouchesMoved` remplacer. Cette substitution de la même façon parcourt le `UITouch` objets stockés dans le `touches` argument et ajoute l’emplacement actuel du doigt pour le chemin d’accès des graphiques :

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

Le `touches` collection contient uniquement les `UITouch` objets pour les doigts qui ont été déplacés depuis le dernier appel à `TouchesBegan` ou `TouchesMoved`. Si vous avez besoin de `UITouch` objets correspondant aux *tous les* doigts actuellement en contact avec l’écran, ces informations sont disponibles via le `AllTouches` propriété de la `UIEvent` argument à la méthode.

Le `TouchesEnded` substitution a deux tâches. Il doit ajouter le dernier point sur le chemin d’accès des graphiques et un transfert le `FingerPaintPolyline` de l’objet à partir de la `inProgressPolylines` dictionnaire pour la `completedPolylines` liste :

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

Le `TouchesCancelled` override est géré par abandonner simplement le `FingerPaintPolyline` objet dans le dictionnaire :

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

Complètement, ce traitement permet la [peinture avec les doigts](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) programme pour effectuer le suivi des doigts individuels et dessiner les résultats à l’écran :

[![](touch-tracking-images/image01.png "Suivi des doigts individuels et les résultats de dessin à l’écran")](touch-tracking-images/image01.png#lightbox)

Vous avez maintenant vu comment vous pouvez effectuer le suivi des doigts sur l’écran et faire la distinction entre eux.



## <a name="related-links"></a>Liens associés

- [Guide de Xamarin Android équivalent](~/android/app-fundamentals/touch/touch-tracking.md)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
