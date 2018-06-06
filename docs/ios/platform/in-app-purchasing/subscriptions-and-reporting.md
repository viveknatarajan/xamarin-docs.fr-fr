---
title: Abonnements et les rapports dans Xamarin.iOS
description: Ce document décrit non renouvellement des abonnements, abonnements gratuits, abonnements renouvelables automatiquement et à l’aide d’iTunes Connect pour créer des rapports sur ces éléments.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7e0873107a60b48e5ebfd8e159f3bf3b85d02867
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787027"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Abonnements et les rapports dans Xamarin.iOS

## <a name="about-non-renewing-subscriptions"></a>Sur les abonnements Non renouvellement

Renouvellement de non des abonnements sont conçus pour les produits qui représentent la vente d’un service avec une restriction de temps, telles que (d’une semaine pour une application de navigation) ou limitée dans le temps accès à une archive de données.   
   
Principales différences entre les abonnements non renouvellement et d’autres types de produit :

-  La définition de produit dans iTunes Connect n’inclut pas le terme. Le code d’application doit être en mesure de déduire la période de validité de l’ID de produit. 
-  Ils peuvent être achetés plusieurs fois (comme un produit consommable). Les applications sont requis pour gérer le terme expiration et le renouvellement et empêcher l’utilisateur d’acheter des abonnements qui se chevauchent. 
-  Les achats ne sont pas pris en charge par la fonction StoreKit restaurer. Si l’abonnement doit être disponible pour les appareils de tous les un utilisateur, l’application aura concevoir et implémenter cette fonctionnalité conjointement avec un serveur distant. Les applications sont également chargées de sauvegarde de l’état de l’abonnement pour le cas lorsqu’un périphérique est sauvegardé puis restauré à partir de-de sauvegarde. 
-  Vue d’ensemble de l’implémentation
-  Non-renouvellement des abonnements doivent normalement être implémentées à l’aide du flux de travail remis de serveur et gérés comme consommables. 


## <a name="about-free-subscriptions"></a>À propos des abonnements gratuits

Abonnements gratuits permettent aux développeurs de placer un contenu disponible dans les applications kiosque (ils ne peuvent pas être utilisés dans les applications non-kiosque). Une fois qu’un abonnement gratuit est démarré, il sera disponible sur périphériques de tous les utilisateurs. Abonnements gratuits n’expirent jamais ; qu’elles se terminent uniquement lorsque l’application est désinstallée.

### <a name="implementation-overview"></a>Vue d’ensemble de l’implémentation

Abonnements gratuits se comportent beaucoup comme des abonnements renouvelables automatiquement. L’application doit disposer d’un produit d’abonnement gratuit disponible pour « achat » dans iTunes Connect. Lors de l’achat par l’utilisateur, l’achat d’abonnement gratuit doit être validé comme un produit de l’abonnement automatique renouvelable. Transactions d’abonnement gratuit peuvent être restaurées.


## <a name="about-auto-renewable-subscriptions"></a>À propos des abonnements renouvelables automatiquement

Abonnements renouvelables automatiquement sont principalement utilisés dans les applications kiosque. Ils représentent un produit qui accorde l’accès au contenu dynamique pour une période donnée, qui est configuré dans iTunes Connect (définir des périodes allant de 7 jours à 1 an). Abonnements de renouveler automatiquement, l’ID Apple aux utilisateurs de charge à la fin de chaque période d’abonnement, sauf si l’utilisateur choisit à la sortie. Ce type de produit fonctionne bien pour les abonnements magazine ou news, où l’utilisateur obtient l’accès à chaque problème publiée alors que leur abonnement est valide.

### <a name="implementation-overview"></a>Vue d’ensemble de l’implémentation

Renouvelables automatiquement les abonnements doivent être implémentées à l’aide du flux de travail Server-Delivered produits (reportez-vous à la *vérification de la réception et les produits Server-Delivered* section).

#### <a name="shared-secret"></a>Secret partagé

Le Secret partagé de fournisseur In-App doit être utilisé dans la demande JSON lors de la vérification des abonnements renouvelables automatiquement sur votre serveur. Le secret partagé est créé/accessibles via iTunes Connect.

À partir de la page d’accueil de connexion, sélectionnez iTunes **mes applications**:   
   
 [![](subscriptions-and-reporting-images/image2.png "Sélectionner Mes apps")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
Sélectionnez une application et cliquez sur le **achats dans l’application** onglet :

[![](subscriptions-and-reporting-images/image6.png "Cliquez sur l’onglet achats dans l’application")](subscriptions-and-reporting-images/image6.png#lightbox)

Au bas de la page, sélectionnez **vue ou de générer un secret partagé**:
   
 [![](subscriptions-and-reporting-images/image40.png "Sélectionnez la vue ou de générer un secret partagé")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "Générer un secret partagé")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 Pour utiliser le secret partagé, vous devez l’inclure dans la charge utile JSON qui est envoyée à des serveurs Apple lors de la validation d’une réception dans l’application d’un abonnement renouvelables automatiquement, comme suit :

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Champ d’état de la réponse sera zéro si le fournisseur est valide, comme avec d’autres types de produit.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Téléchargement d’éléments après le terme de l’abonnement Initial

Dans le cadre de la livraison de produits d’abonnement, le code doit vérifier fréquemment de la dernière réception connue sur des serveurs d’Apple. Si un abonnement a auto-renouvelée depuis la dernière vérification, la réponse JSON contient des champs supplémentaires qui informent l’application de la transaction s’est produite (qui doit étendre la validité des abonnements). Contient la réponse JSON :

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Si l’état est égal à zéro l’abonnement est toujours valide et les autres champs contiennent des données valides. Si l’état est 21006 l’abonnement a expiré. Consultez le [vérification d’un accusé de réception d’abonnement renouvelables automatiquement](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) documentation pour les autres codes d’erreur.

#### <a name="restoring-auto-renewable-subscriptions"></a>Restauration des abonnements renouvelables automatiquement

Vous obtiendrez plusieurs transactions : la transaction d’achat d’origine plus une transaction distincte pour chaque période de renouvellement de l’abonnement. Vous devez suivre les dates de début et les conditions pour comprendre ce qui est la période de validité.   
   
   
   
 L’objet SKPaymentTransaction n’inclut pas la durée de l’abonnement, vous devez utiliser un autre ID de produit pour chaque terme et écrire du code qui peut extrapoler la période d’abonnement à partir de la date d’achat de la transaction.

#### <a name="testing-auto-renewal"></a>Tester le renouvellement automatique.

Pour faciliter le test des abonnements, leurs durées sont compressées lors du test dans le bac à sable. 1 semaine abonnements renouvellement toutes les 3 minutes, 1 an abonnements renouvellement toutes les heures. Les abonnements seront le renouvellement automatique jusqu'à 6 fois lors du test dans le bac à sable.

## <a name="reporting"></a>Rapports

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) fournit :   
   
 **Ventes et les tendances** – affiche les détails de l’application de téléchargements, mises à jour et les achats dans l’application.   
   
 **Paiements et les rapports financiers** – détaille le revenu des coûts par vos applications, ainsi que les paiements de liste qui ont été apportées à vous et combien vous est due.

Vous trouverez ci-dessous un exemple de rapport des ventes et les tendances :   

 [![](subscriptions-and-reporting-images/image42.png "Un exemple de rapport des ventes et les tendances")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 Il existe également un [ **Mobile de se connecter CTI**l’application iOS (lien iTunes)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8).
captures d’écran de l’iPhone pour les statistiques disponibles sont affichés ici :   
   
 [![](subscriptions-and-reporting-images/image43.png "captures d’écran de l’iPhone pour les statistiques disponibles")](subscriptions-and-reporting-images/image43.png#lightbox)
