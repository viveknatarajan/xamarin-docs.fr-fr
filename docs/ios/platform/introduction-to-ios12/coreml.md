---
title: Core ML 2 in Xamarin.iOS
description: Ce document décrit les mises à jour vers Core ML disponible dans le cadre d’e/s 12. En particulier, il examine les améliorations des performances associées à la nouvelle API de prédiction de lot.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/15/2018
ms.openlocfilehash: 50d59f0b6ff2133c5870d84a1d740547768116e0
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58869726"
---
# <a name="core-ml-2-in-xamarinios"></a>Core ML 2 in Xamarin.iOS

Core ML est une technologie machine learning disponible sur iOS, macOS, tvOS et watchOS. Il permet aux applications de faire des prédictions basées sur des modèles d’apprentissage.

Dans iOS 12, Core ML inclut une API de traitement par lots. Cette API Core ML améliore l’efficacité et apporte des améliorations de performances dans les scénarios où un modèle est utilisé pour rendre une séquence de prédictions.

## <a name="sample-app-marshabitatcoremltimer"></a>Exemple d’application : MarsHabitatCoreMLTimer

Pour illustrer les prédictions par lot avec Core ML, examinons le [MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer) exemple d’application. Cet exemple utilise un modèle de Core ML formé pour prédire le coût de la création d’un habitat sur Mars, en fonction de diverses entrées : nombre de panneaux solaires, nombre de serre et nombre d’acres.

Les extraits de code dans ce document proviennent de cet exemple.

## <a name="generate-sample-data"></a>Générer un échantillon de données

Dans `ViewController`, l’exemple d’application `ViewDidLoad` les appels de méthode `LoadMLModel`, ce qui charge le modèle Core ML inclus :

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

Ensuite, l’exemple d’application crée 100 000 `MarsHabitatPricerInput` objets à utiliser comme entrée pour les prédictions Core ML séquentielles. Chaque exemple généré a une valeur aléatoire définie pour le nombre de panneaux solaires, le nombre de serre et le nombre d’acres :

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

En appuyant sur un des trois boutons de l’application exécute deux séquences de prédictions : un à l’aide un `for` boucle et un autre en utilisant le nouveau lot `GetPredictions` méthode introduite dans iOS 12 :

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## <a name="for-loop"></a>for (boucle)

Le `for` version de boucle du test naïvement itère sur le nombre spécifié d’entrées, appeler [ `GetPrediction` ](xref:CoreML.MLModel.GetPrediction*) pour chacun et en ignorant le résultat. La méthode fois la durée nécessaire pour rendre les prédictions :

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="getpredictions-new-batch-api"></a>GetPredictions (nouvelle API de lot)

La version de lot du test crée un `MLArrayBatchProvider` objet à partir du tableau d’entrée (puisqu’il s’agit d’un paramètre d’entrée requis pour le `GetPredictions` (méthode)), crée un [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
objet qui empêche les calculs de prédiction soient restreintes à l’UC et utilise le `GetPredictions` API pour extraire les prédictions à nouveau en ignorant le résultat :

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="results"></a>Résultats

Sur le simulateur et appareil, `GetPredictions` se termine plus rapidement que les prédictions Core ML basé sur boucles.

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer)
- [Quelles sont les nouveautés dans Core ML, partie 1 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [Quelles sont les nouveautés dans Core ML, partie 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Introduction à Core ML dans Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [Core ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [Utilisation de modèles de Core ML](https://developer.apple.com/machine-learning/build-run-models/)
