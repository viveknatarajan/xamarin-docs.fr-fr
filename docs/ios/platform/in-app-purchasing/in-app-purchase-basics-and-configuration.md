---
title: Principes fondamentaux d’achat dans l’application et la Configuration dans Xamarin.iOS
description: Ce document décrit les achats dans l’application dans Xamarin.iOS, traitant des informations pertinentes sur les règles, de configuration et d’iTunes Connect.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4d79988fc2900f1fe58774657344f19fab90f3e4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105088"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>Principes fondamentaux d’achat dans l’application et la Configuration dans Xamarin.iOS

Implémentation des achats dans l’application nécessite l’application d’utiliser l’API StoreKit sur l’appareil. StoreKit gère toutes les communications avec les serveurs d’iTunes d’Apple pour obtenir des informations sur les produits et effectuer des transactions. Le profil d’approvisionnement doit être configuré pour l’achat dans l’application et les informations sur les produits doivent être entrées dans iTunes Connect.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit gère toutes les communications avec d’Apple, comme illustré dans ce graphique")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

À l’aide de l’App Store pour fournir des achats dans l’application nécessite l’installation et la configuration suivante :

-  **iTunes Connect** : configuration des produits à vendre et configuration des comptes d’utilisateurs de bac à sable pour tester des achats. Vous devez également avoir fourni vos informations fiscales et bancaires à Apple afin qu’ils peuvent reverser des fonds collectées à votre place.
-   **Portail de provisionnement iOS** : création d’un identificateur de Bundle et activation de l’accès App Store pour votre application.
-  **Store Kit** – Ajout de code à votre application pour l’affichage des produits, d’achat de produits et de restauration des transactions.
-  **Code personnalisé** – pour suivre les achats effectués par les clients et indiquer les produits ou services qu’ils ont achetés. Vous pouvez également doivent implémenter un processus côté serveur pour valider les accusés de réception si vos produits sont constitués du contenu téléchargé à partir d’un serveur (par exemple, la documentation et les problèmes magazines).


Il existe deux Store Kit « serveur environnements » :

-  **Production** – Transactions avec argent réel. Accessible uniquement via les applications qui ont été soumises et approuvées par Apple. Produits d’achat dans l’application doivent également être vérifiées et approuvées avant d’être disponibles sur l’environnement de production.
-  **Bac à sable** – où vos tests se produit. Produits sont disponibles ici immédiatement après sa création (le processus d’approbation s’applique uniquement à l’environnement de Production). Les transactions dans le bac à sable nécessitent des utilisateurs de test (ID Apple non de vraies) effectuer des transactions.

## <a name="in-app-purchase-rules"></a>Règles d’achat dans l’application

Vous ne peut pas accepter d’autres modes de paiement pour les produits numériques ou des services à l’intérieur de votre application, ni être mentionnées ou y faire référence vos utilisateurs à partir d’une application. Cela signifie que vous ne peut pas accepter les cartes de crédit ou PayPal lors de l’achat dans l’application est le mécanisme de paiement la plus approprié. Il existe un cas spécial pour l’achat de produits numériques en dehors de l’application, mais pour les utiliser dans l’application, comme l’achat de la documentation sur un site Web qui sont associée à une « connexion » spécifique et l’utilisation que « connexion » dans l’application permet l’accès utilisateur, les livres achetés.
Les applications qui fonctionnent de cette façon ne sont pas autorisées à mentionner ou lié à la fonctionnalité d’achat externe : les développeurs doivent communiquer cette fonctionnalité à leurs utilisateurs par d’autres moyens (par exemple par le biais de marketing par e-mail ou certaines autres canal direct).

Toutefois, étant donné que vous ne pouvez pas utiliser achats dans l’application des marchandises physiques, dans ce cas, vous pouvez utiliser un mécanisme de paiement (par ex. carte de crédit, PayPal) à partir de l’application.

Apple doit approuver chaque produit avant sa mise sur vente : le nom, la description et une capture d’écran de la « produit » est requis pour la révision. Temps de révision de produit sont les mêmes que pour des révisions d’application.

Vous ne pouvez pas choisir n’importe quel prix de votre produit : vous pouvez uniquement sélectionner un niveau de prix' ' qui a une valeur spécifique dans chaque pays/devise qui prend en charge par Apple. Vous ne pouvez avoir un niveau de prix différent sur différents marchés.

## <a name="configuration"></a>Configuration

Avant d’écrire du code dans l’application d’achat, vous devez effectuer certaines tâches de configuration dans iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) et le portail de provisionnement iOS ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Ces trois étapes doivent être terminées avant d’écrire de code :

-  **Compte de développeur Apple** – envoyer vos informations bancaires et fiscales à Apple.
-  **Portail de provisionnement iOS** – Vérifiez que votre application dispose d’un ID d’application valide (pas un caractère générique avec un astérisque * qu’il contient) et a dans Achats application activé.
-  **iTunes Connect Application gestion** – ajouter des produits à votre application.


### <a name="apple-developer-account"></a>Compte de développeur Apple

Création et la distribution des applications gratuites nécessitent très peu de configuration dans [iTunes Connect](https://itunesconnect.apple.com), toutefois, à vendre payant applications ou des achats dans l’application vous oblige à fournir Apple avec les informations bancaires et fiscales. Cliquez sur **accords, fiscales et bancaires** dans le menu principal indiqué ici :

 [![](in-app-purchase-basics-and-configuration-images/image2.png "Cliquez sur les contrats, fiscales et bancaires dans le menu principal")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

Votre compte de développeur doit avoir un **des Applications iOS payantes** contrat en vigueur, comme indiqué dans cette capture d’écran :

 [![](in-app-purchase-basics-and-configuration-images/image3.png "Votre compte de développeur doit avoir un iOS Applications payantes de contrat en vigueur")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

Vous ne serez pas en mesure de tester toutes les fonctionnalités StoreKit jusqu'à ce que vous ayez un **des Applications iOS payantes** contrat : appels StoreKit dans votre code échoueront jusqu'à ce que Apple a traité vos **contrats, fiscales et bancaires** plus d’informations.

### <a name="ios-provisioning-portal"></a>Portail de provisionnement iOS

Nouvelles applications sont définies dans le **ID d’application** section de la **portail de provisionnement iOS**. Pour créer un nouvel ID d’application, accédez à la [Member Center du portail de provisionnement iOS](https://developer.apple.com/membercenter/index.action), accédez à **certificats, identificateurs et profils** section du portail et cliquez sur **identificateurs** sous *des applications iOS*. Ensuite, cliquez sur « + » situé en haut à droite pour générer un ID d’application.


Le formulaire de création de nouveaux **ID d’application**

 ressemble à ceci :

 [![](in-app-purchase-basics-and-configuration-images/image4.png "Le formulaire de création de nouveaux ID d’application")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

Entrez un nom approprié pour le *Description*, afin de pouvoir identifier facilement cet ID d’application dans une liste. Pour le *préfixe d’ID d’application*, sélectionnez l’ID d’équipe.

#### <a name="bundle-identifierapp-id-suffix-format"></a>Format de suffixe d’ID / l’application de l’identificateur de bundle

Vous pouvez utiliser n’importe quelle chaîne que vous le souhaitez pour votre **identificateur de Bundle** (tant qu’il est unique dans votre compte), mais recommande d’Apple vous suivez le format DNS inversé plutôt que d’utilisez n’importe quelle chaîne arbitraire. L’exemple d’application qui accompagne cet article utilise com.xamarin.storekit.testing pour l’identificateur de Bundle, mais il est également possible d’utiliser un identificateur, par exemple my_store_example (bien qu’Apple ne sont pas recommandées il).

> [!IMPORTANT]
> Apple permet également l’astérisque comme caractère générique à ajouter à la fin d’un **identificateur de Bundle** afin qu’un ID d’application unique peut être utilisé pour plusieurs applications, toutefois _génériques ID d’application ne peut pas être utilisés pourdansAppPurchase_. Un exemple d’identificateur de Bundle génériques peuvent être com.xamarin.*

#### <a name="enabling-app-services"></a>L’activation de l’application Services

Notez que **In-App Purchase** sera automatiquement activée dans la liste des Services :

 [![](in-app-purchase-basics-and-configuration-images/image5.png "Achat dans l’application sera automatiquement activée dans la liste des Services")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>Profils de provisionnement

Créer des profils de provisionnement de Production et de développement comme vous normalement serait, en sélectionnant l’ID d’application que vous avez configurées pour les achats dans l’application. Reportez-vous à la [iOS Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) et [publication sur l’App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) guides pour plus d’informations.

## <a name="itunes-connect"></a>iTunes Connect

Cliquez sur **mes applications** dans iTunes Connect pour créer ou modifier une entrée d’application iOS. La page de présentation d’application est illustrée ici :

 [![](in-app-purchase-basics-and-configuration-images/image6.png "La page de présentation d’application")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

Cliquez sur **achats dans l’application** pour créer ou modifier vos produits à la vente. Cette capture d’écran montre l’exemple d’application avec plusieurs produits déjà ajoutés :

 [![](in-app-purchase-basics-and-configuration-images/image7.png "L’exemple d’application avec plusieurs produits déjà ajouté")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

Le processus d’ajout de nouveaux produits comporte deux étapes :

1.   Choisissez le type de produit : [![](in-app-purchase-basics-and-configuration-images/image8.png "choisir le type de produit")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   Entrez les attributs du produit, y compris l’Id de produit, le niveau tarifaire et descriptions localisées : [![](in-app-purchase-basics-and-configuration-images/image9.png "entrer les attributs de produits")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

Les champs requis pour chaque produit de l’achat dans l’application sont décrits ci-dessous :


### <a name="reference-name"></a>Nom de la référence

Le nom de référence n’est pas affiché à vos utilisateurs ; Il est à usage interne et apparaît uniquement dans iTunes Connect.

### <a name="product-id-format"></a>Format d’ID de produit

Un identificateur de produit peut contenir uniquement d’alphanumériques (A-Z, a-z, 0-9), trait de soulignement (_) et le point (.). Bien que vous pouvez utiliser n’importe quelle chaîne pour vos identificateurs, Apple recommande d’utiliser le format DNS inversé. Par exemple, l’exemple d’application utilise cet identificateur de Bundle :

 `com.xamarin.storekit.testing`

Par conséquent, la convention pour identifier les produits d’achat dans l’application se présente comme suit :

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Cette convention d’affectation de noms n’est pas appliquée, il vous suffit d’une recommandation pour vous aider à gérer vos produits. En outre, en dépit de suivre la même convention DNS inverse, les identificateurs de produits sont *ne pas liées* à l’identificateur de Bundle et ne font pas obligé de démarrer avec la même chaîne. Il est toujours possible d’utiliser des identificateurs comme photo_product_greyscale (bien qu’Apple ne sont pas recommandées il).

ID de produit n’est pas visible aux utilisateurs, mais il est utilisé pour référencer le produit dans votre code d’application.

### <a name="product-type"></a>Type de produit

Il existe cinq types de produit d’achat dans l’application, que vous pouvez proposer :

1.  **Consommables** – les choses sont 'utilisé haut », par exemple devise dans le jeu que le joueur peut consacrer à. Si l’utilisateur effectue une sauvegarde/restauration ou sinon a actualisé leur appareil, une transaction consommable ne pas obtenir également restaurée (ce qui serait efficacement le lecteur du même avantage tout recommencer). Code d’application doit être sûr de fournir les « consommable » dès que la transaction est terminée.
1.  **Non consommable** – achetés qui « possède » l’utilisateur qu’une seule fois, par exemple un problème de magazine numérique ou d’un niveau de jeu.
1.  **Abonnements renouvelables automatiquement** – il vous suffit comme un abonnement au magazine réelles, à la fin de la période d’abonnement Apple automatiquement facture au client à nouveau et étend la durée d’abonnement, indéfiniment ou jusqu'à ce que le client de manière explicite il annule. C’est la méthode de paiement préférée pour les applications kiosque (en fait, les applications doivent prendre en charge ce mode de paiement doivent être approuvées pour la distribution de Newsstand).
1.  **Abonnement gratuit** : peut uniquement être proposé dans les applications prenant en charge le Newsstand, et permet au client d’accéder au contenu d’abonnement sur tous leurs appareils. Abonnements gratuits ne jamais expirent.
1.  **Abonnement non-renouvellement** – doit être utilisé pour vendre des accès de durée limitée à du contenu statique, tels que l’accès à une archive de photo d’un mois.


 *Ce document traite actuellement des seuls les premier deux produit les types (facilement consommables et Non consommable).*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Niveaux de prix

L’App Store ne vous permet pas de définir un prix arbitraire de vos produits – Apple fournit des niveaux de prix fixe que vous pouvez choisir. Les prix sont fixes dans chaque devise, et Apple réserve le droit d’ajuster les prix relatifs (par exemple, après une modification soutenue dans le taux de change de devises relatif entre une devise et le Dollar américain).

Apple fournit une matrice de prix pour vous aider à sélectionner le niveau approprié pour le prix/devise que vous souhaitez. Voici un extrait de la matrice de prix (août 2012) :

 [![](in-app-purchase-basics-and-configuration-images/image10.png "Un extrait de la matrice de prix août 2012")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

Au moment de l’écriture (juin 2013), il existe des 87 niveaux d’USD 0,99 à 999,99 de USD. La matrice de tarification indique le prix que vos clients payera et également la quantité que vous allez recevoir à partir d’Apple – il s’agit moins leurs frais de 30 %, et également des taxes locales qu’ils sont nécessaires pour collecter (Notez que, dans l’exemple que les vendeurs nord-américains et canadiens recevoir 70c pour un p c 99 roduit, tandis que les vendeurs australiens recevoir uniquement 63 c en raison « marchandises &amp; Services taxe ' perçu sur le prix de vente).

Tarification de votre produit peut être mis à jour à tout moment, y compris les modifications de prix planifié qui en vigueur à une date ultérieure. Cette capture d’écran montre l’ajout d’un changement de prix à une date future, le prix est temporairement modifié de niveau 1 au niveau 3 pour le mois de septembre uniquement :

 [![](in-app-purchase-basics-and-configuration-images/image11.png "Un changement de prix à une date future où le prix est temporairement modifié de niveau 1 au niveau 3 pour le mois de septembre uniquement")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>Produits gratuits ne pas pris en charge

Apple a fourni une option d’abonnement gratuit spéciale pour les applications kiosque, il n’est pas possible de définir un prix (gratuit) à zéro pour tout autre type d’achat dans l’application. Vous pouvez modifier (ie. inférieur) prix de ventes promotions, vous ne pouvez apporter des achats dans l’application gratuits via iTunes Connect.

### <a name="localization"></a>Localisation

Dans iTunes Connect, vous pouvez entrer différents nom et Description un texte pour n’importe quel nombre de langues prises en charge. Chaque langage peut être ajoutés/modifiés dans via une fenêtre contextuelle :

 [![](in-app-purchase-basics-and-configuration-images/image12.png "Chaque langage peut être ajoutés/modifiés dans via une fenêtre contextuelle")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 Lorsque vous affichez des informations sur les produits dans votre application, le texte localisé est disponible pour vous permettent d’afficher par le biais de StoreKit. L’affichage de la devise doit également être localisé pour afficher le symbole correct et la mise en forme décimale – mise en forme est décrite ultérieurement dans le document.

### <a name="app-store-review"></a>Révision de l’App Store

Même en tant qu’applications – chaque produit est examiné par Apple avant d’être autorisé à accéder à la vente. Produits peuvent être rejetées pour tout contenu inapproprié dans le nom ou la Description, ou Apple peut décider que vous avez choisi le type de produit incorrect (par exemple). vous avez créé un livre ou un problème de magazine, mais utilisé le type de produit de consommation). Évaluations de produits peuvent prendre jusqu'à une révision d’application.

La première fois qu’une application est envoyée avec les achats dans l’application est activée (si elle est une nouvelle application, ou la fonctionnalité a été ajoutée à un autre existant) vous devez également choisir certains produits à soumettre avec lui. Portail iTunes Connect vous invitera à le faire, comme indiqué dans cette capture d’écran :

 [![](in-app-purchase-basics-and-configuration-images/image13.png "Portail iTunes Connect vous invitera à soumettre également certains produits")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 L’application et les achats dans l’application seront examinées ensemble, afin que tous les obtenir l’approbation à la fois (de sorte que cette application n’accède pas dans le magasin sans des produits approuvés !).

Une fois votre première version avec fonctionnalité d’achat dans l’application a été approuvée, vous pouvez ajouter d’autres produits et les soumettre pour examen à tout moment. Vous pouvez également choisir soumettre une nouvelle version avec les produits de l’achat dans l’application spécifique, à l’aide de la **détails de la Version** page comme le suggère l’invite.

Reportez-vous à la [directives de révision App Store](https://developer.apple.com/appstore/guidelines.html) pour plus d’informations.

 [Partie 2 : vue d’ensemble du Kit de Store et des informations sur les produits de récupération](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
