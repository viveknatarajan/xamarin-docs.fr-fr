---
title: Démonstration de cycle de vie d’application pour Xamarin.iOS
description: Ce document examine les différents événements de cycle de vie gérées par le délégué de l’application dans une application iOS, démontrer quand et comment ces événements sont gérés.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/17/2018
ms.openlocfilehash: 3beb511c03b328ecea824bf89355d056df003f3e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946158"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Démonstration de cycle de vie d’application pour Xamarin.iOS

Cet article et [exemple de code](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/) montre les États de quatre applications dans iOS et le rôle de la `AppDelegate` méthodes à avertir l’application de modifiés lorsque les États. L’application imprime les mises à jour de la console chaque fois que l’application change d’état :

[![](application-lifecycle-demo-images/image3-sml.png "L’exemple d’application")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "L’application imprime les mises à jour de la console chaque fois que l’application change d’état")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Procédure pas à pas

1. Ouvrez le **cycle de vie** de projet dans le **LifecycleDemo** solution.
1. Ouvrez la `AppDelegate` classe. La journalisation a été ajoutée pour les méthodes de cycle de vie pour indiquer quand l’application a changé d’état :

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Lancez l’application dans le simulateur ou sur l’appareil. `OnActivated` est appelée quand l’application démarre. L’application est désormais dans le _Active_ état.
1. Appuyez sur le bouton Accueil dans le simulateur ou un périphérique à sortir de l’application à l’arrière-plan. `OnResignActivation` et `DidEnterBackground` seront appelés les transitions de l’application à partir de `Active` à `Inactive` et dans le `Backgrounded` état. Dans la mesure où il n’existe aucun jeu de code d’application à exécuter en arrière-plan, l’application est considérée comme _suspendu_ en mémoire.
1. Accédez à l’application pour ramener au premier plan. `WillEnterForeground` et `OnActivated` seront à la fois appelées :

    ![](application-lifecycle-demo-images/image4.png "Modification de l’état est imprimé sur la console")

    La ligne de code dans le contrôleur d’affichage suivante est exécutée lorsque l’application a entré le premier plan à partir de l’arrière-plan et modifie le texte affiché sur l’écran :

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Appuyez sur la **accueil** bouton pour placer l’application en arrière-plan. Ensuite, double-cliquez sur le **accueil** bouton pour afficher le sélecteur d’application. Sur l’iPhone X, effectuez un balayage haut en bas de l’écran :

    [![Le commutateur d’applications](application-lifecycle-demo-images/app-switcher-sml.png "au commutateur d’applications")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Recherchez l’application dans le sélecteur d’application et faites défiler les supprimer (sur iOS 11, long appuyez sur jusqu'à ce que les icônes rouge apparaissent dans l’angle) :

    [![Faites défiler jusqu'à la suppression d’une application en cours d’exécution](application-lifecycle-demo-images/app-switcher-swipe-sml.png "balayez pour retirer une application en cours d’exécution")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

pour fermer l’application iOS. Notez que `WillTerminate` n’est pas appelée, car l’application est déjà _suspendu_ en arrière-plan.

## <a name="related-links"></a>Liens connexes

- [LifecycleDemo (sample)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
