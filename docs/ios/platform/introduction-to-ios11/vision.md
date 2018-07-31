---
title: Framework de vision dans Xamarin.iOS
description: Ce document décrit comment utiliser l’iOS 11 Framework Vision dans Xamarin.iOS. Plus précisément, il aborde la détection de rectangle et détection de visage.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2017
ms.openlocfilehash: 4746de2f351e866fd72946b204f97e997c3e88c4
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350662"
---
# <a name="vision-framework-in-xamarinios"></a>Framework de vision dans Xamarin.iOS

Le framework de Vision ajoute un nombre de fonctionnalités à iOS 11, y compris de traitement de nouvelle image :

- [Détection de rectangle](#rectangles)
- [Détection de visage](#faces)
- Machine Learning d’analyse d’images (abordées dans [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Détection des codes-barres
- Analyse alignement d’image
- Détection de texte
- Détection de l’horizon
- Le suivi des & détection d’objets

![Photographier avec trois rectangles détectés](vision-images/found-rectangles-tiny.png) ![Photographier avec deux visages détectés](vision-images/xamarin-home-faces-tiny.png)

Détection de rectangle et la détection de visage sont abordées plus en détail ci-dessous.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Détection de rectangle

Le [VisionRects exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) montre comment traiter une image et dessiner des rectangles détectés sur celui-ci.

### <a name="1-initialize-the-vision-request"></a>1. Initialiser la requête de Vision

Dans `ViewDidLoad`, créer un `VNDetectRectanglesRequest` qui fait référence à la `HandleRectangles` méthode qui sera appelée à la fin de chaque demande :

Le `MaximumObservations` propriété doit également être définie, sinon par défaut est 1 et qu’un seul résultat s’affichera.

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

Le `OverlayRectangles` méthode dans le **VisionRectangles** exemple comporte trois fonctions :

- Rendu de l’image source,
- Dessin d’un rectangle pour indiquer où chacun d’eux a été détecté, et
- Ajout d’une étiquette de texte pour chaque rectangle à l’aide de CoreGraphics.

Afficher le [source de l’exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) pour la méthode CoreGraphics exacte.

![Photographier avec trois rectangles détectés](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Poursuite du traitement

Détection de rectangle est souvent que la première étape de la chaîne d’opérations, telles que [cet exemple CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), où les rectangles sont passés à un modèle de CoreML pour analyser les chiffres manuscrits.


<a name="faces" />

## <a name="face-detection"></a>Détection de visage

Le [VisionFaces exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) fonctionne de manière similaire à la **VisionRectangles** exemple, à l’aide d’une autre classe de demande de Vision.

### <a name="1-initialize-the-vision-request"></a>1. Initialiser la requête de Vision

Dans `ViewDidLoad`, créer un `VNDetectFaceRectanglesRequest` qui fait référence à la `HandleRectangles` méthode qui sera appelée à la fin de chaque demande.

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

Une fois que la détection de visage est terminée, le gestionnaire s’exécute le `HandleRectangles` méthode qui assure la gestion des erreurs et affiche les limites des visages détectés et appelle le `OverlayRectangles` pour dessiner des rectangles englobants sur l’image d’origine :

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

Le `OverlayRectangles` méthode dans le **VisionFaces** exemple comporte trois fonctions :

- Rendu de l’image source,
- Dessin d’un rectangle pour chaque visage détecté, et
- Ajout d’une étiquette de texte pour chaque face à l’aide de CoreGraphics.

Afficher le [source de l’exemple](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) pour la méthode CoreGraphics exacte.

![Photographier avec deux visages détectés](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Poursuite du traitement

L’infrastructure de Vision inclut des fonctionnalités supplémentaires permettant de détecter des caractéristiques du visage, tels que les yeux et de la bouche. Utilisez le `VNDetectFaceLandmarksRequest` type, qui retournera `VNFaceObservation` résultats comme à l’étape 3 ci-dessus, mais avec de nouvelles `VNFaceLandmark` données.


## <a name="related-links"></a>Liens associés

- [Vision Rectangles (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Vision visages (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Progrès dans l’Image Core - filtres, complète, Vision et plus (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/510/)
