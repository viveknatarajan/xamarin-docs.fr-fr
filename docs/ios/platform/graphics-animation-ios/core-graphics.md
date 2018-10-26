---
title: Graphismes de base dans Xamarin.iOS
description: Cet article décrit les infrastructures iOS de graphismes de base. Il montre comment utiliser des graphismes de base pour dessiner la géométrie, des images et des fichiers PDF.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f3fe22e56a2c45524923a316ef28e54e5a3cc3f8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102969"
---
# <a name="core-graphics-in-xamarinios"></a>Graphismes de base dans Xamarin.iOS

_Cet article décrit les infrastructures iOS de graphismes de base. Il montre comment utiliser des graphismes de base pour dessiner la géométrie, des images et des fichiers PDF._

iOS inclut la [ *Core Graphics* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework pour prendre en charge de dessin bas niveau. Ces infrastructures permettent les riches fonctionnalités de graphiques dans UIKit. 

Graphismes de base est une infrastructure de bas niveau graphique 2D qui permet le dessin graphiques indépendants du périphérique. Tous les 2D de dessin dans UIKit utilise en interne les graphismes de base.

Graphismes de base prend en charge le dessin dans plusieurs scénarios, notamment :

-  [Dessin à l’écran via un `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Dessin d’images en mémoire ou sur l’écran](#Drawing_Images_and_Text).
-  La création et de dessin à un fichier PDF.
-  Lecture et le dessin d’un fichier PDF existant.


## <a name="geometric-space"></a>Espace géométrique

Quel que soit le scénario, tout le dessin effectué avec des graphiques de base est effectué dans l’espace géométrique, ce qui signifie qu’il fonctionne dans les points abstraites plutôt que de pixels. Vous décrivez ce que vous voulez dessiné en termes de géométrie et de dessin d’état telles que les couleurs, styles de ligne, etc. et Core Graphics gère la traduction tout en pixels. Cet état est ajouté à un contexte de graphique, vous pouvez considérer comme zone de dessin d’un peintre.

Il existe quelques avantages de cette approche :

-  Code de dessin devient dynamique et si vous pouvez modifier par la suite de graphiques lors de l’exécution.
-  Réduisant la nécessité d’images statiques dans le bundle d’applications peut réduire la taille de l’application.
-  Graphiques deviennent plus résistantes aux modifications de résolution sur des appareils.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Dessin dans une sous-classe UIView

Chaque `UIView` a un `Draw` méthode est appelée par le système lorsqu’il doit être dessiné. Pour ajouter le code de dessin à une vue de sous-classe `UIView` et remplacer `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Dessin ne doit jamais être appelé directement. Elle est appelée par le système au cours du traitement de la boucle d’exécution. La première fois dans la boucle d’exécution après une vue est ajoutée à la hiérarchie d’affichage, son `Draw` méthode est appelée. Les appels suivants à `Draw` se produisent lorsque la vue est marquée comme devant être dessiné en appelant `SetNeedsDisplay` ou `SetNeedsDisplayInRect` sur la vue.

### <a name="pattern-for-graphics-code"></a>Modèle de Code graphique

Le code dans le `Draw` implémentation doit décrire ce qu’elle veut dessiné. Le code de dessin suit un modèle dans lequel il définit un état de dessin et appelle une méthode pour demander à elle être dessinée. Ce modèle peut être étendue comme suit :

1. Obtenir un contexte graphique.

2. Configurer les attributs de dessin.

3. Créer une géométrie à partir de primitives de dessin.

4. Appeler une méthode de dessin ou au trait.

### <a name="basic-drawing-example"></a>Exemple de dessins de base

Par exemple, considérez l’extrait de code suivant :

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Analysons ce code :

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Avec cette ligne, il obtient d’abord le contexte graphique actuel à utiliser pour le dessin. Vous pouvez considérer un contexte graphique en tant que la zone de dessin que dessin se produit, qui contient tous les États sur le dessin, tels que le trait et les couleurs de remplissage, ainsi que la géométrie à dessiner.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Après avoir obtenu un contexte graphique le code configure certains attributs à utiliser lors du dessin, illustré ci-dessus. Dans ce cas, les couleurs de la largeur, de trait et de remplissage de ligne sont définies. Les dessins suivants utilisera ensuite ces attributs, car ils sont gérés dans un état de contexte de graphique.

Pour créer une géométrie le code utilise un `CGPath`, ce qui permet un chemin d’accès de graphiques à décrire des lignes et des courbes. Dans ce cas, le chemin d’accès ajoute des lignes qui connectent un tableau de points pour créer un triangle. Tel qu’affiché ci-dessous graphismes de base utilise un système de coordonnées pour la vue de dessin, dont l’origine est en haut à gauche, avec x-direct positif vers la droite et le sens positif-y vers le bas :

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Une fois que le chemin d’accès est créé, il est ajouté au contexte graphique afin que l’appel `AddPath` et `DrawPath` respectivement peut dessinez-le.

La vue résultante est indiquée ci-dessous :

 ![](core-graphics-images/00-bluetriangle.png "Le triangle de sortie d’exemple")

## <a name="creating-gradient-fills"></a>Création des dégradés

Formes plus riches de dessin sont également disponibles. Par exemple, graphismes de base permet de créer des dégradés et appliquer les tracés de détourage. Pour dessiner un dégradé dans le chemin d’accès de l’exemple précédent, le chemin d’accès doit tout d’abord être définie en tant que le tracé de détourage :

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Définir le chemin d’accès actuel comme le tracé de détourage contraint tous les dessins suivants dans la géométrie du chemin d’accès, telles que le code suivant, qui dessine un dégradé linéaire :

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Ces modifications génèrent un dégradé comme indiqué ci-dessous :

 ![](core-graphics-images/01-gradient-fill.png "L’exemple avec un dégradé")

## <a name="modifying-line-patterns"></a>Modification des modèles de ligne

Les attributs de dessin de lignes peuvent également être modifiées avec graphismes de base. Cela inclut la modification de la couleur de trait et de la largeur de ligne, ainsi que le modèle de ligne lui-même, comme indiqué dans le code suivant :

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Ajout de ce code avant des résultats des opérations de dessin en unités de traits en pointillés 10 longues, avec 4 unités de l’espacement entre les tirets, comme indiqué ci-dessous :

 ![](core-graphics-images/02-dashed-stroke.png "Ajout de ce code avant des résultats des opérations de dessin de traits en pointillés")
 
Notez que lorsque vous utilisez l’API unifiée dans Xamarin.iOS, le type de tableau doit être un `nfloat`et doit également être explicitement converti à Math.PI.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Texte et Images de dessin

Outre le dessin de chemins d’accès dans le contexte du graphique de l’affichage, Core Graphics prend également en charge les images et du texte dessiné. Pour dessiner une image, créez simplement un `CGImage` et transmettez-le à un `DrawImage` appeler :

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Toutefois, cela génère une image dessinée de haut en bas, comme indiqué ci-dessous :

 ![](core-graphics-images/03-upside-down-monkey.png "Une image dessinée à l’envers")

La raison à cela est Core graphique a son origine pour le dessin de l’image dans l’angle inférieur gauche, tandis que la vue a son origine dans le coin supérieur gauche. Par conséquent, pour afficher l’image correctement, l’origine doit être modifié, ce qui peut être accompli en modifiant le *matrice de Transformation actuelle* *(CTM)*. Le CTM définit où points en direct, également appelé *espace utilisateur*. Inverser la CTM sur l’axe y et un décalage par la hauteur des limites sur l’axe y négatif peuvent retourner l’image.

Le contexte graphique a des méthodes d’assistance pour transformer le CTM. Dans ce cas, `ScaleCTM` « retourne « le dessin et `TranslateCTM` déplace vers l’angle supérieur gauche, comme indiqué ci-dessous :

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

L’image obtenue est ensuite affiché verticalement :

 ![](core-graphics-images/04-upright-monkey.png "Verticale de l’image affichée exemple")

> [!IMPORTANT]
> Modifications apportées au contexte graphique s’appliquent à toutes les opérations de dessins suivantes. Par conséquent, lorsque le CTM est transformé, cela affectera tous les dessins supplémentaires. Par exemple, si vous avez dessiné le triangle après la transformation CTM, elle apparaît à l’envers.

### <a name="adding-text-to-the-image"></a>Ajout de texte à l’Image

Comme avec les chemins d’accès et des images, dessiner du texte avec des graphiques de Core implique le même modèle de base de la définition de l’état de certains graphiques et en appelant une méthode pour dessiner. Dans le cas de texte, la méthode pour afficher le texte est `ShowText`. Lors de l’ajout à l’exemple de dessin de l’image, le code suivant dessine du texte à l’aide de graphiques élémentaires :

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Comme vous pouvez le voir, la définition de l’état des graphiques pour le dessin de texte est similaire au dessin de géométrie. Pour le dessin de texte Toutefois, le mode et la police de dessin de texte sont également appliqués. Dans ce cas, une ombre est également appliquée, bien que l’application d’ombres fonctionne de la même pour le chemin d’accès de dessin.

Le texte résultant s’affiche avec l’image, comme indiqué ci-dessous :

 ![](core-graphics-images/05-text-on-image.png "Le texte résultant s’affiche avec l’image")

## <a name="memory-backed-images"></a>Images reposant sur la mémoire

En plus de dessin pour le contexte du graphique de l’affichage, graphismes de base prend en charge la mémoire de dessin bénéficie des images, également appelé dessin hors écran. Ceci nécessite :

-  Création d’un contexte graphique qui repose sur une mémoire bitmap
-  Définir l’état de dessin et émettre des commandes de dessins
-  Obtention de l’image à partir du contexte
-  Suppression du contexte


Contrairement à la `Draw` (méthode), où le contexte est fourni par la vue, dans ce cas vous créer le contexte de deux manières :

1. En appelant `UIGraphics.BeginImageContext` (ou `BeginImageContextWithOptions`)

2. En créant un nouveau `CGBitmapContextInstance`

 `CGBitmapContextInstance` est utile lorsque vous travaillez directement avec les bits d’image, comme pour les cas où vous utilisez un algorithme de manipulation d’image personnalisée. Dans tous les autres cas, vous devez utiliser `BeginImageContext` ou `BeginImageContextWithOptions`.

Une fois que vous avez un contexte de l’image, ajout de code de dessin est comme si elle se trouve dans un `UIView` sous-classe. Par exemple, l’exemple de code utilisé précédemment pour dessiner un triangle peut être utilisé pour dessiner une image en mémoire plutôt que dans un `UIView`, comme illustré ci-dessous :

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

Une utilisation courante du dessin d’une bitmap reposant sur la mémoire consiste à capturer une image à partir d’un `UIView`. Par exemple, le code suivant restitue la couche d’un affichage à un contexte de bitmap et crée un `UIImage` à partir de celui-ci :

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Dessin d’un fichier PDF

En plus des images, graphismes de base prend en charge le dessin de PDF. Comme les images, vous pouvez restituer un PDF dans la mémoire ainsi que lire un fichier PDF pour le rendu dans un `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF dans un UIView

Graphismes de base prend également en charge la lecture d’un fichier PDF à partir d’un fichier et de restitution dans une vue en utilisant la `CGPDFDocument` classe. Le `CGPDFDocument` classe représente un fichier PDF dans le code et peut être utilisé pour lire et dessiner des pages.

Par exemple, le code suivant dans un `UIView` sous-classe lit un fichier PDF à partir d’un fichier dans un `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

Le `Draw` méthode peut ensuite utiliser le `CGPDFDocument` pour lire une page dans `CGPDFPage` et affichez-la en appelant `DrawPDFPage`, comme illustré ci-dessous :

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>PDF reposant sur la mémoire

Pour un fichier PDF en mémoire, vous devez créer un contexte PDF en appelant `BeginPDFContext`. Dessin au format PDF est précis aux pages. Chaque page est lancée en appelant `BeginPDFPage` et terminée en appelant `EndPDFContent`, avec les graphiques de code entre les deux. En outre, comme avec le dessin de l’image, mémoire soutenu PDF dessin utilise une origine dans l’angle inférieur gauche, ce qui peut être pris en compte en modifiant le CTM simplement comme des images.

Le code suivant montre comment dessiner du texte sur un fichier PDF :

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

Le texte résultant est dessiné sur le fichier PDF, puis contenue dans un `NSData` qui peut être enregistré, chargé, envoyé par courrier électronique, etc.


## <a name="summary"></a>Récapitulatif

Dans cet article nous avons étudié les possibilités de graphiques fournies par le biais de la *Core Graphics* framework. Nous avons vu comment utiliser les graphismes de base pour dessiner la géométrie, des images et des fichiers PDF dans le contexte d’un `UIView,` ainsi qu’aux contextes de graphiques reposant sur la mémoire.

## <a name="related-links"></a>Liens associés

- [Exemple de graphique Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Graphismes et Animation procédure pas à pas](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Animation de base](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Recettes d’Animation de base](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
