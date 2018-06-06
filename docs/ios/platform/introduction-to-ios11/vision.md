---
title: Infrastructure de vision dans Xamarin.iOS
description: Ce document décrit comment utiliser le 11 iOS Framework Vision dans Xamarin.iOS. En particulier, il traite la détection du rectangle et sont confrontés à la détection.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: c44c4b3ab12c1ba448f1befb6f831f5ad9119f18
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787417"
---
# <a name="vision-framework-in-xamarinios"></a>Infrastructure de vision dans Xamarin.iOS

L’infrastructure de Vision ajoute un numéro de la nouvelle image de traitement des fonctionnalités à iOS 11, y compris :

- [Détection de rectangle](#rectangles)
- [Détection de visage](#faces)
- Analyse d’Image de formation de l’ordinateur (présentés dans [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Détection de code-barres
- Analyse alignement d’image
- Détection de texte
- Détection d’horizon
- Suivi et la détection d’objets

![Photographier avec trois rectangles détectés](vision-images/found-rectangles-tiny.png) ![Photographier avec deux faces détectés](vision-images/xamarin-home-faces-tiny.png)

Détection du rectangle et la détection de visage sont décrites plus en détail ci-dessous.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Détection de rectangle

Le [VisionRects exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) montre comment traiter une image et dessiner des rectangles détectés sur celui-ci.

### <a name="1-initialize-the-vision-request"></a>1. Initialisation de la demande de Vision

Dans `ViewDidLoad`, créez un `VNDetectRectanglesRequest` qui fait référence à la `HandleRectangles` méthode qui sera appelée à la fin de chaque demande :

Le `MaximumObservations` propriété doit également être définie dans le cas contraire par défaut est 1 et qu’un seul résultat s’affichera.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Démarrer le traitement de la Vision

Le code suivant démarre le traitement de la demande. Dans le **VisionRects** exemple, ce code s’exécute après que l’utilisateur a sélectionné une image :

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Ce gestionnaire passe le `ciImage` à l’infrastructure de Vision `VNDetectRectanglesRequest` qui a été créé à l’étape 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Gérer les résultats du traitement de la Vision

Une fois que la détection de rectangle est terminée, l’infrastructure exécute la `HandleRectangles` méthode, un résumé de ce qui est indiqué ci-dessous :

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Afficher les résultats

Le `OverlayRectangles` méthode dans le **VisionRectangles** exemple possède trois fonctions :

- L’image source, de rendu
- Dessin d’un rectangle pour indiquer où chacun d’eux a été détecté, et
- Ajout d’une étiquette de texte pour chaque rectangle à l’aide de CoreGraphics.

Afficher le [source de l’exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) pour la méthode CoreGraphics exacte.

![Photographier avec trois rectangles détectés](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Poursuite du traitement

Détection de rectangle est souvent que la première étape de la chaîne d’opérations, telles que [cet exemple CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), où les rectangles sont passés à un modèle de CoreML pour analyser les chiffres manuscrits.


<a name="faces" />

## <a name="face-detection"></a>Détection de visage

Le [VisionFaces exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) fonctionne de façon similaire à la **VisionRectangles** exemple, à l’aide d’une autre classe de demande de Vision.

### <a name="1-initialize-the-vision-request"></a>1. Initialisation de la demande de Vision

Dans `ViewDidLoad`, créez un `VNDetectFaceRectanglesRequest` qui fait référence à la `HandleRectangles` méthode qui sera appelée à la fin de chaque demande.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Démarrer le traitement de la Vision

Le code suivant démarre le traitement de la demande. Dans le **VisionFaces** exemple ce code s’exécute après que l’utilisateur a sélectionné une image :

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Ce gestionnaire passe le `ciImage` à l’infrastructure de Vision `VNDetectFaceRectanglesRequest` qui a été créé à l’étape 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Gérer les résultats du traitement de la Vision

Une fois la détection de visage terminée, le gestionnaire s’exécute le `HandleRectangles` méthode qui effectue la gestion des erreurs et affiche les limites des faces détectés et appelle le `OverlayRectangles` pour dessiner des rectangles englobants sur l’image d’origine :

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Afficher les résultats

Le `OverlayRectangles` méthode dans le **VisionFaces** exemple possède trois fonctions :

- L’image source, de rendu
- Dessin d’un rectangle pour chaque police détecté, et
- Ajout d’une étiquette de texte pour chaque face à l’aide de CoreGraphics.

Afficher le [source de l’exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) pour la méthode CoreGraphics exacte.

![Photographier avec deux faces détectés](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Poursuite du traitement

L’infrastructure de Vision inclut des fonctionnalités supplémentaires à détecter faciale, tels que les yeux et la bouche. Utilisez le `VNDetectFaceLandmarksRequest` type, ce qui permet de renvoyer `VNFaceObservation` les résultats comme à l’étape 3 ci-dessus, mais avec de nouvelles `VNFaceLandmark` données.


## <a name="related-links"></a>Liens associés

- [Rectangles de vision (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Vision Faces (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Progrès de l’Image principale - filtres, complète, Vision et plus (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/510/)
