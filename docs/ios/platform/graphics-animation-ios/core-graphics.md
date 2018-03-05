---
title: Graphique de base
description: "Cet article décrit les infrastructures iOS de graphiques de base. Il montre comment utiliser des graphiques de base pour dessiner la géométrie, des images et PDF."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fe1796839524a271760a9beb82895fd6e93c7ad0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="core-graphics"></a>Graphique de base

_Cet article décrit les infrastructures iOS de graphiques de base. Il montre comment utiliser des graphiques de base pour dessiner la géométrie, des images et PDF._

e/s inclut le [ *graphiques élémentaires* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework pour prendre en charge de dessin bas niveau. Ces infrastructures ne font qu’activer les fonctionnalités de graphiques riches dans UIKit. 

Graphique de base est une infrastructure de graphique 2D de bas niveau qui permet de dessin graphiques indépendants du périphérique. Dessin dans UIKit 2D tous les utilise en interne les graphiques de base.

Graphiques de base prend en charge le dessin dans plusieurs scénarios, notamment :

-  [Dessin à l’écran via un `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Dessin d’images en mémoire ou sur l’écran](#Drawing_Images_and_Text).
-  La création et de dessin à un document PDF.
-  La lecture et le dessin d’un fichier PDF existant.


## <a name="geometric-space"></a>Espace géométrique

Quel que soit le scénario, tout le dessin effectué avec des graphiques de base est effectué dans l’espace géométrique, ce qui signifie qu’il fonctionne dans les points abstraites plutôt que de pixels. Vous décrivez ce que vous souhaitez dessiné en termes de géométrie et d’état telles que les couleurs, styles de ligne, de dessin et graphiques élémentaires gère tout traduire en pixels. Cet état est ajouté à un contexte de graphique, vous pouvez considérer comme zone de dessin d’un peintre.

Il existe quelques avantages de cette approche :

-  Code de dessin devient dynamique, ainsi que par la suite modifier graphique lors de l’exécution.
-  Réduisant la nécessité d’images statiques dans l’offre groupée d’application peut réduire la taille de l’application.
-  Graphiques deviennent plus résistante face aux modifications de résolution pour les appareils.


## <a name="drawing-in-a-uiview-subclass"></a>Dessin dans une sous-classe UIView

Chaque `UIView` a un `Draw` méthode est appelée par le système lorsqu’il doit être dessiné. Pour ajouter le code de dessin à une vue, sous-classe `UIView` et remplacer `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Dessin ne doit jamais être appelée directement. Elle est appelée par le système lors du traitement de la boucle d’exécution. La première fois dans la boucle d’exécution après l’ajout d’une vue à la hiérarchie de la vue, sa `Draw` méthode est appelée. Les appels suivants à `Draw` se produisent lorsque la vue est marquée comme devant être dessiné en appelant `SetNeedsDisplay` ou `SetNeedsDisplayInRect` sur la vue.

### <a name="pattern-for-graphics-code"></a>Modèle pour le Code de graphiques

Le code dans le `Draw` implémentation doit décrire ce qu’elle veut dessinée. Le code de dessin suit un modèle dans lequel elle définit un état de dessin et appelle une méthode pour demander qu’elle est dessinée. Ce modèle peut être généralisé comme suit :

1. Obtenir un contexte graphique.

2. Configurer les attributs de dessin.

3. Créer une géométrie à partir de primitives de dessin.

4. Appeler une méthode de dessin ou de trait.

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
Avec cette ligne, il obtient d’abord le contexte graphique actuel à utiliser pour le dessin. Vous pouvez considérer un contexte graphique en tant que la zone de dessin que dessin se produit, qui contient tous les États sur le dessin, tels que le contour et les couleurs de remplissage, ainsi que la géométrie à dessiner.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Après obtention d’un contexte graphique le code configure certains attributs à utiliser lors du dessin, indiqué ci-dessus. Dans ce cas, les largeur, trait et remplissage couleurs sont définies. Un dessin ultérieur utilise ces attributs, car elles sont gérées dans l’état de contexte graphique.

Pour créer une géométrie le code utilise un `CGPath`, ce qui permet à un chemin d’accès de graphiques représenter des lignes et des courbes. Dans ce cas, le chemin d’accès ajoute des lignes qui connectent un tableau de points pour créer un triangle. Tel qu’affiché ci-dessous graphiques élémentaires utilise un système de coordonnées de la vue de dessin, dont l’origine est en haut à gauche, avec positif x-direct vers la droite et la direction-y positif vers le bas :

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Une fois que le chemin d’accès est créé, il est ajouté au contexte graphique afin que l’appel `AddPath` et `DrawPath` respectivement peut dessiner il.

La vue résultante est indiquée ci-dessous :

 ![](core-graphics-images/00-bluetriangle.png "Le triangle de sortie exemple")

## <a name="creating-gradient-fills"></a>Création des dégradés

Les formes plus riches de dessin sont également disponibles. Par exemple, graphiques de base permet de créer des dégradés et appliquer les tracés de détourage. Pour dessiner un dégradé dans le chemin d’accès de l’exemple précédent, le chemin d’accès doit tout d’abord être définie en tant que le chemin d’accès de l’extrait :

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Définir le chemin d’accès actuel comme du tracé de détourage contraint tous les dessins suivants dans la géométrie du chemin d’accès, telles que le code suivant, qui dessine un dégradé linéaire :

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

Les attributs de dessin de lignes peuvent également être modifiés avec les graphiques principaux. Cela inclut la modification de la couleur du trait et la largeur de ligne, ainsi que le modèle de ligne, comme indiqué dans le code suivant :

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Ajout de ce code avant des résultats des opérations de dessin en unités de traits en pointillés 10 longues, avec 4 unités de l’espacement entre les tirets, comme indiqué ci-dessous :

 ![](core-graphics-images/02-dashed-stroke.png "Ajout de ce code avant les résultats des opérations de dessin de traits en pointillés")
 
Notez que lorsque vous utilisez l’API unifiée dans Xamarin.iOS, le type de tableau doit être un `nfloat`et doit également être explicitement converties en Math.PI.

## <a name="drawing-images-and-text"></a>Texte et des Images de dessin

En plus de dessin de tracés dans le contexte d’une vue du graphique, graphiques élémentaires prend également en charge les images et du texte dessiné. Pour dessiner une image, créez simplement un `CGImage` et passer à un `DrawImage` appeler :

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

 ![](core-graphics-images/03-upside-down-monkey.png "Une image dessinée envers")

La raison à cela est origine du graphique de base pour le dessin de l’image dans l’angle inférieur gauche, tandis que la vue a son origine dans le coin supérieur gauche. Par conséquent, pour afficher l’image correctement, l’origine devant être modifiée, ce qui peut être accompli en modifiant le *matrice de Transformation actuelle* *(CTM)*. Le CTM définit où les points de live, également appelé *espace utilisateur*. Inverser la CTM sur l’axe y et les déplaçant par la hauteur des limites sur l’axe y négatif peuvent retourner l’image.

Contexte graphique a des méthodes d’assistance pour transformer le CTM. Dans ce cas, `ScaleCTM` « retourne « le dessin et `TranslateCTM` déplace vers l’angle supérieur gauche, comme indiqué ci-dessous :

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

L’image résultante est ensuite affiché verticalement :

 ![](core-graphics-images/04-upright-monkey.png "Le montant d’image affichée exemple")

> [!IMPORTANT]
>  **Remarque :** modifications apportées au contexte graphique s’appliquent à toutes les opérations de dessin. Par conséquent, lorsque le CTM est transformé, il affecte un dessin supplémentaire. Par exemple, si vous avez dessiné le triangle après la transformation CTM, il apparaîtrait envers.

### <a name="adding-text-to-the-image"></a>Ajout de texte à l’Image

Comme avec les chemins d’accès et les images, dessiner du texte avec des graphiques de base implique le même modèle de base de l’état de certains graphiques et d’appel d’une méthode pour dessiner. Dans le cas de texte, la méthode pour afficher le texte est `ShowText`. Lors de l’ajout de l’exemple de dessin de l’image, le code suivant dessine du texte à l’aide de graphiques de base :

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

Comme vous pouvez le voir, l’état du graphique pour le dessin de texte est proche de la géométrie de dessin. Pour le dessin de texte Toutefois, le mode et la police de dessin de texte sont également appliqués. Dans ce cas, une ombre est également appliquée, bien que l’application d’ombres fonctionne de la même pour le chemin d’accès de dessin.

Le texte résultant s’affiche avec l’image, comme indiqué ci-dessous :

 ![](core-graphics-images/05-text-on-image.png "Le texte résultant s’affiche avec l’image")

## <a name="memory-backed-images"></a>Reposant sur la mémoire des Images

En plus de dessin au contexte de graphiques d’une vue, prend en charge les graphiques principaux dessin mémoire sauvegardé images, également appelé dessin hors écran. Cela nécessite :

-  Création d’un contexte graphique qui est soutenu par une mémoire bitmap
-  Définir l’état de dessin et émettre des commandes de dessin
-  Mise en route de l’image à partir du contexte
-  Suppression du contexte


Contrairement à la `Draw` (méthode), où le contexte est fourni par la vue, dans ce cas vous créez le contexte de l’une des deux façons :

1. En appelant `UIGraphics.BeginImageContext` (ou `BeginImageContextWithOptions`)

2. En créant un nouveau `CGBitmapContextInstance`

 `CGBitmapContextInstance` est utile lorsque vous travaillez directement avec les bits d’image, comme pour les cas où vous utilisez un algorithme de manipulation d’image personnalisée. Dans tous les autres cas, vous devez utiliser `BeginImageContext` ou `BeginImageContextWithOptions`.

Une fois que vous avez un contexte de l’image, ajout de code de dessin est tout comme il se trouve dans un `UIView` sous-classe. Par exemple, l’exemple de code utilisé précédemment pour dessiner un triangle peut être utilisé pour dessiner une image en mémoire plutôt que dans un `UIView`, comme indiqué ci-dessous :

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

Une utilisation courante de dessin à une image bitmap reposant sur la mémoire est de capturer une image de n’importe quel `UIView`. Par exemple, le code suivant restitue la couche d’un affichage à un contexte de bitmap et crée un `UIImage` à partir de celui-ci :

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Dessin d’un fichier PDF

En plus des images, des graphiques de base prend en charge le dessin de PDF. Comme des images, vous pouvez restituer un fichier PDF dans la mémoire ainsi que lire un fichier PDF pour le rendu dans un `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF dans un UIView

Graphiques de base prend également en charge la lecture d’un fichier PDF à partir d’un fichier et de restitution dans une vue à l’aide de la `CGPDFDocument` classe. La `CGPDFDocument` classe représente un fichier PDF dans le code et peut être utilisé pour lire et dessiner des pages.

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

Le `Draw` méthode peut ensuite utiliser le `CGPDFDocument` pour lire une page dans `CGPDFPage` et de l’afficher en appelant `DrawPDFPage`, comme indiqué ci-dessous :

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

### <a name="memory-backed-pdf"></a>Reposant sur la mémoire PDF

Pour un fichier PDF en mémoire, vous devez créer un contexte de format PDF en appelant `BeginPDFContext`. Dessin au format PDF est granulaire aux pages. Chaque page est lancée en appelant `BeginPDFPage` et terminée en appelant `EndPDFContent`, avec les graphiques de code entre les deux. En outre, comme le dessin de l’image, mémoire sauvegardé PDF dessin utilise une origine dans l’angle inférieur gauche, ce qui peut être pris en compte en modifiant le CTM simplement comme des images.

Le code suivant montre comment dessiner du texte à un document PDF :

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

Le texte résultant est dessiné dans le fichier PDF, puis contenue dans un `NSData` qui peuvent être enregistrées, téléchargées, envoyé par courrier électronique, etc.


## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons étudié les possibilités de graphiques fournies via la *graphiques élémentaires* framework. Nous avons vu comment utiliser des graphiques de base pour dessiner la géométrie, des images et des fichiers PDF dans le contexte d’un `UIView,` ainsi qu’aux contextes de graphiques de reposant sur la mémoire.

## <a name="related-links"></a>Liens associés

- [Exemple de graphique principal](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Procédure pas à pas de l’Animation et de graphique](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Animation de base](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Recettes d’Animation de base](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
