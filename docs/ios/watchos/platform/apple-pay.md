---
title: Apple Pay sur watchOS
description: Cet article décrit les améliorations devenue Apple à Apple Pay watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: b46a0e57ea9abc5c4ec4fc2aba1e6940249b64fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="apple-pay-on-watchos"></a>Apple Pay sur watchOS

Apple a apporté plusieurs améliorations à Apple Pay dans watchOS 3 qui ajoute la prise en charge pour les paiements dans l’application. Cela permet à l’utilisateur à fournir le paiement et coordonnées pour payer physiques biens et services directement à partir de l’Apple Watch en toute sécurité.


## <a name="about-apple-pay-enhancements"></a>Concernant les améliorations de paie Apple

En tant que Stated ci-dessus, Apple a fait plusieurs améliorations à Apple Pay watchOS 3 qui permettent de paiement sécurisé et coordonnées pour payer physiques biens et services directement à partir de l’Apple Watch. Ces améliorations sont fournies par des modifications apportées à l’infrastructure PassKit.

IOS 10 et watchOS 3, plusieurs nouvelles API ont été ajoutés qui fonctionnent avec iOS et watchOS pour prendre en charge des réseaux de paiement dynamique et un nouvel environnement de test de bac à sable.

## <a name="passkit-framework-enhancements"></a>Améliorations PassKit Framework

Dans iOS 10, le framework PassKit a été développé pour prendre en charge d’Apple Pay en dehors de `UIKit` et de permettre aux émetteurs de carte présenter leurs cartes à partir de leurs applications. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Prise en charge de la paie Apple en dehors de UIKit

À l’aide de [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) et [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), une application peut prendre en charge la fonctionnalité fournie par [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) sans utiliser UIKit. Si cette nouvelle API est requise pour prendre en charge Apple Pay sur l’Apple Watch (et ainsi les intentions spécifiques), il est facultatif dans d’autres situations (par exemple, les applications existantes). Toutefois, Apple suggère le déplacement à la nouvelle API dès que possible pour fournir un large Apple Pay prise en charge dans l’ensemble de toutes les applications de développeur avec une seule base de code. Pour plus d’informations sur les modes et intégration Siri, veuillez consulter notre [présentation SiriKit](~/ios/platform/sirikit/index.md) documentation.

### <a name="presenting-issuer-cards-from-within-apps"></a>Présentation des cartes de l’émetteur dans les applications

IOS 10 et watchOS 3, les nouvelles fonctionnalités ont été ajoutées à l’infrastructure PassKit qui autorisent les émetteurs de carte présenter leurs cartes de paiement à partir de leurs propres applications. Le développeur peut ajouter un `PKPaymentButtonTypeInStore` UIButton à l’interface utilisateur de l’application qui permet d’afficher un bouton Apple Pay pour une carte.

Le `PresentPaymentPass` méthode de la [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) classe peut également être utilisée pour afficher par programme la carte.

## <a name="new-payment-network-support"></a>Nouvelle prise en charge réseau de paiement

Nouveau iOS 10 et watchOS 3, une application peut automatiquement prendre en charge un nouveau réseau paiement dès que possible sans que le développeur d’avoir à modifier, de recompiler l’application et de le renvoyer à l’App Store.

La nouvelle [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) méthode de la `PKPaymentNetwork` classe permet à une application découvrir les réseaux disponibles sur l’appareil de l’utilisateur lors de l’exécution. En outre, le [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) propriété a été développée pour effectuer le paiement nom du fournisseur en tant qu’argument. À l’aide de ces méthodes, une application peut automatiquement prendre en charge n’importe quel réseau prenant en charge le service de paiement.

Pour plus d’informations, consultez notre [Configuration de payer Apple](~/ios/platform/apple-pay.md) et d’Apple [Guide de payer Apple](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Nouvel environnement de test

IOS 10 et watchOS 3, Apple a introduit un nouvel environnement de test qui permet au développeur de configurer des cartes de paiement de test directement sur un appareil iOS. Ce nouvel environnement de test retourne ensuite les données de test crypté de paiement à l’application.

Pour activer le nouvel environnement de test, procédez comme suit :

1. Créer un nouveau compte d’iCloud test dans iTunes Connect.
2. Connectez-vous à l’appareil iOS avec le nouveau compte de test.
3. Définir la région de votre choix pour tester l’application dans.
4. Utilisez une des cartes de paiement de test à partir de la [Guide de payer Apple](https://developer.apple.com/apple-pay/) pour les paiements.

> [!NOTE]
> En basculant iCloud comptes, l’appareil bascule automatiquement vers le nouvel environnement de test. Toutefois, Apple est toujours **requiert** l’application doit être testée avec réel cartes dans un environnement de production avant de soumettre à l’iTunes App Store.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les améliorations devenue Apple à Apple Pay dans watchOS 3 et comment les implémenter dans Xamarin.iOS.
