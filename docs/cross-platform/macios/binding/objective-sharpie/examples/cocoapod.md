---
title: Exemple du monde réel à l’aide de CocoaPods
description: Ce document montre comment utiliser Sharpie d’objectif pour générer automatiquement le C# définitions à partir d’un CocoaPod de liaison.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: bac34f662e24c6b08a67cd8da1f41b37b43b3faf
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61200313"
---
# <a name="real-world-example-using-cocoapods"></a>Exemple du monde réel à l’aide de CocoaPods

> [!NOTE]
> Cet exemple utilise le [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Nouveautés de la version 3.0, objectif Sharpie prend en charge la liaison CocoaPods et même inclut une commande (`sharpie pod`) pour effectuer le téléchargement, la configuration et la création de CocoaPods très facile. Vous devez [vous familiariser avec les CocoaPods](https://cocoapods.org) en général avant d’utiliser cette fonctionnalité.

## <a name="creating-a-binding-for-a-cocoapod"></a>Création d’une liaison pour un CocoaPod

Le `sharpie pod` commande a une option globale et deux sous-commandes :

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

Le `init` sous-commande a également une aide utile :

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Plusieurs noms de CocoaPod et subspec peut être fourni à `init`.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

Une fois que votre CocoaPod a été configuré, vous pouvez maintenant créer la liaison :

```bash
$ sharpie pod bind
```

Ainsi, le projet CocoaPod Xcode cours de génération et puis évaluée et analysées par objectif Sharpie. Un grand nombre de sortie de la console sera généré, mais doit aboutir à la définition de la liaison à la fin :

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Étapes suivantes

Après avoir généré le **ApiDefinitions.cs** et **StructsAndEnums.cs** fichiers, examinons la documentation suivante pour générer un assembly à utiliser dans vos applications :

- [Vue d’ensemble de liaison Objective-C](~/cross-platform/macios/binding/overview.md)
- [Bibliothèques de liaison Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Procédure pas à pas : Liaison d’une bibliothèque de Objective-C iOS](~/ios/platform/binding-objective-c/walkthrough.md)
- [Cours de l’Université de Xamarin : Génération d’une bibliothèque de liaisons Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Cours de l’Université de Xamarin : Générer une bibliothèque de liaisons Objective-C avec Sharpie objectif](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
