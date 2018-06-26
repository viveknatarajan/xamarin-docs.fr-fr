---
title: Vérification de Xamarin.iOS et de transactions
description: Ce document décrit la procédure permettre la restauration de leurs achats passés dans une application Xamarin.iOS. Elle explique également comment sécuriser des achats et les produits de remise de serveur.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: ac24c70ed16c6439480903b807add38fb388b4dd
ms.sourcegitcommit: 0be3d10bf08d1f76eab109eb891ed202615ac399
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321387"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Vérification de Xamarin.iOS et de transactions

## <a name="restoring-past-transactions"></a>Restaurer les Transactions passées

Si votre application prend en charge les types de produits qui peuvent être restaurées, vous devez inclure certains éléments d’interface utilisateur pour permettre aux utilisateurs de restaurer ces achats.
Cette fonctionnalité permet à un client pour ajouter le produit à des périphériques supplémentaires ou pour restaurer le produit sur le même périphérique après être nettoyées ou la suppression et la réinstallation de l’application. Types de produits suivants peuvent être restaurées :

-  Produits non consommable
-  Abonnements renouvelables automatiquement
-  Abonnements gratuits

Le processus de restauration doit mettre à jour les enregistrements vous conserver sur l’appareil pour répondre à vos produits. Le client peut choisir de restaurer à tout moment, sur leurs appareils. Le processus de restauration envoie à nouveau toutes les transactions précédentes pour cet utilisateur ; le code d’application doit ensuite déterminer l’action à effectuer avec ces informations (par exemple, vérifier s’il existe déjà un enregistrement de ce fournisseur sur l’appareil et si ne pas, créez un enregistrement de l’achat et l’activation de produit pour l’utilisateur).

### <a name="implementing-restore"></a>Mise en œuvre de la restauration

L’interface utilisateur **restaurer** appelle la méthode suivante, qui déclenche RestoreCompletedTransactions sur la `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit envoie la demande de restauration pour les serveurs Apple en mode asynchrone.   
   
Étant donné que le `CustomPaymentObserver` est inscrit en tant qu’un observateur de la transaction, il reçoit les messages lorsque les serveurs Apple répondent. La réponse contient toutes les transactions qu'utilisateur effectuées dans cette application (sur tous leurs appareils). Le code parcourt de chaque transaction, détecte l’état de restauré et les appels de la `UpdatedTransactions` méthode pour le traiter comme indiqué ci-dessous :

```csharp
// called when the transaction status is updated
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
       case SKPaymentTransactionState.Purchased:
          theManager.CompleteTransaction(transaction);
           break;
       case SKPaymentTransactionState.Failed:
          theManager.FailedTransaction(transaction);
           break;
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

Si aucun produit pouvant être restaurée pour l’utilisateur, `UpdatedTransactions` n’est pas appelée.   
   
Le code de possible la plus simple pour restaurer une transaction donnée dans l’exemple effectue les mêmes actions que lorsqu’un achat intervient, à ceci près que le `OriginalTransaction` propriété est utilisée pour accéder à l’ID de produit :

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

Une implémentation plus sophistiquée peut vérifier des autres `transaction.OriginalTransaction` propriétés, telles que le nombre de date et d’accusé de réception d’origine. Ces informations seront utiles pour certains types de produits (tels que les abonnements).

#### <a name="restore-completion"></a>Restaurer la saisie semi-automatique

Le `CustomPaymentObserver` a deux méthodes supplémentaires StoreKit seront appelées lorsque le processus de restauration est terminée (correctement ou avec un échec), illustré ci-dessous :

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

Dans l’exemple de ces méthodes ne rien font, toutefois, une application réelle peut choisir d’implémenter un message à l’utilisateur ou d’autres fonctionnalités.

## <a name="securing-purchases"></a>Sécurisation des achats

Les deux exemples dans ce document Utilisez `NSUserDefaults` pour suivre les achats :   
   
 **Consommables** – le solde du crédit achats est un simple `NSUserDefaults` valeur entière qui est incrémenté à chaque achat.   
   
 **Non-consommables** – chaque bon de filtre photo est stockée sous forme de paire clé-valeur dans `NSUserDefaults`.

À l’aide de `NSUserDefaults` conserve l’exemple de code simple, mais n’offre pas une solution très sûre qu’il est possible pour les utilisateurs préoccupés par techniquement mettre à jour les paramètres (ignorer le mécanisme de paiement).   
   
Remarque : Les applications réelles doivent adopter un mécanisme sécurisé pour le stockage acheté le contenu qui n’est pas soumis à la falsification de l’utilisateur. Cela peut impliquer le chiffrement et/ou d’autres techniques, y compris l’authentification de serveur distant.   
   
 Le mécanisme doit également être conçu pour tirer parti des fonctionnalités de sauvegarde et de restauration intégrées d’iOS, iTunes et iCloud. Cela garantit qu’une fois un utilisateur restaure une sauvegarde de leurs achats précédentes sera immédiatement disponibles.   
   
Consultez Secure codage Guide d’Apple pour plus d’instructions spécifiques à iOS.

## <a name="receipt-verification-and-server-delivered-products"></a>Vérification de la réception et les produits de remise de serveur

Les exemples de ce document jusqu'à présent ont est composé uniquement de l’application de communiquer directement avec les serveurs App Store d’effectuer des transactions d’achat, qui déverrouiller des fonctionnalités ou capacités déjà codées dans l’application.   
   
Apple offre un niveau supplémentaire de sécurité de l’achat en autorisant reçus à être vérifiés de façon indépendante par un autre serveur, ce qui peut être utile pour valider une demande avant de remettre le contenu numérique en tant que partie d’un bon d’achat (par exemple un livre numérique ou Magazine).   
   
 **Produits intégrés** – comme exemples dans ce document, le produit acheté existe en tant que fonctionnalité fournie avec l’application. Un achat dans l’application permet à l’utilisateur pour accéder à la fonctionnalité.
ID de produit sont codées en dur.   
   
 **Les produits serveur remis** – le produit est constitué de contenu téléchargeable qui est stocké sur un serveur distant jusqu'à ce qu’une transaction réussie entraîne le téléchargement du contenu.
La documentation ou problèmes magazines sont des exemples. ID de produit proviennent généralement d’un serveur externe (où le contenu du produit est également hébergé). Les applications doivent implémenter une solution fiable d’enregistrement lorsqu’une transaction est terminée, afin que si l’échec de téléchargement de contenu puisse être tentée de nouveau sans confusion de l’utilisateur.

### <a name="server-delivered-products"></a>Produits remis de serveur

Certains produits son contenu, telles que la documentation et magazines (ou un niveau de jeu) doivent être téléchargés à partir d’un serveur distant au cours du processus d’achat. Cela signifie que d’un serveur supplémentaire est requis pour stocker et distribuer le contenu de produit après son achat.

#### <a name="getting-prices-for-server-delivered-products"></a>Mise en route de prix pour les produits remis de serveur

Étant donné que les produits sont livrés à distance, il est également possible d’ajouter d’autres produits au fil du temps (sans la mise à jour le code d’application), telles que l’ajout de plus de la documentation ou de nouveaux problèmes d’un magazine. Pour que l’application peut détecter ces produits de news et les afficher à l’utilisateur, le serveur supplémentaire doit stocker et fournir ces informations.   
   
[![](transactions-and-verification-images/image38.png "Mise en route de prix pour les produits remis de serveur")](transactions-and-verification-images/image38.png#lightbox)   
   
1. Informations sur les produits doivent être stockées dans plusieurs emplacements : sur votre serveur et dans iTunes Connect. En outre, chaque produit aura des fichiers de contenu associés. Ces fichiers seront transmises après un achat réussi.   
   
2. Lorsque l’utilisateur souhaite acheter un produit, l’application doit déterminer quels produits sont disponibles. Ces informations peuvent être mises en cache, mais il doivent être remises à partir d’un serveur distant où se trouve la liste principale des produits.   
   
3. Le serveur retourne une liste d’ID de produit pour l’application d’analyse.   
   
4. L’application détermine laquelle de ces ID de produit à envoyer à StoreKit pour récupérer le prix et les descriptions.   
   
5. StoreKit envoie la liste des ID de produit pour les serveurs Apple.   
   
6. Les serveurs iTunes répondent avec des informations de produit valide (description et le prix actuel).   
   
7. L’application `SKProductsRequestDelegate` reçoit les informations de produit pour l’affichage à l’utilisateur.

#### <a name="purchasing-server-delivered-products"></a>Achat de produits remis de serveur

Étant donné que le serveur à distance nécessite un moyen de valider qu’une requête de contenu est valide (c.-à-d. a été payé), les informations de réception sont passées pour l’authentification. Le serveur distant transfère ces données à iTunes pour la vérification et, en cas de réussite, inclut le contenu du produit dans la réponse à l’application.   
   
 [![](transactions-and-verification-images/image39.png "Achat de produits remis de serveur")](transactions-and-verification-images/image39.png#lightbox)   
   
1. L’application ajoute une `SKPayment` à la file d’attente. Si nécessaire l’utilisateur sera invité à entrer leur ID Apple et invité à confirmer le paiement.   
   
2. StoreKit envoie la demande au serveur pour traitement.   
   
3. Lorsque la transaction est terminée, le serveur répond avec un accusé de réception de transaction.   
   
4. Le `SKPaymentTransactionObserver` sous-classe reçoit l’accusé de réception et le traite. Étant donné que le produit doit être téléchargé à partir d’un serveur, l’application lance une demande réseau au serveur distant.   
   
5. La demande de téléchargement est accompagnée par les données de réception afin que le serveur à distance peut vérifier qu’il n’est autorisé à accéder au contenu. Client réseau de l’application attend une réponse à cette demande.   
   
6. Lorsque le serveur reçoit une demande de contenu, il analyse les données de réception et envoie une demande directement aux serveurs iTunes pour vérifier l’accusé de réception est effectuée pour une transaction valide. Le serveur doit utiliser une logique pour déterminer s’il faut envoyer la demande à l’URL de production ou bac à sable. Apple suggère toujours à l’aide de l’URL de production et de basculement vers le bac à sable si votre état 21007 (réception d’un bac à sable envoyée au serveur de production). Reportez-vous à Apple [Guide de programmation de la Validation de réception](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) pour plus d’informations.
   
7. iTunes vérifie l’accusé de réception et retourne un état de zéro si elle est valide.   
   
8. Le serveur attend de réponse des iTunes. S’il reçoit une réponse valide, le code doit rechercher le fichier de contenu produit associé à inclure dans la réponse à l’application.   
  
9. L’application reçoit et traite la réponse, l’enregistrement du contenu du produit au système de fichiers du périphérique.   
   
10. Active le produit de l’application et appelle ensuite de StoreKit `FinishTransaction`. L’application peut afficher puis éventuellement le contenu acheté (par exemple, le premier d’afficher la page d’un livre acheté ou d’un problème magazine).

Une autre implémentation des fichiers de contenu produit très volumineux peut impliquer simplement de stocker la réception de la transaction à l’étape 9 de # afin que la transaction peut être effectuée rapidement et en fournissant une interface utilisateur pour l’utilisateur à télécharger le contenu réel de produit à un moment ultérieur. La demande de téléchargement suivantes peut renvoyer l’accusé de réception stockée pour accéder au fichier de contenu de produit requis.

### <a name="writing-server-side-receipt-verification-code"></a>L’écriture de Code de vérification d’accusé de réception côté serveur

Validation d’un accusé de réception dans le code côté serveur peut faire avec une simple requête HTTP POST demande/réponse qui comprend les étapes #5 à 8 # dans le diagramme de flux de travail.   
   
Extraire le `SKPaymentTansaction.TransactionReceipt` propriété dans l’application. Il s’agit de données qui doit être envoyé à iTunes pour la vérification (étape #5).

Base64-encoder les données de réception de transaction (soit à l’étape #5 ou #6).

Créer une charge JSON simple comme suit :

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST JSON à [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) pour la production ou [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) pour le test.   
   
 La réponse JSON contient les clés suivantes :

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Un état de zéro indique un accusé de réception valide. Votre serveur peut continuer à remplir le contenu du produit acheté. La clé de l’accusé de réception contient un dictionnaire JSON avec les mêmes propriétés que le `SKPaymentTransaction` objet qui a été reçu par l’application, pour que le code du serveur permettre interroger ce dictionnaire pour récupérer des informations telles que le product_id et la quantité de l’achat.

Consultez d’Apple [Guide de programmation de la Validation de réception](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) documentation pour plus d’informations.
