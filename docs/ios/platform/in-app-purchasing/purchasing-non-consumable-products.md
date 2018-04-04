---
title: Les produits Non-consommable d’achat
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0a581dc222e43f8d4742bd52dc56dc691449a8f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-non-consumable-products"></a>Les produits Non-consommable d’achat

Les produits non consommable 'appartiennent » par le client. Il est courant qu’ils seront ont toujours accès à ceux-ci, même si leur appareil est perdu/volé ou acheter un nouveau. Ils sont utiles pour la documentation, des problèmes magazines, niveaux de jeu, filtres photo, « fonctionnalités pro », etc. Une fois qu’un utilisateur a acheté un produit non-consommable, ils n’ont jamais à payer à nouveau. Si votre code accidentellement leur permet d’essayer, StoreKit affiche un message qui a déjà été acheté.

## <a name="non-consumable-products-sample"></a>Les produits non consommable exemple

Le [InAppPurchaseSample code](https://developer.xamarin.com/samples/monotouch/StoreKit/) contient un projet appelé *NonConsumables*. L’exemple de code montre comment implémenter des produits non-consommable par exemple à l’aide de filtres de photo. Une fois que vous avez acheté un filtre vous pouvez l’appliquer à la photo indéfiniment. Jamais, vous devez à nouveau l’acheter.   
   
   
   
 Le processus d’achat est indiqué dans cette série de captures d’écran : le **acheter** bouton devient le bouton d’activation de la fonctionnalité :   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Le processus d’achat est indiqué dans cette série de captures d’écran")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Le processus d’achat est identique à un produit consommable ; la principale différence est en mode de suivi de l’achat dans le code d’application. Dans cet exemple, que le bouton acheter est uniquement disponible si le produit n’a pas déjà été acheté, sinon le bouton Active la fonctionnalité lui-même.   
   
   
   

Le diagramme suivant illustre les interactions entre les classes et le serveur App Store pour effectuer un achat non-consommable produit :   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Les interactions entre les classes et le serveur App Store pour effectuer un produit non-consommable d’achat")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 La principale différence de l’exemple consommable est qu’une fois l’achat terminé l’interface utilisateur est mise à jour pour empêcher l’achat de nouveau. Dans cet exemple, la notification de succès de la transaction met à jour l’interface utilisateur afin que les **acheter** bouton est converti en un bouton qui active la fonctionnalité de lui-même.

## <a name="re-purchasing-non-consumable-products"></a>Produits Non-consommable ré-achats

Votre code doit normalement masquer ou de le réaffecter un bouton achat une fois que le produit a été correctement acheté, pour empêcher l’utilisateur de tenter à nouveau d’acheter le produit. L’exemple d’application fait cela en modifiant le **acheter** bouton dans le bouton qui permet à l’exemple de filtre photo de fonctionner.   
   
   
   
 Il existe des situations où une application ne peut pas déterminer si un produit non-consommable a déjà été acheté :

-  Si une application est supprimée et réinstallée sur un périphérique, tous les enregistrements d’achat sera disparus (sauf si / jusqu'à ce que l’utilisateur effectue une restauration de sauvegarde). 
-  Si l’utilisateur a l’application installée sur les appareils de deux (ou plus) et effectue un achat sur l’un des périphériques. Les autres périphériques continue d’afficher les produits disponibles à l’achat. 
-  Si un client essaie de nouveau acheter un produit de non-consommable dans ces situations, l’App Store effectuera le produit à nouveau sans frais. L’interface utilisateur s’affiche initialement pour effectuer un achat (par exemple, une alerte de confirmation s’affiche et l’ID Apple sera nécessaire) toutefois l’utilisateur s’affiche un message leur indiquant que le produit a déjà été acheté.  
   
   
   
 Le chemin d’accès du code dans ce scénario est exactement identique à un achat régulière, les seules différences sont :

-  L’utilisateur ne pas facturé à nouveau pour le produit.
-  Le `SKPaymentTransaction` objet transmis à l’application aura un `OriginalTransaction` propriété qui fait référence à la transaction qui a été générée lorsque le produit a été initialement acheté. 
-  Les applications qui vendent des produits de Non consommable doivent également implémenter de StoreKit **restaurer** fonctionnalité pour aider les utilisateurs à récupérer les achats existants. 
