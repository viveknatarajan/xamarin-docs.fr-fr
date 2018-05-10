---
title: 'UrhoSharp programmation avec F #'
description: 'Comment créer une simple application UrhoSharp à l’aide de F # dans Visual Studio pour Mac'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: d2b21204d1d328831419308827e1a2de2b6aef1c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="programming-urhosharp-with-f"></a>UrhoSharp programmation avec F #

_Comment créer une simple application UrhoSharp à l’aide de F # dans Visual Studio pour Mac_

UrhoSharp peut être programmé avec F # en utilisant les bibliothèques et les concepts utilisés par les programmeurs c# même. Le [à l’aide de UrhoSharp](~/graphics-games/urhosharp/using.md) article donne une vue d’ensemble du moteur UrhoSharp et doit être lue avant de cet article.

Comme les bibliothèques de nombreuses provenant de l’univers C++, de nombreuses fonctions UrhoSharp retournent des valeurs booléennes ou des entiers indiquant la réussite ou l’échec. Vous devez utiliser `|> ignore` pour ignorer ces valeurs.

Le [exemple de programme](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) est un « Hello World » pour UrhoSharp à partir de F #.

## <a name="creating-an-empty-project"></a>Créez un projet vide

Il n’existe aucun modèle F # pour UrhoSharp encore disponible, par conséquent, pour créer votre propre projet UrhoSharp que vous pouvez démarrer avec la [exemple](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) ou procédez comme suit :

1. À partir de Visual Studio pour Mac, créer un nouveau **Solution**. Choisissez **iOS > application > Afficher l’application unique** et sélectionnez **F #** comme langage de mise en œuvre. 
1. Supprimer le **Main.storyboard** fichier. Ouvrir le **Info.plist** fichier et dans le **iPhone / iPod informations de déploiement** volet, supprimer le `Main` de chaîne dans le **Interface principale** liste déroulante.
1. Supprimer le **ViewController.fs** également le fichier.

## <a name="building-hello-world-in-urho"></a>Construction Hello World dans Urho

Vous êtes maintenant prêt à commencer à définir les classes de votre jeu. Au minimum, vous devez définir une sous-classe de `Urho.Application` et remplacez son `Start` (méthode). Pour créer ce fichier, avec le bouton droit sur votre projet F #, choisissez **ajouter un nouveau fichier...**  et ajouter une classe F # vide à votre projet. Le nouveau fichier sera ajouté à la fin de la liste des fichiers dans votre projet, mais vous devez faire glisser afin qu’il apparaisse *avant* il est utilisé dans **AppDelegate.fs**.

1. Ajoutez une référence au package Urho NuGet.
1. À partir d’un projet existant de Urho, copier les répertoires (grands) **CoreData /** et **données /** dans de votre projet **ressources /** active. Dans votre projet F #, cliquez sur le **ressources** dossier et utilisez **ajouter / ajouter un dossier existant** pour ajouter tous ces fichiers à votre projet.

Structure de votre projet doit maintenant ressembler à :

![](fsharp-images/solutionpane.png "La structure de projet doit maintenant ressembler à")

Définir votre classe qui vient d’être créé comme un sous-type de `Urho.Application` et remplacez son `Start` méthode :

```csharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Le code est très simple. Elle utilise le `Urho.Gui.Text` classe pour afficher une chaîne alignée avec une certaine taille de police et la couleur. 

Avant de ce code peut s’exécuter, cependant, UrhoSharp doit être initialisé. 

Ouvrez le fichier AppDelegate.fs et modifiez le `FinishedLaunching` méthode comme suit :

```csharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

Le `ApplicationOptions.Default` fournit les options par défaut pour une application en mode paysage. Passez ces `ApplicationOptions` le constructeur par défaut pour votre `Application` sous-classe (Notez que lorsque vous avez défini le `HelloWorld` classe, la ligne `inherit Application(o)` appelle le constructeur de classe de base). 

Le `Run` méthode de votre `Application` lance le programme. Il est défini comme retournant un `int`, qui peuvent être redirigé vers `ignore`. 

Le programme qui en résulte doit ressembler à :

![](fsharp-images/helloworldfsharp.png "Le programme qui en résulte doit ressembler à")








## <a name="related-links"></a>Liens associés

- [Parcourir sur GitHub (exemple)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
