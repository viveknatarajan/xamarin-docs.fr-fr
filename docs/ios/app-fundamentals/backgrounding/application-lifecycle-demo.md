---
title: Démonstration de cycle de vie d’application pour Xamarin.iOS
description: Ce document examine les différents événements de cycle de vie gérées par le délégué de l’application dans une application iOS, démontrant quand et comment ces événements sont gérés.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 64c695065012e4bf796c219c260324d9b6278ca5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783582"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Démonstration de cycle de vie d’application pour Xamarin.iOS

Dans cette section, nous allons examiner une application qui montre les quatre états d’Application et le rôle de la `AppDelegate` méthodes de notification de l’application de modification lorsque les États. L’application imprime sur la console des mises à jour chaque fois que l’application change d’état :

 [![](application-lifecycle-demo-images/image3.png "L’exemple d’application")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "L’application imprime les mises à jour de la console chaque fois que l’application change d’état")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Procédure pas à pas


  1. Ouvrez le _cycle de vie_ de projet dans le _LifecycleDemo_ solution.
  1. Ouvrez la `AppDelegate` classe. Notez que nous avons ajouté la journalisation pour les méthodes de cycle de vie pour nous dire lorsque l’application a changé d’état :

            ```chsarp
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

  1. Lancez l’application dans le simulateur, ou sur l’appareil. `OnActivated` est appelé lorsque l’application démarre. L’application est présent dans le _Active_ état.
  1. Cliquez sur le bouton accueil sur le simulateur ou l’appareil pour amener l’application à l’arrière-plan. `OnResignActivation` et `DidEnterBackground` sera appelée en tant que les transitions de l’application à partir de `Active` à `Inactive` et dans le `Backgrounded` état. Étant donné que nous n’avons pas fourni notre application de code à exécuter en arrière-plan, l’application est considérée comme _Suspended_ en mémoire.
  1. Naviguez vers l’application pour ramener au premier plan. `WillEnterForeground` et `OnActivated` sont tous deux être appelées :

        ![](application-lifecycle-demo-images/image4.png "Modification de l’état est imprimé dans la console")

    Notez que nous avons ajouté la ligne de code suivante à notre contrôleur message nous informe que l’application a entré le premier plan de l’arrière-plan de la vue :

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. Appuyez sur la **accueil** pour valider l’application en arrière-plan. Ensuite, double-cliquez sur le **accueil** bouton pour ouvrir le sélecteur de l’application :
    
    ![](application-lifecycle-demo-images/app-switcher-.png "Le commutateur d’applications")
  
1. Recherchez l’application dans le sélecteur de l’application et faites défiler les supprimer :
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "Faites défiler jusqu'à la suppression d’une application en cours d’exécution") 
    
iOS mettra fin à l’application. N’oubliez pas que `WillTerminate` n’est pas appelée, car nous allons arrêter une application qui est _Suspended_ en arrière-plan.

Maintenant que nous comprenons les États d’application iOS et les transitions, nous allons examiner les différentes options disponibles pour backgrounding dans iOS.



## <a name="related-links"></a>Liens associés

- [LifecycleDemo(Part2) (exemple)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
