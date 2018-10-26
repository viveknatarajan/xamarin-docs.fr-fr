---
title: Apple Pay sur watchOS dans Xamarin
description: Cet article aborde les améliorations Apple a apportées à Apple Pay dans watchOS 3 et comment les implémenter dans Xamarin.iOS pour Apple Watch.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 354e03ee1e07ba99fcdeb05617bc65ed89f0e8c2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103086"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Apple Pay sur watchOS dans Xamarin

Apple a apporté plusieurs améliorations à Apple Pay dans watchOS 3 qui ajoute la prise en charge pour les paiements dans l’application. Cela permet à l’utilisateur à fournir le paiement en toute sécurité et les coordonnées à payer pour les services et des marchandises physiques directement à partir de l’Apple Watch.


## <a name="about-apple-pay-enhancements"></a>Concernant les améliorations de paiement Apple

En tant que Stated ci-dessus, Apple a apporté plusieurs améliorations à Apple Pay dans watchOS 3 qui permettent de paiement sécurisé et les coordonnées à payer pour les services et des marchandises physiques directement à partir de l’Apple Watch. Ces améliorations sont fournies par la modification du framework PassKit.

Avec iOS 10 et watchOS 3, plusieurs nouvelles API ont été ajoutés qui fonctionnent avec iOS et watchOS pour prendre en charge des réseaux de paiement dynamique et un nouvel environnement de test de bac à sable.

## <a name="passkit-framework-enhancements"></a>Améliorations du Framework PassKit

Dans iOS 10, le framework PassKit a été développé pour prendre en charge Apple Pay en dehors de `UIKit` et pour permettre des émetteurs de carte présenter leurs cartes à partir de leurs applications. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Prise en charge Apple Pay en dehors de UIKit

À l’aide de [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) et [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), une application peut prendre en charge les mêmes fonctionnalités fournies par [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) sans utiliser UIKit. Cette nouvelle API est requise pour prendre en charge Apple Pay sur l’Apple Watch (et aussi les intentions spécifiques), mais il est facultatif dans d’autres situations (par exemple, les applications existantes). Toutefois, les suggestions d’Apple déplacement à la nouvelle API dès que possible pour fournir d’Apple Pay prise en charge étendue dans l’ensemble de toutes les applications du développeur avec une seule base de code. Pour plus d’informations sur les intentions et l’intégration de Siri, veuillez consulter notre [présentation de SiriKit](~/ios/platform/sirikit/index.md) documentation.

### <a name="presenting-issuer-cards-from-within-apps"></a>Présentation des cartes de l’émetteur à partir d’au sein d’applications

Avec iOS 10 et watchOS 3, les nouvelles fonctionnalités ont été ajoutées au framework PassKit acceptant des émetteurs de carte présenter leurs cartes de paiement à partir de leurs propres applications. Le développeur peut ajouter un `PKPaymentButtonTypeInStore` UIButton à interface utilisateur de l’application qui affiche un bouton Apple Pay pour une carte.

Le `PresentPaymentPass` méthode de la [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) classe peut également être utilisée pour afficher par programme la carte.

## <a name="new-payment-network-support"></a>Nouvelle prise en charge réseau de paiement

Nouveau à iOS 10 et watchOS 3, une application peut automatiquement en charge un nouveau réseau de paiement lorsqu’elle est disponible sans le développeur n’ait à modifier, de recompiler l’application et de le renvoyer à l’App Store.

La nouvelle [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) méthode de la `PKPaymentNetwork` classe permet à une application découvrir les réseaux disponibles sur le périphérique l’utilisateur lors de l’exécution. En outre, le [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) propriété a été étendue pour prendre le paiement nom du fournisseur en tant qu’argument. À l’aide de ces méthodes, une application peut automatiquement prendre en charge n’importe quel réseau prenant en charge le service de paiement.

Pour plus d’informations, consultez notre [Apple payer Configuration](~/ios/platform/apple-pay.md) et d’Apple [Apple payer Guide](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Nouvel environnement de test

Avec iOS 10 et watchOS 3, Apple a introduit un nouvel environnement de test qui permet au développeur de configurer des cartes de paiement de test directement sur un appareil iOS. Ce nouvel environnement de test renvoie ensuite les données de paiement de test chiffré à l’application.

Pour activer le nouvel environnement de test, procédez comme suit :

1. Créer un nouveau compte d’iCloud test dans iTunes Connect.
2. Connectez-vous à l’appareil iOS avec le nouveau compte de test.
3. Définir la région souhaitée pour tester l’application dans.
4. Utilisez une des cartes de paiement de test à partir de la [Apple payer Guide](https://developer.apple.com/apple-pay/) de paiement.

> [!NOTE]
> En basculant les comptes iCloud, l’appareil bascule automatiquement vers le nouvel environnement de test. Toutefois, Apple toujours **requiert** l’application à tester avec de véritables cartes dans un environnement de production avant la soumission à l’App Store d’iTunes.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les améliorations Apple a apportées à Apple Pay dans watchOS 3 et comment les implémenter dans Xamarin.iOS.
