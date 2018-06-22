---
title: Introduction à iOS 9
description: Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans iOS 9 pour les développeurs de Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 10ed9154b92e6f13dd71f83cf4fed47585dc795f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781270"
---
# <a name="introduction-to-ios-9"></a>Introduction à iOS 9

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans iOS 9 pour les développeurs de Xamarin.iOS._

![](images/ios9-sml.png "Le logo d’iOS 9")

Apple a ajouté plusieurs nouvelles API et sur les services dans iOS 9, ainsi que de nombreuses améliorations apportées aux fonctionnalités existantes.

## <a name="3d-touch"></a>3D Touch

Nouvelle iOS 9 iPhone 6 s et iPhone 6 s Plus, 3D Touch ajoute mouvements sensibles de pression à vos applications iOS. 3D Touch, une application iPhone peut désormais non seulement savoir que l’utilisateur touche l’écran du périphérique, il peut également détecter la quantité l’utilisateur est exerçant une pression et répondent aux niveaux différents de la sollicitation de la.

3D Touch fournit les fonctionnalités suivantes à votre application :

- **Sensibilité à la pression** - applications peuvent mesurer maintenant manuels ou lumière l’utilisateur touche l’écran et profitez de ces informations. Par exemple, une application de peinture peut rendre une ligne épaisse ou plus fin en fonction de laquelle l’utilisateur touche l’écran.
- **Lire et Pop** -votre application peut permettent désormais d’interagir avec ses données sans avoir à naviguer en dehors de leur contexte actuel de l’utilisateur. En appuyant sur disque dur sur l’écran, ils peuvent *aperçu* au niveau de l’élément qui les intéresse (par exemple, pour afficher un aperçu d’un message). En appuyant sur plus difficile, ils peuvent *Pop* dans l’élément.
- **Actions rapides** -considérer des Actions rapides, comme les menus contextuels qui peuvent être dépilés à distance lorsqu’un utilisateur clique sur un élément dans une application de bureau. À l’aide des Actions rapides, vous pouvez ajouter courantes, rapide et facile pour les raccourcis de l’accès à des fonctions dans votre application à partir de l’icône d’écran d’accueil sur l’appareil iOS.

Pour plus d’informations, consultez notre [présentation 3D Touch](~/ios/platform/3d-touch.md) guide.

## <a name="app-transport-security"></a>Sécurité de Transport de l’application

Nouveau sur iOS 9, applique la sécurité de Transport d’application (ATS) des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et de votre application. ATS garantit que toutes les communications internet sont conformes pour sécuriser la connexion meilleures pratiques, empêchant ainsi la divulgation accidentelle d’informations sensibles directement par le biais de votre application ou une bibliothèque qu’il utilise.

Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) ou [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) sera soumis pour Exigences de sécurité de couches d’application. Si vos connexions ne répondent pas à ces requise, ne pourront pas avec une exception.

Pour plus d’informations sur les couches d’application, consultez notre [sécurité du Transport application](~/ios/app-fundamentals/ats.md) guide.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Multitâche pour iPad

Avec iOS 9, Apple prend également en charge les travaux multitâches pour exécuter des deux applications en même temps sur un iPad spécifique matériel. Par conséquent, vos applications Xamarin.iOS ne peuvent plus supposer qu’ils sont la seule application en cours d’exécution à un moment donné ou qu’ils aient accès aux ressources de l’appareil plein écran.

Multitâche pour iPad est pris en charge via les fonctionnalités suivantes :

- **Sur la diapositive** -permet à l’utilisateur afin d’exécuter provisoirement une application iOS de seconde dans une diapositive panneau (soit sur le côté droit ou gauche de l’écran en fonction de la direction de langage) qui couvre environ 25 % de l’application principale en cours d’exécution. Faites glisser sur est uniquement disponible sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4.
- **Mode fractionné** -sur le matériel pris en charge iPad (iPad Air 2, iPad Mini 4 et iPad Pro uniquement), l’utilisateur peut choisir une deuxième application et s’exécuter côte à côte avec l’application en cours d’exécution dans un mode d’écran de fractionnement. L’utilisateur peut contrôler le pourcentage de l’écran principal qui occupe de chaque application.
- **Image dans l’image** - pour les applications de lire du contenu vidéo, la vidéo peut maintenant être lu dans une fenêtre déplaçable et redimensionnable qui flotte au-dessus des autres applications en cours d’exécution sur l’appareil iOS. L’utilisateur a un contrôle total sur la taille et la position de cette fenêtre. Image dans l’image est uniquement disponible sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4.

Pour plus d’informations sur les nouvelles possibilités multitâches d’iOS 9, consultez notre [multitâche pour iPad](~/ios/platform/multitasking.md) guide.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Nouveaux Contacts et les infrastructures d’interface utilisateur de Contacts

Avec l’introduction d’iOS 9, Apple a publié les deux infrastructures de nouveau, [Contacts](https://developer.xamarin.com/api/namespace/Contacts/) et [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), qui remplace le carnet d’adresses et les infrastructures d’interface utilisateur de livre adresse utilisés par iOS 8 et versions antérieures.

Ces infrastructures nouvelle, orienté objet fournissent les informations suivantes :

- **Contacts** – Xamarin.iOS de fournit l’accès aux informations de contact de l’utilisateur. Étant donné que la plupart des applications nécessitent uniquement un accès en lecture seule, cette infrastructure a été optimisée pour un accès thread-safe en lecture seule.
- **ContactsUI** : éléments de l’interface utilisateur de Xamarin.iOS fournit à afficher, modifier et sélectionnez Créer des contacts sur les appareils iOS.

Pour plus d’informations, consultez notre [Contacts et l’interface utilisateur de Contacts](~/ios/platform/contacts.md) documentation.


## <a name="new-search-apis"></a>Nouvelles API de recherche

Recherche a été développée dans iOS 9 pour fournir de nouvelles façons d’accéder aux informations à l’intérieur de votre application Xamarin.iOS. À l’aide de la nouvelle API de recherche, vous pouvez effectuer le contenu de votre application disponible pour la recherche Spotlight et Safari résultats de recherche, remise et les rappels de Siri et les Suggestions. Cela permet aux utilisateurs un accès rapide aux activités et informations approfondies dans votre application.

En outre, les nouvelles API de recherche facilitent intégrer la recherche dans votre application sans expérience de mise en œuvre de recherche antérieure. Pour cette raison, Apple revendications qu’il prend généralement quelques heures pour rendre le contenu de l’application iOS 9 universellement consultable à l’aide de la recherche de l’application.

Pour plus d’informations, consultez notre [recherche améliorations](~/ios/platform/search/index.md) documentation.

## <a name="new-stack-view"></a>Nouvelle vue de la pile

Le contrôle de la vue de la pile ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) tire parti de la puissance de disposition automatique et les Classes de taille pour gérer une pile de sous-vues (verticale ou horizontale) qui répond dynamiquement à la taille d’écran et l’orientation du périphérique iOS.

En utilisant le contrôle de la vue de la pile, la quantité de travail nécessaire à la disposition de qu'une interface utilisateur est considérablement réduite. La disposition de tous les sous-vues attaché à une vue de la pile sont gérés automatiquement en fonction de propriétés de développeur défini comme axe, distribution, alignement et l’espacement.

Pour plus d’informations, consultez notre [Introduction à la vue de la pile](~/ios/user-interface/controls/uistackview.md) documentation.


## <a name="collection-view-changes"></a>Afficher les modifications de collection

Dans iOS 9, la vue de Collection ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) désormais prend en charge faites glisser la réorganisation des éléments en dehors de la zone en ajoutant un nouveau module de reconnaissance de mouvement par défaut et plusieurs nouvelles méthodes de prise en charge.

À l’aide de ces nouvelles méthodes, vous pouvez facilement implémenter glisser pour réorganiser dans votre vue de Collection et la possibilité de personnaliser l’apparence des éléments au cours de n’importe quelle étape du processus de réorganisation des éléments.

Pour plus d’informations sur les modifications de la vue de Collection pour iOS 9, consultez notre [afficher les modifications de Collection](~/ios/user-interface/controls/uicollectionview.md) guide.

## <a name="game-enhancements"></a>Améliorations de jeu

Avec iOS 9, Apple a effectuée plusieurs améliorations technologiques les API de jeu qui facilitent l’implémentation de graphismes des jeux et audio dans une application Xamarin.iOS. Ceux-ci incluent les deux facilité de développement par le biais des infrastructures de haut niveau et la puissance de GPU de l’appareil iOS pour l’amélioration de la vitesse et des fonctionnalités de graphique avec des améliorations de bas niveau.

Cela inclut GameplayKit, ReplayKit, e/s du modèle, MetalKit et complète les nuanceurs de performances, ainsi que de nouvelles fonctionnalités améliorées de métal, SceneKit et SpriteKit.

Pour plus d’informations, consultez notre [jeu améliorations](~/ios/platform/gaming/index.md) documentation.

## <a name="homekit-framework-changes"></a>Modifications du Framework HomeKit

Le [HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) framework, introduite dans iOS 8, fournit la possibilité de configurer et contrôler les accessoires HomeKit activé différents (par exemple, les témoins lumineux automatisés, des verrous et garage porte OUVREURS) à partir d’une application Xamarin.iOS. En plus d’être facile à configurer et à configurer, HomeKit accessoires peuvent être contrôlés via des commandes vocales de Siri.

Dans iOS 9, Apple a simplifié le programme d’installation, développé les types d’accessoires pris en charge et fourni plus interactions accessoires (notamment pour contrôler un accessoire à distance via iCloud).

Pour plus d’informations, consultez notre [présentation HomeKit](~/ios/platform/homekit.md), [HomeKitIntro iOS exemple d’application](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) et d’Apple [HomeKit](https://developer.apple.com/homekit/) documentation.

## <a name="handoff-framework-changes"></a>Modifications d’infrastructure de remise

Procédure de transfert (également appelée la continuité des activités) a été introduite par Apple dans iOS 8 et OS X Yosemite (10.10) comme un moyen de l’utilisateur démarrer une activité sur l’un de leurs appareils (iOS ou Mac) et continuer à cette même activité sur un autre de leurs appareils (telles qu’identifiées par iClou de l’utilisateur d compte).

Procédure de transfert a été développé dans iOS 9 prend également en charge, les fonctionnalités de recherche améliorées. Pour plus d’informations, consultez notre [recherche améliorations](~/ios/platform/search/index.md) documentation. Pour plus d’informations sur l’utilisation de remise, consultez notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) documentation.

## <a name="new-extension-points"></a>Nouveaux points d'extension

Dans iOS 8, Apple a introduit des Extensions, les bibliothèques qui sont présentées par le système d’exploitation dans des contextes standards, comme dans le centre de notifications, lorsque l’utilisateur demande un clavier ou lorsque qu’ils modifient une photo.

Avec iOS 9, Apple étend prise en charge de l’Extension en fournissant plusieurs nouveaux _des Points d’Extension_ qui définissent des stratégies d’utilisation et fournissent des API pour l’utilisation dans une zone donnée comme suit :

- **Nouveau Point d’Extension Audio unité** – utiliser ce Point d’Extension pour fournir des effets audio, instruments de musique, générateurs de sons, etc. pour une utilisation dans d’autres applications d’hôte Audio unité (par exemple, GarageBand). Ce Point d’Extension vous permet également de vendre _unités Audio_ (plug-ins audio) sur l’App Store.
- **Nouveau Point d’Extension de Maintenance Index** : utilisez ce Point d’Extension pour prendre en charge la réindexation des données d’application sans nécessiter un redémarrage de l’application.
- **Nouveaux Points d’Extension de réseau** (nécessitent l’autorisation spéciale d’Apple) :
    - **Extension de fournisseur d’application Proxy** : utilisez ce Point d’Extension pour implémenter un proxy transparent de réseau côté client personnalisé.
    - **Filtrer le fournisseur de données / Extension de fournisseur de contrôle de filtre** -utiliser ces Points d’Extension pour implémenter le filtrage sur le périphérique le contenu dynamique de réseau.
    - **Extension du fournisseur paquet Tunnel** : utilisez ce Point d’Extension pour implémenter un VPN personnalisé tunneling protocol côté client.
- **Nouveaux Points d’Extension Safari**:
    - **Blocage de l’Extension de contenu** : utilisez ce Point d’Extension pour définir une liste de contenu bloqué qui s’afficheront pas lorsque l’utilisateur parcourt le web.
    - **Partagé d’Extension de liaisons** : utilisez ce Point d’Extension pour permettre l’affichage du contenu de votre application dans les liens de partage de Safari.

Pour plus d’informations, consultez notre [Introduction aux Extensions](~/ios/platform/extensions.md) et d’Apple [Guide de programmation des extensions d’application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) documentation.

## <a name="keychain-enhancements"></a>Améliorations de trousseau

Dans iOS 9, Apple a amélioré la chaîne de clé pour fournir un nouveau type de clé de chiffrement pour l’Enclave sécurisée et plus d’options de protection d’élément comme suit :

- Une nouvelle contrainte Touch ID invalide trousseau éléments lorsque la base de données de l’empreinte digitale est modifiée.
- Nouvelles contraintes qui autorise la création d’entrées de la liste de contrôle d’accès avec Touch ID ou le mot de passe uniquement.
- Un nouveau contexte d’authentification qui vous permet d’appeler distinct de l’authentification `SecItem` appels.
- Accéder à entropie de liste de contrôle (à l’aide de l’option de l’Application de mot de passe) pour le chiffrement d’élément trousseau fournie par l’application.
- Prend en charge pour la création et l’utilisation de clés à l’intérieur de l’enclave sécurisé (via la `kSecAttrTokenIDSecureEnclave` attribut).

Pour plus d’informations, consultez notre [présentation Touch ID](~/ios/platform/touchid.md) documentation.


## <a name="right-to-left-language-support"></a>Prise en charge linguistique de droite à gauche

IOS 9, Apple a effectué présentant une interface utilisateur renversée plus facile que jamais en fournissant la prise en charge complète des langues de droite à gauche. Ce dernier est détaillé ci-après :

- Standard [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) contrôles seront retourner automatiquement selon les paramètres de paramètres régionaux et linguistiques de périphériques iOS de droite à gauche.
- Le [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) classe fournit des attributs qui vous permettent de définir comment une vue donnée doit apparaître lorsque retournement de droite à gauche.
- La possibilité de faire pivoter une image par programmation à l’aide de la [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) propriété de la [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) classe.

Pour plus d’informations, consultez le site d’Apple [prenant en charge de droite à gauche](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) documentation.



## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

En plus des principales modifications apportées que nous avons vu ci-dessus, Apple a effectué des modifications et améliorations apportées à plusieurs infrastructures existantes pour iOS 9, notamment les suivantes :

- Infrastructure AV Foundation
- AVKit Framework
- CloudKit Framework
- Infrastructure Foundation
- Infrastructure de remise
- HealthKit Framework
- HomeKit Framework
- Infrastructure d’authentification local
- MapKit Framework
- PassKit Framework
- Safari Services Framework
- UIKit Framework

Pour plus d’informations, consultez notre [supplémentaires iOS 9 Framework modifications](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) documentation.

## <a name="deprecated-apis-and-functions"></a>Fonctions et les API déconseillées

Apple a déconseillé les fonctions dans iOS 9 et les API suivantes :

- **Adresse Book et l’interface utilisateur du carnet d’adresses** -ces API ont été remplacés par les infrastructures de Contact et l’interface utilisateur du Contact. Pour plus d’informations, consultez notre [Contacts et l’interface utilisateur de Contacts](~/ios/platform/contacts.md) documentation.
- **CBCentralManager** - le `RetrievePeripherals` et `RetrieveConnectedPeripherals` méthodes de la `CBCentralManager` classe ont été supprimées dans iOS 9. Appel de ces méthodes provoquera une application à se bloquer lors de l’appariement d’accessoire ou sur le démarrage de l’application.
- **FetchAllChanges** - le `FetchAllChanges` de la `CKFetchRecordChangesOperation` classe a été supprimée et sera supprimée dans iOS 9.
- **Media Player** -le Media Player framework a été déconseillée dans iOS 9. Utilisez à la place AVKit ou AV Foundation API.

Pour obtenir une liste complète des deprecations d’API spécifiques, consultez d’Apple [iOS 9.0 différences d’API](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) documentation.

## <a name="ios-9-sample-apps"></a>Exemples d’applications iOS 9

Nous avons certaines [exemples d’iOS 9 spécifiques](https://developer.xamarin.com/samples/ios/iOS9/) prise en main :

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Consultez également les parties iOS de ces exemples (accompagnement Mac OS X versions à venir !) :

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Introduction à la 3D Touch](~/ios/platform/3d-touch.md)
- [Sécurité de transport de l’application](~/ios/app-fundamentals/ats.md)
- [Multitâche pour iPad](~/ios/platform/multitasking.md)
- [Contacts et l’interface utilisateur](~/ios/platform/contacts.md)
- [Nouvelles API de recherche](~/ios/platform/search/index.md)
- [Introduction à l’affichage de la pile](~/ios/user-interface/controls/uistackview.md)
- [Afficher les modifications de collection](~/ios/user-interface/controls/uicollectionview.md)
- [Améliorations de jeu](~/ios/platform/gaming/index.md)
- [Introduction aux HomeKit](~/ios/platform/homekit.md)
- [Introduction à la remise](~/ios/platform/handoff.md)
- [Modifications supplémentaires apportées à l’infrastructure iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Résolution des problèmes](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Mise à jour de vos applications Xamarin.iOS à iOS9 (vidéo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
