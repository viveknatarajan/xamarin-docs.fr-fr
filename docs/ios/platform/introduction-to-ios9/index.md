---
title: Introduction à iOS 9
description: Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans iOS 9 pour les développeurs de Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: d0ad25a1ecff7262b9b4b41a5f9d73a5931bbd1c
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870194"
---
# <a name="introduction-to-ios-9"></a>Introduction à iOS 9

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans iOS 9 pour les développeurs de Xamarin.iOS._

![](images/ios9-sml.png "Le logo d’iOS 9")

Apple a ajouté plusieurs nouvelles API et services dans iOS 9, ainsi que de nombreuses améliorations aux fonctionnalités existantes.

## <a name="3d-touch"></a>3D Touch

Nouveau iOS 9 iPhone 6 s et iPhone 6 s Plus, 3D Touch ajoute des mouvements sensibles pression à vos applications iOS. 3D Touch, une application iPhone peut désormais indiquer à non seulement que l’utilisateur touche l’écran du périphérique, il peut également sense combien l’utilisateur est exerçant une pression et répondre aux niveaux de pression différents.

3D Touch fournit les fonctionnalités suivantes à votre application :

- **Sensibilité à la pression** - applications peuvent mesurer maintenant hard ou light l’utilisateur touche l’écran et profiter de ces informations. Par exemple, une application de peinture peut effectuer une ligne épaisse ou plus fins en fonction de laquelle l’utilisateur touche l’écran.
- **Aperçu et Pop** -votre application peut désormais permettre à l’utilisateur interagir avec ses données sans avoir à naviguer en dehors de leur contexte actuel. En appuyant sur disque dur sur l’écran, ils peuvent *aperçu* à l’élément qui les intéresse (par exemple, pour afficher un aperçu d’un message). En appuyant sur plus difficile, ils peuvent *Pop* dans l’élément.
- **Actions rapides** -pense que des Actions rapides, comme les menus contextuels qui peuvent être dépilés à distance quand un utilisateur clique sur un élément dans une application de bureau. À l’aide des Actions rapides, vous pouvez ajouter courants, rapide et facile aux raccourcis de l’accès aux fonctions dans votre application à partir de l’icône d’écran d’accueil sur l’appareil iOS.

Pour en savoir plus, consultez notre [présentation 3D Touch](~/ios/platform/3d-touch.md) guide.

## <a name="app-transport-security"></a>Sécurité de Transport de l’application

Nouveau à iOS 9, App Transport Security (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et votre application. ATS garantit que toutes les communications internet sont conformes pour sécuriser la connexion meilleures pratiques, ce qui empêche la divulgation accidentelle d’informations sensibles directement par le biais de votre application ou une bibliothèque qui il consomme.

Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de [NSUrlConnection](xref:Foundation.NSUrlConnection), [CFUrl](xref:CoreFoundation.CFUrl) ou [NSUrlSession](xref:Foundation.NSUrlSession) sera soumis pour Exigences de sécurité ATS. Si vos connexions ne répondent pas à ces besoins, ils échoueront avec une exception.

Pour en savoir plus sur ATS, veuillez consulter notre [App Transport Security](~/ios/app-fundamentals/ats.md) guide.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Multitâche pour iPad

Avec iOS 9, Apple a prise en charge multitâche pour l’exécution des deux applications en même temps sur un matériel spécifique iPad. Par conséquent, vos applications Xamarin.iOS ne peuvent plus supposer qu’ils sont la seule application en cours d’exécution à un moment donné ou auxquelles ils ont accès aux ressources de l’appareil plein écran.

Multitâche pour iPad est pris en charge via les fonctionnalités suivantes :

- **Sur la diapositive** -permet à l’utilisateur exécuter temporairement une deuxième application iOS dans une diapositive panneau (soit sur le côté droit ou gauche de l’écran en fonction de l’orientation de la langue) qui couvre environ 25 % de l’application principale en cours d’exécution. Faites glisser sur est disponible uniquement sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4.
- **Mode fractionné** -sur du matériel pris en charge iPad (iPad Air 2, iPad Mini 4 et iPad Pro uniquement), l’utilisateur peut choisir une deuxième application et l’exécuter côte à côte avec l’application en cours d’exécution dans un mode d’écran de fractionnement. L’utilisateur peut contrôler le pourcentage de l’écran principal qui occupe de chaque application.
- **Image dans l’image** : pour les applications que vous lire du contenu vidéo, la vidéo peut maintenant être lues dans une fenêtre déplaçable et redimensionnable qui flotte au-dessus des autres applications en cours d’exécution sur l’appareil iOS. L’utilisateur a un contrôle total sur la taille et la position de cette fenêtre. Image dans l’image est disponible uniquement sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4.

Pour en savoir plus sur les nouvelles possibilités de multitâche d’iOS 9, veuillez consulter notre [multitâche pour iPad](~/ios/platform/multitasking.md) guide.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Nouveaux Contacts et les infrastructures d’interface utilisateur de Contacts

Avec l’introduction d’iOS 9, Apple a publié deux nouvelles infrastructures, [Contacts](xref:Contacts) et [ContactsUI](xref:ContactsUI), qui remplace le carnet d’adresses existante et les infrastructures d’interface utilisateur du livre adresse utilisés par iOS 8 et versions antérieures.

Ces infrastructures nouvelle, orienté objet fournit les informations suivantes :

- **Contacts** – Xamarin.iOS fournit l’accès aux informations de contact de l’utilisateur. Étant donné que la plupart des applications requièrent uniquement un accès en lecture seule, cette infrastructure a été optimisée pour l’accès thread-safe en lecture seule.
- **ContactsUI** – éléments de l’interface utilisateur de Xamarin.iOS fournit à afficher, modifier et sélectionnez Créer des contacts sur les appareils iOS.

Pour plus d’informations, consultez notre [Contacts et l’interface utilisateur de Contacts](~/ios/platform/contacts.md) documentation.


## <a name="new-search-apis"></a>Nouvelles API de recherche

Recherche a été développée dans iOS 9 pour fournir de nouvelles façons d’accéder aux informations à l’intérieur de votre application Xamarin.iOS. Avec les nouvelles API de recherche, vous pouvez rendre contenu de votre application disponible pour la recherche Spotlight et Safari résultats de recherche, remise et les rappels de Siri et les Suggestions. Cela permet aux utilisateurs un accès rapide aux activités et les informations approfondies au sein de votre application.

En outre, les nouvelles API de recherche facilitent l’intégrez la recherche dans votre application sans expérience de mise en œuvre de recherche précédents. Pour cette raison, Apple revendications qu’il prend généralement quelques heures pour rendre le contenu d’une application iOS 9 universellement consultable à l’aide de la recherche de l’application.

Pour plus d’informations, consultez notre [les améliorations de recherche](~/ios/platform/search/index.md) documentation.

## <a name="new-stack-view"></a>Nouvelle vue de la pile

Le contrôle de la vue de la pile ([UIStackView](xref:UIKit.UIStackView) exploite la puissance de la disposition automatique et les Classes de taille pour gérer une pile de sous-vues (verticale ou horizontale) qui répond dynamiquement à la taille d’écran et l’orientation de l’appareil iOS.

En utilisant le contrôle de la vue de la pile, la quantité de travail nécessaire à la disposition de qu'une interface utilisateur est considérablement réduite. La disposition de tous les sous-vues attachée à une vue de la pile sont gérés automatiquement, selon les propriétés de développeur défini comme axe, distribution, alignement et espacement.

Pour plus d’informations, consultez notre [Introduction à la vue de la pile](~/ios/user-interface/controls/uistackview.md) documentation.


## <a name="collection-view-changes"></a>Afficher les modifications de collection

Dans iOS 9, la vue de Collection ([UICollectionView](xref:UIKit.UICollectionView) maintenant prend en charge faites glisser la réorganisation des éléments prêts à l’emploi en ajoutant un nouveau module de reconnaissance de geste par défaut et plusieurs nouvelles méthodes de prise en charge.

À l’aide de ces nouvelles méthodes, vous pouvez facilement implémenter glisser pour réorganiser dans votre vue de Collection et avez la possibilité de personnaliser l’apparence des éléments au cours de n’importe quelle étape du processus de réorganisation.

Pour en savoir plus sur les modifications de la vue de Collection pour iOS 9, veuillez consulter notre [afficher les modifications de Collection](~/ios/user-interface/controls/uicollectionview.md) guide.

## <a name="game-enhancements"></a>Améliorations de jeu

Avec iOS 9, Apple a effectué plusieurs améliorations technologiques à l’API de jeux qui le rendent plus facile à implémenter les graphismes de jeu et de l’audio dans votre application Xamarin.iOS. Ceux-ci incluent les deux facilité de développement par le biais de frameworks de haut niveau et de maîtriser la puissance du GPU de l’appareil iOS pour l’amélioration de la vitesse et des capacités graphiques intègrent les améliorations de bas niveau.

Cela inclut GameplayKit, ReplayKit, e/s du modèle, MetalKit et complète les nuanceurs de performances, ainsi que des fonctionnalités nouvelles et améliorées de métal, SceneKit et SpriteKit.

Pour plus d’informations, consultez notre [jeu améliorations](~/ios/platform/gaming/index.md) documentation.

## <a name="homekit-framework-changes"></a>Modifications du Framework HomeKit

Le [HomeKit](xref:HomeKit) framework, introduit dans iOS 8, fournit la possibilité de configurer et contrôler des accessoires HomeKit activé différents (par exemple, les lumières automatisés, ajoutant des verrous et porte de garage) à partir d’une application Xamarin.iOS. En plus facile à configurer et à configurer, des accessoires HomeKit peuvent être contrôlés via les commandes vocales de Siri.

Dans iOS 9, Apple a facilité le programme d’installation, développé les types d’accessoires pris en charge et fourni plus interactions accessoires (tels que le contrôle un accessoire à distance par le biais d’iCloud).

Pour plus d’informations, consultez notre [présentation d’HomeKit](~/ios/platform/homekit.md), [HomeKitIntro iOS exemple d’application](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) et d’Apple [HomeKit](https://developer.apple.com/homekit/) documentation.

## <a name="handoff-framework-changes"></a>Modifications de Framework de remise

Procédure de transfert (également connu sous la continuité des activités) a été introduite par Apple dans iOS 8 et OS X Yosemite (10.10) comme un moyen pour l’utilisateur de lancer une activité sur l’un de leurs appareils (iOS ou Mac) et continuer cette même activité sur un autre de leurs appareils (telles qu’identifiées par iClou de l’utilisateur d compte).

Procédure de transfert a été développé dans iOS 9 prend également en charge de nouveau, les capacités de recherche améliorées. Pour plus d’informations, consultez notre [les améliorations de recherche](~/ios/platform/search/index.md) documentation. Pour plus d’informations sur l’utilisation de remise, consultez notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) documentation.

## <a name="new-extension-points"></a>Nouveaux points d'extension

Dans iOS 8, Apple a introduit des Extensions, les bibliothèques qui sont présentés par le système d’exploitation dans des contextes standards, comme dans le centre de Notification lorsque l’utilisateur demande un clavier ou lorsqu’il modifie une photo.

Avec iOS 9, Apple étend la prise en charge de l’Extension en fournissant plusieurs nouveaux _Points d’Extension_ qui définissent des stratégies d’utilisation et fournissent des API pour l’utilisation au sein d’une zone donnée, comme suit :

- **Nouveau Point d’Extension Audio Unit** – Utilisez ce Point d’Extension pour fournir des effets audio, des instruments de musique, des générateurs de sons, etc. à utiliser dans d’autres applications d’hôte Audio Unit (par exemple, GarageBand). Ce Point d’Extension vous permet également de vendre _unités Audio_ (plug-ins audio) sur l’App Store.
- **Nouveau Point d’Extension de Maintenance Index** : utilisez ce Point d’Extension pour prendre en charge la réindexation des données d’application sans nécessiter un redémarrage de l’application.
- **Nouveaux Points d’Extension réseau** (qui nécessitent l’autorisation spéciale auprès d’Apple) :
    - **Extension de fournisseur d’application Proxy** : utilisez ce Point d’Extension pour implémenter un proxy transparent de réseau côté client personnalisé.
    - **Filtrer le fournisseur de données / Extension de fournisseur de contrôle de filtre** -utiliser ces Points d’Extension pour implémenter sur le périphérique de filtrage de contenu dynamique de réseau.
    - **Extension de fournisseur de Tunnel de paquets** : utilisez ce Point d’Extension pour implémenter un VPN personnalisé tunneling protocol côté client.
- **Nouveaux Points d’Extension Safari**:
    - **Extension de blocage de contenu** : utilisez ce Point d’Extension pour définir une liste de contenu bloqué qui s’affichera pas lorsque l’utilisateur navigue sur le web.
    - **Partagé d’Extension de liens** : utilisez ce Point d’Extension pour permettre l’affichage de contenu de votre application dans les liens partagés de Safari.

Pour plus d’informations, consultez notre [Introduction aux Extensions](~/ios/platform/extensions.md) et d’Apple [Guide de programmation Extension application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) documentation.

## <a name="keychain-enhancements"></a>Améliorations de trousseau

Dans iOS 9, Apple a amélioré le trousseau pour fournir un nouveau type de clé de chiffrement pour l’Enclave sécurisée et d’autres options de protection d’élément comme suit :

- Une nouvelle contrainte de Touch ID qui invalide les éléments de la chaîne de clé lors de la modification de la base de données d’empreintes digitales.
- Nouvelles contraintes qui autorise la création d’entrées de liste de contrôle d’accès avec Touch ID ou le mot de passe uniquement.
- Un nouveau contexte d’authentification qui permet d’appeler distinct de l’authentification `SecItem` appels.
- Pour le chiffrement d’élément fournie par l’application trousseau d’accès entropie de liste de contrôle (à l’aide de l’option Password de l’Application).
- Prise en charge pour la génération et l’utilisation de clés à l’intérieur de l’enclave sécurisée (via le `kSecAttrTokenIDSecureEnclave` attribut).

Pour plus d’informations, consultez notre [présentation Touch ID](~/ios/platform/touchid.md) documentation.


## <a name="right-to-left-language-support"></a>Prise en charge des langues de droite à gauche

Dans iOS 9, Apple a apporté présentant une interface utilisateur retournée plus facile que jamais en fournissant la prise en charge complète pour les langues de droite à gauche. Ce dernier est détaillé ci-après :

- Standard [UIKit](xref:UIKit) contrôles seront inverser automatiquement selon les paramètres régionaux et linguistiques des appareils iOS de droite à gauche.
- Le [UIView](xref:UIKit.UIView) classe fournit des attributs qui vous permettent de définir la manière dont une vue donnée doit apparaître lorsque retournée de droite à gauche.
- La possibilité de faire pivoter une image par programmation à l’aide de la [FlipsForRightToLeftLayoutDirection](xref:UIKit.UIImage.FlipsForRightToLeftLayoutDirection) propriété de la [UIImage](xref:UIKit.UIImage) classe.

Pour plus d’informations, consultez le site d’Apple [langues prenant en charge de droite à gauche](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) documentation.



## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

En plus des modifications majeures que nous avons indiqué ci-dessus, Apple a apporté des modifications et améliorations apportées à plusieurs infrastructures existantes pour iOS 9, notamment les suivantes :

- Infrastructure AV Foundation
- AVKit Framework
- Framework de CloudKit
- Infrastructure Foundation
- Framework de remise
- Framework d’HealthKit
- Framework d’HomeKit
- Infrastructure d’authentification local
- MapKit Framework
- Framework PassKit
- Safari Services Framework
- Framework UIKit

Pour plus d’informations, consultez notre [modifications d’infrastructure supplémentaires iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) documentation.

## <a name="deprecated-apis-and-functions"></a>Fonctions et les API déconseillées

Apple a déconseillé les fonctions dans iOS 9 et les API suivantes :

- **Livre & de l’interface utilisateur du carnet d’adresses d’adresses** -ces API ont été remplacés par les infrastructures de Contact et de l’interface utilisateur du Contact. Pour plus d’informations, consultez notre [Contacts et l’interface utilisateur de Contacts](~/ios/platform/contacts.md) documentation.
- **CBCentralManager** - le `RetrievePeripherals` et `RetrieveConnectedPeripherals` méthodes de la `CBCentralManager` classe ont été supprimés dans iOS 9. Appel de ces méthodes provoquera une application se bloque lors de l’appariement d’un accessoire ou lors du lancement de l’application.
- **FetchAllChanges** - le `FetchAllChanges` de la `CKFetchRecordChangesOperation` classe a été supprimée et sera supprimée dans iOS 9.
- **Media Player** -le Media Player framework a été déconseillée dans iOS 9. Utilisez plutôt AVKit ou AV Foundation API.

Pour obtenir une liste complète des dépréciations d’API spécifiques, consultez d’Apple [iOS 9.0 différences d’API](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) documentation.

## <a name="ios-9-sample-apps"></a>Exemples d’applications iOS 9

Nous avons certaines [exemples d’iOS 9 spécifiques](https://developer.xamarin.com/samples/ios/iOS9/) pour commencer :

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Consultez également les parties iOS de ces exemples (le Guide Mac OS X versions bientôt !) :

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Liens associés

- [Exemples iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Introduction à la 3D Touch](~/ios/platform/3d-touch.md)
- [Sécurité de Transport de l’application](~/ios/app-fundamentals/ats.md)
- [Multitâche pour iPad](~/ios/platform/multitasking.md)
- [Contacts et l’interface utilisateur](~/ios/platform/contacts.md)
- [Nouvelles API de recherche](~/ios/platform/search/index.md)
- [Introduction à l’affichage de la pile](~/ios/user-interface/controls/uistackview.md)
- [Afficher les modifications de collection](~/ios/user-interface/controls/uicollectionview.md)
- [Améliorations de jeux](~/ios/platform/gaming/index.md)
- [Présentation d’HomeKit](~/ios/platform/homekit.md)
- [Introduction à la procédure de transfert](~/ios/platform/handoff.md)
- [Modifications d’infrastructure supplémentaires iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Résolution des problèmes](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Quelles sont les nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [La mise à jour de vos applications Xamarin.iOS à iOS9 (vidéo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
