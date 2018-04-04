---
title: CoreImage
description: CoreImage est une nouvelle infrastructure introduite avec iOS 5 pour fournir le traitement d’image et fonctionnalités d’amélioration de la vidéo en direct. Cet article présente ces fonctionnalités avec les exemples de Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0bb2c3b8b563da53e432ad16e6518ada67a4655e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="coreimage"></a>CoreImage

_CoreImage est une nouvelle infrastructure introduite avec iOS 5 pour fournir le traitement d’image et fonctionnalités d’amélioration de la vidéo en direct. Cet article présente ces fonctionnalités avec les exemples de Xamarin.iOS._

CoreImage est une nouvelle infrastructure introduite dans iOS 5 qui fournit un certain nombre de filtres intégrés et les effets à appliquer à des images et des vidéos, notamment la détection de visage.

Ce document contient des exemples simples de :

-  Détection des visages.
-  Application de filtres à une image
-  Répertorier les filtres disponibles.


Ces exemples doivent vous aider à commencer incorporation des fonctionnalités de CoreImage dans vos applications Xamarin.iOS.

## <a name="requirements"></a>Spécifications

Vous devez utiliser la version la plus récente de Xcode.

## <a name="face-detection"></a>Détection de visage

La fonctionnalité de détection de visage CoreImage est simplement ne pas confondre : il tente d’identifier une photo de face et retourne les coordonnées de n’importe quel faces qu’il reconnaît. Ces informations peuvent être utilisées pour compter le nombre de personnes dans une image, dessiner des indicateurs sur l’image (par exemple). pour « marquage' personnes dans une photographie), ou tout autre élément que vous pouvez considérer de.

Ce code à partir de CoreImage\SampleCode.cs montre comment créer et utiliser la détection de visage dans une image incorporée :

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Le tableau fonctionnalités contiendra `CIFaceFeature` objets (si les faces ont été détectées). Il existe un `CIFaceFeature` pour chaque police. `CIFaceFeature` a les propriétés suivantes :

-  HasMouthPosition – indique si une bouche a été détectée pour ce type.
-  HasLeftEyePosition – si le œil gauche a été détectée pour ce type.
-  HasRightEyePosition – si le œil droite a été détectée pour ce type. 
-  MouthPosition : les coordonnées de l’entrée de cette image.
-  LeftEyePosition : les coordonnées de le œil gauche de cette image.
-  RightEyePosition : les coordonnées de le œil droit de cette image.


Les coordonnées de toutes ces propriétés ont leur origine dans le coin inférieur gauche – Contrairement à UIKit qui utilise le coin supérieur gauche comme origine. Lorsque vous utilisez les coordonnées sur `CIFaceFeature` veillez '' les retourner. Cette vue de l’image personnalisée très basique dans CoreImage\CoreImageViewController.cs montre comment dessiner les triangles « indicateur face » sur l’image (Notez le `FlipForBottomOrigin` (méthode)) :

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Ensuite dans le fichier SampleCode.cs l’image et les fonctionnalités sont affectées avant que l’image est redessinée :

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

La capture d’écran montre l’exemple de sortie : les emplacements des fonctionnalités visages détectées sont affichés dans un UITextView et dessinées sur l’image source à l’aide de CoreGraphics.

En raison de la façon dont la reconnaissance des visages fonctionne il détectera occasionnellement les opérations en plus des faces humaines (par exemple, ces singes JOUET !).

## <a name="filters"></a>Filtres

Il existe plus de 50 différents filtres intégrés, et l’infrastructure est extensible afin que les nouveaux filtres peuvent être implémentées.

## <a name="using-filters"></a>L’utilisation de filtres

Appliquer un filtre à une image comporte quatre étapes distinctes : chargement de l’image, création du filtre, appliquer le filtre et l’enregistrement (ou affichage) le résultat.

Tout d’abord, charger une image dans un `CIImage` objet.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

En second lieu, créer la classe de filtre et définir ses propriétés.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Troisièmement, accéder à la `OutputImage` propriété et appelez le `CreateCGImage` méthode pour afficher le résultat final.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Enfin, assignez l’image à un affichage pour voir le résultat. Dans une application réelle, l’image obtenue peut être enregistré pour le système de fichiers, l’Album, un Tweet ou un e-mail.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Ces captures d’écran affichent le résultat de la `CISepia` et `CIHueAdjust` filtres sont illustrées dans le CoreImage.zip exemple de code.

Consultez le [ajuster le contrat et luminosité d’une Image de recette](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) pour obtenir un exemple de la `CIColorControls` filtre.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Liste des filtres et leurs propriétés

Ce code à partir de CoreImage\SampleCode.cs génère la liste complète des filtres intégrés et leurs paramètres.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

Le [référence de classe CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) décrit les filtres intégrés 50 et leurs propriétés. En utilisant le code ci-dessus, vous pouvez interroger les classes de filtre, y compris les valeurs par défaut pour les paramètres et les valeurs autorisées minimales et maximales (qui peuvent être utilisés pour valider les entrées avant d’appliquer un filtre).

La sortie de la liste Catégories ressemble à ceci sur le simulateur, vous pouvez faire défiler la liste pour afficher tous les filtres et leurs paramètres.

 [![](introduction-to-coreimage-images/coreimage05.png "La sortie de la liste Catégories ressemble à ceci sur le simulateur")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Chaque filtre répertorié a été exposé en tant que classe dans Xamarin.iOS, vous pouvez Explorer également l’API Xamarin.iOS.CoreImage dans le navigateur de l’Assembly ou à l’aide de la saisie semi-automatique dans Visual Studio pour Mac ou Visual Studio. 

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser certaines des nouvelles fonctionnalités de framework iOS 5 CoreImage telles que la détection de visage et application des filtres à une image. Des dizaines de filtres d’image différents sont disponibles dans le framework à utiliser.

## <a name="related-links"></a>Liens associés

- [Image de base (exemple)](https://developer.xamarin.com/samples/CoreImage/)
- [Ajuster le contrat et luminosité d’une Image de recette](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [L’utilisation de filtres de CoreImage](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Référence de classe de CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
