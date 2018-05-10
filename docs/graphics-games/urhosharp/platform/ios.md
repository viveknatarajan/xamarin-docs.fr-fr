---
title: UrhoSharp iOS et la prise en charge de tvOS
description: iOS et tvOS le programme d’installation spécifique et les fonctionnalités des UrhoSharp.
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 322297e7782a06a2d900b12cd5afc5f469009f69
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS et la prise en charge de tvOS

_iOS et tvOS le programme d’installation spécifiques et des fonctionnalités_

Bien que Urho est une bibliothèque de classes portable et permet la même API à utiliser sur la plateforme différents pour la logique du jeu, vous devez initialiser Urho dans votre pilote plateforme et dans certains cas, vous souhaitez tirer parti des fonctionnalités spécifiques de plateforme .

Dans les pages suivantes, supposons que `MyGame` est un sublcass de la `Application` classe.

## <a name="ios-and-tvos"></a>iOS et tvOS

**Architectures prises en charge :** armv7, arm64, i386

## <a name="creating-a-project"></a>Création d'un projet

Créer un projet iOS, puis ajouter des données dans le répertoire de ressources et vérifiez que tous les fichiers ont **BundleResource** comme le **Action de génération**.

![Le programme d’installation de projet](ios-images/image-4.png "ajouter des données dans le répertoire de ressources")

## <a name="configuring-and-launching-urho"></a>Configuration et lancement Urho

Ajouter à l’aide des instructions pour la `Urho` et `Urho.iOS` espaces de noms, puis ajoutez ce code pour l’initialisation Urho, ainsi que le lancement de votre application :

```csharp
new MyGame().Run();
```

Notez que puisqu’iOS attend `FinishedLaunching` pour terminer, vous devez la file d’attente l’appel à `Run()` pour exécuter une fois la méthode terminée, il s’agit d’un idiome commun :

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

Il est important de désactiver les optimisations PNG, car l’optimiseur PNG par défaut iOS générera des images qui Urho ne peuvent pas consommer actuellement correctement.

## <a name="custom-embedding-of-urho"></a>Incorporation personnalisé de Urho

Vous peut également avoir Urho prendre sur l’écran de l’ensemble de l’application, et pour l’utiliser en tant que composant de votre application, vous pouvez créer un `UrhoSurface` qui est un `UIView` que vous pouvez incorporer dans votre application existante.

C’est ce que vous devez faire :

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

Il hébergera votre classe Urho, par conséquent, puis vous le feriez :

```csharp
new MyGame().Run ();
```

