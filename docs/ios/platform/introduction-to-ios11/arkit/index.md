---
title: Introduction à ARKit dans Xamarin.iOS
description: Ce document décrit la réalité augmentée dans iOS 11 avec ARKit. Elle explique comment ajouter un modèle 3D à une application, configurer l’affichage, implémenter un délégué de la session, placez le modèle 3D dans le monde et suspendre la session de réalité augmentée.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 55ef2004f66cb808f878b2215dfdd59a45015877
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787164"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Introduction à ARKit dans Xamarin.iOS

_Réalité augmentée pour iOS 11_

ARKit permet à un large éventail de jeux et applications de réalité augmentée. Cette section couvre les rubriques suivantes :

- [Prise en main de ARKit](#gettingstarted)
- [À l’aide de ARKit avec UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Prise en main de ARKit

Pour commencer avec la réalité augmentée, les instructions suivantes parcourir d’une application simple : positionnement d’un modèle 3D et ARKit vous permettant de conserver le modèle avec ses fonctionnalités de suivi.

![Modèle 3D de Jet flottante d’image](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Ajouter un modèle 3D

Ressources doivent être ajoutés au projet avec le **SceneKitAsset** action de génération.

![SceneKit des ressources dans un projet](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Configurer l’affichage

Dans le contrôleur de vue `ViewDidLoad` (méthode), charger la ressource de scène et définir le `Scene` propriété sur la vue :

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Vous pouvez également implémenter un délégué de session

Mais non obligatoire pour les cas simples, la mise en œuvre d’un délégué de la session peut être utile pour le débogage de l’état de la session ARKit (et dans les applications réelles, envoyer des commentaires à l’utilisateur). Créez un délégué simple en utilisant le code ci-dessous :

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

Affectez le délégué dans le dans la `ViewDidLoad` méthode :

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Placez le modèle 3D dans le monde

Dans `ViewWillAppear`, le code suivant établit une session ARKit et définit la position d’un modèle 3D dans l’espace relativement à photo l’appareil :

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

Chaque fois que l’application est exécutée ou reprise, le modèle 3D est placé devant l’appareil photo. Une fois que le modèle est positionné, déplacer la caméra et observer ARKit conserve le modèle positionné.

### <a name="5-pause-the-augmented-reality-session"></a>5. Suspendre la session de réalité augmentée

Il est conseillé de suspendre la session ARKit lorsque le contrôleur de la vue n’est pas visible (dans le `ViewWillDisappear` méthode :

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Récapitulatif

Le code ci-dessus génère une simple application ARKit. Des exemples plus complexes d’attendre le contrôleur d’affichage qui héberge la session réalité augmentée pour implémenter `IARSCNViewDelegate`, et des méthodes supplémentaires être implémentée.

ARKit fournit un grand nombre de fonctionnalités plus sophistiquées, telles que le suivi aire de conception et d’interaction de l’utilisateur. Consultez le [UrhoSharp démonstration](urhosharp.md) pour obtenir un exemple de combinaison ARKit UrhoSharp de suivi.


## <a name="related-links"></a>Liens associés

- [Réalité augmentée (Apple)](https://developer.apple.com/arkit/)
- [À l’aide de ARKit avec UrhoSharp](urhosharp.md)
- [Exemple de ARKit simple (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [Objets de placement ARKit (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Présentation de ARKit - réalité augmentée pour iOS (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/602/)
