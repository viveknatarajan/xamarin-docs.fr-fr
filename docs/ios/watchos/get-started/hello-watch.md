---
title: Hello, Espion
description: Prise en main Xamarin et watchOS
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 2281fa801d32e8d8934767ae090503ca523d7eff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="hello-watch"></a>Hello, Espion

Après avoir créé une solution suivant les étapes de [le programme d’installation et Installation](~/ios/watchos/get-started/installation.md), vous aurez 3 projets :

- L’application de Parent iOS qui est utilisée pour le programme d’installation ou d’autres tâches d’administration sur le périphérique. (Avec d’autres types d’Extensions d’iOS, il est souvent appelé l’application « Conteneur ».) Avec les applications de surveillance, il est possible pour les utilisateurs à démarrer l’exécution de l’application de surveillance sans **jamais** exécutant l’application parente ;
- L’Extension de surveillance qui contient le code de programme pour l’application de surveillance ; et
- L’application de surveillance, qui contient les ressources de plan conceptuel et image sont rendus sur la surveillance.

Vérifiez que votre [références sont corrects](~/ios/watchos/get-started/project-references.md): qu’une référence à l’Extension de l’application parente, et que l’Extension possède une référence à l’application de surveillance.

Confirmer que votre offre groupée les identificateurs respectent la \*.watchkitextension \*.watchkitapp convention et que votre fichier Info.plist Extension ont **ID d’offre groupée WKApp** la valeur de l’identificateur de lot votre application de surveillance.

Vous devez être en mesure d’exécuter votre application espion maintenant, mais étant donné que le fichier d’animation dans votre application espion est vide, vous ne pourriez pas savoir.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "L’Explorateur de solutions")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "L’Explorateur de solutions")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
Double-cliquez sur le Interface.storyboard dans votre application de surveillance pour démarrer le Concepteur de Xamarin iOS (si vous êtes sur un Mac, vous pouvez également droit et **ouvrir avec > Xcode Interface Builder**)


1.  Vérifiez le **boîte à outils** et **propriétés** remplit est visibles,
1.  Cliquez pour sélectionner le contrôleur d’Interface
1.  Définir l’identificateur et le titre du contrôleur d’Interface à **interfaceController** et **Hi espion**,
1.  Vérifiez le **classe** a la valeur **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Définir l’identificateur et le titre du contrôleur d’Interface interfaceController et Hi espion")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Double-cliquez sur le Interface.storyboard dans votre application de surveillance pour modifier avec Xamarin iOS concepteur dans Visual Studio :

1.  Ouvrez le volet Propriétés.
1.  Modifiez la classe à **InterfaceController**;
1.  Cliquez sur le contrôleur de l’Interface ; et
1.  Définir l’identificateur et le titre du contrôleur d’Interface à **interfaceController** et **Hi espion**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Définir l’identificateur et le titre du contrôleur d’Interface interfaceController et Hi espion")

-----


Créer votre interface utilisateur :

1. À partir de la **boîte à outils** remplissage,
1. Faites glisser et déposez un **bouton** et un **étiquette** sur la scène, et
1. Définir le texte et les attributs des contrôles comme indiqué :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "Définir le texte et les attributs des contrôles comme indiqué")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "Définir le texte et les attributs des contrôles comme indiqué")

-----

1. Définir le **nom** dans les **propriétés** remplissage pour chaque contrôle. Dans cet exemple, nous avons utilisé `myButton` et `myLabel`.

1. Sélectionnez le bouton sur le plan conceptuel et accédez à la **propriétés** du remplissage **événements** liste, puis

1. Créer un nouveau **Action** en tapant `OnButtonPress` et en appuyant sur **entrée**.
  L’action s’affiche dans la liste, et une méthode partielle sera automatiquement créée en c#.

![](hello-watch-images/buttonaction.png "L’Action OnButtonPress ajouté à un bouton")

Après avoir enregistré le plan conceptuel, le **InterfaceController.designer.cs** est mis à jour avec les noms de contrôle et les actions... Si vous ouvrez ce fichier une fois qu’il a mis à jour, vous pouvez voir comment la `RegisterAttribute` correspond au contrôleur et comment les contrôles d’interface utilisateur correspondent aux variables d’instance c# marqués avec la `OutletAttribute` et comment les Actions sont mappés aux méthodes partielles balisées avec le `ActionAttribute`:

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

À présent ouvrir **InterfaceController.cs** (*pas* InterfaceController.designer.cs) et ajoutez le code suivant :

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

Ce code doit être relativement transparent : la variable d’instance `clickCount` est incrémenté chaque fois que la fonction `OnButtonPress` est appelée. Le texte de `myLabel` est modifié pour refléter ce nombre ; `myLabel`, bien sûr, est le nom d’une des prises que vous avez créé dans XCode. Le `partial` (fonction) est l’implémentation de la fonction associée au nom de l’Action que vous avez spécifié.

S’il n’est pas déjà le projet de démarrage

1. Avec le bouton droit sur votre projet d’Extension de surveillance et choisissez **définir comme projet de démarrage**,

1. Définissez la cible de déploiement sur une image de simulateur compatible espion Kit (tels que les iPhone 6 iOS 8.2)

1. Appuyez sur la **déboguer** bouton pour déclencher un lancement de la build et le simulateur.

    [![](hello-watch-images/readytodebug-sml.png "Les éléments d’interface de Visual Studio")](hello-watch-images/readytodebug.png#lightbox)

Lorsque le simulateur s’ouvre, appuyez sur le bouton pour incrémenter l’étiquette.
Félicitations, vous avez vous-même une application de surveillance.

![](hello-watch-images/running.png "L’application en cours d’exécution dans le simulateur")


## <a name="related-links"></a>Liens associés

- [Mise en route (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Configuration et installation](~/ios/watchos/get-started/installation.md)
- [Première vidéo de l’application de surveillance](http://blog.xamarin.com/your-first-watch-kit-app/)
