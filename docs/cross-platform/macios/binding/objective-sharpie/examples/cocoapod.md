---
title: Exemple du monde réel à l’aide de CocoaPods
description: Ce document montre comment utiliser objectif Sharpie pour générer automatiquement les définitions de liaison C# à partir d’un CocoaPod.
ms.topic: article
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: 24c796cb258578fdfc68c5b4aa1079d3c589da0f
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2018
---
# <a name="real-world-example-using-cocoapods"></a>Exemple du monde réel à l’aide de CocoaPods

> [!NOTE]
> Cet exemple utilise le [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

La version 3.0, objectif Sharpie prend en charge la liaison CocoaPods et même inclut une commande (`sharpie pod`) pour effectuer le téléchargement, la configuration et la création des CocoaPods très facile. Vous devez [vous familiariser avec CocoaPods](https://cocoapods.org) en général avant d’utiliser cette fonctionnalité.

## <a name="creating-a-binding-for-a-cocoapod"></a>Création d’une liaison pour un CocoaPod

Le `sharpie pod` commande possède une option globale et deux sous-commandes :

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

Le `init` sous-commande dispose également d’aide utile :

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Plusieurs noms de CocoaPod et subspec peut être fourni pour `init`.

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

Une fois votre CocoaPod a été défini, vous pouvez maintenant créer la liaison :

```bash
$ sharpie pod bind
```

Ainsi, le projet CocoaPod Xcode en cours générés évaluée et analysé par un objectif Sharpie. Un grand nombre de sortie de la console sera généré, mais doit entraîner dans la définition de la liaison à la fin :

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Étapes suivantes

Après avoir généré le **ApiDefinitions.cs** et **StructsAndEnums.cs** fichiers, examinez la documentation suivante pour générer un assembly à utiliser dans vos applications :

- [Vue d’ensemble de liaison Objective-C](~/cross-platform/macios/binding/overview.md)
- [Bibliothèques de liaison Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Procédure pas à pas : Liaison d’une bibliothèque de Objective-C iOS](~/ios/platform/binding-objective-c/walkthrough.md)

