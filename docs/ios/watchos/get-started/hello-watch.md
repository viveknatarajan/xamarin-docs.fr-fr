---
title: 'Hello, watchOS : procédure pas à pas'
description: Ce document fournit une procédure pas à pas de création d’une application watchOS simple à l’aide de Xamarin. Il décrit comment utiliser dans Visual Studio et Visual Studio pour Mac, travailler avec des storyboards et répondre aux événements dans le code.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/14/2016
ms.openlocfilehash: 7066acc82603b1c74dea2b7f0050bc63c46316f1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122229"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchOS : procédure pas à pas

Une fois que vous avez créé une solution en suivant les étapes décrites dans [le programme d’installation et Installation](~/ios/watchos/get-started/installation.md), vous aurez 3 projets :

- L’application de Parent iOS qui est utilisée pour le programme d’installation ou d’autres tâches administratives sur l’appareil. (Avec d’autres types d’e/s Extensions, cela est souvent appelé l’application « Conteneur ».) Avec les applications de surveillance, il est possible pour les utilisateurs commencer à exécuter l’application Watch sans **jamais** exécution de l’application Parent ;
- L’Extension Watch qui contient le code de programme pour l’application watch ; et
- L’application de surveillance, qui contient les ressources de table de montage séquentiel et image sont rendus sur la surveillance.

Vérifiez que votre [références sont corrects](~/ios/watchos/get-started/project-references.md): que l’application parente a une référence à l’Extension, et que l’Extension a une référence à l’application Watch.

Confirmer que vos identificateurs d’offre groupée suivent le \*.watchkitextension \*.watchkitapp convention et que votre fichier Info.plist Extension ont **ID d’ensemble WKApp** la valeur de l’identificateur de Bundle votre application Watch.

Vous devez être en mesure d’exécuter votre application Watch maintenant, mais étant donné que le fichier d’animation dans votre application Watch est vide, vous ne pourriez pas dire.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "L’Explorateur de solutions")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "L’Explorateur de solutions")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
    
Double-cliquez sur le Interface.storyboard dans votre application Watch pour démarrer le Concepteur de Xamarin iOS (si vous êtes sur un Mac, vous pouvez également droit et **ouvrir avec > Xcode Interface Builder**)


1.  Vérifiez le **boîte à outils** et **propriétés** panneaux sont visibles,
1.  Cliquez pour sélectionner le contrôleur d’Interface,
1.  Définir l’identificateur et le titre du contrôleur d’Interface à **interfaceController** et **Hi espion**,
1.  Vérifiez le **classe** a la valeur **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "La valeur est l’identificateur et le titre du contrôleur d’Interface interfaceController et Hi espion")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Double-cliquez sur le Interface.storyboard dans votre application Watch pour modifier avec le Kit de développement Xamarin iOS concepteur dans Visual Studio :

1.  Ouvrez le volet Propriétés.
1.  Remplacez la classe par **InterfaceController**;
1.  Cliquez sur le contrôleur d’Interface ; et
1.  Définir l’identificateur et le titre du contrôleur d’Interface à **interfaceController** et **Hi espion**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "La valeur est l’identificateur et le titre du contrôleur d’Interface interfaceController et Hi espion")

-----


Créer votre interface utilisateur :

1. À partir de la **boîte à outils** remplissage,
1. Faites glisser et déposez un **bouton** et un **étiquette** sur la scène, et
1. Définir le texte et les attributs des contrôles comme indiqué :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "Définir le texte et les attributs des contrôles comme indiqué")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "Définir le texte et les attributs des contrôles comme indiqué")

-----

1. Définir le **nom** dans le **propriétés** pad pour chaque contrôle. Dans cet exemple, nous avons utilisé `myButton` et `myLabel`.

1. Sélectionnez le bouton sur le plan conceptuel et accédez à la **propriétés** du pavé **événements** liste, puis

1. Créer un nouveau **Action** en tapant `OnButtonPress` et en appuyant sur **entrée**.
  L’action s’affiche dans la liste, et une méthode partielle est créée automatiquement dans C#.

![](hello-watch-images/buttonaction.png "L’Action OnButtonPress ajouté à un bouton")

Après avoir enregistré le plan conceptuel, le **InterfaceController.designer.cs** est mis à jour avec les noms de contrôle et les actions... Si vous ouvrez ce fichier après qu’il a mis à jour, vous pouvez voir comment la `RegisterAttribute` correspond au contrôleur et la manière dont les contrôles d’interface utilisateur correspondent à C# variables d’instance marqués avec le `OutletAttribute` et comment les Actions correspondent aux méthodes partielles marqués avec le `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Ouvrez maintenant **InterfaceController.cs** (*pas* InterfaceController.designer.cs) et ajoutez le code suivant :

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

Ce code doit être relativement transparent : la variable d’instance `clickCount` est incrémenté chaque fois que la fonction `OnButtonPress` est appelée. Le texte de `myLabel` est modifié pour refléter ce nombre ; `myLabel`, bien sûr, est le nom d’une des prises que vous avez créé dans XCode. Le `partial` (fonction) est l’implémentation de la fonction associée au nom de l’Action que vous avez spécifié.

Si elle n’est pas déjà le projet de démarrage

1. Avec le bouton droit sur votre projet d’Extension Watch et choisissez **définir comme projet de démarrage**,

1. Définissez la cible de déploiement sur une image de simulateur espion Kit compatibles (par exemple, iPhone 6 iOS 8.2)

1. Appuyez sur la **déboguer** bouton pour déclencher le lancement d’un simulateur et build.

    [![](hello-watch-images/readytodebug-sml.png "Les éléments d’interface de Visual Studio")](hello-watch-images/readytodebug.png#lightbox)

Lancement par le simulateur, appuyez sur le bouton pour incrémenter l’étiquette.
Félicitations, vous doterez d’une application Watch.

![](hello-watch-images/running.png "L’application en cours d’exécution dans le simulateur")


## <a name="related-links"></a>Liens associés

- [Mise en route (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Configuration et installation](~/ios/watchos/get-started/installation.md)
- [Première vidéo de l’application Apple Watch](http://blog.xamarin.com/your-first-watch-kit-app/)
