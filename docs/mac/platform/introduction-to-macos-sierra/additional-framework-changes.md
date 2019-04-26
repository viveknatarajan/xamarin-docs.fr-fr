---
title: Autres modifications du framework macOS Sierra
description: Ce document décrit les changements mineurs et améliorations apportées aux infrastructures existantes, introduits dans macOS Sierra. Il examine les modifications apportées à l’infrastructure d’accélération, AppKit, AVFoundation, données de base, Image Core, Foundation et bien plus encore.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6ed827c018931e5b79887dc355f136e2a84623d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61031625"
---
# <a name="additional-macos-sierra-framework-changes"></a>Autres modifications du framework macOS Sierra

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Accélérez les améliorations de Framework

Les améliorations suivantes ont été apportées à l’infrastructure accélérer pour macOS Sierra :

- Ajout de Quadrature (calcul intégral).
- Fonctions de base ajoutées pour construire des réseaux neuronaux.
- Fonctions de prédicat géométriques ajoutées à tester pour des opérations comme l’intersection de deux objets géométriques.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Améliorations AppKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure AppKit pour macOS Sierra :

- Plusieurs améliorations `NSCollectionView` telles que :
    - **Sections réductibles** -permet à l’utilisateur de réduire une section de vue de Collection en une seule ligne horizontale.
    - **Flottante en-têtes** -en-têtes et pieds de page peuvent désormais flotter (dans une disposition de flux) à l’aide de la même API que [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) dans iOS.
    - **Les vues d’arrière-plan à défilement** -un arrière-plan de vues de collection peut désormais être défini pour faire défiler, ainsi que le contenu.
- La phase de disposition de vue différée a été optimisée et étendue.
- L’API de glisser-déplacer inclut désormais le nouveau `NSFilePromiseProvider` et `NSFilePromiseReceiver` flocage faire glisser des classes pour prendre en charge.
- Plusieurs constructeurs de commodité ont été ajoutées à des contrôles existants :
    -  `NSButton` inclut de nouveaux constructeurs pour la création de boutons de commande, des boutons radio et les cases à cocher.
    -  `NSTextField` inclut de nouveaux constructeurs pour la création de renvoi et sans habillage des étiquettes, des étiquettes avec attributs et des champs de texte modifiables.
    -  `NSSegmentedControl` inclut de nouveaux constructeurs pour créer des contrôles segmentés à partir d’un groupe d’étiquettes ou des images.
    -  `NSSlider` inclut de nouveaux constructeurs pour la création des curseurs linéaires horizontales.
    -  `NSImageView` inclut de nouveaux constructeurs pour créer des vues d’image non modifiable à partir d’une donnée `NSImage`.
-  Le nouveau `NSGridView` a été ajouté à une collection de vues de sub dans une grille avec la variable en taille réelle des lignes et colonnes qui peuvent être masquées ou affichées dynamiquement de la mise en page automatique.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>Améliorations AVFoundation Framework

Les améliorations suivantes ont été apportées à l’infrastructure AVFoundation pour macOS Sierra :

- Dans macOS, l’application n’a plus à implémenter différentes [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) comportements selon le type de contenu. Il suffit de définir le `Rate` propriété et AVFoundation déterminent quand suffisamment contenu n’est disponible pour la lecture sans blocage.
- La nouvelle `AVPlayerLooper` classe facilite la boucle d’un élément donné de média pendant la lecture.
- Le `AVAssetDownloadURLSession` classe permet le téléchargement et une lecture ultérieure de FairPlay chiffré flux HLS.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Améliorations de Framework de données centrales

Les améliorations suivantes ont été apportées à l’infrastructure de données principale pour macOS Sierra :

- Racine [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) prend en charge les objets simultanés de provoquer une erreur et de récupération sans sérialisation.
- Le [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) classe gère un pool de magasins de données SQLite.
- Le [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objets avec des banques de données SQLite dans la prise en charge du Mode du Journal WAL la nouvelle génération de requête de fonctionnalités où contextes gérés de l’objet (MOC) peuvent être épinglés aux versions de base de données spécifique pour l’extraction des futures et transactions défaillante.
- À l’aide de la vue `NSPersistenceContainer` pour référencer le `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) et d’autres ressources de configuration des données de base.
- Plusieurs nouvelles méthodes pratiques ont été ajoutés à `NSManagedObject` rend plus facile à effectuer des extractions et créer les sous-classes.

Pour plus d’informations, consultez le site d’Apple [Core données Framework référence](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Améliorations de Framework d’Image

Les améliorations suivantes ont été apportées à l’infrastructure d’Image de base pour macOS Sierra :

- Le `ImageWithExtent` méthode de la [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) classe peut être utilisée pour insérer un traitement personnalisé dans l’opération de filtrage. Affiche ou appeler le rappel donné entre les filtres lors du traitement d’une image pour la sortie Image Core.
- L’application peut désormais traiter des images dans un espace de couleurs en dehors de l’espace colorimétrique de travail du contexte de l’Image de base par la conversion vers et depuis l’espace de couleurs avant et après le traitement.
- Le noyau de l’Image de base permettre désormais demander un format de sortie de pixel spécifique.
- Les nouveaux filtres d’image suivants ont été ajoutés : `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` et `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Améliorations de Framework de base

Les améliorations suivantes ont été apportées à l’infrastructure Foundation pour macOS Sierra :

- Utilisez le [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) API pour la représentation, conversion et affichant la plupart des unités physiques courantes telles que de masse, longueur, de vitesse, de durée et de température.
- Utilisez le [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) dates formatées de classe pour l’analyse et la génération de ISO 8601.
- Utilisez la nouvelle [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) classe pour effectuer les calculs d’intervalle de date et heure telles que des durées, pour comparer les intervalles et de test pour les intersections d’intervalle.
- Utilisez le [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) classe pour analyser les éléments d’un nom de personne à partir d’une chaîne.
- Utilisez la nouvelle [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) classe pour obtenir des métriques pour une URL mise en réseau de la session.

Pour plus d’informations, consultez le site d’Apple [Foundation Notes de publication pour OS X v10.12 et iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>Améliorations GameKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure GameKit pour macOS Sierra :

- Le **Game Center application** a été déconseillé et supprimé de macOS. Si l’application utilise GameKit, il _doit_ présenter son propre interface pour afficher les fonctionnalités GameKit (leaderboards, etc.). 
- Un nouveau type de compte iCloud uniquement a été implémenté par le [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) classe.
- La nouvelle [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) classe fournit une solution généralisée pour la gestion de stockage des données persistantes sur Game Center. `GKGameSession` gère une liste de joueurs et que l’application est responsable de la mise en œuvre quand et comment date participant est stocké, récupérés ou échangés entre joueurs. Dans de nombreux cas Sessions de jeu peut remplacer les correspondances existants en alternance, correspondances en temps réel ou persistant jeu d’enregistrement méthodes.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>Améliorations GamePlayKit Framework

Les améliorations suivantes ont été apportées à l’infrastructure GamePlayKit pour macOS Sierra :

- Génération de bruit procédurales a été ajoutée peut être utilisée pour améliorer le réalisme des textures d’aspect naturel, ajouter réalisme aux mouvements de caméra et aider à générer des jeux aux univers élaborés.
- Utiliser le partitionnement Spatial pour partitionner les données du monde du jeu pour la recherche efficace.
- Un nouveau stratège de Monte-Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) a été ajoutée pour le calcul de déplacement possibles exhaustive.
- Une nouvelle API d’arborescence de décision a été ajoutée ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) et [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) pour améliorer l’intelligence artificielle de création de jeu.
- Prise en charge 3D a été ajouté à l’agent existant et les comportements de recherche de chemin d’accès à l’aide de la nouvelle [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) et [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
- Utilisez la nouvelle [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) classe pour fournir des chemins d’accès hautes performances, l’aspect naturel.
- La nouvelle [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) et [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) make classes combinant GameplayKit et SpriteKit plus facile que jamais.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Améliorations de l’infrastructure complète

Les améliorations suivantes ont été apportées à l’infrastructure complète pour macOS Sierra :

- Peuvent désormais utiliser les applications et jeux 3D _pavage_ pour afficher efficacement des scènes complexes et geometry via le GPU.
- Spécialisation de la fonction permet de créer une collection fortement optimisé de matériel et les fonctions de combinaison clair pour une scène.
- Fournir un contrôle précis de l’allocation des ressources pour optimiser les performances de métal en fonction des applications à l’aide des ressources de segments de mémoire et cibles de rendu Memoryless.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation complète](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Améliorations du modèle de Framework d’e/s

Les améliorations suivantes ont été apportées à l’infrastructure d’e/s de modèle pour macOS Sierra :

- Le format de fichier (USD) est maintenant pris en charge.
- Utilisez la nouvelle `MDLMaterialPropertyGraph` classe facilement prendre en charge runtime remplace les modèles.
- Signé champ Distance prise en charge a été ajouté à la [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) classe.
- Utilisez la nouvelle `MDLLightProbeIrradianceDataSource` classe pour faciliter la sélection élective de sondage de la lumière.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Améliorations de Framework de photos

Les améliorations suivantes ont été apportées à l’infrastructure de Photos pour macOS Sierra :

- Modification dynamique de Photo est désormais disponible pour les applications qui prennent en charge l’infrastructure de Photos et aux extensions de type retouche photo (pour une utilisation à l’intérieur de la photo et Photos applications).
- Utilisez la nouvelle [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) classe pour appliquer des modifications à la vidéo et toujours le contenu de Photos en direct.
- Utilisez le [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) et [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) classes pour tirer parti de la nouvelle fonctionnalité de processeur Core Image pour effectuer des modifications.
- Pour prendre en charge des Photos en direct, le [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) et [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) classes ont été portées à partir d’iOS sur Mac OS.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>Améliorations de l’infrastructure de SceneKit

Les améliorations suivantes ont été apportées à l’infrastructure SceneKit pour macOS Sierra :

- Inclut désormais un nouveau système physiquement en fonction de rendu (PBR) pour des résultats plus réalistes avec la création de ressource plus simple.
- Utilisez la nouvelle [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) ombrage modéliser une grande variété d’effets d’ombrage réaliste au produit tout en nécessitant uniquement trois propriétés fondamentales (`Diffuse`, `Metalness` et `Roughness`).
- Depuis PBR ombrage fonctionne mieux avec éclairage basé sur l’environnement, utilisez le `LightingEnvironment` propriété à attribuer à base d’images d’éclairage à tan scène entière.
- Utilisez le `IESProfileURL` propriété à importer des appareils d’éclairage réel qui définissent la base d’éclairage sur les valeurs réelles telles qu’intensité (lumens) et la couleur de température (en degrés Kelvin).
- Le [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) classe peut fournir réalisme supérieur à l’aide des fonctionnalités HDR et effets. Exposition adaptative permet de créer des effets automatique ou utilisez vignettage, frange de couleur et dégradé de couleur pour ajouter des effets filmatic au jeu.
- Fonctionnalités d’appareil photo PBR et HDR fournissent les meilleurs résultats que les techniques traditionnelles de rendu et, par conséquent, SceneKit procède désormais à tous les calculs de couleur dans un espace de couleur linéaire (à l’aide de la gamme de couleurs P3 sur les écrans de couleur à l’échelle de l’appareil).
- SceneKit maintenant couleur correspond à toutes les couleurs en lisant les informations de profil de couleur.
- SceneKit interprète les valeurs de composant de couleur dans un espace de couleurs RVB linéaire pour tous les types de nuanceur.
- Dans la mesure où SceneKit lit et ajuste en fonction des informations de profil de couleurs dans les images de la texture, utilisez les catalogues de composants pour toutes les images pour garantir que ces informations sont fournies.
- Rendu de l’espace de couleurs à la fois linéaire et l’échelle de couleurs peut être désactivée en spécifiant le `SCNDisableLinearSpaceRendering` et `SCNDisableWideGamut` clés dans l’application `Info.plist`.
- Générer des primates polygone arbitraire (soit chargé à partir de fichiers soit généré par programmation) pour spécifier la géométrie avec le nouveau [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) classe.

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Améliorations d’infrastructure de sécurité

Les améliorations suivantes ont été apportées à l’infrastructure de sécurité pour macOS Sierra :

- Le `SecKey` interface a été modernisée et unifiée sur toutes les plateformes (iOS, tvOS, watchOS et macOS).

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>Améliorations de l’infrastructure de SpriteKit

Les améliorations suivantes ont été apportées à l’infrastructure SpriteKit pour macOS Sierra :

- Tilemaps prennent désormais en charge la vignette carrée, hexagonale et isométrique formes pour 2D, 2,5 D et défilement à côté des jeux à l’aide de la `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` et `SKTileSet` classes.
- Utilisez la nouvelle `SKWarpGeometry` classe pour étirer ou de déformer [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) ou [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendu. La nouvelle [SKAction](https://developer.apple.com/reference/spritekit/skaction) classe peut être utilisée pour animer des transitions entre les effets de warp.
- Des nuanceurs personnalisés peuvent fournir des attributs (`SKAttribute`) qui peut être configuré séparément à chaque nœud qui utilise le nuanceur en fournissant une valeur d’attribut (`SKAttributeValue`).
- Le [SKView](https://developer.apple.com/reference/spritekit/skview) classe fournit plusieurs nouvelles méthodes pour donner un contrôle affiné sur quand et comment une scène est affichée.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Nouvelles infrastructures

Les infrastructures suivantes ont été ajoutées à macOS Sierra :

- **Framework d’intentions** -cette infrastructure permettent à l’application examiner les interactions (par exemple, les actions d’emplacement ou utilisateur) et agir en fonction de ces informations.
- **SafariServices Framework** -cette infrastructure permet l’application de développer des extensions d’application de Safari (par exemple, des bloqueurs de contenu) pour macOS et iOS.

## <a name="related-links"></a>Liens associés

- [Exemples Mac](https://developer.xamarin.com/samples/mac/)
- [Quelles sont les nouveautés dans OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
