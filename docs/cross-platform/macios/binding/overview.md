---
title: Vue d'ensemble
description: "Détails du fonctionnement du processus de liaison"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e5b0cd151d282869cc9272f5ea3b0a83a6d04369
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="overview"></a>Vue d'ensemble

_Détails du fonctionnement du processus de liaison_

Liaison d’une bibliothèque Objective-C pour une utilisation avec Xamarin prend trois étapes :

1. Écrire un c# « Définition de l’API » pour décrire comment l’API native est exposé dans .NET, et comment il est mappé au sous-jacent objectif-C. Cette opération est effectuée à l’aide de c# standard construit comme `interface` et liaison différentes **attributs** (voir [exemple simple](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Après avoir écrit la « définition de l’API » en langage c#, compiler pour produire un assembly de « liaison ». Cette opération peut être effectuée sur le [ **ligne de commande** ](#commandline) ou en utilisant un [ **projet de liaison** ](#bindingproject) dans Visual Studio pour Mac ou Visual Studio.

3. Cet assembly « liaison » est ensuite ajouté à votre projet d’application Xamarin, donc vous pouvez accéder à la fonctionnalité native à l’aide de l’API que vous avez défini.
  Le projet de liaison est complètement distinct de vos projets d’application.

**Remarque :** étape 1 peut être automatisée à l’aide de [ **objectif Sharpie**](#objectivesharpie). Il examine l’API Objective-C et génère un proposé c# « Définition de l’API ». Vous pouvez personnaliser les fichiers créés par un objectif Sharpie et les utiliser dans un projet de liaison (ou sur la ligne de commande) pour créer votre assembly de liaison. Objectif Sharpie ne crée pas de liaisons par lui-même, il est simplement une partie facultative du processus plus volumineux.

Vous pouvez également lire plus de détails techniques de [son fonctionnement](#howitworks), ce qui vous aide à écrire vos liaisons.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Liaisons de la ligne de commande

Vous pouvez utiliser la `btouch-native` pour Xamarin.iOS (ou `bmac-native` si vous utilisez Xamarin.Mac) pour créer des liaisons directement. Il fonctionne en passant les définitions de l’API c# que vous avez créée manuellement (ou à l’aide d’objectif Sharpie) à l’outil de ligne de commande (`btouch-native` pour iOS ou `bmac-native` pour Mac).


La syntaxe générale pour l’appel de ces outils est la suivante :

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

La commande ci-dessus génère le fichier `cocos2d.dll` dans le répertoire actif, et il contiendra la bibliothèque entièrement liée que vous pouvez utiliser dans votre projet. Ceci est l’outil Visual Studio pour Mac utilise pour créer vos liaisons si vous utilisez un projet de liaison (décrites [ci-dessous](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Projet de liaison

Un projet de liaison peut être créé dans Visual Studio pour Mac ou Visual Studio (Visual Studio prend uniquement en charge les liaisons iOS) et le rend plus facile de modifier et de générer des définitions de l’API de liaison (et non pas à l’aide de la ligne de commande).

Suivez cette [mise en route](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) pour savoir comment créer et utiliser un projet de liaison pour produire une liaison.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objectif Sharpie

Objectif Sharpie est un autre outil de ligne commande qui permet des premières étapes de création d’une liaison. Il ne crée pas une liaison par lui-même, au lieu de cela, il automatise la première étape de la génération d’une définition de l’API de la bibliothèque native cible.

Lecture la [docs d’objectif Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md) pour savoir comment analyser des bibliothèques natives, les structures natives et CocoaPods dans les définitions d’API qui peuvent être intégrées dans les liaisons.

<a name="howitworks" />

# <a name="how-binding-works"></a>Fonctionne de la liaison

Il est possible d’utiliser le [[Enregistrer]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) attribut, [[Exporter]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) attribut, et [invocation de sélecteur manuelle Objective-C](~/ios/internals/objective-c-selectors.md) pour lier manuellement les nouvelles (précédemment types Objective-C indépendants).

Tout d’abord, recherchez un type que vous souhaitez lier. Pour la discussion à des fins (et la simplicité), nous allons lier le [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) type (qui a déjà été lié dans [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); l’implémentation ci-après est simplement par exemple à des fins).

Ensuite, nous devons créer le type c#. Vous voudrez probablement que cela placer dans un espace de noms ; Étant donné que Objective-C ne prend pas en charge les espaces de noms, nous aurons besoin afin d’utiliser le `[Register]` attribut pour modifier le nom de type Xamarin.iOS s’inscrire avec le runtime Objective-C. Le type c# doit également hériter [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Troisièmement, passez en revue la documentation Objective-C et créer [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instances pour chaque sélecteur que vous souhaitez utiliser. Le placer dans le corps de la classe :

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Quatrième, votre type devrez fournissent des constructeurs. Vous *doit* chaîner votre appel de constructeur pour le constructeur de classe de base. Le `[Export]` attributs autorisent code Objective-C pour appeler le constructeur avec le nom de la sélection spécifiée :

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Cinquième, fournissent des méthodes pour chacun des sélecteurs déclarées à l’étape 3. Ces filtres utilisent `objc_msgSend()` pour appeler le sélecteur sur l’objet natif. Notez l’utilisation de [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) pour convertir un `IntPtr` dans un correctement typé `NSObject` type (sous-). Si vous souhaitez que la méthode puisse être appelée à partir de code Objective-C, le membre *doit* être **virtuels**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Vue d’ensemble :

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

