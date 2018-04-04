---
title: Modifications des infrastructures supplémentaires tvOS 10
description: Cet article traite des modifications supplémentaires, secondaires ou des améliorations apportées aux infrastructures existantes de tvOS 10.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c53726b91a0cd3e79041b9b1d51e9f7fbb1c79bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="additional-tvos-10-frameworks-changes"></a>Modifications des infrastructures supplémentaires tvOS 10

_Cet article traite des modifications supplémentaires, secondaires ou des améliorations apportées aux infrastructures existantes de tvOS 10._


En plus des principales modifications apportées à tvOS, Apple a apporté des modifications et améliorations apportées à plusieurs infrastructures existantes dans tvOS 10.

<a name="AV-Foundation-Framework" />

## <a name="av-foundation-framework-additions"></a>Ajouts de Framework AV Foundation

Le framework AVFoundation inclut les améliorations suivantes :

 - Dans tvOS 10, l’application n’implémente plus différents [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) comportements basés sur le type de contenu. Il suffit de définir la `Rate` propriété et AVFoundation déterminent quand assez de contenu est disponible pour la lecture sans blocage.
 - La nouvelle `AVPlayerLooper` classe facilite la boucle d’une partie donnée du média pendant la lecture.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>Améliorations AVKit Framework

Le framework AVKit inclut les améliorations suivantes :

 - L’application a désormais contrôler son comportement est ignoré de le [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) donc un mouvement en ignorant peuvent se déplacer vers l’élément suivant dans la sélection ou l’avance dans l’élément actuel.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Améliorations de données de base

tvOS 10 inclut les améliorations suivantes à l’infrastructure de données de base :

 - Racine [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) prend en charge les objets simultanés de provoquer une erreur et d’extraction sans sérialisation.
 - Le [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) classe gère un pool de magasins de données SQLite.
 - Le [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objets avec des banques de données SQLite dans la prise en charge du Mode de Journal WAL la nouvelle génération de requête de fonctionnalité où contextes gérés de l’objet (cours) peuvent être épinglés à des versions spécifiques de la base de données pour l’extraction des futures et transactions défaillant.
 - À l’aide de la vue `NSPersistenceContainer` référence le `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) et d’autres ressources de configuration des données de base.
 - Plusieurs nouvelles méthodes pratiques ont été ajoutés à `NSManagedObject` rend plus faciles à effectuer des extractions et créer des sous-classes.

Pour plus d’informations, consultez le site d’Apple [Core données Framework Reference](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Améliorations du graphique principal

tvOS 10 inclut les améliorations suivantes à l’infrastructure de graphiques de base :

 - La nouvelle [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) classe peut être utilisée pour effectuer une série de conversions de couleurs.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Améliorations de l’Image Core

tvOS 10 apporte les améliorations suivantes à l’infrastructure de l’Image de base :

 - Le `ImageWithExtent` méthode de la [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) classe peut être utilisée pour insérer de traitement personnalisé dans l’opération de filtrage. Affiche ou appeler le rappel donné entre les filtres lors du traitement d’une image pour la sortie Image principale.
 - L’application peut traiter maintenant les images dans un espace de couleurs en dehors de l’espace colorimétrique de travail du contexte de l’Image principale par la conversion vers et depuis l’espace colorimétrique avant et après le traitement.
 - Plusieurs améliorations de performances de rendu ont été apportées à `UIImage` (lorsqu’il est pris en charge par les magasins d’image Image Core) de rendu dans `UIImageView` objets. 
 - `UIImage` objets avec balises gamme apparaîtront comme gamme de couleurs dans `UIImageView` objets sur les appareils iOS qui prennent en charge les couleurs étendue.
 - Le code de noyau Core Image peut maintenant demander pixel spécifique des formats de sortie.

En outre, les nouveaux filtres Image Core suivants ont été ajoutés :

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Améliorations de base

Les améliorations suivantes ont été apportées à l’infrastructure de base de tvOS 10 :

 - Utilisez la nouvelle [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) classe pour effectuer des calculs d’intervalle de date et heure telles que des durées, des intervalles de comparaison et de test pour les intersections d’intervalle.
 - Plusieurs nouvelles propriétés ont été ajoutées à la [NSLocal](https://developer.apple.com/reference/foundation/nslocale) classe pour acquérir des informations locales et les formats d’affichage disponible.
 - Utilisez la nouvelle [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) classe pour convertir entre différentes unités de mesure (unité) ou effectuer des calculs sur les valeurs d’unités différentes.
 - Utilisez la nouvelle [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) classe pour mettre en forme des mesures localisées pour l’affichage à l’utilisateur final.
 - Utilisez la nouvelle [NSUnit](https://developer.apple.com/reference/foundation/nsunit) et [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) classes pour la représentation des unités spécifiques.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>Améliorations de GameKit

Les améliorations suivantes ont été apportées à l’infrastructure GameKit dans tvOS 10 :

 - Un nouveau type de compte iCloud uniquement a été implémenté par le [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) classe.
 - La nouvelle [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) classe fournit une solution généralisée pour la gestion du stockage de données persistantes sur Game Center. `GKGameSession` gère une liste de lecteurs et de l’application est responsable de mise en œuvre quand et comment la date participant est stocké, extraites ou échangées entre des lecteurs. Dans de nombreux cas les Sessions de jeu peuvent remplacer correspondances existantes à tour, les correspondances en temps réel ou jeu persistant méthodes de sauvegarde.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>Améliorations de GameplayKit

Les améliorations suivantes ont été apportées à l’infrastructure GameplayKit dans tvOS 10 :

 - Génération de bruit procédurales a été ajoutée et peut être utilisée pour améliorer le réalisme des textures d’aspect naturel, ajouter réalisme aux mouvements de la caméra et aider à générer des jeux aux univers élaborés.
 - Utiliser le partitionnement Spatial pour partitionner les données du monde du jeu de recherche efficace.
 - Une nouvelle stratégie de Monte-Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) a été ajoutée pour le calcul de déplacement possibles exhaustive.
 - Une nouvelle API d’arborescence de décision a été ajoutée ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) et [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) pour améliorer la création de jeu AI.
 - Prise en charge 3D a été ajouté à l’agent existant et les comportements de recherche de chemin d’accès à l’aide de la nouvelle [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) et [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
 - Utilisez la nouvelle [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) classe afin de fournir des chemins d’accès de hautes performances, l’aspect naturel.
 - La nouvelle [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) et [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Vérifiez classes combinaison GameplayKit et SpriteKit plus facile que jamais.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Améliorations complètes

Les améliorations suivantes ont été apportées à l’infrastructure complète dans tvOS 10 :

 - Peuvent désormais utiliser les applications et jeux 3D _pavage_ afin d’afficher efficacement les scènes complexes et géométrie via le GPU.
 - Utilisez la spécialisation de la fonction pour créer une collection fortement optimisé de matériel et les fonctions de lumière combinaison pour une scène.
 - Fournir un contrôle précis de l’allocation de ressources pour optimiser les performances de métal Memoryless cibles de rendu et les applications à l’aide des ressources de segments de mémoire.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation complète](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Améliorations des performances complète de nuanceurs

Les améliorations suivantes ont été apportées à l’infrastructure complète performances nuanceurs dans tvOS 10 :

 - Nombreux noyaux ont été ajoutés à l’infrastructure complète performances nuanceurs pour permettre à l’application tirer parti des calculs hautement optimisée, les données en parallèle telles que les conversions d’espace de couleurs et les opérations de réseau neuronal.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>Améliorations de ModelIO

Les améliorations suivantes ont été apportées à l’infrastructure ModelIO dans tvOS 10 :

 - Le format de fichier (USD) est maintenant pris en charge.
 - Utilisez la nouvelle `MDLMaterialPropertyGraph` classe facilement prendre en charge runtime remplace les modèles.
 - Signé champ Distance prise en charge a été ajouté à la [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) classe.
 - Utilisez la nouvelle `MDLLightProbeIrradianceDataSource` classe pour faciliter la sélection élective de sonde de la lumière.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>Améliorations de SceneKit

Les améliorations suivantes ont été apportées à l’infrastructure SceneKit dans tvOS 10 :

 - SceneKit inclut désormais un nouveau système physiquement en fonction de rendu (PBR) pour des résultats plus réalistes avec création d’élément multimédia plus simple.
 - Utilisez la nouvelle [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) ombrage de modèle de produit un large éventail d’effets d’ombrage réaliste tout en exigeant que trois propriétés fondamentales (`Diffuse`, `Metalness` et `Roughness`).
 - Depuis PBR ombrage fonctionne mieux avec éclairage de basée sur l’environnement, vous devez utiliser le `LightingEnvironment` propriété à attribuer à base d’image éclairage tan scène.
 - Utilisez le `IESProfileURL` propriété à importer des éléments réels qui définissent d’éclairage base sur les valeurs réelles comme intensité (dans lumens) et la couleur de température (en degrés Kelvin).
 - Le [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) classe peut fournir réalisme supérieur à l’aide des fonctionnalités de l’en-tête et les effets. Exposition adaptative permet de créer des effets automatique ou utilisez vignettage, frange de couleur et dégradé de couleur pour ajouter des effets filmatic au jeu.
 - Fonctionnalités d’appareil photo PBR et HDR fournissent les meilleurs résultats que les techniques traditionnelles de rendu et, par conséquent, SceneKit s’exécute maintenant tous les calculs de couleur dans un espace colorimétrique linéaire (à l’aide de la gamme de couleurs P3 sur les appareils à l’échelle de couleurs affiche).
 - SceneKit maintenant de couleur correspond à toutes les couleurs en lisant les informations de profil de couleurs.
 - SceneKit interprète les valeurs des composants de couleur dans un espace de couleurs RVB linéaire pour tous les types de nuanceur.
 - SceneKit lit et ajuster les informations de profil de couleurs dans les images de texture, utilisez catalogues Asset pour toutes les images pour garantir que cette information est fournie.
 - Rendu de l’espace de couleurs à la fois linéaire et à l’échelle de couleurs peut être désactivée en spécifiant le `SCNDisableLinearSpaceRendering` et `SCNDisableWideGamut` clés dans l’application `Info.plist`.
 - Build primates polygone arbitraire (soit chargé à partir de fichiers ou générée par programmation) pour spécifier la géométrie avec le nouveau [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) classe.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>Améliorations de SpriteKit

Les améliorations suivantes ont été apportées à l’infrastructure SpriteKit dans tvOS 10 :

 - Tilemaps prennent désormais en charge la vignette carrée, hexagonal et isométrique formes pour 2D, 2,5 D et jeux de défilement côté à l’aide de la `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` et `SKTileSet` classes.
 - Utilisez la nouvelle `SKWarpGeometry` classe pour étendre ou déformer [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) ou [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendu. La nouvelle [SKAction](https://developer.apple.com/reference/spritekit/skaction) classe peut être utilisée pour animer les transitions entre les effets de distorsion.
 - Les nuanceurs personnalisés peuvent fournir des attributs (`SKAttribute`) qui peut être configuré séparément par chaque nœud qui utilise le nuanceur en fournissant une valeur d’attribut (`SKAttributeValue`).
 - Le [SKView](https://developer.apple.com/reference/spritekit/skview) classe fournit plusieurs nouvelles méthodes pour accorder un contrôle affiné sur quand et comment une scène est rendue.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>Améliorations de UIKit

Les améliorations suivantes ont été apportées à l’infrastructure UIKit dans tvOS 10 :

 - Le focus API a été amélioré pour prendre en charge de focus de l’élément d’affichage non en outre à `UIViews`. Les éléments qui prennent en charge le focus _doit_ implémenter la `IUIFocusItem` interface.
 - La nouvelle `UIGraphicsRender` classe fournit une méthode orientée objet de création des fichiers bitmap ou des fichiers PDF à partir de UIKit rendu ou graphiques élémentaires et remplace déconseillées `UIGraphicsBeginImageContext` (méthode).
 - La `UIUserInterfaceStyle` classe a été ajoutée afin de déterminer le thème de l’interface utilisateur (sombre ou clair) est actuellement actif.
 - Nouvelle prise en charge entièrement interactif, objet, interruptible animation a été ajouté et van être liés aux mouvements. Du Veuillez consultez Apple [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [référence de classe UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Référence de classe UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) et [référence de classe UISpringTimingParameter](https://developer.apple.com/reference/uikit/uispringtimingparameters) pour plus d’informations.
 - La nouvelle `UIPreviewInteraction` et `UIPreviewInteractionDelegate` permettent à l’application de l’application fournir une interface personnalisée pour les opérations de lecture et pop.
 - La nouvelle `UIAccessibilityCustomRotor` classe permet à l’application fournir des fonctionnalités personnalisées, spécifiques de contexte pour les technologies d’assistance comme la voix sur.
 - Utilisez le `UIAccessibilityIsAssistiveTouchRunning` et `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symboles pour déterminer si AssistiveTouch est activée.
 - Utilisez le `UIAccessibilityHearingDevicePairedEar` et `UIAccessibilityHearingDevicePairedEarDidChangeNotification` pour obtenir l’état de tous les symboles associés auditifs IFM.
 - La nouvelle [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API propose de nouvelles options (telles que les limitations de la durée de vie) et allez déclarer automatiquement les types de contenu compatibles pour les types de classe.
 - Pour prendre en charge le Type dynamique dans les étiquettes, utilisent les nouveaux champs de texte et des zones de texte `PreferredFontForTextStyle` méthode de la `UIFont` classe.
 - Pour déterminer si un élément doit mettre à jour police lorsque les appareils `UIContentSizeCategory` modifiée, utilisez la `AdjustsFontForContentSizeCategory` propriété de la `UIContentSizeCategoryAdjusting` déléguer.
 - L’application peut désormais contrôler l’apparence du badge pour les éléments de barre d’onglet telles que la couleur de texte et d’arrière-plan.
 - Le contrôle de l’actualisation désormais pris en charge dans toutes les vues de défilement et défilement sous-classes (tel que `UICollectionView`).
 - Le `OpenURL` méthode de la `UIApplication` classe est appelée de façon asynchrone prend désormais en charge qui est appelée après l’ouverture d’un gestionnaire d’achèvement.
 - Démarrer le partage de CloudKit et modifier ses propriétés à l’aide de la nouvelle `UICloudSharingController` et `UICloudSharingControllerDelegate` classes.
 - Tirer parti des cellules prérécupérées pour améliorer l’expérience de défilement de `UICollectionViews` avec la nouvelle `UICollectionViewDataSourcePrefetching` déléguer.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Nouveautés de tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
