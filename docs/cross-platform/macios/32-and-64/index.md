---
title: "Considérations relatives à la plateforme 32/64 bits"
description: "Considérations lors du ciblage des architectures 32 bits et 64 bits pour votre application"
ms.topic: article
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: af266bf9484f3a0da45173f4cfba22e0378ace22
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="3264-bit-platform-considerations"></a>Considérations relatives à la plateforme 32/64 bits

IOS et macOS ont historiquement prend en charge les applications 32 et 64 bits, Apple a déconseillé progressivement prise en charge 32 bits.

À partir d’iOS 11, les applications 32 bits ne sont plus démarre et [toutes les soumissions à l’App Store doivent prendre en charge 64 bits](https://developer.apple.com/news/?id=06282017b).

À compter de janvier 2018, [soumises à Mac App Store de nouvelles applications doivent prendre en charge 64 bits](https://developer.apple.com/news/?id=06282017a), et les applications existantes doivent être mises à jour en juin 2018.

Les API classique de Xamarin (`XamMac.dll` et `monotouch.dll`) pris en charge les applications 32 bits uniquement. Toutefois, utilisent les nouvelles applications Xamarin.iOS et Xamarin.Mac le [API unifiée](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` et `Xamarin.Mac`) par défaut, et peut donc cible 32 et 64 bits, si nécessaire.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>L’activation de 64 bits génère des applications de Xamarin.iOS

> [!WARNING]
> Cette section est fournie pour des raisons historiques et pour aider à déplacer les anciens projets Xamarin.iOS à l’API unifiée et prend en charge 64 bits. Tous les nouveaux projets Xamarin.iOS utilise les API unifiée et une cible 64 bits par défaut.

Pour les applications mobiles Xamarin.iOS qui ont été converties à l’API unifiée, les développeurs doivent mettre à jour manuellement les paramètres de génération pour des cibles 64 bits :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **Solution remplissage**, double-cliquez sur le projet de l’application pour ouvrir le **Options du projet** fenêtre.
2. Sélectionnez **iOS Build**.
3. Pour le simulateur, iPhone dans le **prise en charge des architectures** liste déroulante, sélectionnez **x86\_64** ou **i386 + x86\_64**:

   [![Définition des architectures prises en charge pour x86\_64 ou i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Pour les périphériques physiques, sélectionnez une des **ARM64** combinaisons :

   [![Définition des architectures prises en charge pour une des combinaisons de ARM64](Images/Image02.png "paramètre pris en charge les architectures à une des combinaisons ARM64")](Images/Image02-large.png#lightbox)

5. Cliquez sur **OK**.
6. Effectuer un nettoyage de build.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans le **l’Explorateur de solutions**, cliquez sur le projet de l’application et sélectionnez **propriétés**.
2. Sélectionnez **iOS Build**.
3. Pour le simulateur iPhone, définissez **prise en charge des architectures** soit **x86\_64** ou **i386 + x86\_64**: 

   [![Définition des architectures prises en charge sur x86_64 ou i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Pour les périphériques physiques, sélectionnez une des **ARM64** combinaisons :
    
   [![Définition des architectures prises en charge pour une des combinaisons de ARM64](Images/VS01.png "paramètre pris en charge les architectures à une des combinaisons ARM64")](Images/VS01-large.png#lightbox)

5. Enregistrez les modifications apportées.
6. Effectuer un nettoyage de build.

-----

ARMv7s est pris en charge uniquement par le processeur A6 inclus dans l’iPhone 5 (ou version ultérieure). ARMv7 code est plus rapide et plus petit que le ARMv6, uniquement compatible avec l’iPhone 3GS et version ultérieure et est requis par Apple lors du ciblage iPad ou une version minimale iOS 5.0. ARMv6 fonctionne sur tous les appareils, mais n’est plus pris en charge par le compilateur fourni avec Xcode 4.5 et versions ultérieures. 

ARM64 est requis pour prendre en charge d’iOS 8 sur iPhone 6 ou d’autres périphériques 64 bits et devront par Apple lorsque vous soumettez de nouveau ou quitter des applications dans l’App Store iTunes avec mise à jour.

Pour une vision détaillée les fonctionnalités de différents appareils iOS, consultez Apple [la compatibilité des périphériques](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) document.

### <a name="64-bit-and-binary-size-increases"></a>augmente la taille de 64 bits et binaire

Au cours de passage d’Apple iOS de 32 bits à 64 bits, les applications doivent s’exécuter sur du matériel 32 bits et 64 bits. Pour cette raison, API unifiée de Xamarin permet aux développeurs de cibler à la fois.

Ciblage des architectures 32 bits et 64 bits peut accroître considérablement la taille d’une application. Cependant, cette opération donc permet d’autoriser les périphériques plus récents exécuter du code optimisé toujours prise en charge de périphériques plus anciens.

> [!IMPORTANT]
> Si vous recevez le message suivant lors de l’envoi d’une application iOS à l’iTunes App Store, _« avertissement éléments-9000 : pas de prise en charge 64 bits. À partir du 1er février 2015, les e/s nouvelle applications téléchargées vers le magasin d’applications doivent inclure la prise en charge de 64 bits et être générées avec iOS 8 SDK, inclus dans Xcode 6 ou version ultérieure. Pour activer 64 bits dans votre projet, nous recommandons d’utiliser la valeur par défaut de paramètre de « architectures Standard » de la build Xcode pour générer un seul fichier binaire avec du code 32 bits et 64 bits. »_ Vous devez basculer les architectures prises en charge pour une des **ARM64** combinaison (comme indiqué ci-dessus), de recompilation et soumettez à nouveau.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> À partir de janvier 2018, toutes les applications Mac nouveau soumises à Mac App Store doivent prendre en charge 64 bits. Les applications Mac App Store existantes et leurs mises à jour doivent prendre en charge 64 bits à partir de juin 2018. Consultez [annonce d’Apple](https://developer.apple.com/news/?id=06282017a) et [un guide qui décrit comment mettre à jour vos applications Xamarin.Mac à 64 bits](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Les ordinateurs Mac les plus récents prennent en charge les applications 32 bits et 64 bits.   MacOS 10.6 (Snow Leopard) a été le dernier système d’exploitation pour s’exécuter sur les systèmes 32 bits.   La plupart des Mac publiés depuis 2010 prend en charge les deux systèmes.

Contrairement à iOS, la plupart des infrastructures de nouvelles introduites dans les versions récentes de macOS sont uniquement pris en charge en mode 64 bits (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, sociale et MapKit, entre autres).

L’API unifiée permettent aux développeurs de choisir le type d’applications produire : 32 bits ou 64 bits.

**les applications 32 bits** sera exécuté sur 32 bits et 64 bits des ordinateurs Mac, ont un espace d’adressage limité à 32 bits et nécessitent que toutes les bibliothèques sont 32 bits.

Vous utiliserez généralement ce mode si vous avez des dépendances de 32 bits qui n’exécutent pas en mode 64 bits, si vous souhaitez avoir un téléchargement plus petit ou s’il n’y aucun avantage de performances en migrant vers 64 bits.

Ce mode limite que vous ne pourrez peut-être utiliser plusieurs infrastructures disponibles dans macOS Mavericks et macOS Yosemite.

**applications 64 bits** s’exécute uniquement sur les appareils Mac 64 bits.

Pour Mac, il s’agit du mode de fonctionnement préféré comme la plupart des Mac en cours d’utilisation actuelles prennent en charge que le mode 64 bits, et que vous avez accès à l’ensemble des infrastructures fournies par Apple.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>L’activation de 64 bits génère des applications de Xamarin.Mac

Pour plus d’informations sur la création d’une application 64 bits à l’aide de Xamarin.Mac, consultez la [unifiée de Xamarin.Mac mise à jour des applications 64 bits](~/cross-platform/macios/32-and-64/mac-64-bit.md) guide.

## <a name="related-links"></a>Liens connexes

- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
