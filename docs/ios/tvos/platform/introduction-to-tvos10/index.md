---
title: "Présentation de tvOS 10"
description: "Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans tvOS 10 pour les développeurs de Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8bc379e507287cd609dca8440b1210b7f1514114
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-tvos-10"></a>Présentation de tvOS 10

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans tvOS 10 pour les développeurs de Xamarin.tvOS._

Avec la nouvelle tvOS 10 SDK Apple a inclus les nouvelles API et services qui permettent au développeur de créer des catégories d’applications et fonctionnalités. 

Pour plus d’informations sur tvOS 10, consultez le site d’Apple [tvOS + applications](https://developer.apple.com/tvos/) documentation.

## <a name="whats-new-in-tvos-10"></a>Nouveautés dans tvOS 10

Apple a ajouté plusieurs nouvelles API et sur les services dans tvOS 10, ainsi que de nombreuses améliorations apportées aux fonctionnalités existantes, y compris :

## <a name="new-user-interface-styles"></a>Nouveaux Styles de l’Interface utilisateur

tvOS 10 maintenant prend en charge un thème sombre et l’Interface utilisateur de lumière thème que tous le UIKit build dans les contrôles seront automatiquement adapter, en fonction des préférences de l’utilisateur.

Lors de la création et l’implémentation des nouveaux contrôles d’interface utilisateur personnalisés, le développeur doit utiliser le [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) classe pour s’adapter à thème sélectionné de l’utilisateur.

Pour plus d’informations, consultez notre [nouveaux Styles de l’Interface utilisateur](~/ios/tvos/platform/user-interface-styles.md) documentation.

## <a name="security-and-privacy-enhancements"></a>Améliorations de la confidentialité et de sécurité

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans tvOS 10 qui aideront le développeur d’améliorer la sécurité de leurs applications et de garantir la confidentialité de l’utilisateur final.

Par conséquent, les applications exécutées sur watchOS 3 (ou version ultérieure) doivent déclarer statiquement intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés spécifiques confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pour lesquelles l’application souhaite accéder.

Étant donné que tvOS 10 partage ces modifications avec iOS 10, consultez notre iOS 10 [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) guide pour plus d’informations.

## <a name="video-subscriber-account"></a>Compte de l’abonné vidéo

Nouvelles de tvOS 10, la structure du compte d’abonné vidéo permet aux applications cette prise en charge authentifié de diffusion en continu ou la vidéo à la demande pour s’authentifier avec leur fournisseur TV câble ou satellite à l’aide d’une seul-expérience de connexion pour l’utilisateur final.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Couleur large

tvOS 10 étend la prise en charge pour les formats de pixel de plage étendue et les espaces d’une gamme de couleurs dans l’ensemble du système, y compris les infrastructures telles que les graphiques de base, l’Image de base, complète et AVFoundation. Prise en charge pour les appareils avec une couleur large affiche est facilité davantage en fournissant ce comportement dans toute la pile de graphique entière.

En outre, `UIKit` a été modifié pour fonctionner dans la nouvelle étendue **sRVB** espace colorimétrique, facilitant le mélange de couleurs dans les gammes de couleur large sans perte de performances significatifs.

Apple offre les meilleures pratiques suivantes lorsque vous travaillez avec des couleurs larges :

 - `UIColor` maintenant utilise le sRGB espace de couleurs et ne sera plus limiter les valeurs à la `0.0` à `1.0` plage. Si l’application s’appuie sur le comportement précédent de la limite, elle devrez modifiables de tvOS 10.
 - Si l’application effectue le rendu personnalisé de `UIImages`, utilisez la nouvelle [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) classe pour spécifier l’utilisation des formats de plage étendue ou une plage de standard.
 - Lorsque vous utilisez une API de bas niveau tels que les graphiques de base ou complète pour fournir le traitement d’image, l’application doit utiliser un format de couleur plage étendue espace et de pixels qui prend en charge les valeurs à virgule flottante 16 bits. Le cas échéant, l’application aura à la fixation des valeurs de couleur composant manuellement.
 - Graphiques de base, Image Core et complète les nuanceurs de performances fournissent des nouvelles méthodes pour la conversion entre les espaces de deux couleurs.

Pour plus d’informations, consultez notre [Introduction à la couleur large](~/ios/platform/wide-color.md) guide.

## <a name="newly-available-existing-frameworks"></a>Infrastructures existantes qui vient d’être disponibles.

Plusieurs infrastructures qui étaient disponibles sur iOS (et pas tvOS), ont été apportées disponible pour tvOS 10 tels que :

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - Photos
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

Outre les modifications majeures framework et les ajouts répertoriés ci-dessus, Apple a fait de nombreuses modifications supplémentaires framework mineure tvOS 10.

Pour plus d’informations, consultez notre [des modifications supplémentaires Framework](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) guide.

## <a name="deprecated-apis"></a>API déconseillées

Aucune API ou des infrastructures ont été déconseillées, tvOS 10. Consultez d’Apple [tvOS 10 différences d’API](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) documentation pour obtenir la liste complète des modifications de l’API.



## <a name="related-links"></a>Liens associés

- [Exemples de tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Nouveautés de tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
