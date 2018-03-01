---
title: "Configuration et les concepts de base dans l’application fournisseur"
ms.topic: article
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8e0f5b24ff6790aa3bf63eb9112790e0a62ce0a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="in-app-purchase-basics-and-configuration"></a>Configuration et les concepts de base dans l’application fournisseur

Implémentation des achats dans l’application nécessite que l’application d’utiliser l’API StoreKit sur l’appareil. StoreKit gère toutes les communications avec les serveurs d’iTunes d’Apple pour obtenir des informations sur les produits et effectuer des transactions. Le profil de configuration doit être configuré pour l’achat dans l’application et les informations de produit doivent être entrées dans iTunes Connect.

 [ ![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit gère toutes les communications avec d’Apple, comme illustré dans ce graphique")](in-app-purchase-basics-and-configuration-images/image1.png)

À l’aide de l’App Store pour fournir l’achat dans l’application nécessite l’installation et la configuration suivante :

-  **iTunes Connect** : configuration des produits à vendre et configuration des comptes d’utilisateurs de bac à sable pour tester des achats. Vous devez également avoir fourni vos informations bancaires et les taxes à Apple afin qu’ils peuvent remettre les fonds collectés en votre nom.
-   **Portail d’approvisionnement iOS** : création d’un identificateur de lot et activation de l’accès du magasin d’applications pour votre application.
-  **Stocker Kit** – Ajout de code à votre application pour l’affichage des produits, d’acheter des produits et de restauration des transactions.
-  **Code personnalisé** : pour suivre les achats effectués par les clients et indiquer les produits ou services qu’ils ont achetés. Vous pouvez également doivent implémenter un processus côté serveur pour valider les accusés de réception si vos produits se composent de contenu téléchargé à partir d’un serveur (par exemple, la documentation et les problèmes magazines).


Il existe deux Store Kit « environnements serveur » :

-  **Production** – Transactions avec argent réel. Accessible uniquement via des applications qui ont été soumises et approuvées par Apple. Dans l’application achat produits également doivent être révisées et approuvés avant d’être disponibles sur l’environnement de production.
-  **Bac à sable** – où vos tests se produit. Les produits sont disponibles ici immédiatement après la création (le processus d’approbation s’applique uniquement à l’environnement de Production). Les transactions dans le bac à sable requièrent des utilisateurs de test (pas de véritable Apple ID) effectuer des transactions.

## <a name="in-app-purchase-rules"></a>Règles de bon de dans l’application

Vous ne peut pas accepter d’autres modes de paiement pour les produits numériques ou des services à l’intérieur de votre application, ni mentionnent les ou y faire référence vos utilisateurs à partir d’une application. Cela signifie que vous ne peut pas accepter les cartes de crédit ou PayPal lors de l’achat dans l’application est le mécanisme de paiement plus approprié. Il existe un cas spécial pour l’achat de produits numériques en dehors de l’application, mais pour les utiliser dans l’application, telles que la documentation sur un site Web qui sont associée à un « connexion » spécifique de l’achat et à l’aide de la « connexion » dans l’application vous permet de l’accès utilisateur livres achetées.
Les applications qui fonctionnent de cette façon ne sont pas autorisées à mentionner ou un lien vers la fonction d’achat externe, les développeurs doivent communiquer cette fonctionnalité pour les utilisateurs par d’autres moyens (par exemple via e-mail marketing ou certaines autres canal direct).

Toutefois, étant donné que vous ne pouvez pas utiliser dans l’application des achats de biens, dans ce cas, vous pouvez utiliser un mécanisme de paiement (par exemple). carte de crédit, PayPal) à partir de l’application.

Apple devez approuver tous les produits qu’il passe sur vente : le nom, la description et une capture d’écran de la « produit » est requis pour la révision. Temps de révision de produit sont les mêmes que pour les révisions d’application.

Vous ne pouvez pas choisir les prix de votre produit, vous ne pouvez sélectionner un niveau de prix' ' qui a une valeur spécifique dans chaque pays/la devise Apple prend en charge. Vous ne pouvez avoir un niveau de prix différent dans divers marchés.

## <a name="configuration"></a>Configuration

Avant d’écrire du code dans l’application d’achat, vous devez effectuer certaines tâches de configuration dans iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) et le portail d’approvisionnement iOS ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Ces trois étapes doivent être terminées avant d’écrire de code :

-  **Compte de développeur Apple** – soumettre vos informations bancaires et de taxation à Apple.
-  **Portail d’approvisionnement iOS** : Vérifiez votre application dispose d’un ID d’application valide (pas un caractère générique avec un astérisque * qu’elle contient) et a activé les dans application d’achat.
-  **iTunes gestion des applications de se connecter** – ajouter des produits à votre application.


### <a name="apple-developer-account"></a>Compte de développeur Apple

Génération et de distribuer des applications gratuites nécessitent très peu de configuration dans [iTunes Connect](https://itunesconnect.apple.com); Toutefois, pour les revendre payant applications ou les achats dans l’application vous oblige à fournir Apple avec les informations bancaires et de taxation. Cliquez sur **accords, taxes et bancaire** dans le menu principal est indiqué ici :

 [ ![](in-app-purchase-basics-and-configuration-images/image2.png "Cliquez sur les accords, taxes et bancaire dans le menu principal")](in-app-purchase-basics-and-configuration-images/image2.png)

Votre compte de développeur doit avoir un **des Applications iOS payé** en effet, comme indiqué dans cette capture d’écran de contrat :

 [ ![](in-app-purchase-basics-and-configuration-images/image3.png "Votre compte de développeur doit avoir un iOS payé des Applications de contrat en vigueur")](in-app-purchase-basics-and-configuration-images/image3.png)

Vous ne serez pas en mesure de tester toutes les fonctionnalités StoreKit jusqu'à ce que vous ayez un **payé des Applications iOS** contrat : appels StoreKit dans votre code échoueront jusqu'à ce que Apple a traité votre **contrats, les taxes et bancaire** plus d’informations.

### <a name="ios-provisioning-portal"></a>Portail de provisionnement iOS

Nouvelles applications sont définies dans le **ID d’application** section de la **iOS Provisioning Portal**. Pour créer un nouvel ID d’application, accédez à la [Member Center du portail d’approvisionnement iOS](https://developer.apple.com/membercenter/index.action), accédez à **profils, des identificateurs et des certificats** section du portail, cliquez sur **identificateurs** sous *des applications iOS*. Ensuite, cliquez sur « + » en haut à droite pour générer un nouvel ID d’application.


Le formulaire de création de nouveaux **ID d’application**

 ressemble à ceci :

 [ ![](in-app-purchase-basics-and-configuration-images/image4.png "Le formulaire de création de nouveaux ID d’application")](in-app-purchase-basics-and-configuration-images/image4.png)

Entrez un nom approprié pour le *Description*, afin de pouvoir identifier facilement cet ID d’application dans une liste. Pour le *préfixe d’ID d’application*, sélectionnez l’ID d’équipe.

#### <a name="bundle-identifierapp-id-suffix-format"></a>Format de suffixe d’ID de regroupement identificateur/application

Vous pouvez utiliser n’importe quelle chaîne que vous le souhaitez pour votre **identificateur de lot** (tant qu’il est unique dans votre compte), mais recommande d’Apple vous suivez le format DNS inverse plutôt que d’utilisez n’importe quelle chaîne arbitraire. L’exemple d’application qui accompagne cet article utilise com.xamarin.storekit.testing pour l’identificateur de lot, mais il est également possible d’utiliser un identificateur comme my_store_example (même si Apple ne la recommandons).

> [!IMPORTANT]
> **Remarque**: Apple permet également l’astérisque comme caractère générique à ajouter à la fin d’un **identificateur de lot** afin qu’un ID d’application unique peut être utilisé pour plusieurs applications, toutefois _génériques App ID ne peut pas être utilisés pour Dans AppPurchase_. Un exemple d’identificateur de lot génériques peuvent être com.xamarin.*

#### <a name="enabling-app-services"></a>L’activation des Services d’application

Notez que **In-App Purchase** seront automatiquement activés dans la liste des Services :

 [ ![](in-app-purchase-basics-and-configuration-images/image5.png "In-App Purchase seront automatiquement activé dans la liste des Services")](in-app-purchase-basics-and-configuration-images/image5.png)

#### <a name="provisioning-profiles"></a>Profils de configuration

Créer des profils de configuration de Production et de développement comme vous normalement serait, en sélectionnant l’ID d’application que vous avez configuré pour l’achat dans l’application. Reportez-vous à la [configuration d’appareil iOS](~/ios/get-started/installation/device-provisioning/index.md) et [de publication à l’App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) guides pour plus d’informations.

## <a name="itunes-connect"></a>iTunes Connect

Cliquez sur **mes applications** dans iTunes Connect pour créer ou modifier une entrée d’application iOS. La page Vue d’ensemble d’applications est illustrée ici :

 [ ![](in-app-purchase-basics-and-configuration-images/image6.png "La page Vue d’ensemble d’applications")](in-app-purchase-basics-and-configuration-images/image6.png)

Cliquez sur **achats dans l’application** pour créer ou modifier vos produits. Cette capture d’écran montre l’exemple d’application avec plusieurs produits déjà ajoutés :

 [ ![](in-app-purchase-basics-and-configuration-images/image7.png "L’exemple d’application avec plusieurs produits déjà ajouté")](in-app-purchase-basics-and-configuration-images/image7.png)

Le processus d’ajout de nouveaux produits comporte deux étapes :

1.   Choisissez le type de produit : [ ![ ] (in-app-purchase-basics-and-configuration-images/image8.png "choisir le type de produit")](in-app-purchase-basics-and-configuration-images/image8.png) 
2.   Entrez les attributs du produit, y compris l’Id de produit, le niveau tarifaire et descriptions localisées : [ ![ ] (in-app-purchase-basics-and-configuration-images/image9.png "entrer les attributs de produits")](in-app-purchase-basics-and-configuration-images/image9.png)

Les champs requis pour chaque produit dans l’application achat sont décrites ci-dessous :


### <a name="reference-name"></a>Nom de la référence

Le nom de référence n’est pas affiché à vos utilisateurs ; Il est à usage interne et n’apparaît que dans iTunes Connect.

### <a name="product-id-format"></a>Format d’ID de produit

Un identificateur de produit peut uniquement contenir alphanumériques (A-Z, a-z, 0-9), trait de soulignement (_) et le point (.). Bien que vous pouvez utiliser n’importe quelle chaîne pour vos identificateurs, Apple recommande le format DNS inverse. Par exemple, l’exemple d’application utilise cet identificateur de lot :

 `com.xamarin.storekit.testing`

Par conséquent, la convention pour identifier les produits dans l’application fournisseur serait comme suit :

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Cette convention d’affectation de noms n’est pas appliquée, il vous suffit d’une recommandation pour vous aider à gérer vos produits. En outre, en dépit de suivant la même convention de DNS à l’inverse, les identificateurs de produits sont *ne pas liées* à l’identificateur de lot et ne font pas obligé de démarrer avec la même chaîne. Il est toujours possible d’utiliser des identificateurs comme photo_product_greyscale (même si Apple ne la recommandons).

ID de produit n’est pas visible aux utilisateurs, mais il est utilisé pour référencer le produit dans votre code d’application.

### <a name="product-type"></a>Type de produit

Il existe cinq types de produit dans l’application fournisseur que vous pouvez proposer :

1.  **Consommable** : les éléments qui sont « utilisé des', telles que devise dans le jeu qui permet de passer le lecteur. Si l’utilisateur effectue une sauvegarde/restauration ou sinon a actualisé leur appareil, une transaction peut consommer ne pas obtenir restaurée (ce qui serait efficacement le lecteur du même avantage de nouveau). Code de l’application doit être sûr de fournir des « consommable » dès que la transaction est terminée.
1.  **Non consommable** – achetés détenus par l’utilisateur '' qu’une seule fois, par exemple un problème de magazine numérique ou d’un niveau de jeu.
1.  **Abonnements renouvelables automatiquement** – simplement comme un abonnement de magazine du monde réel, à la fin de la période d’abonnement Apple automatiquement facture au client à nouveau et étend la durée de l’abonnement, indéfiniment ou jusqu'à ce que le client de manière explicite elle annule. C’est la méthode de paiement par défaut pour les applications de kiosque (en fait, les applications doivent prendre en charge ce mode de paiement doivent être approuvées pour la distribution de kiosque).
1.  **Inscription gratuite** – peuvent uniquement être proposés dans les applications kiosque et permet au client pour accéder à l’abonnement sur leurs appareils. Abonnements gratuits n’expirent jamais.
1.  **Abonnement de renouvellement de non** – doit être utilisé pour vendre l’accès à du contenu statique, tels que l’accès d’un mois dans une archive photo limitée dans le temps.


 *Actuellement, ce document couvre uniquement les premiers types de deux produit (consommable et Non consommable).*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Niveaux de prix

Le magasin d’applications ne vous permet de définir un prix arbitraire de vos produits – Apple fournit les niveaux de prix fixe que vous pouvez sélectionner. Les prix sont résolus dans chaque devise et Apple réserve le droit d’ajuster les prix relatifs (par exemple, après une modification soutenue du taux de change de devises relatif entre une devise et le Dollar américain).

Apple offre une matrice de prix pour vous aider à sélectionner le niveau approprié pour la devise/prix que vous souhaitez. Voici un extrait de la matrice de prix (août 2012) :

 [ ![](in-app-purchase-basics-and-configuration-images/image10.png "Un extrait de la matrice de prix août 2012")](in-app-purchase-basics-and-configuration-images/image10.png)

Au moment de l’écriture (juin 2013), il existe 87 niveaux à partir de USD 0,99 pour USD 999,99. La matrice de tarification affiche le prix que payer vos clients et également la quantité que vous recevez à partir d’Apple – il s’agit moins les frais de 30 % et les impôts locaux qu’ils sont également requises pour collecter (Notez dans l’exemple vendeurs aux États-Unis et de réception 70c pour un p c 99 accusé, alors que les vendeurs australiennes reçoivent uniquement 63 c en raison de ' marchandises &amp; Services taxe ' perçu sur le prix de vente).

La tarification de votre produit peut être mis à jour à tout moment, y compris les modifications de prix planifié qui prennent effet à une date ultérieure. Cette capture d’écran montre l’ajout d’une modification à une date future de prix, le prix est temporairement modifié de niveau 1 au niveau 3 pour le mois de septembre uniquement :

 [ ![](in-app-purchase-basics-and-configuration-images/image11.png "Une modification à une date future de prix où le prix est temporairement modifié de niveau 1 au niveau 3 pour le mois de septembre uniquement")](in-app-purchase-basics-and-configuration-images/image11.png)

### <a name="free-products-not-supported"></a>Produits gratuits ne pas pris en charge

Apple a fourni une option gratuite spéciale pour les applications de kiosque, il n’est pas possible de définir un prix zéro (gratuit) pour tous les autres types dans l’application fournisseur. Bien que vous pouvez modifier (ie. inférieure) prix de ventes promotions, vous ne pouvez apporter des achats dans l’application 'gratuits' via iTunes Connect.

### <a name="localization"></a>Localisation

Dans iTunes Connect, vous pouvez entrer un nom et une Description texte différent pour n’importe quel nombre de langues prises en charge. Chaque langage peut être ajouté/modifié dans via une fenêtre contextuelle :

 [ ![](in-app-purchase-basics-and-configuration-images/image12.png "Chaque langage peut être ajouté/modifié dans via une fenêtre contextuelle")](in-app-purchase-basics-and-configuration-images/image12.png)   
   
   
   
 Lorsque vous affichez des informations sur les produits dans votre application, le texte localisé est disponible pour vous permettent d’afficher via StoreKit. L’affichage de la devise doit également être localisé pour afficher le symbole correct et la mise en forme décimale – mise en forme est couvert plus loin dans le document.

### <a name="app-store-review"></a>Révision de l’App Store

Identique à celui des applications : chaque produit est examiné par Apple avant d’être autorisé à accéder à la vente. Les produits peuvent être rejetés contenu inapproprié dans le nom ou la Description, ou Apple peut décider que vous avez choisi le type de produit incorrect (par exemple). vous avez créé un livre ou un problème magazine mais utilisé le type de produit consommable). Évaluations de produits peuvent prendre une révision d’application.

La première fois qu’une application est envoyée dans l’application d’achat activé (si elle est une nouvelle application ou la fonctionnalité a été ajoutée à un autre existant) vous devez également choisir des produits à soumettre avec lui. Le portail de se connecter iTunes vous invitera à cela, comme indiqué dans cette capture d’écran :

 [ ![](in-app-purchase-basics-and-configuration-images/image13.png "Le portail de se connecter iTunes vous invitera à soumettre également certains produits")](in-app-purchase-basics-and-configuration-images/image13.png)   
   
   
   
 L’application et les achats dans l’application sont examinées ensemble, afin que tous les obtient à la fois approuvées (de sorte que cette application ne passe dans le magasin des produits approuvés !).

Une fois que la première version avec fonction d’achat de dans l’application a été approuvée, vous pouvez ajouter d’autres produits et les envoyer pour révision à tout moment. Vous pouvez également choisir d’envoyer une nouvelle version, ainsi que des produits du fournisseur dans l’application spécifique, à l’aide de la **détails de la Version** page comme l’indique l’invite de commandes.

Reportez-vous à la [directives de révision App Store](https://developer.apple.com/appstore/guidelines.html) pour plus d’informations.

 [Partie 2 : vue d’ensemble du Kit Store et les informations de produit de la récupération des paramètres](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
