---
title: ARKit 2 dans Xamarin.iOS
description: Ce document décrit les mises à jour à ARKit dans iOS 12. Il se concentre sur l’utilisation des images et des objets de référence pour la détection inclut le code de l’environnement de sprites et décrit les problèmes courants dans la programmation de ARKit.
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/22/2018
ms.openlocfilehash: 4d5319005b28c5afa0906c44cfa59f0cad40de76
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617616"
---
# <a name="arkit-2-in-xamarinios"></a>ARKit 2 dans Xamarin.iOS

ARKit a considérablement évolué depuis son introduction dernière année dans iOS 11. Tout d’abord, vous pouvez maintenant détecter verticale, ainsi que des plans horizontaux, ce qui améliore considérablement la faisabilité des expériences de réalité augmentée intérieur. En outre, il existe de nouvelles fonctionnalités :

* Reconnaissance des images de référence et des objets en tant que la jonction entre le monde réel et les images numériques
* Un nouveau mode d’éclairage qui simule l’éclairage réel
* La possibilité de partager et de conserver les environnements AR
* Un nouveau format de fichier par défaut pour le stockage de contenu de AR

## <a name="recognizing-reference-objects"></a>Reconnaissance des objets de référence

Une fonctionnalité de showcase ARKit 2 est la possibilité de reconnaître les images de référence et des objets. Images de référence peuvent être chargées à partir des fichiers d’image normale ([abordée plus tard](#more-tracking-configurations)), mais référence les objets doivent être analysés, à l’aide de l’axés sur le développeur [ `ARObjectScanningConfiguration` ](https://developer.xamarin.com/api/type/ARKit.ARObjectScanningConfiguration/).

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>Exemple d’application : analyse et de détection d’objets 3D

Le [d’analyse et de détection d’objets 3D](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/) exemple est un port d’un [projet d’Apple](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc) qui montre :

* À l’aide de gestion d’application état [ `NSNotification` ](https://developer.xamarin.com/api/type/Foundation.NSNotification/) objets
* Visualisation personnalisée
* Mouvements complexes
* Objet d’analyse
* Stocker un [`ARReferenceObject`](https://developer.xamarin.com/api/type/ARKit.ARReferenceObject/)

Analyse un objet de référence est batterie - et sollicitant beaucoup le processeur et les appareils plus anciens ont souvent des difficultés à atteindre suivi stable.

### <a name="state-management-using-nsnotification-objects"></a>Gestion de l’état à l’aide des objets de NSNotification

Cette application utilise une machine à états qui effectue la transition entre les états suivants :

* `AppState.StartARSession`
* `AppState.NotReady`
* `AppState.Scanning`
* `AppState.Testing`

En outre utilise un jeu incorporé des États et transitions en `AppState.Scanning`:

* `Scan.ScanState.Ready`
* `Scan.ScanState.DefineBoundingBox`
* `Scan.ScanState.Scanning`
* `Scan.ScanState.AdjustingOrigin`

L’application utilise une architecture réactive qui publie les notifications de transition d’état à [ `NSNotificationCenter` ](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/) et s’abonne à ces notifications. Le programme d’installation ressemble à cet extrait de code à partir de `ViewController.cs`:

```csharp
// Configure notifications for application state changes
var notificationCenter = NSNotificationCenter.DefaultCenter;

notificationCenter.AddObserver(Scan.ScanningStateChangedNotificationName, State.ScanningStateChanged);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxCreatedNotificationName, State.GhostBoundingBoxWasCreated);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxRemovedNotificationName, State.GhostBoundingBoxWasRemoved);
notificationCenter.AddObserver(ScannedObject.BoundingBoxCreatedNotificationName, State.BoundingBoxWasCreated);
notificationCenter.AddObserver(BoundingBox.ScanPercentageChangedNotificationName, ScanPercentageChanged);
notificationCenter.AddObserver(BoundingBox.ExtentChangedNotificationName, BoundingBoxExtentChanged);
notificationCenter.AddObserver(BoundingBox.PositionChangedNotificationName, BoundingBoxPositionChanged);
notificationCenter.AddObserver(ObjectOrigin.PositionChangedNotificationName, ObjectOriginPositionChanged);
notificationCenter.AddObserver(NSProcessInfo.PowerStateDidChangeNotification, DisplayWarningIfInLowPowerMode);

```

Un gestionnaire de notification standard sera mise à jour de l’interface utilisateur et éventuellement modifier l’état de l’application, telles que ce gestionnaire met à jour lors de l’analyse de l’objet :

```csharp
private void ScanPercentageChanged(NSNotification notification)
{
    var pctNum = TryGet<NSNumber>(notification.UserInfo, BoundingBox.ScanPercentageUserKey);
    if (pctNum == null)
    {
        return;
    }
    double percentage = pctNum.DoubleValue;
    // Switch to the next state if scan is complete
    if (percentage >= 100.0)
    {
        State.SwitchToNextState();
    }
    else
    {
        DispatchQueue.MainQueue.DispatchAsync(() => navigationBarController.SetNavigationBarTitle($"Scan ({percentage})"));
    }
}

```

Enfin, `Enter{State}` méthodes modifient le modèle et l’expérience utilisateur comme il convient pour le nouvel état :

```csharp
internal void EnterStateTesting()
{
    navigationBarController.SetNavigationBarTitle("Testing");
    navigationBarController.ShowBackButton(false);
    loadModelButton.Hidden = true;
    flashlightButton.Hidden = false;
    nextButton.Enabled = true;
    nextButton.SetTitle("Share", UIControlState.Normal);

    testRun = new TestRun(sessionInfo, sceneView);
    TestObjectDetection();
    CancelMaxScanTimeTimer();
}
```

### <a name="custom-visualization"></a>Visualisation personnalisée

L’application montre le bas niveau « point cloud » de l’objet contenu dans un cadre englobant projetés sur un plan horizontal détecté.

Ce nuage de points est disponible pour les développeurs dans le [ `ARFrame.RawFeaturePoints` ](https://developer.xamarin.com/api/property/ARKit.ARFrame.RawFeaturePoints/) propriété. Visualisation du nuage de points efficacement peut être un problème épineux. Itération sur les points, puis créant et en plaçant un nouveau nœud SceneKit pour chaque point supprimerais la fréquence d’images. Vous pouvez également, s’effectue de façon asynchrone, il y aurait un décalage. L’exemple gère les performances avec une stratégie de trois parties :

* À l’aide de code unsafe pour épingler les données de placent et interprètent les données comme une mémoire tampon brute d’octets.
* Conversion de cette mémoire tampon brute dans un [ `SCNGeometrySource` ](https://developer.xamarin.com/api/type/SceneKit.SCNGeometrySource/) et la création d’un « modèle » [ `SCNGeometryElement` ](https://developer.xamarin.com/api/type/SceneKit.SCNGeometryElement/) objet.
* Rapidement « assemblage » les données brutes et le modèle à l’aide [`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](https://developer.xamarin.com/api/member/SceneKit.SCNGeometry.Create/p/SceneKit.SCNGeometrySource[]/SceneKit.SCNGeometryElement[]/)

```csharp
internal static SCNGeometry CreateVisualization(NVector3[] points, UIColor color, float size)
{
  if (points.Length == 0)
  {
    return null;
  }

  unsafe
  {
    var stride = sizeof(float) * 3;

    // Pin the data down so that it doesn't move
    fixed (NVector3* pPoints = &amp;points[0])
    {
      // Important: Don't unpin until after `SCNGeometry.Create`, because geometry creation is lazy

      // Grab a pointer to the data and treat it as a byte buffer of the appropriate length
      var intPtr = new IntPtr(pPoints);
      var pointData = NSData.FromBytes(intPtr, (System.nuint) (stride * points.Length));

      // Create a geometry source (factory) configured properly for the data (3 vertices)
      var source = SCNGeometrySource.FromData(
        pointData,
        SCNGeometrySourceSemantics.Vertex,
        points.Length,
        true,
        3,
        sizeof(float),
        0,
        stride
      );

      // Create geometry element
      // The null and bytesPerElement = 0 look odd, but this is just a template object
      var template = SCNGeometryElement.FromData(null, SCNGeometryPrimitiveType.Point, points.Length, 0);
      template.PointSize = 0.001F;
      template.MinimumPointScreenSpaceRadius = size;
      template.MaximumPointScreenSpaceRadius = size;

      // Stitch the data (source) together with the template to create the new object
      var pointsGeometry = SCNGeometry.Create(new[] { source }, new[] { template });
      pointsGeometry.Materials = new[] { Utilities.Material(color) };
      return pointsGeometry;
    }
  }
}
```

Le résultat ressemble à ceci :

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>Mouvements complexes

L’utilisateur peut mettre à l’échelle, faire pivoter et faites glisser la zone englobante qui entoure l’objet cible. Il existe deux choses intéressantes dans les modules de reconnaissance de mouvement associé.

Tout d’abord, tous les modules de reconnaissance de mouvement activer uniquement après qu’un seuil a été transmis ; par exemple, un doigt a déplacé autant pixels ou la rotation dépasse un angle. La technique consiste à cumuler le déplacement jusqu'à ce que le seuil est dépassé, puis appliquer de façon incrémentielle :

```csharp
// A custom rotation gesture recognizer that fires only when a threshold is passed
internal partial class ThresholdRotationGestureRecognizer : UIRotationGestureRecognizer
{
    // The threshold after which this gesture is detected.
    const double threshold = Math.PI / 15; // (12°)

    // Indicates whether the currently active gesture has exceeded the threshold
    private bool thresholdExceeded = false;

    private double previousRotation = 0;
    internal double RotationDelta { get; private set; }

    internal ThresholdRotationGestureRecognizer(IntPtr handle) : base(handle)
    {
    }

    // Observe when the gesture's state changes to reset the threshold
    public override UIGestureRecognizerState State
    {
        get => base.State;
        set
        {
            base.State = value;

            switch(value)
            {
                case UIGestureRecognizerState.Began :
                case UIGestureRecognizerState.Changed :
                    break;
                default :
                    // Reset threshold check
                    thresholdExceeded = false;
                    previousRotation = 0;
                    RotationDelta = 0;
                    break;
            }
        }
    }

    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);

        if (thresholdExceeded)
        {
            RotationDelta = Rotation - previousRotation;
            previousRotation = Rotation;
        }

        if (! thresholdExceeded && Math.Abs(Rotation) > threshold)
        {
            thresholdExceeded = true;
            previousRotation = Rotation;
        }
    }
}
```

La deuxième chose intéressante effectuée par rapport aux gestes est la façon dont la zone englobante est déplacée par rapport à détecté les plans du monde réel. Cet aspect est abordé dans [ce billet de blog Xamarin](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/).

## <a name="other-new-features-in-arkit-2"></a>Autres nouvelles fonctionnalités dans ARKit 2

### <a name="more-tracking-configurations"></a>Plusieurs configurations de suivi

Maintenant, vous pouvez utiliser les éléments suivants comme base pour une expérience de réalité mixte :

* Uniquement l’accéléromètre de périphérique ([`AROrientationTrackingConfiguration`](https://developer.xamarin.com/api/type/ARKit.AROrientationTrackingConfiguration/), iOS 11)
* Faces ([`ARFaceTrackingConfiguration`](https://developer.xamarin.com/api/type/ARKit.ARFaceTrackingConfiguration/), iOS 11)
* Images de référence ([`ARImageTrackingConfiguration`](https://developer.xamarin.com/api/type/ARKit.ARImageTrackingConfiguration/), iOS 12)
* Analyse des objets 3D ([`ARObjectScanningConfiguration`](https://developer.xamarin.com/api/type/ARKit.ARObjectScanningConfiguration/), iOS 12)
* Odométrie inertie Visual ([`ARWorldTrackingConfiguration`](https://developer.xamarin.com/api/type/ARKit.ARWorldTrackingConfiguration/)amélioré dans iOS 12)

`AROrientationTrackingConfiguration`, présentés dans [ce billet de blog et F# exemple](https://github.com/lobrien/FSharp_Face_AR), est plus limité et fournit une mauvaise expérience de réalité mixte, car il place uniquement les objets numériques en relation avec le mouvement de l’appareil, sans essayer de lier l’appareil et dans l’écran le monde réel.

Le `ARImageTrackingConfiguration` reconnaître réelles des images 2D (peintures, logos, etc.) et utilisez-les pour l’imagerie numérique d’ancrage vous permet de :

```csharp
var imagesAndWidths = new[] {
    ("cover1.jpg", 0.185F),
    ("cover2.jpg", 0.185F),
     //...etc...
    ("cover100.jpg", 0.185F),
};

var referenceImages = new NSSet<ARReferenceImage>(
    imagesAndWidths.Select( imageAndWidth =>
    {
      // Tuples cannot be destructured in lambda arguments
        var (image, width) = imageAndWidth;
        // Read the image
        var img = UIImage.FromFile(image).CGImage;
        return new ARReferenceImage(img, ImageIO.CGImagePropertyOrientation.Up, width);
    }).ToArray());

configuration.TrackingImages = referenceImages;
```

Il existe deux aspects intéressants à cette configuration :

* Il est efficace et peut être utilisé avec un nombre potentiellement important des images de référence
* L’imagerie numérique est ancrée à l’image, même si cette image se déplace dans le monde réel (par exemple, si le capot d’un livre est reconnu, il suivra le livre comme il est récupéré à l’emploi, disposition vers le bas, etc..).

Le `ARObjectScanningConfiguration` a été abordé [précédemment](#recognizing-reference-objects) et est une configuration axées sur les développeurs pour l’analyse des objets 3D. Il est extrêmement processeur et la batterie intensive et ne doit pas être utilisé dans les applications de l’utilisateur final. L’exemple [d’analyse et de détection d’objets 3D](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/) illustre l’utilisation de cette configuration.

La configuration du suivi finale, `ARWorldTrackingConfiguration` , est le produit phare de la plupart des expériences de réalité mixte. Cette configuration utilise « odométrie inertie visual » pour lier des « points de la fonctionnalité » monde réel à imagerie numérique. Géométrie numérique ou sprites sont ancrés par rapport aux plans horizontaux et verticaux de réels ou relatif à détectée `ARReferenceObject` instances. Dans cette configuration, le monde a son origine position d’origine de l’appareil photo dans l’espace avec l’axe des z alignée sur la gravité, et des objets numériques » restent en place » par rapport aux objets dans le monde réel.

### <a name="environmental-texturing"></a>L’environnement de sprites

ARKit 2 prend en charge « l’environnement sprites » qui utilise des images capturées pour estimer l’éclairage et même appliquer les surbrillances spéculaires aux objets brillants. La carte cubique l’environnement est créée manière dynamique et, une fois que l’appareil photo a examinées dans toutes les directions, peut produire une expérience plus impressionnant est réaliste :

![image de démonstration de texture l’environnement](images/arkit_env_texturing.png)

Pour pouvoir utiliser la texture de l’environnement :

* Votre [ `SCNMaterial` ](https://developer.xamarin.com/api/type/SceneKit.SCNMaterial/) objets doivent utiliser [ `SCNLightingModel.PhysicallyBased` ](https://developer.xamarin.com/api/property/SceneKit.SCNLightingModel.PhysicallyBased/) et affecter une valeur dans la plage de 0 à 1 pour [ `Metalness.Contents` ](https://developer.xamarin.com/api/property/SceneKit.SCNMaterial.Metalness/) et [ `Roughness.Contents` ](https://developer.xamarin.com/api/property/SceneKit.SCNMaterialProperty.Contents/) et
* Votre configuration de suivi doit définir [ `EnvironmentTexturing` ](https://developer.xamarin.com/api/property/ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing/)  =  [AREnvironmentTexturing.Automatic'](https://developer.xamarin.com/api/field/ARKit.AREnvironmentTexturing.Automatic/) :

```csharp
var sphere = SCNSphere.Create(0.33F);
sphere.FirstMaterial.LightingModelName = SCNLightingModel.PhysicallyBased;
// Shiny metallic sphere
sphere.FirstMaterial.Metalness.Contents = new NSNumber(1.0F);
sphere.FirstMaterial.Roughness.Contents = new NSNumber(0.0F);

// Session configuration:
var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic
};
```

Bien que la texture parfaitement réfléchissante indiquée dans l’extrait de code précédent est amusants dans un exemple, la texture de l’environnement est probablement mieux utilisé avec modération peur elle déclenche une réponse « PRÉDISPOSITION valley » (la texture n'est qu’une estimation basée sur le l’appareil photo enregistrées).


### <a name="shared-and-persistent-ar-experiences"></a>Expériences de AR partagés et permanents

Un autre ajout majeur à ARKit 2 est le [ `ARWorldMap` ](https://developer.xamarin.com/api/type/ARKit.ARWorldMap/) (classe), ce qui vous permet de partager ou stocker des données de suivi de monde. Vous obtenez la carte du monde actuel avec [ `ARSession.GetCurrentWorldMapAsync` ](https://developer.xamarin.com/api/member/ARKit.ARSession.GetCurrentWorldMapAsync()/) ou [ `GetCurrentWorldMap(Action<ARWorldMap,NSError>` ](https://developer.xamarin.com/api/member/ARKit.ARSession.GetCurrentWorldMap/p/System.Action%7BARKit.ARWorldMap,Foundation.NSError%7D/) :

```csharp
// Local storage
var PersistentWorldPath => Environment.GetFolderPath(Environment.SpecialFolder.Personal) + "/arworldmap";

// Later, after scanning the environment thoroughly...
var worldMap = await Session.GetCurrentWorldMapAsync();
if (worldMap != null)
{
    var data = NSKeyedArchiver.ArchivedDataWithRootObject(worldMap, true, out var err);
    if (err != null)
    {
        Console.WriteLine(err);
    }
    File.WriteAllBytes(PersistentWorldPath, data.ToArray());
}
```

Pour partager ou restaurer la carte du monde :

1. Charger les données à partir du fichier,
2. Décompresse dans un `ARWorldMap` objet,
3. Utiliser comme la valeur de la [ `ARWorldTrackingConfiguration.InitialWorldMap` ](https://developer.xamarin.com/api/property/ARKit.ARWorldTrackingConfiguration.InitialWorldMap/) propriété :

```csharp
var data = NSData.FromArray(File.ReadAllBytes(PersistentWorldController.PersistenWorldPath));
var worldMap = (ARWorldMap)NSKeyedUnarchiver.GetUnarchivedObject(typeof(ARWorldMap), data, out var err);

var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic,
    InitialWorldMap = worldMap
};
```

Le `ARWorldMap` contient seulement les données de suivi de monde non visibles et [ `ARAnchor` ](https://developer.xamarin.com/api/type/ARKit.ARAnchor/) objets, c’est le cas _pas_ contiennent des ressources numériques. Pour partager une géométrie ou imagerie, vous devrez développer votre propre stratégie appropriée à votre cas d’usage (par exemple en stockage/la transmission uniquement l’emplacement et l’orientation de la géométrie et de son application statique `SCNGeometry` ou peut-être en stockage/la transmission objets sérialisés). L’avantage de la `ARWorldMap` est que les ressources, une fois placés par rapport à un partage `ARAnchor`, apparaîtra cohérente entre les périphériques ou les sessions.

### <a name="universal-scene-description-file-format"></a>Format de fichier de Description de la scène universel

La fonctionnalité de titre final de ARKit 2 est l’adoption d’Apple de Pixar [Universal Description de scène](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html) format de fichier. Ce format remplace le format DAE de Collada en tant que le format par défaut pour le partage et de stockage ARKit actifs. Prise en charge de visualisation des ressources est intégrée à iOS 12 et Mojave. L’extension de fichier USDZ est une archive zip non compressés et non chiffré contenant les fichiers USD. Pixar [fournit des outils pour travailler avec les fichiers USD](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit) mais il n’est pas encore prise en charge beaucoup tiers.

## <a name="arkit-programming-tips"></a>Conseils de programmation ARKit

### <a name="manual-resource-management"></a>Gestion des ressources manuelle

Dans ARKit, il est essentiel gérer les ressources manuellement. Non seulement cela autorise-t-elle fréquences d’images élevés, il s’agit en fait _nécessaire_ afin d’éviter une confusion entre « gel de l’écran. » Le framework ARKit est différé sur la définition d’un nouveau frame de l’appareil photo ([`ARSession.CurrentFrame`](https://developer.xamarin.com/api/property/ARKit.ARSession.CurrentFrame/)). Jusqu'à ce que l’actuel [ `ARFrame` ](https://developer.xamarin.com/api/type/ARKit.ARFrame/) a `Dispose()` appelé dessus, ARKit n’offre pas un nouveau frame ! Cela entraîne la vidéo « geler » même si le reste de l’application est réactif. La solution consiste à toujours accéder aux `ARSession.CurrentFrame` avec un `using` bloquer ou appeler manuellement `Dispose()` dessus.

Tous les objets dérivés de `NSObject` sont `IDisposable` et `NSObject` implémente le [modèle Dispose](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern), de sorte que vous devez généralement suivre [ce modèle d’implémentation `Dispose` sur une dérivée classe](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose).

### <a name="manipulating-transform-matrices"></a>Manipulation des matrices de transformation

Dans n’importe quelle application 3D, vous allez être confronté à des matrices de transformation de 4 x 4 qui décrivent de manière compacte comment déplacer, faire pivoter et déformer un objet dans l’espace 3D. Dans SceneKit, il s’agit des [ `SCNMatrix4` ](https://developer.xamarin.com/api/type/SceneKit.SCNMatrix4/) objets.  

Le [ `SCNNode.Transform` ](https://developer.xamarin.com/api/property/SceneKit.SCNNode.Transform/) propriété retourne le `SCNMatrix4` matrice de transformation pour le [ `SCNNode` ](https://developer.xamarin.com/api/type/SceneKit.SCNNode/) _comme soutenu par_ ligne-major `simdfloat4x4` type. Par conséquent, par exemple :

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

Comme vous pouvez le voir, la position est encodée dans les trois premiers éléments de la ligne bas.

Dans Xamarin, le type commun pour manipuler les matrices de transformation est `NVector4`, qui par convention est interprété de manière colonne-major. Autrement dit, le composant de traduction ou de la position est attendu dans M14, M24, M34, pas M41, M42, M43 :

![Visual Studio de ligne-champ colonne-major](images/arkit_row_vs_column.png)

Cohérente avec le choix de l’interprétation de la matrice impérativement le comportement approprié. Dans la mesure où les matrices de transformation 3D sont 4 x 4, les erreurs de cohérence ne produira pas n’importe quel type d’exception lors de la compilation ou même de l’exécution, c’est que les opérations seront agir de façon inattendue. Si votre SceneKit / ARKit objets semblent être bloqué, piloter absent ou instabilité, une matrice de transformation incorrecte a de fortes chances. La solution est simple : [ `NMatrix4.Transpose` ](https://developer.xamarin.com/api/member/OpenTK.NMatrix4.Transpose) effectuera une transposition in situ d’éléments.

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – analyse et de détection d’objets 3D](https://developer.xamarin.com/samples/monotouch/iOS12/ScanningAndDetecting3DObjects/)
- [Quelles sont les nouveautés dans ARKit 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [ARKit de présentation de suivi et de détection (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Introduction à ARKit dans Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
