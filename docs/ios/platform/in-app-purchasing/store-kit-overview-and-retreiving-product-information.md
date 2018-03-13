---
title: "Stocker les informations de produit lors de la récupération et de vue d’ensemble du Kit"
ms.topic: article
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: bafada037f912007201fd5e81b17302b21de3092
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="store-kit-overview-and-retrieving-product-information"></a>Stocker les informations de produit lors de la récupération et de vue d’ensemble du Kit

L’interface utilisateur pour un achat dans l’application est indiqué dans les captures d’écran ci-dessous.
Avant toute transaction se produit, l’application doit récupérer des prix du produit et une description pour l’affichage. Lorsque l’utilisateur appuie sur **acheter**, l’application effectue une demande à StoreKit qui gère la connexion d’ID Apple et la boîte de dialogue de confirmation. La transaction puis réussit, StoreKit notifie le code d’application, qui doit stocker le résultat de la transaction et fournir à l’utilisateur d’accéder à leur achat.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit notifie le code d’application, qui doit stocker le résultat de la transaction et fournir à l’utilisateur d’accéder à leur achat")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Classes

Implémentation des achats dans l’application nécessite les classes suivantes à partir du framework StoreKit :   
   
 **SKProductsRequest** – une demande StoreKit pour les produits approuvés pour les revendre (magasin d’application). Peut être configuré avec un numéro d’ID de produit.

-   **SKProductsRequestDelegate** – déclare des méthodes pour gérer les demandes de produit et les réponses. 
-   **SKProductsResponse** – envoyés vers le délégué à partir de StoreKit (magasin d’application). Contient le SKProducts qui correspond au produit ID envoyées avec la demande. 
-   **SKProduct** – un produit extraite StoreKit (que vous avez configurée dans iTunes Connect). Contient des informations sur le produit, telles que le titre, Description, ID de produit et le prix. 
-   **SKPayment** – créé avec un ID de produit et ajouté à la file d’attente de paiement pour effectuer un achat. 
-   **SKPaymentQueue** – en file d’attente des demandes de paiement pour être envoyée à Apple. Les notifications sont déclenchées à la suite de chaque paiement en cours de traitement. 
-   **SKPaymentTransaction** – représente une transaction (une demande d’achat qui a été traitée par l’App Store et renvoyée à votre application via StoreKit). La transaction peut être acheté, restauré ou échec. 
-   **SKPaymentTransactionObserver** – sous-classe personnalisée qui répond aux événements générés par la file d’attente du paiement StoreKit. 
-   **Les opérations StoreKit sont asynchrones** – après un SKProductRequest est démarré ou un SKPayment est ajouté à la file d’attente, le contrôle soit retourné à votre code. StoreKit appelle les méthodes sur votre sous-classe SKProductsRequestDelegate ou SKPaymentTransactionObserver lorsqu’il reçoit des données à partir des serveurs Apple. 


Le diagramme suivant montre les relations entre les différentes classes de StoreKit (les classes abstraites doivent être implémentées dans votre application) :   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "Les relations entre les différentes classes abstraites de classes de StoreKit doivent être implémentées dans l’application")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 Ces classes sont expliquées plus en détail plus loin dans ce document.

## <a name="testing"></a>Test

La plupart des opérations StoreKit nécessitent un périphérique réel pour le test. Récupération des informations de produit (c'est-à-dire,). prix &amp; description) fonctionne dans le simulateur mais l’achat et les opérations de restauration renvoie une erreur (tel que le Code de FailedTransaction = 5002 une erreur inconnue s’est produite).

Remarque : StoreKit ne fonctionne pas dans le simulateur iOS. Lorsque vous exécutez votre application dans iOS Simulator, StoreKit consigne un avertissement si votre application tente de récupérer la file d’attente de paiement. Le magasin de test doit être effectuée sur des appareils réels.   
   
   
   
 Important : Ne vous connectez pas avec votre compte de test dans l’application de paramètres. Vous pouvez utiliser l’application de paramètres pour l’authentification en dehors de tout compte ID Apple existant, puis vous devez vous attendre à être invité *au sein de votre séquence de In-App Purchase* pour vous connecter à l’aide d’un test ID Apple   
   
   
   

Si vous essayez de vous connecter à dans le magasin réel avec un compte de test, elle sera automatiquement convertie en un ID Apple réel Ce compte n’est plus utilisable pour le test.

Pour tester le code de StoreKit, vous devez le déconnecter de votre compte de test iTunes régulière et la connexion avec un compte de test spéciale (créé dans iTunes Connect) qui est lié à la banque de test. Pour vous déconnecter de la visite du compte actuel **Paramètres > iTunes et App Store** comme indiqué ici :

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "Pour vous déconnecter de la visite du compte actuel paramètres iTunes et App Store")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
puis connectez-vous avec un compte de test *lorsqu’il est demandé par StoreKit au sein de votre application*:



Pour créer des utilisateurs de test dans iTunes Connect cliquent sur **utilisateurs et rôles** dans la page principale.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "Pour créer des utilisateurs de test dans iTunes Connect cliquez sur utilisateurs et rôles dans la page principale")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

Sélectionnez **testeurs de bac à sable**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "En sélectionnant les testeurs de bac à sable")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

La liste des utilisateurs existants s’affiche. Vous pouvez ajouter un nouvel utilisateur ou supprimer un enregistrement existant. Le portail n’effectue pas (actuellement) vous permettent d’afficher ou modifier les existants de tester les utilisateurs, il est recommandé de conserver un enregistrement de bon de chaque utilisateur de test qui est créé (en particulier le mot de passe). Une fois que vous supprimez un utilisateur de test de l’adresse de messagerie ne peut pas être réutilisée pour un autre compte de test.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "La liste des utilisateurs existants s’affiche.")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Nouveaux utilisateurs de test ont des attributs similaires à un ID Apple réel (par exemple, le nom, un mot de passe, question et la réponse). Conservez un enregistrement de tous les détails de l’entrée ici. Le **sélectionner un magasin iTunes** champ détermine la devise et de langue achats dans l’application utilisera quand connecté en tant que cet utilisateur.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "Le champ de sélectionner un magasin iTunes déterminera la devise et la langue pour leurs achats dans l’application de l’utilisateur")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>La récupération des informations de produit

La première étape de vente d’un produit dans l’application achat affiche : récupérer le prix actuel et la description à partir du magasin d’applications pour l’affichage.   
   
   
   
 Quel que soit le type de produits une application vend (consommable, Non consommable ou un type d’abonnement), le processus de récupération des informations de produit pour l’affichage est la même. Le code InAppPurchaseSample qui accompagne cet article contient un projet nommé *consommables* qui montre comment récupérer des informations de production pour l’affichage. Il montre comment :

-  Créer une implémentation de `SKProductsRequestDelegate` et implémenter la `ReceivedResponse` méthode abstraite. L’exemple de code appelle cela la `InAppPurchaseManager` classe. 
-  Vérifiez auprès de StoreKit pour voir si les paiements sont autorisées (à l’aide de `SKPaymentQueue.CanMakePayments` ). 
-  Instancier une `SKProductsRequest` avec l’ID de produit qui ont été définis dans iTunes Connect. Cette opération est effectuée dans l’exemple `InAppPurchaseManager.RequestProductData` (méthode). 
-  Appelez la méthode Start sur le `SKProductsRequest` . Cela déclenche un appel asynchrone pour les serveurs App Store. Le délégué ( `InAppPurchaseManager` ) sera appelée différée avec les résultats. 
-  Celui du délégué ( `InAppPurchaseManager` ) `ReceivedResponse` méthode met à jour l’interface utilisateur avec les données retournées à partir du Store (prix des produits et des descriptions ou messages sur les produits non valides). 

L’interaction globale ressemble à ceci ( **StoreKit** est intégrée pour iOS et le **App Store** représente des serveurs Apple) :

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Graphique lors de la récupération d’informations sur le produit")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Afficher l’exemple des informations de produit

Le [InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *consommables* exemple de code montre comment les informations de produit peuvent être récupérées. L’écran principal de l’exemple affiche des informations pour les deux produits qui sont récupérées à partir du Store :   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "L’écran principal affiche des informations sur les produits extraites de l’App Store")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 L’exemple de code pour récupérer et afficher des informations sur le produit est expliquée plus en détail ci-dessous.


#### <a name="viewcontroller-methods"></a>Méthodes ViewController

La `ConsumableViewController` classe gère l’affichage des prix de deux produits dont les ID produit sont codées en dur dans la classe.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

Au niveau de la classe de niveau de doit être un NSObject déclarés qui sera utilisé pour le programme d’installation un `NSNotificationCenter` Observateur :

```csharp
NSObject priceObserver;
```

Dans la méthode ViewWillAppear l’observateur est créé et affecté à l’aide du centre de notification par défaut :

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 À la fin de la `ViewWillAppear` méthode, appelez le `RequestProductData` méthode pour lancer la requête StoreKit. Une fois que cette demande a été effectuée, StoreKit contacte serveurs Apple pour obtenir les informations et les importer vers votre application en mode asynchrone. Pour cela, le `SKProductsRequestDelegate` sous-classe ( `InAppPurchaseManager`) qui est expliquée dans la section suivante.

```csharp
iap.RequestProductData(products);
```

Le code pour afficher le prix et la description extrait les informations de la SKProduct et lui assigne aux contrôles de UIKit (Notez que nous affichons les `LocalizedTitle` et `LocalizedDescription` – StoreKit résout automatiquement le texte approprié et les coûts en fonction des paramètres de compte de l’utilisateur). Le code suivant appartienne dans la notification que nous avons créés ci-dessus :

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

Enfin, le `ViewWillDisappear` méthode doit garantir l’observateur est supprimé :

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>Méthodes SKProductRequestDelegate (InAppPurchaseManager)

Le `RequestProductData` méthode est appelée lorsque l’application souhaite récupérer le prix du produit et autres informations. Il analyse la collection d’ID de produit dans le type de données approprié, puis crée un `SKProductsRequest` avec ces informations. Appel de la méthode de démarrage provoque une demande réseau à apporter aux serveurs Apple. La demande sera exécuté de façon asynchrone et appelez le `ReceivedResponse` méthode du délégué lorsqu’il se termine avec succès.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS route automatiquement la demande vers la version « bac à sable » ou « production » de l’App Store selon le profil de configuration que l’application est en cours d’exécution avec-donc lorsque vous développez ou testez votre application la demande aura accès à tous les produits configuré dans iTunes Connect (même ceux qui n’ont pas encore été soumis ni approuvé par Apple). Lorsque votre application est en production, les demandes StoreKit renvoie uniquement les informations sur **approuvé** produits.   
   
   
   
 Le `ReceivedResponse` méthode substituée est appelée une fois que les serveurs Apple ont répondu avec des données. Car il est appelé en arrière-plan, le code doit analyser les données valides et une notification pour envoyer les informations de produit à n’importe quel ViewControllers « écoute » pour cette notification. Le code pour collecter des informations de produit valide et envoyer une notification est indiqué ci-dessous :

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

Bien que ne pas indiqué dans le diagramme, le `RequestFailed` méthode doit également être remplacée afin que vous pouvez fournir des commentaires à l’utilisateur au cas où les serveurs de magasin d’applications ne sont pas accessibles (ou une autre erreur se produit). L’exemple de code écrit simplement dans la console, mais une application réelle peut choisir d’interroger `error.Code` propriété et implémenter un comportement personnalisé (par exemple, une alerte à l’utilisateur).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Cette capture d’écran montre l’exemple d’application immédiatement après le chargement (lorsqu’aucune information de produit n’est disponible) :

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "L’exemple d’application immédiatement après le chargement lorsque aucune information de produit est disponible")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Produits non valides

Un `SKProductsRequest` peut également retourner une liste d’ID de produit non valide. Les produits non valides sont généralement retournés à cause d’une des opérations suivantes :   
   
   
   
 **ID de produit a été saisi correctement** – uniquement les ID produit valides sont acceptées.   
   
 **Produit n’a pas été approuvé** – lors du test, tous les produits qui sont désactivées pour la vente doivent être retournées par une `SKProductsRequest`; toutefois en production uniquement les produits approuvé sont retournés.   
   
 **ID d’application n’est pas explicite** – Wildcard application IDs (avec un astérisque) n’autorisent pas l’achat dans l’application.   
   
 **Incorrecte de profil de configuration** : Si vous apportez des modifications à la configuration de votre application dans le portail de configuration (par exemple, l’activation des achats dans l’application), n’oubliez pas de générer de nouveau et utiliser le profil de configuration correct lors de la génération de l’application.   
   
 **contrat de payer des Applications iOS n’est pas en place** – StoreKit fonctionnalités ne fonctionneront pas du tout sauf s’il existe un contrat valid pour votre compte de développeur Apple.   
   
 **Le fichier binaire est dans l’état Rejeté** : si l’état Rejeté (soit par l’équipe de l’App Store, soit par le développeur) est un fichier binaire précédemment soumis StoreKit fonctionnalités ne fonctionnera pas.

Le `ReceivedResponse` méthode dans l’exemple de code génère les produits non valides à la console :

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Affichage des prix localisées

Les niveaux de prix spécifient un prix spécifique pour chaque produit entre tous les magasins d’applications internationales. Pour garantir les prix sont correctement affichées pour chaque devise, utilisez la méthode d’extension (défini dans `SKProductExtension.cs`) au lieu de la propriété de prix de chaque `SKProduct`:

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

Le code qui définit titre du bouton utilise la méthode d’extension comme suit :

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

À l’aide de deux comptes de test iTunes différents (une pour le magasin American) et l’autre pour le japonais magasin des résultats dans les captures d’écran suivantes :   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "Deux iTunes autres test comptabilité de langue des résultats spécifiques")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 Notez que le magasin affecte la langue utilisée pour la devise de prix et les informations de produit, tandis que le paramètre de langue de l’appareil affecte les étiquettes et autre contenu localisé.   
   
   
   
 Rappelez-vous que d’utiliser un autre magasin de test compte, vous devez **se déconnecter** dans les **Paramètres > iTunes et App Store** , puis redémarrez l’application pour se connecter avec un autre compte. Pour modifier la langue de l’appareil accédez à **Paramètres > Général > International > langue**.

