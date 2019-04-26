---
title: TextKit dans Xamarin.iOS
description: Ce document décrit comment utiliser TextKit dans Xamarin.iOS. TextKit fournit des fonctionnalités de mise en page et le rendu de texte puissant.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: f08e37d17cc32e45232d54cc4a51bb48d7ec94b1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61184519"
---
# <a name="textkit-in-xamarinios"></a>TextKit dans Xamarin.iOS

TextKit est une nouvelle API qui offre des fonctionnalités de mise en page et rendu de texte puissant. Il repose sur le framework Core texte de bas niveau, mais il est beaucoup plus facile à utiliser que le texte de base.

Pour rendre les fonctionnalités de TextKit disponible pour les contrôles standard, plusieurs contrôles de texte iOS ont été réimplémentées à utiliser TextKit, y compris :

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>Architecture

TextKit fournit une architecture en couches qui sépare le stockage de texte à partir de la disposition et l’affichage, y compris les classes suivantes :

-  `NSTextContainer` – Fournit le système de coordonnées et la géométrie qui est utilisé pour le texte de la mise en page.
-  `NSLayoutManager` – Distille texte en activant le texte dans des glyphes. 
-  `NSTextStorage` : Contient les données texte, ainsi que gère les mises à jour de propriété de texte batch. Les mises à jour par lots sont transmis au Gestionnaire de disposition pour le traitement des modifications, telles que recalculer la disposition et le rafraîchissement du dessin du texte.


Ces trois classes sont appliquées à une vue qui restitue le texte. Le texte intégré, gestion des vues, telles que `UITextView`, `UITextField`, et `UILabel` en avez pas défini, mais vous pouvez créer et les appliquer à tout `UIView` instance également.

La figure suivante illustre cette architecture :

 ![](textkit-images/textkitarch.png "Cette figure illustre l’architecture TextKit")

## <a name="text-storage-and-attributes"></a>Attributs et stockage de texte

Le `NSTextStorage` classe contient le texte affiché par une vue. Il communique également toutes les modifications au texte - telles que les modifications de caractères ou de leurs attributs : pour le Gestionnaire de disposition pour l’affichage. `NSTextStorage` hérite de `MSMutableAttributed` chaîne, ce qui permet des modifications apportées aux attributs de texte doit être spécifié dans les lots entre `BeginEditing` et `EndEditing` appels.

Par exemple, l’extrait de code suivant spécifie une modification au premier plan et arrière-plan, respectivement et certaines gammes de cible :

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Après avoir `EndEditing` est appelée, les modifications sont envoyées au Gestionnaire de présentation, qui à son tour effectue toute disposition nécessaire et les calculs de rendu pour le texte à afficher dans la vue.

## <a name="layout-with-exclusion-path"></a>Mise en page avec le chemin d’Exclusion

TextKit également prend en charge la mise en page et permet des scénarios complexes, telles que plusieurs colonne texte et placer du texte autour des chemins d’accès spécifiés appelé *chemins d’accès d’exclusion*. Chemins d’accès d’exclusion sont appliquées pour le conteneur de texte, ce qui modifie la géométrie de la disposition de texte, ce qui entraîne le texte à circuler dans les chemins d’accès spécifiés.

Ajout d’un chemin d’accès d’exclusion requiert la définition du `ExclusionPaths` propriété sur le Gestionnaire de disposition. Cette propriété, le Gestionnaire de disposition invalider la disposition de texte et de placer le texte autour du chemin d’exclusion.

### <a name="exclusion-based-on-a-cgpath"></a>Exclure selon une CGPath

Considérez les éléments suivants `UITextView` implémentation de sous-classe :

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Ce code ajoute la prise en charge permettant de dessiner sur l’affichage de texte à l’aide des graphismes de base. Dans la mesure où la `UITextView` classe est désormais intégrée pour utiliser TextKit pour son rendu de texte et la disposition, il peut utiliser toutes les fonctionnalités de TextKit, telles que la définition des chemins d’accès d’exclusion.

> [!IMPORTANT]
> Sous-classes de cet exemple `UITextView` pour ajouter une touche de prise en charge de dessin. Sous-classement `UITextView` n’est pas nécessaire pour obtenir les fonctionnalités de TextKit.



Une fois que l’utilisateur dessine sur l’affichage de texte, le texte dessiné `CGPath` est appliqué à un `UIBezierPath` instance en définissant le `UIBezierPath.CGPath` propriété :

```csharp
bezierPath.CGPath = exclusionPath;
```

La mise à jour de la ligne de code suivante rend la disposition du texte à mettre à jour dans le chemin d’accès :

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

La capture d’écran suivante illustre comment la disposition de texte change pour transmettre le chemin d’accès dessinée autour de :

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "Cette capture d’écran illustre comment la disposition de texte change pour transmettre le chemin d’accès dessinée autour de")

Notez que le Gestionnaire de disposition `AllowsNonContiguousLayout` propriété est définie sur false dans ce cas. Cela entraîne la mise en page à être recalculé, pour tous les cas où le texte est modifié. Valeur true peut bénéficier des performances en évitant une actualisation complète à disposition, en particulier dans le cas de documents volumineux. Toutefois, l’affectation `AllowsNonContiguousLayout` à true empêcheraient le chemin d’exclusion de la mise à jour de la mise en page dans certaines circonstances - par exemple, si le texte est entré lors de l’exécution sans retour de chariot fin avant le chemin d’accès définie.


## <a name="related-links"></a>Liens associés

- [Introduction à iOS 7 (exemple)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Vue d'ensemble de l'interface utilisateur iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
