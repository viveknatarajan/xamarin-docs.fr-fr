---
title: Transactions et la vérification dans Xamarin.iOS
description: Ce document explique comment autoriser pour la restauration de leurs achats passés dans une application Xamarin.iOS. Il aborde également les méthodes permettant de sécuriser les achats et produits assurée par le serveur.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 83f5fd233c004271169a4d00d0a65e70aa925b95
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117653"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Transactions et la vérification dans Xamarin.iOS

## <a name="restoring-past-transactions"></a>Restaurer les Transactions passées

Si votre application prend en charge les types de produits qui peuvent être restaurées, vous devez inclure certains éléments d’interface utilisateur pour permettre aux utilisateurs de restaurer ces achats.
Cette fonctionnalité permet à un client pour ajouter le produit à des appareils supplémentaires ou pour restaurer le produit sur le même appareil après être nettoyées ou la suppression et la réinstallation de l’application. Les types de produits suivants peuvent être restaurées :

-  Produits non consommables
-  Abonnements renouvelables automatiquement
-  Abonnements gratuits

Le processus de restauration doit mettre à jour les enregistrements vous conservez sur l’appareil pour répondre à vos produits. Le client peut choisir de restaurer à tout moment, sur leurs appareils. Le processus de restauration envoie à nouveau toutes les transactions précédentes pour cet utilisateur ; le code d’application doit ensuite déterminer l’action à entreprendre avec ces informations (par exemple, la vérification s’il existe déjà un enregistrement de cet achat sur l’appareil et si ne pas, créez un enregistrement de l’achat et l’activation de produit pour l’utilisateur).

### <a name="implementing-restore"></a>Implémentation de restauration

L’interface utilisateur **restaurer** bouton appelle la méthode suivante, ce qui déclenche RestoreCompletedTransactions sur le `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit envoie la demande de restauration pour les serveurs d’Apple en mode asynchrone.   
   
Étant donné que le `CustomPaymentObserver` est inscrit en tant qu’un observateur de la transaction, il recevra les messages lorsque les serveurs Apple répondent. La réponse contiendra toutes les transactions que cet utilisateur effectuées dans cette application (sur tous leurs appareils). Le code parcourt chaque transaction, détecte l’état de restauration et les appels de la `UpdatedTransactions` méthode pour le traiter comme indiqué ci-dessous :

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
   
Le code de possible la plus simple pour restaurer une transaction donnée dans l’exemple effectue les mêmes actions que quand un achat a lieu, à ceci près que le `OriginalTransaction` propriété est utilisée pour accéder à l’ID de produit :

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

Une implémentation plus sophistiquée peut vérifier autres `transaction.OriginalTransaction` propriétés, telles que le nombre de date et d’accusé de réception d’origine. Ces informations seront utiles pour certains types de produits (tels que les abonnements).

#### <a name="restore-completion"></a>Restaurer la saisie semi-automatique

Le `CustomPaymentObserver` possède deux méthodes supplémentaires qui seront appelées par StoreKit lorsque le processus de restauration est terminée (correctement ou avec un échec), illustré ci-dessous :

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

Les deux exemples dans ce document utilise `NSUserDefaults` pour effectuer le suivi des achats :   
   
 **Consommables** – le solde de crédit achats est une simple `NSUserDefaults` valeur entière qui est incrémenté à chaque achat.   
   
 **Non-consommables** – chaque achat de filtre photo est stocké sous la forme d’une paire clé-valeur dans `NSUserDefaults`.

À l’aide de `NSUserDefaults` conserve l’exemple de code simple, mais n’offre pas une solution très sécurisée comme il est possible pour les utilisateurs préoccupés par techniquement mettre à jour les paramètres (ignorer le mécanisme de paiement).   
   
Remarque : Les applications réelles doivent adopter un mécanisme sécurisé pour le stockage acheté le contenu qui n’est pas soumis aux manipulations d’utilisateur. Cela peut impliquer le chiffrement et/ou d’autres techniques, notamment l’authentification de serveur distant.   
   
 Le mécanisme doit également être conçu pour tirer parti des fonctionnalités intégrées de sauvegarde et de restauration d’iOS, iTunes et iCloud. Cela garantit qu’une fois un utilisateur restaure une sauvegarde de leurs achats précédentes peut être immédiatement.   
   
Consultez Secure codage Guide d’Apple pour obtenir des instructions plus spécifique à iOS.

## <a name="receipt-verification-and-server-delivered-products"></a>Vérification de la réception et les produits assurée par le serveur

Les exemples de ce document jusqu'à présent ont est composé uniquement de l’application de communiquer directement avec les serveurs App Store d’effectuer des transactions d’achat, qui déverrouiller les fonctionnalités ou fonctions déjà codées dans l’application.   
   
Apple fournit un niveau supplémentaire de sécurité d’achat en autorisant les reçus d’achat être vérifié en toute indépendance par un autre serveur, ce qui peut être utile pour valider une demande avant de remettre le contenu numérique dans le cadre de l’achat (par exemple un livre numérique ou Magazine).   
   
 **Produits intégrés** – comme dans les exemples dans ce document, le produit acheté existe en tant que fonctionnalités fournies avec l’application. Un achat dans l’application permet à l’utilisateur à accéder à la fonctionnalité.
ID de produit sont codées en dur.   
   
 **Produits assurée par le serveur** – le produit se compose d’un contenu téléchargeable qui est stocké sur un serveur distant jusqu'à ce qu’une transaction réussie entraîne le téléchargement de contenu.
Exemples peuvent inclure la documentation ou problèmes magazines. ID de produit proviennent généralement un serveur externe (où le contenu du produit est également hébergé). Les applications doivent implémenter une solution robuste d’enregistrement lorsqu’une transaction est terminée, afin que si l’échec de téléchargement de contenu peut être tentée de nouveau sans confusion pour l’utilisateur.

### <a name="server-delivered-products"></a>Produits assurée par le serveur

Certains produit son contenu, telles que la documentation et magazines (ou un niveau de jeu) doit être téléchargés à partir d’un serveur distant pendant le processus d’achat. Cela signifie que d’un serveur supplémentaire est nécessaire pour stocker et fournir le contenu du produit après son achat.

#### <a name="getting-prices-for-server-delivered-products"></a>Obtenir les prix pour les produits assurée par le serveur

Étant donné que les produits sont transmises à distance, il est également possible d’ajouter d’autres produits au fil du temps (sans la mise à jour le code d’application), telles que l’ajout de plus de la documentation ou de nouveaux problèmes d’un magazine. Afin que l’application peut détecter ces produits actualités et les afficher à l’utilisateur, le serveur supplémentaire doit stocker et fournir ces informations.   
   
[![](transactions-and-verification-images/image38.png "Obtenir les prix pour les produits assurée par le serveur")](transactions-and-verification-images/image38.png#lightbox)   
   
1. Informations sur les produits doivent être stockées dans plusieurs endroits : sur votre serveur et dans iTunes Connect. En outre, chaque produit aura des fichiers de contenu associés. Ces fichiers seront transmises après un achat réussi.   
   
2. Lorsque l’utilisateur souhaite acheter un produit, l’application doit déterminer quels produits sont disponibles. Ces informations peuvent être mises en cache, mais il doivent être remises à partir d’un serveur distant où se trouve la liste principale des produits.   
   
3. Le serveur renvoie une liste d’ID de produit pour l’application à analyser.   
   
4. L’application puis détermine laquelle de ces ID de produit à envoyer à StoreKit pour récupérer les prix et les descriptions.   
   
5. StoreKit envoie la liste des ID de produit pour les serveurs d’Apple.   
   
6. Les serveurs iTunes répondent avec des informations de produit valide (description et prix actuel).   
   
7. L’application `SKProductsRequestDelegate` est passé les informations de produit pour l’affichage à l’utilisateur.

#### <a name="purchasing-server-delivered-products"></a>Achat de produits assurée par le serveur

Étant donné que le serveur à distance nécessite un moyen de valider qu’une demande de contenu est valide (ie. n’a été payé), les informations de réception ne sont transmises pour l’authentification. Le serveur distant transfère ces données à iTunes pour la vérification et, en cas de réussite, inclut le contenu du produit dans la réponse à l’application.   
   
 [![](transactions-and-verification-images/image39.png "Achat de produits assurée par le serveur")](transactions-and-verification-images/image39.png#lightbox)   
   
1. L’application ajoute un `SKPayment` à la file d’attente. Si nécessaire l’utilisateur sera invité à entrer leur ID Apple et invité à confirmer le paiement.   
   
2. StoreKit envoie la demande au serveur pour traitement.   
   
3. Lorsque la transaction est terminée, le serveur répond avec un accusé de réception de transaction.   
   
4. Le `SKPaymentTransactionObserver` sous-classe reçoit l’accusé de réception et le traite. Étant donné que le produit doit être téléchargé à partir d’un serveur, l’application lance une demande réseau au serveur distant.   
   
5. La demande de téléchargement est accompagnée par les données de réception afin que le serveur distant peut vérifier qu’il est autorisé à accéder au contenu. Client réseau de l’application attend une réponse à cette demande.   
   
6. Lorsque le serveur reçoit une demande de contenu, il analyse les données de réception et envoie une demande directement aux serveurs iTunes pour vérifier l’accusé de réception est pour une transaction valide. Le serveur doit utiliser une logique pour déterminer s’il faut envoyer la demande à l’URL de production ou bac à sable. Apple suggère toujours à l’aide de l’URL de production et en basculant vers le bac à sable si votre réception état 21007 (réception d’un bac à sable envoyée au serveur de production). Reportez-vous à Apple [Guide de programmation de la Validation de réception](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) pour plus d’informations.
   
7. iTunes vérifie l’accusé de réception et retourne un état de zéro si elle est valide.   
   
8. Le serveur attend de réponse des iTunes. S’il reçoit une réponse valide, le code devrait localiser le fichier de contenu produit associé à inclure dans la réponse à l’application.   
  
9. L’application reçoit et traite la réponse, l’enregistrement du contenu du produit au système de fichiers de l’appareil.   
   
10. L’application active le produit et appelle ensuite de StoreKit `FinishTransaction`. L’application peut afficher puis éventuellement le contenu acheté (par exemple, afficher la première page d’un livre achetée ou d’un problème magazine).

Une autre implémentation pour les fichiers de contenu de produit de très grande taille peut impliquer simplement stocker la réception de la transaction à l’étape 9 de # afin de la transaction peut être effectuée rapidement et en fournissant une interface utilisateur pour l’utilisateur à télécharger le contenu réel de produit à un moment ultérieur. La demande de téléchargement suivantes peut renvoyer l’accusé de réception stockée pour accéder au fichier de contenu produit requis.

### <a name="writing-server-side-receipt-verification-code"></a>Écriture de Code de vérification de réception du côté serveur

Validation d’un accusé de réception dans le code côté serveur peut être effectuée avec une simple requête HTTP POST demande/réponse qui comprend les étapes #5 à 8 # dans le diagramme de flux de travail.   
   
Extraire le `SKPaymentTansaction.TransactionReceipt` propriété dans l’application. Il s’agit de données qui doivent être envoyé à iTunes pour la vérification (étape #5).

Au format Base64 les données de réception de transaction (soit à l’étape #5 ou #6).

Créer une charge JSON simple comme suit :

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST le JSON sur [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) pour la production ou [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) pour le test.   
   
 La réponse JSON contient les clés suivantes :

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Un état de zéro indique un accusé de réception valide. Votre serveur peut continuer à remplir le contenu du produit acheté. La clé de l’accusé de réception contient un dictionnaire JSON avec les mêmes propriétés que le `SKPaymentTransaction` objet qui a été reçu par l’application, pour que le code du serveur puisse interroger ce dictionnaire pour récupérer des informations telles que le product_id et la quantité de l’achat.

Consultez d’Apple [Guide de programmation de la Validation de réception](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) documentation pour plus d’informations.
