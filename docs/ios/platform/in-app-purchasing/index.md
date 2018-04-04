---
title: Achats dans l’application
description: les applications iOS peuvent vendre des produits numériques et des services à l’aide de l’API de Kit de magasin. Les produits sont créés et gérés dans le portail de se connecter iTunes. Apple gère le traitement des transactions et approuve tous les produits avant qu’ils peuvent être vendus et des frais pour chaque transaction (actuellement 30 %). Apple requiert que vous utilisez les achats dans l’application pour toutes les ventes numériques dans votre application, mais vous ne pouvez pas l’utiliser pour la vente de biens ou les services non numériques. Les applications qui offrent des options de paiement pour les services et produits numériques sont susceptibles d’être rejetées. Ce document explique comment configurer votre application pour utiliser le Kit de magasin et fournit des exemples de Xamarin.iOS de scénarios d’achat dans l’application courants.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7a8dec6051caeba55c45df29c085ecfcddd160d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="in-app-purchasing"></a>Achats dans l’application

_les applications iOS peuvent vendre des produits numériques et des services à l’aide de l’API de Kit de magasin. Les produits sont créés et gérés dans le portail de se connecter iTunes. Apple gère le traitement des transactions et approuve tous les produits avant qu’ils peuvent être vendus et des frais pour chaque transaction (actuellement 30 %). Apple requiert que vous utilisez les achats dans l’application pour toutes les ventes numériques dans votre application, mais vous ne pouvez pas l’utiliser pour la vente de biens ou les services non numériques. Les applications qui offrent des options de paiement pour les services et produits numériques sont susceptibles d’être rejetées. Ce document explique comment configurer votre application pour utiliser le Kit de magasin et fournit des exemples de Xamarin.iOS de scénarios d’achat dans l’application courants._


les applications iOS peuvent vendre des produits numériques ou des services à l’aide de StoreKit – un ensemble d’API fournies par iOS qui communiquent avec les serveurs Apple d’effectuer des transactions financières avec l’utilisateur via leur ID Apple. Les APIs StoreKit concernent principalement la récupération des informations de produit et la réalisation des transactions : il n’existe aucun composant de l’interface utilisateur. Les applications qui implémentent l’achat dans l’application doivent créer leur propre interface utilisateur et effectuer le suivi des articles achetés avec du code personnalisé pour fournir des produits requis ou des services à l’utilisateur.

Fournir des fonctionnalités de l’achat de dans l’application nécessite plusieurs étapes :

-  **Configuration de votre application** : profil de configuration de l’application doit être configuré correctement.
-  **Création de produits** – prix et les descriptions de produit doivent être créés dans le portail de se connecter iTunes.
-  **Implémentation StoreKit** – l’API StoreKit doit être implémentée selon les types de produits vendus.
-  **Création de l’interface utilisateur et les produits eux-mêmes** – les produits doivent être appliquées, y compris des mécanismes pour effectuer le suivi de chaque fournisseur et sauvegarde/restauration les si nécessaire.
-  **Analyse des ventes et la réception de fonds** – utilisez les informations fournies par iTunes Connect pour surveiller les tendances des ventes et de suivre vos revenus.


Ce document explique comment effectuer toutes ces étapes pour fournir des achats dans l’application à l’aide de Xamarin.iOS.


## <a name="requirements"></a>Spécifications

Pour prendre en charge de l’achat dans l’application, vous devez utiliser Xamarin.iOS 5.0 ou version ultérieure avec Xcode 7 et versions ultérieures.

## <a name="contents"></a>Sommaire

 * [Principes de base et configuration de l’achat dans l’application](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Vue d’ensemble du Store Kit et récupération des informations sur le produit](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Achat de produits consommables](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Achat de produits non consommables](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Transactions et vérification](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Abonnements et création de rapports](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)


## <a name="summary"></a>Récapitulatif

Cet article a a introduit le concept de l’achat de dans l’application, décrit comment configurer votre application pour tirer parti de celui-ci et présentées à l’aide de Xamarin.iOS des exemples. Il est couvert :

-  **Portail d’approvisionnement iOS** : instructions d’activation dans l’application d’achat des fonctionnalités.
-  **iTunes Connect** : configuration des produits à vendre dans votre application.
-  **Stocker Kit** – explication des classes utilisées pour créer des fonctionnalités dans l’application fournisseur.
-  **Votre application pour l’achat de codage** – exemples de génération dans l’application achat dans une application Xamarin.iOS.
-  **Création de rapports** – vue d’ensemble des statistiques disponibles via iTunes Connect.


## <a name="related-links"></a>Liens associés

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [Dans le Guide de programmation d’application achat](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [Guide du développeur iTunes Connect](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Référence de Framework du Kit de magasin](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Identificateurs de produits in-App Purchase Q & r](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Note technique du fournisseur de dans l’application](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Soumission de votre premier App Store](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Centre de ressources App Store](https://developer.apple.com/appstore/index.html)
- [Conseils pour la soumission à l’App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Recommandations pour l’évaluation dans l’App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [La gestion de vos applications](https://developer.apple.com/appstore/resources/managing/index.html)
