---
title: Référencement des bibliothèques natives dans Xamarin.iOS
description: Ce document explique comment lier des bibliothèques C natifs dans une application Xamarin.iOS. Il décrit comment créer des bibliothèques natives universels et l’accès aux méthodes C à partir de C#.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/28/2016
ms.openlocfilehash: 7ed8fc18624f46abd4a9fc293d8c33a1722da7dd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61424619"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Référencement des bibliothèques natives dans Xamarin.iOS

Xamarin.iOS prend en charge la liaison avec les bibliothèques C natives et les bibliothèques Objective-C. Ce document explique comment lier vos bibliothèques C natifs avec votre projet Xamarin.iOS. Pour plus d’informations sur la même opération pour les bibliothèques Objective-C, consultez notre [Objective-C, les Types de liaison](~/ios/platform/binding-objective-c/index.md) document.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Création de bibliothèques natives (i386 ARMv7 et ARM64)

Il est souvent souhaitable de créer vos bibliothèques natives pour chacune des plateformes prises en charge pour le développement iOS (i386 appareils eux-mêmes pour le simulateur et ARMv7/ARM64). Si vous disposez déjà d’un projet Xcode pour votre bibliothèque, c’est vraiment facile à faire.

Pour générer la version i386 de votre bibliothèque native, exécutez la commande suivante à partir d’un terminal :

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Il en résulte une bibliothèque statique native sous `MyProject.xcodeproj/build/Release-iphonesimulator/`. Copier (ou déplacer) le fichier d’archive de bibliothèque (libMyLibrary.a) à un endroit sûr pour une utilisation ultérieure, en lui attribuant un nom unique (tel que **libMyLibrary-i386.a**) afin qu’il n’entrer en conflit avec les versions arm64 et armv7 de la même bibliothèque dont vous avez besoin Générez ensuite.

Pour générer la version ARM64 de votre bibliothèque native, exécutez la commande suivante :

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Cette fois la bibliothèque native générée se trouve dans `MyProject.xcodeproj/build/Release-iphoneos/`. Une fois encore, copier (ou déplacer) ce fichier dans un emplacement sûr, en le renommant à quelque chose comme **libMyLibrary-arm64.a** afin qu’il ne sont pas entrer en conflit.

Générez maintenant la version ARMv7 de la bibliothèque :

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Copier (ou déplacer) le fichier de bibliothèque au même emplacement vous avez déplacé les 2 autres versions de la bibliothèque, en le renommant à quelque chose comme **libMyLibrary-armv7.a**.

Pour rendre un universal binaire, vous pouvez utiliser le `lipo` outil comme suit :

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Cette opération crée `libMyLibrary.a` qui sera une bibliothèque (fat) universelle qui convient à utiliser pour toutes les cibles de développement iOS.


### <a name="missing-required-architecture-i386"></a>I386 d’Architecture requis manquant

Si vous obtenez un `does not implement methodSignatureForSelector` ou `does not implement doesNotRecognizeSelector` message dans votre sortie Runtime lorsque vous tentez d’utiliser une bibliothèque Objective-C dans le simulateur, votre bibliothèque iOS probablement n’a pas été compilé pour l’architecture i386 (voir la [universelle de construction Bibliothèques natives](#building_native) section ci-dessus).

Pour vérifier les architectures prises en charge par une bibliothèque donnée, utilisez la commande suivante dans le Terminal :

```bash
lipo -info /full/path/to/libraryname.a
```

Où `/full/path/to/` est le chemin d’accès complet à la bibliothèque consommée et `libraryname.a` est le nom de la bibliothèque en question.

Si vous avez la source à la bibliothèque, vous devez compiler et l’offre groupée de l’architecture i386 ainsi, si vous souhaitez tester votre application sur le simulateur iOS.

### <a name="linking-your-library"></a>Votre bibliothèque de liaison

N’importe quelle bibliothèque de fournisseurs tiers que vous consommez doit être liée statiquement à votre application. 

Si vous souhaitez lier statiquement la bibliothèque « libMyLibrary.a » que vous avez obtenu à partir d’Internet ou d’une build avec Xcode, vous devez effectuer quelques opérations :

-  Importer la bibliothèque dans votre projet
-  Configurer Xamarin.iOS pour la bibliothèque de liens
-  Accéder aux méthodes de la bibliothèque.


Pour **placer la bibliothèque dans votre projet**, sélectionnez le projet dans l’Explorateur de solutions et appuyez sur **Option + commande + a**. Accédez à la libMyLibrary.a et ajoutez-le au projet. Lorsque vous y êtes invité, indiquer à Visual Studio pour Mac ou Visual Studio pour le copier dans le projet. Après l’avoir ajoutée, trouver le libFoo.a dans le projet, cliquez avec le bouton droit sur celui-ci et définir le **Action de génération** à **aucun**.

Pour **Xamarin.iOS configurer à la bibliothèque de liens**, sur les options de projet pour votre fichier exécutable final (pas la bibliothèque elle-même, mais le programme final), vous devez ajouter dans **Build iOS**d’argument supplémentaire (il s’agit dans le cadre de vos options de projet) les «-gcc_flags « option suivie d’une chaîne entre guillemets contenant toutes les bibliothèques supplémentaires sont nécessaires pour votre programme, par exemple :

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

L’exemple ci-dessus sera liée **libMyLibrary.a**

Vous pouvez utiliser `-gcc_flags` pour spécifier n’importe quel jeu d’arguments de ligne de commande à passer au compilateur GCC permettent de faire le lien de votre fichier exécutable final. Par exemple, cette ligne de commande, référence également l’infrastructure CFNetwork :

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Si votre bibliothèque native contient du code C++ vous devez également passer l’indicateur - cxx dans votre « Arguments supplémentaires » afin que Xamarin.iOS sait qu’il doit pour utiliser le compilateur correct. Pour C++, les options précédentes ressemblerait à :

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>L’accès aux méthodes C à partir de C&#35;

Il existe deux types de bibliothèques natives sur iOS :

-  Des bibliothèques partagées qui font partie du système d’exploitation.

-  Bibliothèques statiques que vous envoyez avec votre application.


Pour accéder aux méthodes définies dans l’un d'entre eux, vous utilisez [fonctionnalité de P/Invoke de Mono](https://www.mono-project.com/docs/advanced/pinvoke/) qui est la même technologie que vous utiliseriez dans .NET, qui est à peu près :

-  Déterminer la fonction C que vous souhaitez appeler
-  Déterminer sa signature
-  Déterminer la bibliothèque dans laquelle il se trouve dans
-  Écrire la déclaration P/Invoke appropriée

Lorsque vous utilisez P/Invoke, vous devez spécifier le chemin d’accès de la bibliothèque que vous établissez une liaison. Quand à l’aide d’iOS des bibliothèques partagées, vous pouvez soit coder en dur le chemin d’accès, ou vous pouvez utiliser les constantes de commodité, nous avons défini dans notre `Constants`, ces constantes doivent couvrir les bibliothèques iOS partagés.

Par exemple, si vous souhaitez appeler la méthode UIRectFrameUsingBlendMode à partir de la bibliothèque de UIKit d’Apple qui possède cette signature dans C:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Votre déclaration P/Invoke ressemblerait à ceci :

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Le Constants.UIKitLibrary est simplement une constante définie en tant que « / System/Library/Frameworks/UIKit.framework/UIKit », le point d’entrée permet de spécifier éventuellement le nom externe (UIRectFramUsingBlendMode) lors de l’exposition d’un autre nom dans C#, le RectFrameUsingBlendMode plus court.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>L’accès aux Dylibs C

Si vous avez besoin de consommer un Dylib C dans votre application Xamarin.iOS, il est un peu de configuration supplémentaire requise avant d’appeler le `DllImport` attribut.

Par exemple, si nous avons un `Animal.dylib` avec un `Animal_Version` méthode que nous appellerons dans notre application, nous devons informer Xamarin.iOS de l’emplacement de la bibliothèque avant d’essayer de le consommer.

Pour ce faire, modifiez le `Main.CS` de fichier et qu’elle ressemble à ce qui suit :

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Où `/full/path/to/` est le chemin complet vers le fichiers Dylib consommée. Avec ce code en place, nous pouvons ensuite lier à la `Animal_Version` méthode comme suit :

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Bibliothèques statiques

Dans la mesure où vous pouvez uniquement utiliser des bibliothèques statiques sur iOS, il n’existe aucune bibliothèque partagée externe pour créer un lien avec, par conséquent, le paramètre de chemin d’accès dans l’attribut DllImport doit utiliser le nom spécial `__Internal` (Remarquez le double trait de soulignement au début du nom) par opposition à le nom de chemin d’accès.

Cela force DllImport pour rechercher le symbole de la méthode que vous référencez dans le programme en cours, au lieu de tenter de charger à partir d’une bibliothèque partagée.

