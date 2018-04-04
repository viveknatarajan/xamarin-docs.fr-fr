---
title: 'Partie 1 : création d’un MonoGame inter-plateformes'
description: Cette procédure pas à pas montre comment créer un nouveau projet pour iOS et Android à l’aide de MonoGame. Le résultat est un Visual Studio pour la solution Mac avec un projet de code partagé multiplateforme, ainsi qu’un projet pour chaque plateforme. Ce projet affiche un écran bleu vide lors de l’exécution.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 0cd12f23f8cb269b2a41a08bf641db08e18fb82b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Partie 1 : création d’un MonoGame inter-plateformes

_Cette procédure pas à pas montre comment créer un nouveau projet pour iOS et Android à l’aide de MonoGame. Le résultat est un Visual Studio pour la solution Mac avec un projet de code partagé multiplateforme, ainsi qu’un projet pour chaque plateforme. Ce projet affiche un écran bleu vide lors de l’exécution._

MonoGame permet le développement de jeux multiplateformes avec une grande partie de la réutilisation de code. Cette procédure pas à pas se concentrera sur la configuration d’une solution qui contient des projets pour iOS et Android, ainsi que d’un projet de code partagé pour le code multiplateforme.

Lorsque nous avons terminé, nous ont un projet qui a la structure appropriée pour effectuer la mise à jour la logique et le dessin logique à 30 images par seconde de jeu. Il peut être utilisé en tant que le projet de base pour n’importe quel projet MonoGame. Lors de l’exécution, notre projet doit ressembler à ceci :

![](part1-images/image1.png "Le projet doit ressembler à ceci lors de l’exécution")


# <a name="adding-monogame-to-visual-studio-for-mac"></a>Ajout de MonoGame à Visual Studio pour Mac

MonoGame peut être ajouté en tant qu’un complément pour Visual Studio pour Mac. Sur Mac, sélectionnez **Visual Studio pour Mac** > **Gestionnaire de compléments...**  . Sous Windows, sélectionnez ** Outils ** > **Gestionnaire de compléments...**  . Sélectionnez le **galerie** onglet, développez le **développement de jeux** catégorie et sélectionnez **MonoGame Addin**, puis cliquez sur **installer**:

![](part1-images/image2.png "Sélectionnez l’onglet Bibliothèque, développez la catégorie de développement de jeux et sélectionnez MonoGame Addin, puis cliquez sur Installer")

> [!IMPORTANT]
> **Remarque**: si le **développement de jeux** section n’apparaît pas dans le Gestionnaire de compléments, vous pouvez télécharger et installer la dernière version à partir d’ici manuellement : http://www.monogame.net/downloads/. Vous devrez peut-être redémarrer Visual Studio pour Mac pour les modèles à afficher.



Une fois installé, les modèles de MonoGame seront affiche dans Visual Studio pour Mac, comme nous le verrons dans la section suivante.


# <a name="creating-a-new-solution"></a>Création d’une solution

Dans Visual Studio pour, sélectionnez Mac **fichier > Nouvelle Solution**. Dans le **nouveau projet** boîte de dialogue, cliquez sur **divers**, faites défiler vers le **général** section, sélectionnez la ** application universelle MonoGame Mobile ** option et cliquez sur Suivant.

![](part1-images/image3.png "Dans la boîte de dialogue Nouveau projet, cliquez sur divers, faites défiler la section générale, sélectionnez l’option d’application universelle MonoGame Mobile et cliquez sur Suivant")

Nommez le projet WalkingGame et cliquez sur Créer :

![](part1-images/image4.png "Nommez le projet WalkingGame et cliquez sur Créer")

Désormais, notre projet s’exécute comme n’importe quel autre iOS ou Android projet. Le projet doit s’exécuter à afficher un arrière-plan bleu vif bleue :

![](part1-images/image5.png "Le projet doit s’exécuter à afficher un arrière-plan bleu vif bleu")


# <a name="fixing-android-compile-errors"></a>Résolution des erreurs de compilation Android

La version actuelle des modèles de MonoGame inclut quelques erreurs de syntaxe dans le d’Android `Activity1.cs` fichier. Pour résoudre ces problèmes, remplacez le `OnCreate` fonction avec les éléments suivants :


```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```


# <a name="summary"></a>Récapitulatif

Cette procédure pas à pas expliqué comment créer un projet de MonoGame multiplateforme à l’aide de Visual Studio pour Mac. La conséquence de cela est un écran bleu vide. Ce projet peut être utilisé comme point de départ pour tous les iOS et Android jeu.

## <a name="related-links"></a>Liens associés

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Documentation de l’API de MonoGame](http://www.monogame.net/documentation/?page=main)
