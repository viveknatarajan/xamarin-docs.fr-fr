---
title: "Demande de révision de l’application"
description: "Cet article traite de la méthode RequestReview que Apple ajouté à iOS 10 et comment l’implémenter dans Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 2e63f2c47bbcd6da0f0d5370ebfc231d19a10e7d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="request-app-review"></a>Demande de révision de l’application

_Cet article traite de la méthode RequestReview que Apple ajouté à iOS 10 et comment l’implémenter dans Xamarin.iOS._

Vous débutez avec iOS 10.3, le `RequestReview()` méthode permet à une application iOS demander à l’utilisateur à évaluer ou réviser. Lorsque cette méthode est appelée dans une application de livraison que l’utilisateur a installé à partir de l’App Store, iOS 10 sera gérer l’évaluation entière et passez en revue le processus pour le développeur. Étant donné que ce processus est régi par la stratégie de l’App Store, une alerte peut ou ne peut pas s’afficher.

![](request-app-review-images/review01.png "Un échantillon d’alerte demande de révision")

## <a name="requesting-a-rating-or-review"></a>Demande une évaluation ou vérification

Alors que le `RequestReview()` méthode statique de la `SKStoreReviewController` classe peut être appelée à tout moment où il est préférable de l’expérience utilisateur, le processus de révision est régi et géré par la stratégie de l’App Store. Par conséquent, cette méthode peut ou peut ne pas affiche une alerte et ne doit jamais être appelée en réponse à une action de l’utilisateur, par exemple en appuyant sur un bouton.

Par exemple, une application peut demander une revue après que qu’elle a été lancée à un certain nombre de fois ou une partie peut demander une révision issue d’un niveau par le lecteur.

Pour les demandes une revue dès qu’une application Xamarin.iOS termine de lancement, apportez les modifications suivantes à la `AppDelegate.cs` fichier :

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }
        
        ...
        
    }
}
```

> [!NOTE]
> L’appel `RequestReview()` dans le développement d’un trait de soulignement application affichera toujours l’évaluation et passez en revue la boîte de dialogue afin qu’il peut être testé. Cela ne s’applique pas aux applications qui ont été distribuées via TestFlight, où l’appel de méthode sera ignoré.

Lorsque le `RequestReview()` est appelée dans une application de livraison que l’utilisateur a installé à partir de l’App Store, iOS 10 gère l’ensemble du processus de contrôle d’accès et de révision pour le développeur. Là encore, étant donné que ce processus est régi par la stratégie de l’App Store, une alerte peut ou ne peut pas s’afficher.

## <a name="linking-to-an-app-store-product-page"></a>Lien vers une Page de produit App Store 

En plus des nouveaux `RequestReview` (méthode), le développeur peut toujours fournir un lien ciblé pour la page de l’application produit dans l’App Store à partir d’une application. En ajoutant `action=write-review` à la fin de l’URL de la page produit, une page s’ouvre dans laquelle l’utilisateur peut écrire automatiquement une révision de l’application. 

## <a name="summary"></a>Récapitulatif

Cet article a couvert de la méthode RequestReview que Apple ajouté à iOS 10 et comment l’implémenter dans Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [iOSTenThree exemple](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
