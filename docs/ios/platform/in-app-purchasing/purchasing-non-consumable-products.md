---
title: Achat de produits Non consommables dans Xamarin.iOS
description: Ce document décrit les produits non consommables dans Xamarin.iOS, qui sont des fonctionnalités achetées par un utilisateur qui restent disponibles indéfiniment, quel que soit d’appareil.
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 060403baf8ac28b9b160632a01471b9828735069
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118498"
---
# <a name="purchasing-non-consumable-products-in-xamarinios"></a>Achat de produits Non consommables dans Xamarin.iOS

Les produits non consommables 'appartiennent » par le client. L’attente est qu’ils aura toujours d’y accéder, même si leur appareil est perdu/volé ou acheter un nouveau. Elles sont utiles pour la documentation, magazine datant, niveaux de jeu, les filtres de photo, « fonctionnalités pro », etc. Une fois qu’un utilisateur a acheté un produit non consommables, ils n’ont pas à payer à nouveau. Si votre code accidentellement vous permet de les essayer, StoreKit affiche un message qui a déjà été acquis.

## <a name="non-consumable-products-sample"></a>Exemples de produits non consommables

Le [InAppPurchaseSample code](https://developer.xamarin.com/samples/monotouch/StoreKit/) contient un projet appelé *NonConsumables*. L’exemple de code montre comment implémenter les produits non consommables à l’aide de filtres de photo comme exemple. Une fois que vous avez acheté un filtre vous pouvez l’appliquer à la photo indéfiniment. Jamais, vous devez à nouveau l’acheter.   
   
   
   
 Le processus d’achat est indiqué dans cette série de captures d’écran : le **acheter** bouton devienne le bouton d’activation de la fonctionnalité :   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Le processus d’achat est indiqué dans cette série de captures d’écran")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Le processus d’achat est identique à un produit de consommation ; la principale différence est en mode de suivi de l’achat dans le code d’application. Dans cet exemple, que le bouton acheter est uniquement disponible si le produit n’a pas déjà été acheté, sinon le bouton Active la fonctionnalité elle-même.   
   
   
   

Le diagramme suivant illustre les interactions entre les classes et le serveur App Store pour effectuer un achat non consommables produit :   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Acheter les interactions entre les classes et le serveur App Store pour effectuer un produit non consommables")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 La principale différence de l’exemple consommable est qu’une fois l’achat terminé l’interface utilisateur est mise à jour pour empêcher l’achat de nouveau. Dans cet exemple, la notification d’une transaction réussie met à jour l’interface utilisateur afin que le **acheter** bouton est converti en un bouton qui active la fonctionnalité elle-même.

## <a name="re-purchasing-non-consumable-products"></a>Produits Non consommables ré-achats

Votre code doit normalement masquer ou de le réaffecter un bouton acheter une fois que le produit a été correctement acheté, pour empêcher l’utilisateur d’essayer d’achat du produit à nouveau. L’exemple d’application fait ceci en modifiant le **acheter** bouton dans le bouton qui facilite le travail de l’exemple de filtre de photo.   
   
   
   
 Il existe des situations où une application ne peut pas déterminer si un produit non consommables a déjà été acheté :

-  Si une application est supprimée et réinstallée sur un appareil, tous les enregistrements d’achats auront disparu (sauf si / jusqu'à ce que l’utilisateur effectue une restauration de sauvegarde). 
-  Si l’utilisateur possède l’application installée sur les appareils de deux (ou plus) et effectue un achat sur l’un des appareils. Les autres périphériques continue d’afficher le produit disponible à l’achat. 
-  Si un client tente de nouveau acheter un produit non consommables dans ces situations, l’application Store effectuera le produit sans frais. L’interface utilisateur s’affiche initialement pour effectuer un achat (par exemple, une alerte de confirmation s’affiche et l’ID Apple devront) toutefois l’utilisateur voit alors un message leur indiquant que le produit a déjà été acheté.  
   
   
   
 Le chemin d’accès du code dans ce scénario est exactement identique à un achat standard, les seules différences sont :

-  L’utilisateur ne pas être facturée à nouveau pour le produit.
-  Le `SKPaymentTransaction` objet transmis à l’application aura un `OriginalTransaction` propriété qui fait référence à la transaction qui a été générée lorsque le produit a été initialement acheté. 
-  Les applications qui vendent les produits Non consommables doivent également implémenter du StoreKit **restaurer** fonctionnalité pour aider les utilisateurs à récupérer les achats existants. 
