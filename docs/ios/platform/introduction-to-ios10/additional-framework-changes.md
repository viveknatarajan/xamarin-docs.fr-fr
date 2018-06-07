---
title: Modifications des infrastructures supplémentaires iOS 10
description: Ce document décrit les changements mineurs et améliorations apportées aux infrastructures existantes dans iOS 10 et explique comment vous utilisez ces mises à jour dans Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 4b9a230157593b66446e2949e57a925d94208752
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787557"
---
# <a name="additional-ios-10-frameworks-changes"></a>Modifications des infrastructures supplémentaires iOS 10

_Cet article traite des modifications supplémentaires, secondaires ou des améliorations apportées aux infrastructures existantes pour iOS 10._

## <a name="av-foundation-framework-additions"></a>Ajouts de Framework AV Foundation

Le framework AVFoundation inclut les améliorations suivantes :

- Dans iOS 10, le développeur n’a plus à implémenter différentes [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) comportements basés sur le type de contenu. Il suffit de définir la `Rate` propriété et AVFoundation déterminent quand assez de contenu est disponible pour la lecture sans blocage.
- La nouvelle [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) classe remplace déconseillées `AVCaptureStillImageOutput` classe et fournit une méthode unifiée pour gérer tous les workflows photographie en fournissant un contrôle sophistiqué et de surveiller le processus de capture et prend en charge pour les nouvelles fonctionnalités telles que les Photos de Live et le format brut de capture.
- La nouvelle `AVPlayerLooper` classe facilite la boucle d’une partie donnée du média pendant la lecture.
- La `AVAssetDownloadURLSession` classe permet le téléchargement et une lecture ultérieure de FairPlay chiffré flux HLS.
- Par défaut, le [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) classe prend en charge automatiquement à l’échelle de couleurs, de la gamme de capture lorsque le matériel du périphérique prend en charge. Consultez d’Apple [iOS référence de compatibilité des périphériques](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) pour plus d’informations.

## <a name="avkit-additions"></a>Ajouts de AVKit

Le framework AVKit inclut désormais la nouvelle `UpdatesNowPlayingInfoCenter` propriété pour indiquer lorsque le centre d’informations jouer maintenant doit être mis à jour.

## <a name="core-data-enhancements"></a>Améliorations de données de base

iOS 10 inclut les améliorations suivantes à l’infrastructure de données de base :

- Le [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) objets avec des banques de données SQLite dans la prise en charge du Mode de Journal WAL la nouvelle génération de requête de fonctionnalité où contextes gérés de l’objet (cours) peuvent être épinglés à des versions spécifiques de la base de données pour l’extraction des futures et transactions défaillant.
- Racine [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) prend en charge les objets simultanés de provoquer une erreur et d’extraction sans sérialisation.
- Le [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) classe gère un pool de magasins de données SQLite.
- Plusieurs nouvelles méthodes pratiques ont été ajoutés à `NSManagedObject` rend plus faciles à effectuer des extractions et créer des sous-classes.
- À l’aide de la vue `NSPersistenceContainer` référence le `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) et d’autres ressources de configuration des données de base.

Pour plus d’informations, consultez le site d’Apple [Core données Framework Reference](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Améliorations de l’Image Core

iOS 10 apporte les améliorations suivantes à l’infrastructure de l’Image de base :

- Le développeur peut traiter maintenant les images dans un espace de couleurs en dehors de l’espace colorimétrique de travail du contexte de l’Image principale par la conversion vers et depuis l’espace colorimétrique avant et après le traitement.
- Pour les appareils iOS qui utilisent le A8 ou A9 UC, le format d’image brut est maintenant pris en charge. Image Core prend désormais en charge pour le décodage des images brutes à partir de le l’appareil photo iSight intégré ou d’un appareil photo de tiers 3e. Utilisez le `FilterWithImageData` ou `FilterWithImageURL` méthodes de la [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) classe pour traiter des images RAW.
- Plusieurs améliorations de performances de rendu ont été apportées à `UIImage` (lorsqu’il est pris en charge par les magasins d’image Image Core) de rendu dans `UIImageView` objets. 
- `UIImage` objets avec balises gamme apparaîtront comme gamme de couleurs dans `UIImageView` objets sur les appareils iOS qui prennent en charge les couleurs étendue.
- Le code de noyau Core Image peut maintenant demander pixel spécifique des formats de sortie.
- Le `ImageWithExtent` méthode de la [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) classe peut être utilisée pour insérer de traitement personnalisé dans l’opération de filtrage. Affiche ou appeler le rappel donné entre les filtres lors du traitement d’une image pour la sortie Image principale.

En outre, les nouveaux filtres Image Core suivants ont été ajoutés :

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Ajouts de mouvement Core

Nouveau pour iOS 10, le framework Core mouvement inclut les événements PODOMÈTRE qui permettent à une application recevoir des notifications en temps réel de l’utilisateur, suspension et reprise de suivi en cours d’exécution rapide,. Utilisez le [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) pour enregistrer les événements de PODOMÈTRE au premier plan ou en arrière-plan.

## <a name="foundation-enhancements"></a>Améliorations de base

Les améliorations suivantes ont été apportées à l’infrastructure Foundation pour iOS 10 :

- Utilisez la nouvelle [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) classe pour mettre en forme des mesures localisées pour l’affichage à l’utilisateur final.
- Utilisez la nouvelle [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) classe pour effectuer des calculs d’intervalle de date et heure telles que des durées, des intervalles de comparaison et de test pour les intersections d’intervalle.
- Utilisez la nouvelle [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) classe pour convertir entre différentes unités de mesure (unité) ou effectuer des calculs sur les valeurs d’unités différentes.

- Utilisez la nouvelle [NSUnit](https://developer.apple.com/reference/foundation/nsunit) et [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) classes pour la représentation des unités spécifiques.
- Plusieurs nouvelles propriétés ont été ajoutées à la [NSLocal](https://developer.apple.com/reference/foundation/nslocale) classe pour acquérir des informations locales et les formats d’affichage disponible.

## <a name="gamekit-enhancements"></a>Améliorations de GameKit

Les améliorations suivantes ont été apportées à l’infrastructure GameKit dans iOS 10 :

- Le **Game Center App** a été déconseillé et supprimé à partir d’iOS. Si l’application utilise GameKit, il _doit_ présenter son propre interface pour afficher les fonctionnalités GameKit telles qu’ajouterons, etc. 
- Un nouveau type de compte iCloud uniquement a été implémenté par le [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) classe.
- La nouvelle [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) classe fournit une solution généralisée pour la gestion du stockage de données persistantes sur Game Center. `GKGameSession` gère une liste de lecteurs et de l’application est chargée d’implémenter comment et quand date participant est stocké, récupérée ou échangé entre les lecteurs. Dans de nombreux cas les Sessions de jeu peuvent remplacer correspondances existantes à tour, les correspondances en temps réel ou jeu persistant méthodes de sauvegarde.

## <a name="gameplaykit-enhancements"></a>Améliorations de GameplayKit

Les améliorations suivantes ont été apportées à l’infrastructure GameplayKit dans iOS 10 :

- Utilisez la nouvelle [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) classe afin de fournir des chemins d’accès de hautes performances, l’aspect naturel.
- Génération de bruit procédurales a été ajoutée et peut être utilisée pour améliorer le réalisme des textures d’aspect naturel, ajouter réalisme aux mouvements de la caméra et aider à générer des jeux aux univers élaborés.
- Utiliser le partitionnement Spatial pour partitionner les données du monde du jeu de recherche efficace.
- Une nouvelle stratégie de Monte-Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) a été ajoutée pour le calcul de déplacement possibles exhaustive.
- Prise en charge 3D a été ajouté à l’agent existant et les comportements de recherche de chemin d’accès à l’aide de la nouvelle [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) et [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
- La nouvelle [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) et [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Vérifiez classes combinaison GameplayKit et SpriteKit plus facile que jamais.
- Une nouvelle API d’arborescence de décision a été ajoutée ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) et [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) pour améliorer la création de jeu AI.

## <a name="healthkit-enhancements"></a>Améliorations de HealthKit

Les améliorations suivantes ont été apportées à l’infrastructure HealthKit dans iOS 10 :

- Nouvelles clés de métadonnées ont été ajoutés pour les types de météo (tel que `HKWeatherConditionClear` et `HKWeatherConditionCloudy`) et les types d’entraînement (tel que `HKWorkoutActivityTypeFlexibility` et `HKWorkoutActivityTypeWheelchairRunPace`) ont été ajoutés.
- La nouvelle `HKCDADocument` classe a été ajoutée pour représenter une Architecture de Document cliniques (CDA) mis en forme de document.
- Utilisez la nouvelle [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) classe pour spécifier le `ActivityType` et `LocationType` d’Use.
- La nouvelle [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) et `WheelchairUse` méthode de la [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) classe ont été ajoutées pour travailler avec fauteuil relatives des données d’intégrité.

## <a name="homekit-enhancements"></a>Améliorations de HomeKit

Les améliorations suivantes ont été apportées à l’infrastructure HomeKit dans iOS 10 :

- Caractéristiques et nouveaux services ont été ajoutés.
- Un iPad peut être configuré pour agir comme un concentrateur HomeKit pour fournir l’accès à distance Accessoires, exécuter les déclencheurs d’automation et activer partagé des autorisations utilisateur.
- Prise en charge a été ajoutée pour les appareils photo et sonnette accessoires.
- Plus de contexte et de configurations ont été fournies pour les accessoires.

Veuillez consulter notre [présentation HomeKit](~/ios/platform/homekit.md) documentation pour plus d’informations.

## <a name="metal-enhancements"></a>Améliorations complètes

Les améliorations suivantes ont été apportées à l’infrastructure complète dans iOS 10 :

- Peuvent désormais utiliser les applications et jeux 3D _pavage_ afin d’afficher efficacement les scènes complexes et géométrie via le GPU.
- Fournir un contrôle précis de l’allocation de ressources pour optimiser les performances de métal Memoryless cibles de rendu et les applications à l’aide des ressources de segments de mémoire.
- Utilisez la spécialisation de la fonction pour créer une collection fortement optimisé de matériel et les fonctions de lumière combinaison pour une scène.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation complète](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>Améliorations de ModelIO

Les améliorations suivantes ont été apportées à l’infrastructure ModelIO dans iOS 10 :

 - Le format de fichier (USD) est maintenant pris en charge.
 - Signé champ Distance prise en charge a été ajouté à la [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) classe.
 - Utilisez la nouvelle `MDLLightProbeIrradianceDataSource` classe pour faciliter la sélection élective de sonde de la lumière.
 - Utilisez la nouvelle `MDLMaterialPropertyGraph` classe facilement prendre en charge runtime remplace les modèles.

## <a name="photos-enhancements"></a>Améliorations de photos

Les améliorations suivantes ont été apportées à l’infrastructure de Photos dans iOS 10 :

- Utilisez le [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) et [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) classes pour tirer parti de la nouvelle fonctionnalité de processeur d’Image mémoire pour effectuer des modifications.
- Une modification en direct Photo est désormais disponible pour les applications qui prennent en charge l’infrastructure de Photos et à la modification des extensions de photos (pour une utilisation à l’intérieur de la photo et Photos applications).
- Utilisez la nouvelle [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) classe pour appliquer des modifications à la vidéo et toujours le contenu en direct des photos.

## <a name="replaykit-enhancements"></a>Améliorations de ReplayKit

Les améliorations suivantes ont été apportées à l’infrastructure ReplayKit dans iOS 10 :

- Utilisez le [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) et [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) classes pour prendre en charge la diffusion d’enregistrement media via 3ème partie sites.
- Les extensions de l’interface utilisateur de la diffusion et la diffusion télécharger sont requis pour prendre en charge des services de diffusion ReplayKit 3e partie dans l’application.

## <a name="scenekit-enhancements"></a>Améliorations de SceneKit

Les améliorations suivantes ont été apportées à l’infrastructure SceneKit dans iOS 10 :

- Le [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) classe peut fournir réalisme supérieur à l’aide des fonctionnalités de l’en-tête et les effets. Exposition adaptative permet de créer des effets automatique ou utilisez vignettage, frange de couleur et dégradé de couleur pour ajouter des effets fillmatic le jeu.
- SceneKit inclut désormais un nouveau système physiquement en fonction de rendu (PBR) pour des résultats plus réalistes avec création d’élément multimédia plus simple.
- Utilisez la nouvelle [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) ombrage de modèle de produit un large éventail d’effets d’ombrage réaliste tout en exigeant que trois propriétés fondamentales (`Diffuse`, `Metalness` et `Roughness`).
- Depuis PBR ombrage fonctionne mieux avec éclairage de basée sur l’environnement, vous devez utiliser le `LightingEnvironment` propriété à attribuer d’éclairage de base d’image pour une scène.
- Utilisez le `IESProfileURL` basée sur les propriétés pour importer des éléments réels qui définissent l’éclairage sur les valeurs réelles telles que l’intensité (en lumens) et de température de couleur (en degrés Kelvin).
- Fonctionnalités d’appareil photo PBR et HDR fournissent les meilleurs résultats que les techniques traditionnelles de rendu et, par conséquent, SceneKit s’exécute maintenant tous les calculs de couleur dans un espace colorimétrique linéaire (à l’aide de la gamme de couleurs P3 sur les appareils à l’échelle de couleurs affiche).
- SceneKit maintenant de couleur correspond à toutes les couleurs en lisant les informations de profil de couleurs.
- SceneKit interprète les valeurs des composants de couleur dans un espace de couleurs RVB linéaire pour tous les types de nuanceur.
- Rendu de l’espace de couleurs à la fois linéaire et à l’échelle de couleurs peut être désactivée en spécifiant le `SCNDisableLinearSpaceRendering` et `SCNDisableWideGamut` clés dans l’application `Info.plist`.
- Build primates polygone arbitraire (soit chargé à partir de fichiers ou générée par programmation) pour spécifier la géométrie avec le nouveau [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) classe.
- SceneKit lit et ajuster les informations de profil de couleurs dans les images de texture, utilisez catalogues Asset pour toutes les images pour garantir que cette information est fournie.

## <a name="spritekit-enhancements"></a>Améliorations de SpriteKit

Les améliorations suivantes ont été apportées à l’infrastructure SpriteKit dans iOS 10 :

- Les nuanceurs personnalisés peuvent fournir des attributs (`SKAttribute`) qui peut être configuré séparément par chaque nœud qui utilise le nuanceur en fournissant une valeur d’attribut (`SKAttributeValue`).
- Tilemaps prennent désormais en charge la vignette carrée, hexagonal et isométrique formes pour 2D, 2,5 D et jeux de défilement côté à l’aide de la `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` et `SKTileSet` classes.
- Utilisez la nouvelle `SKWarpGeometry` classe pour étendre ou déformer [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) ou [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) rendu. La nouvelle [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) classe peut être utilisée pour animer les transitions entre les effets de distorsion.
- Le [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) classe fournit plusieurs nouvelles méthodes pour accorder un contrôle affiné sur quand et comment une scène est rendue.

## <a name="scrollview-enhancements"></a>Améliorations de ScrollView

Les améliorations suivantes ont été apportées au contrôle ScrollView dans iOS 10.3 :

- `UIScrollView` incluent désormais le `IndexDisplayMode` propriété pour contrôler la manière dont l’index apparaissent alors que l’utilisateur fait défiler comme un `UIScrollViewIndexDisplayMode` de :
    - `Automatic` -L’affichage de l’index est contrôlée par le système d’exploitation.
    - `AlwaysHidden` -L’affichage de l’index est toujours masquée.

Consultez le [iOSTenThree exemple](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) pour l’utilisation.

## <a name="uikit-enhancements"></a>Améliorations de UIKit

Les améliorations suivantes ont été apportées à l’infrastructure UIKit dans iOS 10 :

- La nouvelle [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API propose de nouvelles options (telles que les limitations de la durée de vie) et allez déclarer automatiquement les types de contenu compatibles pour les types de classe.
- Nouvelle prise en charge entièrement interactif, objet, interruptible animation a été ajouté et peut être lié à des mouvements. Du Veuillez consultez Apple [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [référence de classe UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Référence de classe UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) et [référence de classe UISpringTimingParameter](https://developer.apple.com/reference/uikit/uispringtimingparameters) pour plus d’informations.
- La nouvelle `UIPreviewInteraction` et `UIPreviewInteractionDelegate` permettent à l’application de développeur fournir une interface personnalisée pour les opérations de lecture et pop.
- La nouvelle `UIAccessibilityCustomRotor` classe permet à l’application fournir des fonctionnalités personnalisées, spécifiques de contexte pour les technologies d’assistance comme la voix sur.
- Utilisez le `UIAccessibilityIsAssistiveTouchRunning` et `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symboles pour déterminer si AssistiveTouch est activée.
- Utilisez le `UIAccessibilityHearingDevicePairedEar` et `UIAccessibilityHearingDevicePairedEarDidChangeNotification` pour obtenir l’état de tous les symboles associés auditifs IFM.
- Pour prendre en charge le Type dynamique dans les étiquettes, utilisent les nouveaux champs de texte et des zones de texte `PreferredFontForTextStyle` méthode de la `UIFont` classe.
- Pour déterminer si un élément doit mettre à jour sa police lors du périphérique `UIContentSizeCategory` modifiée, utilisez la `AdjustsFontForContentSizeCategory` propriété de la `UIContentSizeCategoryAdjusting` déléguer.
- Le `OpenURL` méthode de la `UIApplication` classe est appelée de façon asynchrone et prend désormais en charge un gestionnaire d’achèvement qui est appelée après l’action d’ouverture est terminé.
- Démarrer le partage de CloudKit et modifier ses propriétés à l’aide de la nouvelle `UICloudSharingController` et `UICloudSharingControllerDelegate` classes.
- Tirer parti des cellules prérécupérées pour améliorer l’expérience de défilement de `UICollectionViews` avec la nouvelle `UICollectionViewDataSourcePrefetching` déléguer.
- Les développeurs peuvent désormais contrôler l’apparence du badge pour les éléments de barre d’onglet (par exemple, la couleur de texte et d’arrière-plan).
- Le contrôle de l’actualisation est désormais prise en charge dans tous les affichage et sous-classes de mode de défilement (tels que `UICollectionView`).

## <a name="webkit-enhancements"></a>Améliorations de WebKit

Les améliorations suivantes ont été apportées à l’infrastructure WebKit dans iOS 10 :

- Aperçu et prise en charge pop a été ajouté à la `WKWebView` classe. Utilisez la `ShouldPreviewElement` méthode pour déterminer si un affichage web donné doit afficher un aperçu.


## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Quelles sont les nouveautés dans iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
