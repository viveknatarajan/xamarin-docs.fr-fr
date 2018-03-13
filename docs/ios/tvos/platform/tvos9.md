---
title: "Introduction à tvOS 9"
description: "Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans tvOS 9 pour les développeurs de Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 55e83658e09bc7e5c12bb3ef3f508497651ec46c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-tvos-9"></a>Introduction à tvOS 9

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans tvOS 9 pour les développeurs de Xamarin.tvOS._


Apple a publié la 4e génération du matériel Apple TV présentant repensé, activer touch distant, le nouveau système d’exploitation tvOS (basé sur iOS 9) en cours d’exécution.

Pour la première fois, tvOS s’ouvre à la plateforme Apple TV au développeur, ce qui vous permet de créer des applications riches et réalistes et de les libérer par le biais du magasin d’applications Apple téléviseurs intégrée dans un processus similaire à celle de l’écriture et la libération des applications pour iOS à l’aide de l’application iTunes Magasin.

Si vous êtes familiarisé avec le développement de Xamarin.iOS, vous devez rechercher la transition vers tvOS relativement simple. La plupart des API et des fonctionnalités est les mêmes, toutefois, de nombreuses API courantes est pas disponibles (par exemple WebKit). En outre, travailler avec les à la distance Siri pose des défis de conception qui ne sont pas présentes dans l’écran tactile en fonction des appareils iOS.

Ce guide donne une introduction à tous les nouveaux et modifiés API et les fonctionnalités disponibles dans tvOS 9 pour les développeurs de Xamarin.tvOS. Pour plus d’informations sur tvOS, consultez le site d’Apple [développement pour la nouvelle Apple TV](https://developer.apple.com/tvos/) documentation.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Fonctionnalités prises en charge et non pris en charge

les applications tvOS en cours d’exécution sur l’Apple TV ont les éléments suivants pris en charge capacités et fonctionnalités :

 - App Groups
 - Background Modes
 - Data Protection
 - Game Center
 - Contrôleurs de jeu
 - iCloud
 - Achats dans l’application
 - Keychain Sharing

Les fonctionnalités suivantes ne sont pas prises en charge :

 - Apple Pay
 - Application Sandbox
 - Associated Domains
 - HealthKit
 - HomeKit
 - Inter-App Audio
 - Maps
 - VPN personnel
 - Notifications Push
 - Wallet
 - Configuration d’accessoires sans fil

Veuillez consulter notre [prise en charge des assemblys](~/ios/tvos/internals/assemblies.md) et [prise en charge des infrastructures](~/ios/tvos/internals/frameworks.md) documentation pour plus d’informations.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV matériel

La nouvelle Apple TV présente les spécifications matérielles suivantes :

 - processeur de A8 64 bits
 - 32 ou 64 Go de stockage
 - 2 Go de RAM
 - 10/100 Mbits/s Ethernet
 - Wi-Fi 802.11a/b/g/n/ac
 - résolution 1080p
 - HDMI
 - Port C (pour les développeurs et diagnostic uniquement)
 - Nouveau Siri distant ou Apple TV à distance (selon la région)

### <a name="siri-remote"></a>Siri distant

En fonction de la région, fourni Apple TV distant entrent dans deux configurations un : Siri distant ou Apple TV à distance.

Siri à distance est actuellement disponible dans les pays suivants :

 - Australie
 - Canada
 - France
 - Allemagne
 - Japon
 - Espagne
 - Royaume-Uni
 - États-Unis

Tous les autres pays recevra l’Apple TV à distance qui remplace le bouton Siri avec un bouton de recherche qui affiche l’écran de recherche par défaut avec une entrée pour la recherche de texte :

[![](tvos9-images/remote02.png "Siri distant")](tvos9-images/remote02.png#lightbox)

Pour plus d’informations, consultez notre [Siri distants et les contrôleurs Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) documentation.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>L’approvisionnement d’Apple TV

Tout comme le développement pour iOS, le nouveau tvOS nécessitera le profil de configuration approprié pour le développement et de distribution selon l’appartenance de l’équipe et les identités de signature que vous avez déjà établies auprès d’Apple.

Mise en service approprié est également nécessaire pour accéder aux fonctionnalités de tvOS comme iCloud KVS ou CloudKit des magasins de données. Veuillez consulter notre [stockage des données et ressources](~/ios/tvos/app-fundamentals/resources-data-storage.md) pour plus d’informations sur la prise en charge d’iCloud dans vos applications Xamarin.tvOS.

Profils de configuration sont créés et installés la même façon que l’utilisation des applications de Xamarin.iOS. Par conséquent, consultez notre iOS [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) documentation pour plus de détails.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV applications

Le nouveau matériel Apple TV et tvOS 9 prend en charge deux types d’applications : traditionnelles et les applications client-serveur.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Applications traditionnelles

Les applications traditionnelles achetées auprès de l’App Store d’Apple TV et sont installées directement sur l’appareil. Ces applications peuvent faire des jeux, des utilitaires ou des applications multimédias qui sont développées à l’aide des mêmes infrastructures et techniques en tant qu’applications de Xamarin.iOS.

Les applications Apple TV ont une taille maximale de 200 Mo et peuvent télécharger des 2 Go de contenu à l’aide des ressources de la demande. Veuillez consulter notre [stockage des données et ressources](~/ios/tvos/app-fundamentals/resources-data-storage.md) pour plus d’informations.

Consultez notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md) pour vous familiariser avec les outils et les concepts nécessaires pour développer des applications de tvOS à l’aide de Xamarin.tvOS.

<a name="Summary" />

### <a name="client-server-apps"></a>Applications client-serveur

En plus des applications traditionnelles installées, Apple TV permet de facilement créer un média web-client-serveur de diffusion en continu des applications à l’aide de technologies web (HTTPS, XML et JavaScript). Vous allez concevoir l’interface utilisateur à l’aide du langage de balisage TVML d’Apple et utiliser JavaScript pour définir les comportements de l’application à l’aide de TVMLKit.

Pour plus d’informations, consultez le site d’Apple [Apple TV balisage de référence du langage](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS Framework référence](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit Framework référence](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [sur Diffusion en continu HTTP](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) et [spécification de création de TLS pour Apple TV](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) documentation.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Problèmes d’Interface utilisateur

Contrairement à iOS et OS X, l’Apple TV n’a pas d’un écran tactile ou une souris qui autorise l’utilisateur à sélectionner directement et d’interagir avec une application ou de son contenu. Au lieu de cela ils utilisateur distant Siri nouveau ou un contrôleur de jeu Bluetooth pour naviguer dans l’Interface utilisateur d’une application. Pour plus d’informations, consultez notre [Siri distants et les contrôleurs Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) documentation.

En outre, l’expérience utilisateur globale est considérablement différent de celui des applications iOS ou Mac qui ont tendance à être des expériences utilisateur unique. Avec l’Apple TV, des expériences utilisateur ont tendance à être plus social par nature, où plusieurs personnes peuvent être assis sur le canapé interagir avec une application unique et l’autre. Pour créer une expérience d’application Apple TV réussie (soit une nouvelle application ou que vous portez une existante), ces modifications doivent prendre en considération. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Utilisation de Focus et les Images de parallaxe

Comme indiqué ci-dessus, les utilisateurs de votre application Xamarin.tvOS pas interagir avec son interface directement en tant qu’iOS où ils appuyer sur les images sur l’écran du périphérique, mais indirectement à partir de sur l’espace à l’aide de l’instance distante de Siri. Pour présenter et gérer l’interaction utilisateur, l’Apple TV utilise un modèle de Focus en fonction. 

En tant que le focus change, effets (par exemple, l’effet parallaxe sur des images) et les animations subtiles servent à identifier clairement l’élément d’Interface utilisateur qui a actuellement le focus.

Si l’utilisateur effectue un mouvement lent, circulaire sur Siri à distance, l’élément actif sera balancement en temps réel en réponse à ce déplacement. Le balancement se produit, un reflet lumineux est appliquée à son image de rendre la surface apparaissent se traduisent. Après un délai d’inactivité donné, tout contenu hors du focus estompe et l’élément actif augmentera supérieur.

Pour plus d’informations, consultez notre [utilisation de Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) et [fonctionne avec des icônes et des Images](~/ios/tvos/app-fundamentals/icons-images.md) documentation.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>L’écran d’accueil

L’écran d’accueil de Apple TV montre toutes les applications qui sont installées et fournit un moyen d’accéder aux préférences de l’utilisateur :

[![](tvos9-images/home01.png "L’écran d’accueil")](tvos9-images/home01.png#lightbox)

L’utilisateur accède à une grille des icônes d’application à l’aide des entrées tactiles multipoints sur Siri distant à l’aide de focus pour sélectionner une application et lancer. L’icône d’application est le votre première chance d’effectuer une impression excellente sur les utilisateurs potentiels et doit communiquer l’objectif de votre application en un coup de œil.

Chaque application doit fournir un petit et une version de grande taille de son icône de l’application. La petite icône servira à l’écran d’accueil de Apple TV lorsque l’application est installée. La version de grande taille est utilisée par l’App Store. L’icône d’application volumineux doit reproduire l’apparence de la version de la petite icône.

Pour plus d’informations, consultez notre [fonctionne avec des icônes et des Images](~/ios/tvos/app-fundamentals/icons-images.md) documentation.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Le haut de l’étagère

Si l’utilisateur a placé votre application Xamarin.tvOS sur la ligne en haut de l’écran d’accueil de Apple TV, une grande Image de l’étagère haut s’affichera lorsque votre application sera sélectionnée par l’utilisateur. Cette image doit mettre en évidence les fonctionnalités de votre application ou fournir des liens directs vers son contenu.

[![](tvos9-images/topshelf01.png "Le haut de l’étagère")](tvos9-images/topshelf01.png#lightbox)

L’Image de l’étagère Top peut être fourni en tant que statique unique `.png` ou `.lsr` fichiers, ou il peut être créé dynamiquement lors de l’exécution en tant qu’une seule ligne d’éléments peut être actif.

Au lieu d’afficher en haut de l’étagère Image statique, il peut contenir une ligne dynamique ou les éléments peut être actif ou un ensemble dynamique de défilement bannières. Deux ces style dynamique vous permettent de mettre en surbrillance le contenu fourni par votre application ou d’un saut dans ses fonctionnalités utilisées le plus.

Pour plus d’informations, consultez notre [fonctionne avec des icônes et des Images](~/ios/tvos/app-fundamentals/icons-images.md) documentation et Apple [TVServices Framework référence](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) pour plus d’informations sur l’ajout d’une Extension de l’étagère haut à votre application pour fournir contenu dynamique en haut de l’étagère.






## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Création d’applications tvOS avec Xamarin (vidéo)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
