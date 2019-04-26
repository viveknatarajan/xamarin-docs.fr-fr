---
title: Considérations relatives à la plateforme 32/64 bits
description: Ce document décrit les différentes considérations à prendre en compte lors du ciblage d’architectures 32 bits et 64 bits pour une application Xamarin.iOS ou Xamarin.Mac.
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 31eb0bfae58ecdca40548e46d1d9d95828be67b4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61347836"
---
# <a name="3264-bit-platform-considerations"></a>Considérations relatives à la plateforme 32/64 bits

Bien qu’iOS et macOS ont toujours pris en charge les applications 32 et 64 bits, Apple a déconseillé progressivement prise en charge 32 bits.

À compter d’iOS 11, les applications 32 bits ne sont plus démarre et [toutes les soumissions à l’App Store doivent prendre en charge 64 bits](https://developer.apple.com/news/?id=06282017b).

À compter de janvier 2018, [nouvelles applications soumises au Store d’application Mac doivent prendre en charge 64 bits](https://developer.apple.com/news/?id=06282017a), et les applications existantes doivent être mis à jour en juin 2018.

API classique de Xamarin (`XamMac.dll` et `monotouch.dll`) pris en charge uniquement les applications 32 bits. Toutefois, utilisent les nouvelles applications Xamarin.iOS et Xamarin.Mac le [API unifiée](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` et `Xamarin.Mac`) par défaut, et peut par conséquent cible 32 et 64 bits, en fonction des besoins.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>L’activation de 64 bits génère des applications Xamarin.iOS

> [!WARNING]
> Cette section est fournie pour des raisons historiques et pour aider à déplacer les anciens projets Xamarin.iOS à l’API unifiée et prendre en charge 64 bits. Tous les nouveaux projets Xamarin.iOS utilisent l’API unifiée et une cible 64 bits par défaut.

Pour les applications mobiles Xamarin.iOS qui ont été converties vers l’API unifiée, les développeurs doivent mettre à jour manuellement les paramètres de génération pour des cibles 64 bits :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Dans le **panneau solutions**, double-cliquez sur le projet de l’application pour ouvrir le **Options du projet** fenêtre.
2. Sélectionnez **Build iOS**.
3. Pour le simulateur, iPhone dans le **architectures prises en charge** liste déroulante, sélectionnez **x86\_64** ou **i386 + x86\_64**:

   [![Définir les architectures prises en charge sur x86\_64 ou i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Pour les appareils physiques, sélectionnez une des **ARM64** combinaisons :

   [![Affectant les architectures prises en charge l’une des combinaisons ARM64](Images/Image02.png "paramètre pris en charge les architectures à une des combinaisons ARM64")](Images/Image02-large.png#lightbox)

5. Cliquez sur **OK**.
6. Effectuer un nettoyage de build.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Dans le **l’Explorateur de solutions**, cliquez sur projet de l’application et sélectionnez **propriétés**.
2. Sélectionnez **Build iOS**.
3. Pour le simulateur iPhone, définissez **architectures prises en charge** soit **x86\_64** ou **i386 + x86\_64**: 

   [![Définissant les architectures prises en charge pour x86_64 ou i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Pour les appareils physiques, sélectionnez une des **ARM64** combinaisons :
    
   [![Affectant les architectures prises en charge l’une des combinaisons ARM64](Images/VS01.png "paramètre pris en charge les architectures à une des combinaisons ARM64")](Images/VS01-large.png#lightbox)

5. Enregistrez les modifications apportées.
6. Effectuer un nettoyage de build.

-----

ARMv7s est pris en charge uniquement par le processeur A6 inclus dans l’iPhone 5 (ou supérieure). ARMv7 code est plus rapide et plus petit que le ARMv6 uniquement fonctionne avec l’iPhone 3GS et versions ultérieures et est requis par Apple lors du ciblage de l’iPad ou une version iOS minimale de 5.0. ARMv6 fonctionne sur tous les appareils, mais n’est plus pris en charge par le compilateur livré avec Xcode 4.5 et versions ultérieures. 

ARM64 est requis pour prendre en charge iOS 8 sur iPhone 6 ou autres périphériques 64 bits et doivent par Apple lors de la soumission de nouvelle ou mise à jour quitter des applications dans l’App Store d’iTunes.

Pour plus d’informations sur les fonctionnalités des différents appareils iOS, consultez d’Apple [compatibilité des périphériques](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) document.

### <a name="64-bit-and-binary-size-increases"></a>augmentation de la taille de 64 bits et binaire

Pendant la transition d’Apple à partir d’iOS de 32 bits vers 64 bits, les applications doivent exécuter sur du matériel 32 bits et 64 bits. Pour cette raison, Unified API de Xamarin permet aux développeurs de cibler à la fois.

Ciblage des architectures 32 bits et 64 bits vise à augmenter considérablement la taille d’une application. Toutefois, cela permettrait des appareils récents exécuter du code optimisé tout en prenant en charge des appareils plus anciens.

> [!IMPORTANT]
> Si vous recevez le message suivant lors de l’envoi d’une application iOS vers l’iTunes App Store, _« avertissement ITMS-9000 : Prise en charge de 64 bits manquant. Applications téléchargées vers l’App Store à partir du 1er février 2015, la nouvelle iOS doivent inclure la prise en charge de 64 bits et être générées avec iOS 8 SDK, inclus dans Xcode 6 ou version ultérieure. Pour activer 64 bits dans votre projet, nous vous recommandons d’utiliser la valeur par défaut de paramètre « architectures Standard » de la build Xcode pour créer un fichier binaire unique avec du code 32 bits et 64 bits. »_ Vous devez basculer les architectures prises en charge pour une des **ARM64** combinaison (comme indiqué ci-dessus), de recompilation et soumettez à nouveau.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> À compter de janvier 2018, toutes les nouvelles applications Mac envoyées vers le Store d’application Mac doivent prendre en charge 64 bits. Les applications Mac App Store existantes et leurs mises à jour doivent prendre en charge 64 bits à compter de juin 2018. Consultez [announcment de d’Apple](https://developer.apple.com/news/?id=06282017a) et [un guide qui décrit comment mettre à jour vos applications Xamarin.Mac vers 64 bits](~/cross-platform/macios/32-and-64/mac-64-bit.md).

La plupart des ordinateurs Mac prennent en charge les applications 32 bits et 64 bits.   Mac OS 10.6 (Snow Leopard) a été le dernier système d’exploitation pour s’exécuter sur les systèmes 32 bits.   La plupart des Macs publiés depuis 2010 prend en charge les deux systèmes.

Contrairement à iOS, la plupart des nouvelles infrastructures introduites dans les versions récentes de macOS sont uniquement pris en charge en mode 64 bits (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, Social, et MapKit, entre autres).

L’API unifiée permet aux développeurs de choisir quelle sorte d’applications produire : 32 bits ou 64 bits.

**les applications 32 bits** aura s’exécutent sur les ordinateurs Mac 32 bits et 64 bits, un espace d’adressage limité à 32 bits et que toutes les bibliothèques doivent être 32 bits.

Vous utiliserez généralement ce mode si vous avez des dépendances 32 bits qui n’exécutent pas en mode 64 bits, si vous souhaitez avoir un téléchargement plus petits, ou s’il en existe aucun avantage de performances dans le passage à 64 bits.

Ce mode limite que vous ne pourrez peut-être utiliser de nombreuses infrastructures disponibles dans macOS Mavericks et macOS Yosemite.

**applications 64 bits** s’exécutent uniquement sur les appareils Mac 64 bits.

Pour Mac, il s’agit du mode de fonctionnement préféré comme la plupart des ordinateurs Mac en cours d’utilisation actuelles prennent en charge que le mode 64 bits, et que vous avez accès à l’ensemble complet des infrastructures fournies par Apple.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>L’activation de 64 bits génère des applications de Xamarin.Mac

Pour plus d’informations sur la création d’une application 64 bits à l’aide de Xamarin.Mac, veuillez consulter la [unifiée de Xamarin.Mac la mise à jour des applications 64 bits](~/cross-platform/macios/32-and-64/mac-64-bit.md) guide.

## <a name="related-links"></a>Liens connexes

- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
