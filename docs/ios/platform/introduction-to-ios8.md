---
title: "Introduction à iOS 8"
description: "Avec iOS 8, Apple a fourni une multitude de nouvelles infrastructures et d’API excite et aux développeurs de concernée. Dans ce guide, nous introduire ces nouvelles API et voir comment iOS 8 peut bénéficier aux développeurs et aux utilisateurs."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 3a77d1a3b597667d8944156b040c2819a5c79ca2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-8"></a>Introduction à iOS 8

_Avec iOS 8, Apple a fourni une multitude de nouvelles infrastructures et d’API excite et aux développeurs de concernée. Dans ce guide, nous introduire ces nouvelles API et voir comment iOS 8 peut bénéficier aux développeurs et aux utilisateurs._

iOS 7 modifier visuellement l’interface utilisateur d’iOS entière à partir d’utilisateurs et des développeurs avaient habitué, directement à partir de la première iPhone du système d’exploitation. IOS 8 continue avec ce en fournissant plusieurs infrastructures pour les développeurs, ce qui permet aux utilisateurs de contrôler pratiquement tous les aspects de leur vie droite à partir de leur iPhone. Par exemple l’intégrité et adéquation peuvent être analysés avec *HealthKit*, les codes secrets sont obsolète à l’aide de l’authentification biométrique *LocalAuthentication*, *extensions d’application*ouvrir un canal de communication entre les applications de tiers 3e, et *HomeKit* permet de transformer votre maison d’une maison du futur. 

Si iOS 7 a été sur delighting les utilisateurs, iOS 8 se concentre sur toute une série de ces nouveaux outils gustative aux développeurs delighting. 

Ce guide présente les nouvelles API pour les développeurs de Xamarin.iOS.  

Il existe également quelques API ont été déconseillées dans iOS 8, qui sont décrites en détail à la fin de ce document.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour créer des applications iOS 8 dans Visual Studio pour Mac :

- **Xcode 7 et iOS 8 ou version ultérieure** – Apple dernière Xcode et iOS API doivent être installés et configurés sur l’ordinateur du développeur.
- **Visual Studio pour Mac** – la version la plus récente de Visual Studio pour Mac doit être installée et configurée sur l’appareil de l’utilisateur.
- **iOS 8 appareil ou simulateur** – un appareil iOS exécutent la dernière version d’iOS 8 pour le test.

## <a name="home-and-leisure"></a>Page d’accueil et loisirs

iOS 8 a permis de plantes bien Apple et l’appareil iOS dans le cœur de la maison via l’utilisation HomeKit et HealthKit. Dans cette section, nous allons examiner comment les deux de ces infrastructures nouvelle fonctionnent et comment ils peuvent être intégrés dans votre application Xamarin.iOS.

## <a name="homekit"></a>HomeKit

Contrôle de vos appareils à partir de votre iPhone n’est pas une nouvelle application de la technologie ; de nombreux produits connectés à distance peuvent être contrôlés via une application iOS. Toutefois HomeKit prend désormais cela un peu plus loin en promouvant un protocole commun pour les appareils automation de base et en effectuant une API publique disponible pour certains fabricants, tels qu’iHome, Philips et Honeywell. À l’utilisateur, cela signifie que contrôler presque chaque aspect de leur domicile en toute transparence à l’intérieur d’une application. Il n’est pas pertinent pour eux de savoir qu’ils utilisent une ampoule Philips teinte ou une alerte d’imbrication. Les utilisateurs peuvent également chaîner des nombreux processus accueil actives dans « En arrière-plan ».

Avec HomeKit, les applications tierces Siri peut découvrir les accessoires et les ajouter à leur base de données de configuration de base personnel, modifier et agir sur ces données et communiquer avec les accessoires et leurs services pour exécuter une action.

### <a name="configuration"></a>Configuration

Le diagramme suivant montre la hiérarchie de base de la configuration de HomeKit accessoires :

![](introduction-to-ios8-images/image1.png "Ce diagramme montre la hiérarchie de base de la configuration de HomeKit accessoires")
 
Pour commencer avec HomeKit, les développeurs doivent vous assurer que leur profil de configuration a le service HomeKit sélectionné. Apple a également fourni aux développeurs un complément simulateur HomeKit pour Xcode. Cela peut être trouvé dans le [Apple Developer Center](https://developer.apple.com/downloads/index.action), sous `Hardware IO Tools for Xcode`. 

Pour plus d’informations, consultez notre [HomeKit](~/ios/platform/homekit.md) guide.

## <a name="healthkit"></a>HealthKit

HealthKit est une infrastructure introduite dans iOS 8 qui fournit un magasin de données centralisé et coordonné sécurisé pour les informations relatives au contrôle d’intégrité. Le système d’exploitation garantit la confidentialité et la sécurité des informations d’intégrité et, avec l’application de contrôle d’intégrité, un tableau de bord pour l’utilisateur. Avec l’autorisation de l’utilisateur, les applications peuvent lire et écrire une grande variété d’informations d’intégrité.

Pour plus d’informations sur l’utiliser dans votre application Xamarin.iOS, reportez-vous à la [présentation HealthKit](~/ios/platform/healthkit.md) guide.

## <a name="extending-iphone-functionality"></a>IPhone d’extension des fonctionnalités
Avec iOS8, les développeurs sont en cours données beaucoup plus de contrôle sur leur application et des capacités pouvant utiliser pour les communications ouvertes entre des applications tierces. Fonctionnalités, telles que les Extensions d’application et de sélecteur de Document ouvrir un monde de possibilités de la façon dont les applications peuvent être utilisées dans l’écosystème de d’Apple.

### <a name="app-extensions"></a>Extensions d’application
Les Extensions d’application, pour simplifier, sont un moyen pour les applications de tiers communiquer avec un autre. Pour maintenir des normes de sécurité haute et maintenir l’intégrité des applications bac à sable, cette communication ne se produit pas directement entre les applications. Au lieu de cela, elle est effectuée par un *Extension* au milieu.

La première étape de création d’une Extension d’application consiste à définir le point d’extension correct, il est important pour garantir le comportement et la disponibilité des API corrects. Pour créer une Extension d’application dans Visual Studio pour Mac, ajoutez-la à une application existante en ajoutant un nouveau projet à votre solution.

Dans le **nouveau projet** boîte de dialogue Atteindre **c#** > **iOS** > **API unifiée**  >   **Extensions**, comme illustré dans la capture d’écran ci-dessous :

![](introduction-to-ios8-images/image2.png "Création d’une nouvelle extension")
 
La boîte de dialogue Nouveau projet fournit sept nouveaux modèles de projet pour créer des Extensions d’application et sont décrites ci-dessous. Notez que la plupart des extensions sont liés à d’autres nouvelles API dans iOS, telles que le sélecteur de Document :

- **Action** : Cela permet aux développeurs de créer des boutons d’action personnalisée unique permettant aux utilisateurs d’effectue certaines tâches
- **Clavier personnalisés** – Cela permet aux développeurs d’ajouter à la plage de générés dans Apple claviers par ajouter leurs propres personnalisé. Le clavier populaires, Swype utilise pour mettre leur clavier pour iOS.
- **Sélecteur de document** – il contient un contrôleur de vue de sélecteur de Document qui permet aux utilisateurs d’accéder aux fichiers en dehors de bac à sable de l’application.
- **Fournisseur de sélecteur de fichier de document** – cela offre un stockage sécurisé des fichiers à l’aide du sélecteur de Document.
- **Édition photo** : cette se développe sur les filtres et les outils déjà fournies par Apple dans l’application de Photos pour donner aux utilisateurs plus de contrôle et davantage d’options lors de la modification de leurs photos de modification.
- **Aujourd'hui** – cela donne la possibilité d’afficher des widgets dans la section aujourd'hui de centre de notifications aux applications.

Pour plus d’informations sur l’utilisation des Extensions d’application dans Xamarin, reportez-vous à la [présentation des Extensions d’application](~/ios/platform/extensions.md) guide.

### <a name="touch-id"></a>ID tactile

ID de contact a été introduite dans iOS 7 comme un moyen d’authentifier l’utilisateur, semblable à un code secret. Toutefois, il a été limité à déverrouiller l’appareil, à l’aide de l’App Store, à l’aide d’iTunes et l’authentification de la chaîne de la clé iCloud uniquement 

Il existe désormais deux façons d’utiliser Touch ID comme mécanisme d’authentification dans les applications iOS 8 à l’aide de l’API d’authentification Local. Actuellement, il n’est pas possible d’utiliser l’authentification locale pour l’authentification à distance.

Tout d’abord, il facilite les services existants de trousseau via l’utilisation de la nouvelle Keychain Access Control Lists (ACL). Les données de chaîne de clé peuvent être déverrouillées à l’aide de l’authentification réussie d’une empreinte digitale utilisateurs.

Deuxièmement, LocalAuthentication fournit deux méthodes pour authentifier votre application localement. Les développeurs doivent utiliser `CanEvaluatePolicy` pour déterminer si l’appareil est capable d’accepter Touch ID, puis `EvaluatePolicy` pour démarrer l’opération d’authentification.

Pour plus d’informations sur les ID tactile et pour savoir comment intégrer une application Xamarin.iOS, reportez-vous à la [Introduction à Touch ID](~/ios/platform/touchid.md) guides.

### <a name="document-picker"></a>Sélecteur de document

Fonctionnement de sélecteur de document avec un lecteur d’iCloud les utilisateurs pour permettre à l’utilisateur d’ouvrir des fichiers qui ont été créés dans une autre application, d’importent et de les manipulent et de les exporter de nouveau à nouveau. Cela crée un flux de travail intuitive et par conséquent une bien meilleure expérience, pour les utilisateurs. iCloud la synchronisation prend une étape supplémentaire, toutes les modifications apportées dans une application également être reflète cohérente sur tous vos appareils.

Pour en savoir plus sur le sélecteur de Document de manière plus approfondie et pour savoir comment intégrer une application Xamarin.iOS, reportez-vous à la [Introduction pour le sélecteur de Document](~/ios/platform/document-picker.md) guide.

### <a name="handoff"></a>Procédure de transfert

Procédure de transfert, qui fait partie de la fonctionnalité de continuité d’activité supérieure, prend une étape supplémentaire intégration OS X et iOS. Cela inclut les AirDrop inter-plateformes, d’afficher les appels de l’iPhone, SMS sur les iPad et Mac et améliorations de la connexion à partir de votre iPhone.

Procédure de transfert fonctionne avec iOS 8 et Yosemite et nécessite un compte iCloud à se connecter à un autre périphérique que vous souhaitez utiliser. Il doit fonctionner avec les applications Apple plus préinstallées, y compris Safari, iWork, des cartes, des calendriers et des Contacts.

Pour plus d’informations, consultez notre [remise](~/ios/platform/handoff.md) guide.

## <a name="unified-storyboards"></a>Storyboards unifiées
iOS 8 inclut un nouvel plus simple d’utiliser le mécanisme de création de l’interface utilisateur, le plan conceptuel unifié. Avec un seul plan conceptuel pour couvrir toutes les tailles d’écran de matériel différents, les vues de rapides et réactives peuvent être créées dans un style « conception unique, utiliser un grand nombre » true.

Avant d’iOS8, les développeurs utilisaient `UIInterfaceOrientation` faire la distinction entre les modes portrait et paysage, et `UIInterfaceIdiom` faire la distinction entre les appareils iOS. Dans iOS8 n’est plus nécessaire de créer des animations distinctes pour les appareils iPhone et iPad, l’orientation et appareils sont déterminées à l’aide de *taille Classes*.

Chaque périphérique est définie par une classe de taille, dans la verticale et l’axe horizontal, et il existe deux types de classes de taille dans iOS 8 :

- **Régulière** -il s’agit d’une taille d’écran de grande taille (tels qu’un iPad) ou d’un gadget qui donne l’impression d’une grande taille (par exemple, une UIScrollView
- **Compact** -il s’agit de périphériques de petite taille (tels que l’iPhone). Cette taille prend en compte l’orientation de l’appareil.

Si les deux concepts sont utilisés ensemble, le résultat est une grille de 2 x 2 qui définit les différentes tailles possibles qui peuvent être utilisés dans les deux les différentes orientations, comme indiqué dans le diagramme suivant :

![](introduction-to-ios8-images/image3.png "Un diagramme représentant la grille de 2 x 2 qui définit les différentes tailles qui peuvent être utilisés dans les deux les orientations différentes")
 
Pour plus d’informations sur les classes de taille, reportez-vous à la [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Kit de photo
Kit de la photo est une nouvelle infrastructure qui permet aux applications de la bibliothèque d’images de système de requête et créer des interfaces utilisateur personnalisées pour afficher et modifier son contenu. Il comprend des classes qui représentent l’image et vidéos actifs, ainsi que les collections de ressources, comme des albums et des dossiers.

Pour plus d’informations, consultez notre [PhotoKit](~/ios/platform/photokit.md) guide.

## <a name="games"></a>Jeux

<a name="scenekit" />

### <a name="scene-kit"></a>Kit de la scène

Kit de la scène est un API qui simplifie l’utilisation de graphiques 3D du graphique de scène 3D. Il a été introduite dans OS X 10.8 et est venu iOS 8. Kit de scène la création de visualisations 3D réalistes et jeux 3D informels ne nécessite pas expérimenté dans OpenGL. S’appuyant sur les concepts de graphique de scène communs, Kit de scène élimine les complexités des OpenGL et OpenGL ES, rendant très facile d’ajouter 3D contenu à une application. Toutefois, si vous êtes un expert OpenGL, Kit de scène a réunissant directement avec OpenGL également très bien en charge. Il comprend de nombreuses fonctionnalités qui complètent les graphiques 3D, tels que physique et s’intègre parfaitement avec plusieurs autres infrastructures de Apple, telles que l’Animation de base, l’Image de base et Sprite Kit.

Pour plus d’informations, consultez notre [SceneKit](~/ios/platform/gaming/scenekit.md) documentation.

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite Kit

Kit Sprite, l’infrastructure de jeux 2D auprès d’Apple, a certaines nouvelles fonctionnalités intéressantes dans iOS 8 et OS X Yosemite. Ceux-ci incluent l’intégration avec la scène Kit, prise en charge du nuanceur, d’éclairage, ombres, contraintes, génération d’une carte de normale et améliorations de physique. En particulier, les nouvelles fonctionnalités physique rendent très facile d’ajouter des effets réalistes à un jeu.

Pour plus d’informations, consultez notre [SpriteKit](~/ios/platform/gaming/spritekit.md) documentation.

## <a name="other-changes"></a>Autres modifications
Ainsi que les modifications majeures dans iOS 8 décrites ci-dessus, Apple a mis à jour en outre les nombreuses infrastructures existantes. Ceux-ci sont décrits ci-dessous :

- **[Image de base](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  – Apple a développée lors de son infrastructure de traitement d’image en ajoutant une meilleure prise en charge pour la détection de régions rectangulaires et des codes QR à l’intérieur des images. Mike Bluestein explore ceci dans son blog post intitulée [détection d’images dans iOS 8](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>API déconseillées
Avec toutes les améliorations apportées dans iOS 8, un certain nombre d’API ont déconseillées. Certains d'entre eux sont détaillées ci-dessous.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  : les méthodes et les propriétés utilisées pour l’inscription des notifications à distance ont été déconseillées. Il s’agit de registerForRemoteNotificationTypes et enabledRemoteNotificationTypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – les caractéristiques et les classes de taille ont remplacé les méthodes et les propriétés utilisées pour décrire l’orientation de l’interface. Faire référence à la [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) pour plus d’informations sur la façon de les utiliser.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  : cela a été remplacé par UISearchController iOS8.

## <a name="summary"></a>Récapitulatif
Dans cet article, nous avons quelques-unes des nouvelles fonctionnalités introduites par Apple dans iOS 8.



## <a name="related-links"></a>Liens associés

- [UIKitEnhancements (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Présentation des Extensions d’application](~/ios/platform/extensions.md)
- [Présentation de CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Introduction au sélecteur de Document](~/ios/platform/document-picker.md)
- [Introduction aux HealthKit](~/ios/platform/healthkit.md)
- [Introduction aux contrôles de l’appareil photo manuel](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Introduction à Touch ID](~/ios/platform/touchid.md)
- [Introduction à des plans conceptuels unifiées](~/ios/user-interface/storyboards/unified-storyboards.md)
