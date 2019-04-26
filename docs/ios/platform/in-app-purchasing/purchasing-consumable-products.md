---
title: Achat de produits consommables dans Xamarin.iOS
description: Ce document décrit les produits consommables dans Xamarin.iOS. Produits consommables sont des éléments à usage unique de fonctionnalités telles que la devise de dans le jeu.
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: b55465a700974e0ce5ceb8893d96311d920e04ae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61366574"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Achat de produits consommables dans Xamarin.iOS

Produits consommables sont le plus simple à implémenter, car il n’existe aucune exigence 'restore'. Elles sont utiles pour des produits comme devise de jeu ou une partie à usage unique des fonctionnalités. Les utilisateurs peuvent acheter nouveau de produits consommables over-et-over à nouveau.

## <a name="built-in-product-delivery"></a>Remise de produit intégrée

L’exemple de code qui accompagne ce document illustre les produits intégrés : les ID de produit sont codées en dur dans l’application, car ils sont étroitement couplés au code permettant de « déverrouille » la fonctionnalité après le paiement. Le processus d’achat peut être visualisé comme suit :   
   
[![La visualisation de processus d’achat](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Le workflow de base est :   
   
 1. L’application ajoute un `SKPayment` à la file d’attente. Si nécessaire l’utilisateur sera invité à entrer leur ID Apple et invité à confirmer le paiement.   
   
 2. StoreKit envoie la demande au serveur pour traitement.   
   
 3. Lorsque la transaction est terminée, le serveur répond avec un accusé de réception de transaction.   
   
 4. Le `SKPaymentTransactionObserver` sous-classe reçoit l’accusé de réception et le traite.   
   
 5. Le produit permet à l’application (en mettant à jour `NSUserDefaults` ou tout autre mécanisme), puis appelle de StoreKit `FinishTransaction`.

Il existe un autre type de flux de travail – *Server-Delivered produits* : autrement dit décrits plus loin dans le document (consultez la section *vérification de la réception et les produits Server-Delivered*).

## <a name="consumable-products-example"></a>Exemple de produits consommable

Le [InAppPurchaseSample code](https://developer.xamarin.com/samples/monotouch/StoreKit/) contient un projet appelé *consommables* qui implémente un basic 'dans le jeu currency' (appelé « certain nombre de manipulations crédits »). L’exemple montre comment implémenter les deux produits d’achat dans l’application pour autoriser l’utilisateur à l’acheter comme de nombreux « crédits monkey » qu’ils le souhaitent – dans une application réelle il serait également un moyen de leur dépense !   
   
   
   
 L’application est indiquée dans ces captures d’écran : chaque achat ajoute plus « crédits monkey » au solde de l’utilisateur :   
   
   
   
 [![Chaque achat ajoute des crédits monkey le solde d’utilisateurs](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Les interactions entre les classes personnalisées, StoreKit et l’App Store ressemble à ceci :   
   
   
   
 [![Les interactions entre les classes personnalisées, StoreKit et Store de l’application](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>Méthodes ViewController

En plus des propriétés et méthodes requises pour récupérer des informations de produit, le contrôleur d’affichage nécessite des observateurs de notification supplémentaire à l’écoute des notifications liés à l’achat. Il s’agit simplement `NSObjects` qui sera enregistrée et supprimée dans `ViewWillAppear` et `ViewWillDisappear` respectivement.

```csharp
NSObject succeededObserver, failedObserver;
```

Le constructeur crée également le `SKProductsRequestDelegate` sous-classe ( `InAppPurchaseManager`) qui à son tour de crée et enregistre le `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 La première partie du traitement d’une transaction d’achat dans l’application consiste à gérer l’appuyez sur le bouton lorsque l’utilisateur souhaite acheter quelque chose, comme indiqué dans le code suivant à partir de l’exemple d’application :

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 La deuxième partie de l’interface utilisateur gère la notification que la transaction est réussie, dans ce cas en mettant à jour le solde affiché :

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

La dernière partie de l’interface utilisateur affiche un message si une transaction est annulée pour une raison quelconque. Dans l’exemple de code un message est écrit simplement dans la fenêtre Sortie :

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Outre ces méthodes sur le contrôleur d’affichage, une transaction d’achat de produits consommables requiert également code sur le `SKProductsRequestDelegate` et `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>Méthodes InAppPurchaseManager

L’exemple code implémente un nombre d’acheter les méthodes associées sur la classe InAppPurchaseManager, y compris le `PurchaseProduct` méthode qui crée un `SKPayment` de l’instance et l’ajoute à la file d’attente pour le traitement :

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Ajout du paiement à la file d’attente est une opération asynchrone. L’application reprend le contrôle alors que StoreKit traite la transaction et l’envoie vers les serveurs d’Apple. Il est à ce stade qu’iOS vérifiera l’utilisateur est connecté à l’App Store et l’invite pour un ID Apple et le mot de passe si nécessaire.   
   
   
   
 En supposant que l’utilisateur a été authentifié auprès de l’App Store et n’accepte pas la transaction, le `SKPaymentTransactionObserver` recevra de réponse du StoreKit et appeler la méthode suivante pour répondre à la transaction et finaliser.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

La dernière étape consiste à vous assurer que vous informiez StoreKit que vous avez correctement satisfait la transaction, en appelant `FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

Une fois que le produit est livré, `SKPaymentQueue.DefaultQueue.FinishTransaction` doit être appelée pour supprimer la transaction à partir de la file d’attente de paiement.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Méthodes SKPaymentTransactionObserver (CustomPaymentObserver)

Les appels StoreKit le `UpdatedTransactions` méthode lorsqu’il reçoit une réponse à partir de serveurs Apple et passe un tableau de `SKPaymentTransaction` pour votre code inspecter les objets. La méthode effectue une itération sur chaque transaction et effectue une fonction différente selon l’état des transactions (comme indiqué ici) :

```csharp
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
           default:
               break;
       }
   }
}
```

Le `CompleteTransaction` méthode a été décrite précédemment dans cette section : elle enregistre les détails de l’achat à `NSUserDefaults`, finalise la transaction avec StoreKit et enfin avertit l’interface utilisateur pour mettre à jour.

### <a name="purchasing-multiple-products"></a>Achat de plusieurs produits

Si cela est pertinent dans votre application pour acheter plusieurs produits, utilisez la `SKMutablePayment` classe et définissez le champ Quantity :

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Le code qui gère la transaction terminée doit également interroger la propriété de quantité commandée pour répondre correctement à l’achat :

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Lorsque l’utilisateur achète des quantités multiples, l’alerte de confirmation StoreKit reflète la quantité, le prix unitaire et le prix total, qu'ils sont facturés, comme illustré dans la capture d’écran suivante :

[![Un achat confirmé](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Gestion des pannes du serveur

Achats dans l’application nécessitent une connexion réseau pour StoreKit communiquer avec les serveurs d’Apple. Si une connexion réseau n’est pas disponible, puis dans l’application d’achat n’est pas disponible.

### <a name="product-requests"></a>Demandes de produit

Si le réseau n’est pas disponible lors de la création d’un `SKProductRequest`, le `RequestFailed` méthode de la `SKProductsRequestDelegate` sous-classe ( `InAppPurchaseManager`) est appelé, comme indiqué ci-dessous :

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

ViewController est à l’écoute pour la notification, puis affiche un message dans les boutons d’achat :

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Car une connexion réseau peut être temporaire sur les appareils mobiles, applications peuvent à surveiller l’état de réseau à l’aide de l’infrastructure SystemConfiguration et nouvelle tentative quand une connexion réseau est disponible. Consultez d’Apple ou qui l’utilise.

### <a name="purchase-transactions"></a>Transactions d’achat

La file d’attente de paiement StoreKit stockera et vers l’avant achat demande si possible, afin de l’effet d’une panne réseau varient en fonction lorsque le réseau a échoué pendant le processus d’achat.   
   
   
   
 Si une erreur se produit pendant une transaction, le `SKPaymentTransactionObserver` sous-classe ( `CustomPaymentObserver`) aura le `UpdatedTransactions` méthode appelée et la `SKPaymentTransaction` classe sera dans l’état d’échec.

```csharp
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
           default:
               break;
       }
   }
}
```

Le `FailedTransaction` méthode détecte si l’erreur était dû à une annulation de l’utilisateur, comme illustré ici :

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Même si une transaction échoue, le `FinishTransaction` méthode doit être appelée pour supprimer la transaction à partir de la file d’attente de paiement :

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

L’exemple de code envoie une notification que ViewController puisse l’afficher un message. Les applications ne doivent pas montrer un autre message si l’utilisateur a annulé la transaction. Autres codes d’erreur qui peuvent se produire sont les suivantes :

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Restrictions de traitement

Le **Paramètres > Général > Restrictions** fonctionnalité d’e/s permet aux utilisateurs de verrouiller certaines fonctionnalités de leur appareil.   
   
   
   
 Vous pouvez interroger si l’utilisateur est autorisé à effectuer des achats dans l’application via le `SKPaymentQueue.CanMakePayments` (méthode). Si la valeur renvoyée est false puis l’utilisateur ne peut pas accéder aux achats dans l’application. StoreKit s’affiche automatiquement un message d’erreur à l’utilisateur si un achat est tenté. En vérifiant cette valeur votre application peut à la place masquer les boutons de l’achat ou exécute une autre action pour aider l’utilisateur.   
   
   
   
 Dans le `InAppPurchaseManager.cs` fichier la `CanMakePayments` méthode encapsule la fonction StoreKit comme suit :

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Pour tester cette méthode, utilisez la **Restrictions** fonctionnalité d’e/s pour désactiver **achats dans l’application**:   
   
   
   
 [![Utilisez la fonctionnalité de Restrictions d’e/s pour désactiver les achats dans l’application](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Cet exemple de code à partir de `ConsumableViewController` réagit à `CanMakePayments` retournant la valeur false en affichant **AppStore désactivé** texte sur les boutons désactivés.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

L’application ressemble lors la **achats dans l’application** fonctionnalité est limitée : les boutons d’achat sont désactivés.   
   
   
   
 [![L’application ressemble à ceci lorsque les achats In-App fonctionnalité est limitée à l’achat de boutons sont désactivés](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Informations sur les produits peuvent toujours être demandées quand `CanMakePayments` étant défini sur false, l’application peut toujours récupérer et afficher les prix. Cela signifie que si nous avons supprimé le `CanMakePayments` vérification à partir du code, les boutons d’achat seront quand même être actif, toutefois, lorsqu’un achat est tenté l’utilisateur verra un message qui **achats dans l’application ne sont pas autorisés** (générés par StoreKit Lorsque la file d’attente de paiement est accessible) :   
   
   
   
 [![Achats dans l’application ne sont pas autorisés.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Applications réelles peuvent adopter une approche différente à la gestion de la restriction, telles que masquer les boutons complètement et peut-être offre un message plus détaillé que l’alerte StoreKit affiche automatiquement.

