---
title: Apple Pay
description: "Ce guide présente la configuration de l’environnement Xamarin.iOS pour une utilisation avec Apple Pay pour payer des marchandises physiques, telles que des produits alimentaires, les divertissements et les appartenances via votre application. Il inclut des informations sur les identificateurs requis, les certificats et les droits."
ms.topic: article
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af899bb1c5708e3fc0be88db6224d9127f5a5c6d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay"></a>Apple Pay

_Ce guide présente la configuration de l’environnement Xamarin.iOS pour une utilisation avec Apple Pay pour payer des marchandises physiques, telles que des produits alimentaires, les divertissements et les appartenances via votre application. Il inclut des informations sur les identificateurs requis, les certificats et les droits._


Apple Pay a été introduite avec iOS 8, permettant aux utilisateurs de payer pour les marchandises physiques tels que des produits alimentaires, les divertissements et les appartenances via leurs périphériques iOS. Il est disponible sur iPhone 6 et iPhone 6 Plus et peuvent également être associés à l’Apple Watch pour des achats dans le magasin. Lorsqu’il est utilisé sur un iPhone, il utilise Touch ID comme un moyen de confirmer et autoriser d’un utilisateur carte de crédit ou débit des transactions.


## <a name="requirements"></a>Spécifications

Apple Pay est uniquement disponible dans iOS 8 et versions ultérieures et par conséquent nécessite un minimum de Xcode 6.

Les éléments suivants sont également requis pour intégrer Apple Pay dans votre application :

 - Plateforme de processeur de paiement
 - Identificateur de partenaire
 - Un certificat Apple Pay
 - Apple Pay des droits

Ce document examine ces éléments plus en détail.

## <a name="differences-between-apple-pay-and-iap"></a>Différences entre Apple salaire et IAP

La principale différence entre Apple Pay et *dans l’application d’achat* (IAP), se rapporte aux produits qu’ils vendent. *Physique* biens vendus via Apple Pay ; produits alimentaires, hébergement et divertissement physique (par exemple, les tickets de cinéma) sont tous les exemples. En revanche, vend des IAP *virtuels* marchandises, par exemple premium ou des contenus supplémentaires et des abonnements : réflexion autres mois d’un service de diffusion en continu, ou vie supplémentaire dans un jeu.

Les infrastructures utilisés sont également une différence essentielle ; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) est utilisée pour payer d’Apple, alors que [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) fournit l’API de l’infrastructure d’IAP.

Avec Apple Pay, Apple [états](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) [qu’il « ne] facture pas les utilisateurs, partenaires ou les développeurs à utiliser le salaire d’Apple pour les paiements ». En comparaison, IAP a une charge de 30 % pour chaque transaction. En outre, avec Apple Pay, la transaction ne passe pas par Apple du tout, au lieu de cela, il passe par une plateforme de paiement.


## <a name="using-a-payment-processor-platform"></a>À l’aide d’une plateforme de processeur de paiement

Une des parties fondamentales de Apple Pay est le traitement des paiements. S’il est possible de le faire vous-même, il requiert une connaissance significative de la cryptographie
- comme détaillé dans Apple [guide de traitement du paiement](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Plateformes de traitement de paiement, gérer en revanche, ces opérations pour vous, ce qui vous permet de vous concentrer sur la génération de votre application.

Deux options sont les suivantes :

- **Stripe** -s’inscrire à [Stripe.com](https://stripe.com/) pour accéder à leurs API.

- **JudoPay** -extraire leurs [Xamarin exemple de code sur github](https://github.com/Judopay/Xamarin-Sample-App)et pour vous inscrire [JudoPay.com](https://www.judopay.com/).


## <a name="provisioning-for-apple-pay"></a>Configuration pour payer d’Apple

La configuration d’une application pour utiliser Apple Pay requiert que le programme d’installation sur le portail des développeurs Apple et au sein de votre application. Il existe un nombre d’étapes à suivre pour correctement configurer votre application pour payer d’Apple :

1. Créer un ID de vendeur :
    - Suivez les étapes [ici](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Créer un ID d’application avec la fonctionnalité Appliquer payer et lui ajouter le partenaire :
    - Suivez les étapes [ici](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Générer un certificat pour l’ID de partenaire :
    - Suivez les étapes [ici](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Générer un profil de configuration avec l’ID d’application nouvellement créé :
    - Suivez les étapes [ici](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Ajoutez Apple payer des droits :
    - Sélectionnez les droits de paie Apple comme expliqué [ici](~/ios/deploy-test/provisioning/entitlements.md), ou ajoutez manuellement la paire clé/valeur dans le fichier à partir de [ici](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Utilisation de la paie d’Apple

Apple a apporté plusieurs améliorations à Apple Pay dans iOS 10 permettant à l’utilisateur pour les paiements sécurisés à partir de sites Web et via l’interaction avec Siri et des mappages.

Avec iOS 10, plusieurs nouvelles API ont été ajoutés qui fonctionnent avec iOS et watchOS pour prendre en charge des réseaux de paiement dynamique et un nouvel environnement de test de bac à sable.


### <a name="apple-pay-website-integration"></a>Intégration de site Web de Apple paie

Nouveau pour iOS 10, le développeur peut incorporer Apple Pay directement dans leurs sites Web à l’aide de **ApplePay JS**. Les utilisateurs qui parcourent le site Web avec Safari dans iOS ou macOS peuvent effectuer des paiements avec Apple Pay en validant la transaction sur leur iPhone ou votre Apple Watch. Pour plus d’informations, consultez le site d’Apple [ApplePay JP Framework référence](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Améliorations PassKit Framework

Dans iOS 10, le framework PassKit a été développé pour prendre en charge d’Apple Pay en dehors de `UIKit` et de permettre aux émetteurs de carte présenter leurs propres cartes à partir de leurs applications.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Prise en charge de la paie Apple en dehors de UIKit

À l’aide de [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) et [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), une application peut prendre en charge la fonctionnalité fournie par [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) sans utiliser UIKit. Si cette nouvelle API est requise pour prendre en charge Apple Pay sur l’Apple Watch (et ainsi les intentions spécifiques), il est facultatif dans d’autres situations (par exemple, les applications existantes). Toutefois, Apple suggère le déplacement à la nouvelle API dès que possible pour fournir un large Apple Pay prise en charge dans l’ensemble de toutes les applications de développeur avec une seule base de code. Pour plus d’informations sur les modes et intégration Siri, veuillez consulter notre [présentation SiriKit](~/ios/platform/sirikit/index.md) documentation.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Présentation des cartes de l’émetteur dans les applications

Avec iOS 10, les nouvelles fonctionnalités ont été ajoutées à l’infrastructure PassKit qui autorisent les émetteurs de carte présenter leurs cartes à partir de leurs propres applications. Le développeur peut ajouter un `PKPaymentButtonTypeInStore` UIButton à l’interface utilisateur de l’application qui permet d’afficher un bouton Apple Pay pour une carte.

Le `PresentPaymentPass` méthode de la [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) classe peut également être utilisée pour afficher par programme la carte.

### <a name="new-payment-network-support"></a>Nouvelle prise en charge réseau de paiement

Nouveau pour iOS 10, une application peut automatiquement prendre en charge un nouveau réseau paiement dès que possible sans que le développeur d’avoir à modifier, de recompiler l’application et de le renvoyer à l’App Store.

La nouvelle [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) méthode de la `PKPaymentNetwork` classe permet à une application découvrir les réseaux disponibles sur l’appareil de l’utilisateur lors de l’exécution. En outre, le [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) propriété a été développée pour effectuer le paiement nom du fournisseur en tant qu’argument. À l’aide de ces méthodes, une application peut automatiquement prendre en charge n’importe quel réseau prenant en charge le service de paiement.

Pour plus d’informations, consultez notre [Configuration de payer Apple](~/ios/platform/apple-pay.md) et d’Apple [Guide de payer Apple](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Nouvel environnement de test

Avec iOS 10, Apple a introduit un nouvel environnement de test qui permet au développeur de configurer des cartes de paiement de test directement sur un appareil iOS. Ce nouvel environnement de test retourne ensuite les données de test crypté de paiement à l’application.

Pour activer le nouvel environnement de test, procédez comme suit :

1. Créer un nouveau compte d’iCloud test dans iTunes Connect.
2. Connectez-vous à l’appareil iOS avec le nouveau compte de test.
3. Définir la région de votre choix pour tester l’application dans.
4. Utilisez une des cartes de paiement de test à partir de la [Guide de payer Apple](https://developer.apple.com/apple-pay/) pour les paiements.

> [!IMPORTANT]
> En basculant iCloud comptes, l’appareil bascule automatiquement vers le nouvel environnement de test. Toutefois, Apple est toujours **requiert** l’application doit être testée avec réel cartes dans un environnement de production avant de soumettre à l’iTunes App Store.

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons exploré les différents éléments requis pour utiliser Apple Pay au sein de votre application. Nous avons examiné comment créer un ID de partenaire, et comment elle est utilisée dans les **Entitlements.plist**, qui doit être modifié manuellement.


## <a name="related-links"></a>Liens associés

- [Achats dans l’application](~/ios/platform/in-app-purchasing/index.md)
- [Introduction à PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (sample)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
