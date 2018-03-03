---
title: "Modifications du Framework Sierra de macOS supplémentaires"
description: "Cet article traite des modifications supplémentaires, secondaires ou des améliorations apportées aux infrastructures existantes pour macOS Sierra."
ms.topic: article
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d49276d6367a52a05d486cd5cab198c666965bf7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="additional-macos-sierra-framework-changes"></a>Modifications du Framework Sierra de macOS supplémentaires

_Cet article traite des modifications supplémentaires, secondaires ou des améliorations apportées aux infrastructures existantes pour macOS Sierra._

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Accélérer les améliorations de l’infrastructure

Les améliorations suivantes ont été apportées à l’infrastructure accélérer pour macOS Sierra :

- Quadrature ajouté (calcul intégral).
- Fonctions de base ajoutées pour construire des réseaux neuronaux.
- Fonctions de prédicat géométriques ajoutées à tester pour des opérations comme l’intersection de deux objets géométriques.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Améliorations Appkit Framework

Les améliorations suivantes ont été apportées à l’infrastructure AppKit pour macOS Sierra :

- Plusieurs améliorations `NSCollectionView` telles que :
    - **Sections réductibles** -lui permet de réduire une section de la vue de Collection en une seule ligne horizontale.
    - **Flottante en-têtes** -en-têtes et pieds de page peuvent maintenant être flotter (dans une mise en page fluide) à l’aide de la même API que [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) dans iOS.
    - **Les vues en arrière-plan permettant le défilement** -un arrière-plan de vues de collection peut désormais être défini pour faire défiler en même temps que le contenu.
- La passe de disposition différée de l’affichage a été optimisée et étendue.
- L’API de glisser-déposer inclut désormais la nouvelle `NSFilePromiseProvider` et `NSFilePromiseReceiver` flocage faire glisser des classes pour prendre en charge.
- Plusieurs constructeurs de commodité ont été ajoutés à des contrôles existants :
    -  `NSButton` inclut de nouveaux constructeurs pour créer des boutons, boutons de cases à cocher et des cases d’option.
    -  `NSTextField` inclut de nouveaux constructeurs pour créer des étiquettes d’enroulement et sans habillage, étiquettes avec attributs et champs de texte modifiables.
    -  `NSSegmentedControl` inclut de nouveaux constructeurs pour créer des contrôles segmentés à partir d’un groupe d’étiquettes ou des images.
    -  `NSSlider` inclut de nouveaux constructeurs pour créer des curseurs linéaires horizontales.
    -  `NSImageView` inclut de nouveaux constructeurs pour créer des vues non modifiables par l’image à partir d’une donnée `NSImage`.
-  Le nouveau `NSGridView` a été ajouté à une collection de vues de sub dans une grille avec variable taille des lignes et colonnes qui peuvent être masqués ou affichés dynamiquement de la mise en page automatique.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>Améliorations AVFoundation Framework

Les améliorations suivantes ont été apportées à l’infrastructure AVFoundation pour macOS Sierra :

- Dans macOS, l’application ne dispose plus d’implémenter différents [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) comportements basés sur le type de contenu. Il suffit de définir la `Rate` propriété et AVFoundation déterminent quand assez de contenu est disponible pour la lecture sans blocage.
- La nouvelle `AVPlayerLooper` classe facilite la boucle d’une partie donnée du média pendant la lecture.
- La `AVAssetDownloadURLSession` classe permet le téléchargement et une lecture ultérieure de FairPlay chiffré flux HLS.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Améliorations de Framework Core des données

Les améliorations suivantes ont été apportées à l’infrastructure de données principale pour macOS Sierra :

- Racine [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) prend en charge les objets simultanés de provoquer une erreur et d’extraction sans sérialisation.
- Le [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) classe gère un pool de magasins de données SQLite.
- Le [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objets avec des banques de données SQLite dans la prise en charge du Mode de Journal WAL la nouvelle génération de requête de fonctionnalité où contextes gérés de l’objet (cours) peuvent être épinglés à des versions spécifiques de la base de données pour l’extraction des futures et transactions défaillant.
- À l’aide de la vue `NSPersistenceContainer` référence le `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) et d’autres ressources de configuration des données de base.
- Plusieurs nouvelles méthodes pratiques ont été ajoutés à `NSManagedObject` rend plus faciles à effectuer des extractions et créer des sous-classes.

Pour plus d’informations, consultez le site d’Apple [Core données Framework Reference](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Améliorations de Framework Core Image

Les améliorations suivantes ont été apportées à l’infrastructure d’Image de base pour macOS Sierra :

- Le `ImageWithExtent` méthode de la [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) classe peut être utilisée pour insérer de traitement personnalisé dans l’opération de filtrage. Affiche ou appeler le rappel donné entre les filtres lors du traitement d’une image pour la sortie Image principale.
- L’application peut traiter maintenant les images dans un espace de couleurs en dehors de l’espace colorimétrique de travail du contexte de l’Image principale par la conversion vers et depuis l’espace colorimétrique avant et après le traitement.
- Le noyau de l’Image principale peut maintenant demander un format de pixel spécifique de sortie.
- Les nouveaux filtres d’image suivants ont été ajoutés : `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` et `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Améliorations de Framework Foundation

Les améliorations suivantes ont été apportées à l’infrastructure de base pour macOS Sierra :

- Utilisez le [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) API pour représentant, conversion et l’affichage de la plupart des unités physiques courantes tels que de masse, la longueur, la vitesse, la durée et température.
- Utilisez le [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) classe pour l’analyse et la génération de ISO 8601 mis en forme des dates.
- Utilisez la nouvelle [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) classe pour effectuer des calculs d’intervalle de date et heure telles que des durées, des intervalles de comparaison et de test pour les intersections d’intervalle.
- Utilisez le [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) classe pour analyser les éléments d’un nom de personne à partir d’une chaîne.
- Utilisez la nouvelle [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) classe pour obtenir des mesures pour une URL de mise en réseau de la session.

Pour plus d’informations, consultez le site d’Apple [Foundation Notes de publication pour OS X v10.12 et iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>Améliorations GameKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure GameKit pour macOS Sierra :

- Le **Game Center App** a été déconseillé et supprimé de macOS. Si l’application utilise GameKit, il _doit_ présenter son propre interface pour afficher les fonctionnalités GameKit telles qu’ajouterons, etc. 
- Un nouveau type de compte iCloud uniquement a été implémenté par le [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) classe.
- La nouvelle [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) classe fournit une solution généralisée pour la gestion du stockage de données persistantes sur Game Center. `GKGameSession` gère une liste de lecteurs et de l’application est responsable de mise en œuvre quand et comment la date participant est stocké, extraites ou échangées entre des lecteurs. Dans de nombreux cas les Sessions de jeu peuvent remplacer correspondances existantes à tour, les correspondances en temps réel ou jeu persistant méthodes de sauvegarde.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>Améliorations GamePlayKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure GamePlayKit pour macOS Sierra :

- Génération de bruit procédurales a été ajoutée et peut être utilisée pour améliorer le réalisme des textures d’aspect naturel, ajouter réalisme aux mouvements de la caméra et aider à générer des jeux aux univers élaborés.
- Utiliser le partitionnement Spatial pour partitionner les données du monde du jeu de recherche efficace.
- Une nouvelle stratégie de Monte-Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) a été ajoutée pour le calcul de déplacement possibles exhaustive.
- Une nouvelle API d’arborescence de décision a été ajoutée ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) et [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) pour améliorer la création de jeu AI.
- Prise en charge 3D a été ajouté à l’agent existant et les comportements de recherche de chemin d’accès à l’aide de la nouvelle [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) et [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
- Utilisez la nouvelle [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) classe afin de fournir des chemins d’accès de hautes performances, l’aspect naturel.
- La nouvelle [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) et [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) Vérifiez classes combinaison GameplayKit et SpriteKit plus facile que jamais.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Améliorations de l’infrastructure complète

Les améliorations suivantes ont été apportées à l’infrastructure complète pour macOS Sierra :

- Peuvent désormais utiliser les applications et jeux 3D _pavage_ afin d’afficher efficacement les scènes complexes et géométrie via le GPU.
- Utilisez la spécialisation de la fonction pour créer une collection fortement optimisé de matériel et les fonctions de lumière combinaison pour une scène.
- Fournir un contrôle précis de l’allocation de ressources pour optimiser les performances de métal Memoryless cibles de rendu et les applications à l’aide des ressources de segments de mémoire.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation complète](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Améliorations du modèle d’e/s Framework

Les améliorations suivantes ont été apportées à l’infrastructure d’e/s de modèle pour macOS Sierra :

- Le format de fichier (USD) est maintenant pris en charge.
- Utilisez la nouvelle `MDLMaterialPropertyGraph` classe facilement prendre en charge runtime remplace les modèles.
- Signé champ Distance prise en charge a été ajouté à la [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) classe.
- Utilisez la nouvelle `MDLLightProbeIrradianceDataSource` classe pour faciliter la sélection élective de sonde de la lumière.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Améliorations de Framework photos

Les améliorations suivantes ont été apportées à l’infrastructure de Photos pour macOS Sierra :

- Une modification en direct Photo est désormais disponible pour les applications qui prennent en charge l’infrastructure de Photos et à la modification des extensions de photos (pour une utilisation à l’intérieur de la photo et Photos applications).
- Utilisez la nouvelle [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) classe pour appliquer des modifications à la vidéo et toujours le contenu en direct des photos.
- Utilisez le [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) et [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) classes pour tirer parti de la nouvelle fonctionnalité de processeur d’Image mémoire pour effectuer des modifications.
- Pour prendre en charge les Photos Live, le [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) et [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) classes ont été transférés à partir d’iOS pour macOS.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>Améliorations SceneKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure SceneKit pour macOS Sierra :

- Inclut désormais un nouveau système physiquement en fonction de rendu (PBR) pour des résultats plus réalistes avec création d’élément multimédia plus simple.
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Améliorations de sécurité Framework

Les améliorations suivantes ont été apportées à l’infrastructure de sécurité pour macOS Sierra :

- Le `SecKey` interface a été modernisé et unifiée sur toutes les plateformes (iOS, tvOS, watchOS et macOS).

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>Améliorations SpriteKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure SpriteKit pour macOS Sierra :

- Tilemaps prennent désormais en charge la vignette carrée, hexagonal et isométrique formes pour 2D, 2,5 D et jeux de défilement côté à l’aide de la `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` et `SKTileSet` classes.
- Utilisez la nouvelle `SKWarpGeometry` classe pour étendre ou déformer [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) ou [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendu. La nouvelle [SKAction](https://developer.apple.com/reference/spritekit/skaction) classe peut être utilisée pour animer les transitions entre les effets de distorsion.
- Les nuanceurs personnalisés peuvent fournir des attributs (`SKAttribute`) qui peut être configuré séparément par chaque nœud qui utilise le nuanceur en fournissant une valeur d’attribut (`SKAttributeValue`).
- Le [SKView](https://developer.apple.com/reference/spritekit/skview) classe fournit plusieurs nouvelles méthodes pour accorder un contrôle affiné sur quand et comment une scène est rendue.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Nouvelles infrastructures

Les infrastructures suivantes ont été ajoutés à macOS Sierra :

- **Intentions Framework** -cette infrastructure permettent à l’application examiner les interactions (par exemple, les actions de l’emplacement ou l’utilisateur) et prenez les mesures en fonction de ces informations.
- **SafariServices Framework** -cette infrastructure permettent à l’application permet de développer des extensions d’application pour Safari (tels que des blocages de contenu) pour macOS et iOS.

## <a name="related-links"></a>Liens associés

- [Exemples Mac](https://developer.xamarin.com/samples/mac/)
- [Nouveautés du système d’exploitation X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
