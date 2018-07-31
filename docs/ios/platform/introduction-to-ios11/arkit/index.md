---
title: Introduction à ARKit dans Xamarin.iOS
description: Ce document décrit la réalité augmentée dans iOS 11 avec ARKit. Il explique comment ajouter un modèle 3D à une application, configurer l’affichage, implémenter un délégué de la session, positionner le modèle 3D dans le monde et suspendre la session de réalité augmentée.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 14bbb35477c098738e9cd7e2cb92154422d394ee
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350613"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Introduction à ARKit dans Xamarin.iOS

_Réalité augmentée pour iOS 11_

ARKit permet un large éventail de jeux et applications de réalité augmentée. Cette section couvre les rubriques suivantes :

- [Prise en main ARKit](#gettingstarted)
- [À l’aide de ARKit avec UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Prise en main ARKit

Pour vous familiariser avec la réalité augmentée, les instructions suivantes guident une application simple : un modèle 3D de positionnement et ARKit vous permettant de conserver le modèle en place avec ses fonctionnalités de suivi.

![Modèle 3D de Jet flottante d’image](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Ajouter un modèle 3D

Ressources doivent être ajoutés au projet avec le **SceneKitAsset** action de génération.

![Dans un projet de ressources SceneKit](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Configurer l’affichage

Dans le contrôleur d’affichage `ViewDidLoad` (méthode), charger la ressource de la scène et définir le `Scene` propriété sur la vue :

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Si vous le souhaitez implémenter un délégué de session

Mais non obligatoire pour les cas simples, un délégué de la session de mise en œuvre peut être utile pour le débogage de l’état de la session ARKit (et dans les applications réelles, envoi de commentaires à l’utilisateur). Créer un délégué simple en utilisant le code ci-dessous :

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Affecter le délégué dans le dans la `ViewDidLoad` méthode :

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Positionner le modèle 3D dans le monde

Dans `ViewWillAppear`, le code suivant établit une session ARKit et définit la position d’un modèle 3D dans l’espace par rapport à photo l’appareil :

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

Chaque fois que l’application est exécutée ou reprise, le modèle 3D est placé devant la caméra. Une fois que le modèle est positionné, déplacez l’appareil photo et écoutez ARKit conserve le modèle positionné.

### <a name="5-pause-the-augmented-reality-session"></a>5. Suspendre la session de réalité augmentée

Il est conseillé de suspendre la session ARKit lorsque le contrôleur d’affichage n’est pas visible (dans le `ViewWillDisappear` méthode :

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Récapitulatif

Le code ci-dessus entraîne une simple application ARKit. Des exemples plus complexes d’attendre le contrôleur d’affichage qui héberge la session de réalité augmentée pour implémenter `IARSCNViewDelegate`, et des méthodes supplémentaires être implémentée.

ARKit fournit un grand nombre de fonctionnalités plus sophistiquées, telles que le suivi aire de conception et d’interaction de l’utilisateur. Consultez le [UrhoSharp démonstration](urhosharp.md) pour obtenir un exemple de combinaison ARKit UrhoSharp de suivi.


## <a name="related-links"></a>Liens associés

- [Réalité augmentée (Apple)](https://developer.apple.com/arkit/)
- [À l’aide de ARKit avec UrhoSharp](urhosharp.md)
- [Exemple de simple ARKit (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit placer des objets (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Présentation de ARKit - réalité augmentée pour iOS (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/602/)
