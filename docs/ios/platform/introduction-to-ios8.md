---
title: Introduction à iOS 8
description: Avec iOS 8, Apple a fourni une multitude de nouvelles infrastructures et les API qui Motivez et soulage les développeurs. Dans ce guide, nous présenter ces nouvelles API et découvrez comment iOS 8 peut avantager les développeurs et les utilisateurs.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 8e95e674f52cb23be769473e27dc8eb785a77820
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116041"
---
# <a name="introduction-to-ios-8"></a>Introduction à iOS 8

_Avec iOS 8, Apple a fourni une multitude de nouvelles infrastructures et les API qui Motivez et soulage les développeurs. Dans ce guide, nous présenter ces nouvelles API et découvrez comment iOS 8 peut avantager les développeurs et les utilisateurs._

iOS 7 modifié visuellement l’interface utilisateur entière iOS à partir de quels utilisateurs et les développeurs avaient habitué, directement à partir de l’iPhone premier système d’exploitation. IOS 8 continue avec ce grâce à de nombreuses infrastructures pour les développeurs, ce qui permet aux utilisateurs de contrôler presque tous les aspects de leur vie directement depuis leur iPhone. Par exemple l’intégrité et pertinence (fitness) peuvent être analysés avec *HealthKit*, codes secrets sont obsolète avec à l’aide de l’authentification biométrique *LocalAuthentication*, *extensions d’application*ouvrir un canal de communication entre les applications tierces 3e, et *HomeKit* permet de transformer votre maison en une maison du futur. 

Si iOS 7 concernait faire plaisir à ses utilisateurs, iOS 8 se concentre sur plaisent aux développeurs un ensemble de ces nouveaux outils tasty. 

Ce guide présente les nouvelles API pour les développeurs de Xamarin.iOS.  

Il existe également certaines API qui ont été déconseillées dans iOS 8, qui sont détaillées à la fin de ce document.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour créer des applications iOS 8 dans Visual Studio pour Mac :

- **Xcode 7 et iOS 8 ou version ultérieure** – Apple dernières Xcode et API iOS doivent être installés et configurés sur l’ordinateur du développeur.
- **Visual Studio pour Mac** – la dernière version de Visual Studio pour Mac doit être installée et configurée sur l’appareil de l’utilisateur.
- **iOS 8 appareil ou simulateur** : un appareil iOS la dernière version d’iOS 8 pour le test en cours d’exécution.

## <a name="home-and-leisure"></a>Accueil et loisirs

iOS 8 a permis de plantes fermement Apple et l’appareil iOS directement dans le cœur de votre maison grâce à l’utilisation de HomeKit et HealthKit. Dans cette section, nous allons examiner comment les deux nouvelles ces infrastructures fonctionnent et comment ils peuvent être intégrées dans votre application Xamarin.iOS.

## <a name="homekit"></a>HomeKit

Contrôle de vos appliances à partir de votre iPhone n’est pas une nouvelle application de la technologie ; de nombreux produits connectés à distance peuvent être contrôlés via une application iOS. Toutefois HomeKit maintenant prend cela d’aller plus loin en promouvant un protocole commun pour les appareils domotique et en mettant à disposition une API publique pour certains fabricants, tels qu’iHome, Philips et Honeywell. À l’utilisateur, cela signifie que presque tous les aspects de leur domicile en toute transparence à partir de pouvoir contrôler à l’intérieur d’une application. Il est sans importance pour eux de savoir qu’ils utilisent une ampoule Philips Hue, ou une alarme d’imbrication. Les utilisateurs peuvent également chaîner des nombreux processus accueil actives en « En coulisses ».

Avec HomeKit, des applications tierces et Siri peuvent détecter accessoires et les ajouter à leur base de données de configuration de base personnel, modifier et agir sur ces données et communiquer avec les accessoires et leurs services pour effectuer une action.

### <a name="configuration"></a>Configuration

Le diagramme ci-dessous montre la hiérarchie de base de la configuration des accessoires HomeKit :

![](introduction-to-ios8-images/image1.png "Ce diagramme montre la hiérarchie de base de la configuration des accessoires HomeKit")
 
Pour vous familiariser avec HomeKit, les développeurs doivent vous assurer que leur profil d’approvisionnement dispose le service d’HomeKit sélectionné. Apple a également fourni aux développeurs un complément de simulateur HomeKit pour Xcode. Cela peut être trouvé dans le [Developer Center d’Apple](https://developer.apple.com/downloads/index.action), sous `Hardware IO Tools for Xcode`. 

Pour plus d’informations, consultez notre [HomeKit](~/ios/platform/homekit.md) guide.

## <a name="healthkit"></a>HealthKit

HealthKit est un framework introduit dans iOS 8 qui fournit un magasin de données centralisé, coordonné et sécurisé pour les informations relatives à l’intégrité. Le système d’exploitation garantit la confidentialité et la sécurité des informations d’intégrité et, avec l’application de contrôle d’intégrité, un tableau de bord pour l’utilisateur. Avec l’autorisation de l’utilisateur, les applications peuvent lire et écrire une grande variété d’informations d’intégrité.

Pour plus d’informations sur son utilisation dans votre application Xamarin.iOS, reportez-vous à la [présentation d’HealthKit](~/ios/platform/healthkit.md) guide.

## <a name="extending-iphone-functionality"></a>Extension des fonctionnalités d’iPhone
Avec iOS8, les développeurs sont en cours donnés davantage le contrôle qui peuvent utiliser leur application et des capacités pour la communication plus ouverte entre des applications tierces. Fonctionnalités, telles que les Extensions d’application et le sélecteur de documents ouvrir un monde de possibilités en matière de la façon dont les applications peuvent être utilisées dans l’écosystème de d’Apple.

### <a name="app-extensions"></a>Extensions d’application
Extensions d’application, pour simplifier, constituent un moyen pour des applications tierces communiquer entre eux. Pour respecter les normes de sécurité élevée et de préserver l’intégrité des applications sable, cette communication ne se produit pas directement entre les applications. Au lieu de cela, elle est effectuée par un *Extension* au milieu.

La première étape de création d’une Extension d’application consiste à définir le point d’extension correct, cela est important pour garantir le comportement et la disponibilité des API corrects. Pour créer une Extension d’application dans Visual Studio pour Mac, ajouter à une application existante en ajoutant un nouveau projet à votre solution.

Dans le **nouveau projet** boîte de dialogue Atteindre **C#**  >  **iOS** > **API unifiée**  >  **Extensions**, comme illustré dans la capture d’écran ci-dessous :

![](introduction-to-ios8-images/image2.png "Création d’une nouvelle extension")
 
La boîte de dialogue Nouveau projet offre sept nouveaux modèles de projet pour créer des Extensions d’application et sont décrits ci-dessous. Notez que la plupart des extensions sont liées aux autres nouvelles API dans iOS, par exemple de sélecteur de documents :

- **Action** : Cela permet aux développeurs de créer des boutons d’action personnalisée unique ce qui permet aux utilisateurs d’effectue certaines tâches
- **Clavier personnalisé** : Cela permet aux développeurs d’ajouter à la plage d’Apple claviers intégrées en ajoutant leur propres celui personnalisé. Le clavier populaires, Swype utilise pour mettre leur clavier à iOS.
- **Document Picker** – il contient un contrôleur d’affichage de sélecteur de Document qui permet aux utilisateurs d’accéder aux fichiers en dehors de bac à sable de l’application.
- **Fournisseur de sélecteur de fichier de document** – cela offre un stockage sécurisé pour les fichiers à l’aide du sélecteur de Document.
- **Édition photo** : cette s’appuie sur les filtres et les outils déjà fournies par Apple dans l’application de Photos pour permettre aux utilisateurs davantage de contrôle et davantage d’options lors de la modification de leurs photos de modification.
- **Aujourd'hui** – cela donne la possibilité d’afficher des widgets dans la section aujourd'hui du centre de Notification aux applications.

Pour plus d’informations sur l’utilisation des Extensions d’application dans Xamarin, reportez-vous à la [Introduction aux Extensions d’application](~/ios/platform/extensions.md) guide.

### <a name="touch-id"></a>Touch ID

Touch ID a été introduit dans iOS 7 comme un moyen de l’authentification de l’utilisateur, semblable à un code secret. Toutefois, il était limité à déverrouiller l’appareil, à l’aide de l’App Store, à l’aide d’iTunes et l’authentification de la chaîne de clé iCloud uniquement 

Il existe désormais deux façons d’utiliser Touch ID comme mécanisme d’authentification dans les applications iOS 8 à l’aide de l’API d’authentification Local. Actuellement, il n’est pas possible d’utiliser l’authentification locale pour l’authentification à distance.

Tout d’abord, il aide les services de trousseau existants via l’utilisation de la nouvelle Keychain Access Control Lists (ACL). Données de chaîne de clé peuvent être déverrouillées avec l’authentification réussie d’une empreinte digitale d’utilisateurs.

Deuxièmement, LocalAuthentication fournit deux méthodes pour authentifier votre application localement. Les développeurs doivent utiliser `CanEvaluatePolicy` pour déterminer si l’appareil est capable d’accepter de Touch ID, puis `EvaluatePolicy` pour démarrer l’opération d’authentification.

Pour plus d’informations sur Touch ID et pour apprendre à intégrer dans une application Xamarin.iOS, reportez-vous à la [Introduction à TouchID](~/ios/platform/touchid.md) guides.

### <a name="document-picker"></a>Sélecteur de documents

Fonctionnement de sélecteur de document avec un lecteur d’iCloud les utilisateurs à l’utilisateur ouvrir des fichiers qui ont été créés dans une autre application, importent et les manipulent et les exporter de nouveau à nouveau. Cela crée un flux de travail intuitif et par conséquent une expérience nettement meilleure, pour les utilisateurs. iCloud la synchronisation va encore plus, toutes les modifications apportées dans une application être reflète également cohérente sur tous vos appareils.

Pour en savoir plus sur le sélecteur de documents de façon plus approfondie et pour apprendre à intégrer dans une application Xamarin.iOS, reportez-vous à la [présentation pour le sélecteur de documents](~/ios/platform/document-picker.md) guide.

### <a name="handoff"></a>Procédure de transfert

Procédure de transfert, qui fait partie de la fonctionnalité de continuité d’activité plus grande, prend d’aller plus loin pour intégrer OS X et iOS. Cela inclut AirDrop inter-plateformes, la possibilité de prendre des appels de l’iPhone, SMS sur les iPad et Mac et améliorations de la connexion à partir de votre iPhone.

Procédure de transfert fonctionne avec iOS 8 et Yosemite et nécessite un compte iCloud pour être connecté à tous les appareils différents à utiliser. Cela devrait fonctionner avec des applications de Apple plus préinstallées, Safari, iWork, cartes, calendriers et Contacts.

Pour plus d’informations, consultez notre [remise](~/ios/platform/handoff.md) guide.

## <a name="unified-storyboards"></a>Storyboards unifiés
iOS 8 inclut un nouveau plus simple d’utiliser un mécanisme pour la création de l’interface utilisateur, la table de montage séquentiel unifiée. Avec un storyboard unique pour couvrir toutes les tailles d’écran de matériel différents, rapides et réactifs des vues peuvent être créés dans un style « concevoir une seule fois, utilisent de nombreuses » true.

Avant d’iOS8, les développeurs utilisaient `UIInterfaceOrientation` pour faire la distinction entre les modes portrait et paysage, et `UIInterfaceIdiom` faire la distinction entre les appareils iOS. Dans iOS8 il n’est plus nécessaire de créer des plans conceptuels distincts pour les appareils iPhone et iPad, l’orientation et l’appareil sont déterminées à l’aide de *catégories de taille*.

Chaque appareil est définie par une classe de taille, dans la verticale et de l’axe horizontal, et il existe deux types de classes de taille dans iOS 8 :

- **Régulière** -il s’agit d’une taille d’écran de grande taille (tels qu’un iPad) ou un gadget qui donne l’impression de grande taille (par exemple, une UIScrollView
- **Compact** -il s’agit pour les appareils plus petits (par exemple, l’iPhone). Cette taille prend en compte l’orientation de l’appareil.

Si les deux concepts sont utilisées ensemble, le résultat est une grille de 2 x 2 qui définit les différentes tailles possible qui peuvent être utilisées dans les deux les différentes orientations, comme indiqué dans le diagramme suivant :

![](introduction-to-ios8-images/image3.png "Un diagramme représentant la grille de 2 x 2 qui définit les différentes tailles possible qui peuvent être utilisés dans les deux les orientations différentes")
 
Pour plus d’informations sur les classes de taille, reportez-vous à la [Introduction aux Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Kit de photo
Kit de photo est une nouvelle infrastructure qui permet aux applications d’interroger la bibliothèque d’images système et créer des interfaces utilisateur personnalisées pour afficher et modifier son contenu. Il inclut un nombre de classes qui représentent l’image et les ressources vidéo, ainsi que les collections de ressources telles que des albums et des dossiers.

Pour plus d’informations, consultez notre [PhotoKit](~/ios/platform/photokit.md) guide.

## <a name="games"></a>Jeux

<a name="scenekit" />

### <a name="scene-kit"></a>Kit de scène

Kit de la scène est un graphique de scène 3D API qui simplifie l’utilisation des graphiques 3D. Il a été introduite dans OS X 10.8 et est maintenant venue à iOS 8. Avec le Kit de scène création de visualisations 3D IMMERSIFS et des jeux en 3D occasionnels ne nécessite pas l’expertise dans OpenGL. S’appuyant sur les concepts communs de graphique de scène, Kit de scène élimine la complexité de OpenGL et OpenGL ES, rendant très facile d’ajouter 3D contenu pour une application. Toutefois, si vous êtes un expert OpenGL, Kit de scène a une aide appréciable pour lier des directement avec OpenGL également. Il inclut de nombreuses fonctionnalités qui complètent les graphiques 3D, tels que physique et s’intègre parfaitement avec plusieurs autres infrastructures de Apple, telles que Core Animation, l’Image de base et Spritekit.

Pour plus d’informations, consultez notre [SceneKit](~/ios/platform/gaming/scenekit.md) documentation.

<a name="spritekit" />

### <a name="sprite-kit"></a>Spritekit

Kit du sprite, le framework de jeux 2D à partir d’Apple, a des nouvelles fonctionnalités intéressantes dans iOS 8 et OS X Yosemite. Ceux-ci incluent l’intégration avec Kit de scène, prise en charge de nuanceur, éclairage, ombres, contraintes, génération d’une carte de normale et physique améliorations. En particulier, les nouvelles fonctionnalités de moteur physique rendent très facile d’ajouter des effets réalistes à un jeu.

Pour plus d’informations, consultez notre [SpriteKit](~/ios/platform/gaming/spritekit.md) documentation.

## <a name="other-changes"></a>Autres modifications
Ainsi que les modifications majeures dans iOS 8 décrites ci-dessus, Apple a mis à jour en outre les nombreuses infrastructures existantes. Ceux-ci sont décrits ci-dessous :

- **[Image de base](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  : Apple a développé lors de son infrastructure de traitement d’image en ajoutant une meilleure prise en charge pour la détection de régions rectangulaires, et les codes QR à l’intérieur des images. Mike Bluestein explore ceci dans son blog post intitulée [détection d’images dans iOS 8](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>API déconseillées
Avec toutes les améliorations apportées dans iOS 8, un certain nombre d’API ont déconseillées. Certaines d'entre elles sont détaillées ci-dessous.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  : les méthodes et propriétés utilisées pour l’inscription de notifications à distance ont été déconseillées. Il s’agit de registerForRemoteNotificationTypes et enabledRemoteNotificationTypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – les caractéristiques et les classes de taille ont remplacé les méthodes et les propriétés utilisées pour décrire l’orientation de l’interface. Reportez-vous à la [Introduction aux Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md) pour plus d’informations sur la façon de les utiliser.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  : cela a été remplacé par UISearchController dans iOS8.

## <a name="summary"></a>Récapitulatif
Dans cet article, nous avons vu certaines des nouvelles fonctionnalités introduites par Apple dans iOS 8.



## <a name="related-links"></a>Liens associés

- [UIKitEnhancements (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Introduction aux Extensions d’application](~/ios/platform/extensions.md)
- [Présentation de CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Introduction au sélecteur de Document](~/ios/platform/document-picker.md)
- [Présentation d’HealthKit](~/ios/platform/healthkit.md)
- [Introduction aux contrôles de l’appareil photo manuel](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Présentation de TouchID](~/ios/platform/touchid.md)
- [Introduction aux Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md)
