---
title: Introduction à CoreML dans Xamarin.iOS
description: Ce document décrit CoreML, ce qui permet l’apprentissage sur iOS. Ce document explique comment démarrer avec CoreML et comment l’utiliser avec l’infrastructure de Vision.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 8b489fd1a1bcce474decf6881e8eb6620c2ee2e3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240734"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Introduction à CoreML dans Xamarin.iOS

CoreML place machine learning pour iOS : les applications peuvent tirer parti de modèles d’apprentissage machine formé pour effectuer toutes sortes de tâches, à partir de la résolution des problèmes de reconnaissance d’image.

Cette présentation couvre les sujets suivants :

- [Prise en main de CoreML](#coreml)
- [À l’aide de CoreML avec l’infrastructure de Vision](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Prise en main de CoreML

Ces étapes expliquent comment ajouter CoreML à un projet iOS. Reportez-vous à la [exemple de Mars Habitat Pricer](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) pour obtenir un exemple pratique.

![Capture d’écran exemple PRÉDICTEUR de prix Habitat mars](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. Ajouter le modèle CoreML au projet

Ajouter un modèle CoreML (un fichier avec le **.mlmodel** extension) pour le **ressources** répertoire du projet. 

Dans les propriétés du fichier de modèle, son **action de génération** a la valeur **CoreMLModel**. Cela signifie qu’il sera compilé dans un **.mlmodelc** fichier lorsque l’application est générée.

### <a name="2-load-the-model"></a>2. Charger le modèle

Charger le modèle à l’aide de la `MLModel.Create` méthode statique :

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Définir les paramètres

Les paramètres de modèle sont passés en entrée et de sortie à l’aide d’une classe de conteneur qui implémente `IMLFeatureProvider`.

Classes de fournisseur de fonctionnalités se comportent comme un dictionnaire de chaîne et `MLFeatureValue`s, où chaque valeur de la fonctionnalité peut être une simple chaîne ou nombre, un tableau ou données ou une mémoire tampon de pixels contenant une image.

Code pour un fournisseur de fonctionnalités de valeur unique est indiqué ci-dessous :

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

Paramètres d’entrée peuvent être fournis à l’aide de classes comme suit, d’une manière qui est comprise par CoreML. Les noms des fonctionnalités (telles que `myParam` dans l’exemple de code) doit correspondre à celui attendu par le modèle.

### <a name="4-run-the-model"></a>4. Exécuter le modèle

À l’aide du modèle nécessite que le fournisseur de fonctionnalités à instancier et paramètres définis, puis qui la `GetPrediction` méthode appelée :

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Extraire les résultats

Le résultat de prédiction `outFeatures` est également une instance de `IMLFeatureProvider`; sortie valeurs sont accessibles à l’aide de `GetFeatureValue` avec le nom de chaque paramètre de sortie (telles que `theResult`), comme dans cet exemple :

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>À l’aide de CoreML avec l’infrastructure de Vision

CoreML peut également servir conjointement avec l’infrastructure de Vision pour effectuer des opérations sur l’image, telles que la reconnaissance des formes, identification de l’objet et d’autres tâches.

Les étapes ci-dessous décrivent comment CoreML et la Vision sont utilisées dans les [CoreMLVision exemple](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). L’exemple associe le [reconnaissance des rectangles](~/ios/platform/introduction-to-ios11/vision.md#rectangles) à partir de l’infrastructure de Vision avec le _MNINSTClassifier_ modèle CoreML pour identifier un chiffre manuscrit dans une photo.

![Reconnaissance d’image de numéro 3](coreml-images/vision3.png) ![Reconnaissance d’image de nombre 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Créer un modèle de Vision CoreML

Le modèle CoreML _MNISTClassifier_ est chargé et ensuite encapsulée dans un `VNCoreMLModel` ce qui rend le modèle disponible pour les tâches de Vision. Ce code crée également deux demandes de Vision : tout d’abord pour rechercher les rectangles dans une image, puis pour le traitement d’un rectangle avec le modèle CoreML :

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

La classe doit toujours implémenter la `HandleRectangles` et `HandleClassification` méthodes pour les demandes de Vision, indiqués dans les étapes 3 et 4 ci-dessous.

### <a name="2-start-the-vision-processing"></a>2. Démarrer le traitement de la Vision

Le code suivant démarre le traitement de la demande. Dans le **CoreMLVision** exemple, ce code s’exécute après que l’utilisateur a sélectionné une image :

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Ce gestionnaire passe le `ciImage` à l’infrastructure de Vision `VNDetectRectanglesRequest` qui a été créé à l’étape 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Gérer les résultats du traitement de la Vision

Une fois que la détection de rectangle est terminée, il exécute la `HandleRectangles` méthode rogne l’image pour extraire le premier rectangle, convertit l’image de rectangle en nuances de gris et le passe au modèle CoreML pour la classification.

Le `request` passés à cette méthode contient les détails de la demande de Vision et à l’aide de la `GetResults<VNRectangleObservation>()` méthode, il retourne une liste de rectangles trouvé dans l’image. Le premier rectangle `observations[0]` est extrait et transmis au modèle CoreML :

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

Le `ClassificationRequest` a été initialisée à l’étape 1 pour utiliser le `HandleClassification` méthode définie dans l’étape suivante.

### <a name="4-handle-the-coreml"></a>4. Gérer le CoreML

Le `request` passés à cette méthode contient les détails de la demande CoreML et à l’aide de la `GetResults<VNClassificationObservation>()` méthode, il retourne une liste de résultats possibles, classés par confiance (confiance plus élevé première) :

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>Exemples

Il existe trois échantillons CoreML pour essayer :

* Le [exemple du PRÉDICTEUR de prix Habitat Mars](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) avec les entrées numériques simples et des sorties.

* Le [exemple Vision & CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) accepte un paramètre d’image et utilise l’infrastructure de Vision pour identifier les zones carrés dans l’image, qui sont passées à un modèle de CoreML qui reconnaît un seul chiffre.

* Enfin, le [exemple de reconnaissance d’Image CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) utilise CoreML pour identifier les fonctionnalités d’une photo. Par défaut, il utilise la plus petite **SqueezeNet** modèle (5 Mo), mais il a été écrit afin que vous pouvez télécharger et intégrer le plus grand **VGG16** modèle (553 Mo). Pour plus d’informations, consultez la [fichier Lisezmoi de l’exemple](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).

## <a name="related-links"></a>Liens associés

- [Apprentissage (Apple)](https://developer.apple.com/machine-learning/)
- [Exemple CoreML (Mars Habitat) (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML et Vision (numéro de reconnaissance) (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [Reconnaissance d’Image CoreML (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML avec Azure personnalisée Vision (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Présentation de CoreML (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/703/)
