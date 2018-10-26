---
title: Demande de révision de l’application dans Xamarin.iOS
description: Cet article décrit la méthode RequestReview par Apple ajouté à iOS 10 et explique comment l’implémenter dans Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: f72aaa781b0712e206cf02725cfc434594287f41
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109781"
---
# <a name="request-app-review-in-xamarinios"></a>Demande de révision de l’application dans Xamarin.iOS

_Cet article traite de la méthode RequestReview par Apple ajouté à iOS 10 et comment l’implémenter dans Xamarin.iOS._

Nouveau à iOS 10.3, le `RequestReview()` méthode permet à une application iOS demander à évaluer ou réviser l’utilisateur. Lorsque cette méthode est appelée dans une application de livraison que l’utilisateur a installé à partir de l’App Store, iOS 10 sera gérer l’évaluation entière et passez en revue le processus pour les développeurs. Étant donné que ce processus est régi par la stratégie de l’App Store, une alerte peut ou ne peut pas s’afficher.

![](request-app-review-images/review01.png "Un exemple d’alerte demande de révision")

## <a name="requesting-a-rating-or-review"></a>Demande une évaluation ou un avis

Alors que le `RequestReview()` méthode statique de la `SKStoreReviewController` classe peut être appelée à tout moment où il est judicieux de l’expérience utilisateur, le processus de révision est régi et géré par la stratégie de l’App Store. Par conséquent, cette méthode peut ou peut ne pas affiche une alerte et ne doit jamais être appelée en réponse à une action de l’utilisateur, par exemple en appuyant sur un bouton.

Par exemple, une application peut demander une revue après que qu’elle a été lancé un nombre donné de fois ou une partie peut demander une révision une fois que le joueur termine un niveau.

Pour les demandes une revue dès qu’une application Xamarin.iOS fin de son lancement, apportez les modifications suivantes à la `AppDelegate.cs` fichier :

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
> L’appel `RequestReview()` dans un développement incomplète application affichera toujours l’évaluation et passez en revue la boîte de dialogue afin qu’il peut être testé. Cela ne s’applique pas aux applications qui ont été distribuées dans TestFlight, où l’appel de méthode sera ignoré.

Lorsque le `RequestReview()` est appelée dans une application de livraison que l’utilisateur a installé à partir de l’App Store, iOS 10 gèrent l’ensemble du processus de révision et de contrôle d’accès pour le développeur. Là encore, étant donné que ce processus est régi par la stratégie de l’App Store, une alerte peut ou ne peut pas s’afficher.

## <a name="linking-to-an-app-store-product-page"></a>Liaison à une Page de produit App Store 

En plus des nouveaux `RequestReview` (méthode), le développeur peut toujours fournir un lien profond vers la page de produit de l’application dans le Store à partir d’application au sein d’une application. En ajoutant `action=write-review` à la fin de l’URL de page de produit, une page s’ouvre dans laquelle l’utilisateur peut écrire une révision de l’application automatiquement. 

## <a name="summary"></a>Récapitulatif

Cet article a présenté à la méthode RequestReview par Apple ajouté à iOS 10 et comment l’implémenter dans Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [iOSTenThree exemple](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
