---
title: Référencement des bibliothèques natives
ms.topic: article
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: 99e565c2268bec6d80c4976e604333cbd2f160a3
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="referencing-native-libraries"></a>Référencement des bibliothèques natives

Xamarin.iOS prend en charge la liaison avec les bibliothèques C natifs et les bibliothèques Objective-C. Ce document explique comment lier vos bibliothèques C natifs avec votre projet Xamarin.iOS. Pour plus d’informations sur la même opération pour les bibliothèques Objective-C, consultez notre [Objective-C, les Types de liaison](~/ios/platform/binding-objective-c/index.md) document.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Création des bibliothèques natives universels (i386, ARMv7 et ARM64)

Il est souvent souhaitable de génération de vos bibliothèques natives pour chacune des plateformes prises en charge pour le développement iOS (i386 pour les appareils eux-mêmes pour le simulateur et ARMv7/ARM64). Si vous avez déjà un projet Xcode pour votre bibliothèque, il est très facile à faire.

Pour générer la version i386 de votre bibliothèque native, exécutez la commande suivante à partir d’un terminal :

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Il en résulte une bibliothèque statique native sous `MyProject.xcodeproj/build/Release-iphonesimulator/`. Copier (ou déplacer) le fichier d’archive de bibliothèque (libMyLibrary.a) à un endroit sûr pour une utilisation ultérieure, en lui donnant un nom unique (tel que **libMyLibrary-i386.a**) afin qu’il ne sont en conflit avec les versions arm64 et armv7 de la même bibliothèque que vous devrez Générez ensuite.

Pour générer la version ARM64 de la bibliothèque native, exécutez la commande suivante :

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Cette heure se trouve dans la bibliothèque native générée `MyProject.xcodeproj/build/Release-iphoneos/`. Une fois encore, copier (ou déplacer) ce fichier dans un emplacement sûr, en le renommant à quelque chose comme **libMyLibrary-arm64.a** afin qu’il ne sera pas entrer en conflit.

Maintenant, générez la version ARMv7 de la bibliothèque :

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Copier (ou déplacer) le fichier de bibliothèque au même emplacement vous avez déplacé les 2 autres versions de la bibliothèque, à quelque chose comme **libMyLibrary-armv7.a**.

Pour rendre un universal binaire, vous pouvez utiliser la `lipo` outil comme suit :

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Cette opération crée `libMyLibrary.a` qui sera une bibliothèque (fat) universelle qui sera appropriée à utiliser pour toutes les cibles de développement iOS.


### <a name="missing-required-architecture-i386"></a>I386 d’Architecture requis manquant

Si vous obtenez un `does not implement methodSignatureForSelector` ou `does not implement doesNotRecognizeSelector` message dans la sortie de l’exécution lorsque vous tentez d’utiliser une bibliothèque Objective-C dans iOS Simulator, votre bibliothèque probablement n’a pas été compilée pour l’architecture i386 (consultez la [construction universel Bibliothèques natives](#building_native) section ci-dessus).

Pour vérifier les architectures prises en charge par une bibliothèque donnée, utilisez la commande suivante dans le Terminal :

```bash
lipo -info /full/path/to/libraryname.a
```

Où `/full/path/to/` est le chemin d’accès complet à la bibliothèque consommée et `libraryname.a` est le nom de la bibliothèque en question.

Si vous disposez de la source à la bibliothèque, vous devez compiler et regrouper à l’architecture i386 ainsi, si vous souhaitez tester votre application sur le simulateur iOS.

### <a name="linking-your-library"></a>Liaison de votre bibliothèque.

Toute bibliothèque tierce qui vous consommez doit être liée de manière statique avec votre application. 

Si vous souhaitez lier statiquement la bibliothèque « libMyLibrary.a » que vous avez obtenu à partir d’Internet ou d’une build avec Xcode, vous devez effectuer certaines opérations :

-  Rend la bibliothèque de votre projet
-  Configurer Xamarin.iOS pour lier la bibliothèque
-  Accéder aux méthodes de la bibliothèque.


Pour **rend la bibliothèque de votre projet**, sélectionnez le projet dans l’Explorateur de solutions et appuyez sur **commande + Option + a**. Accédez à la libMyLibrary.a et l’ajouter au projet. Lorsque vous y êtes invité, indiquez à Visual Studio pour Mac ou Visual Studio pour le copier dans le projet. Après l’avoir ajoutée, trouver le libFoo.a dans le projet, cliquez avec le bouton droit sur celui-ci et définir le **Action de génération** à **aucun**.

Pour **Xamarin.iOS configurer à la bibliothèque de liens**, sur les options de projet de votre exécutable final (pas la bibliothèque elle-même, mais le programme final), vous devez ajouter dans **Build iOS**d’argument supplémentaire (qui sont dans le cadre de vos options de projet) la «-gcc_flags « option suivie d’une chaîne entre guillemets qui contient toutes les bibliothèques supplémentaires sont nécessaires pour votre programme, par exemple :

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

L’exemple ci-dessus établit un lien **libMyLibrary.a**

Vous pouvez utiliser `-gcc_flags` pour spécifier n’importe quel jeu d’arguments de ligne de commande à passer au compilateur GCC utilisé pour effectuer le lien final de votre fichier exécutable. Par exemple, cette ligne de commande, fait également référence à l’infrastructure CFNetwork :

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Si votre bibliothèque native contient le code C++ que vous devez également passer l’indicateur - cxx votre supplémentaire « Arguments » afin que Xamarin.iOS sache qu’il peut pour utiliser le compilateur correct. Pour C++, les options précédentes ressemble à :

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>L’accès aux méthodes C à partir de C&#35;

Il existe deux types de bibliothèques natives sur iOS :

-  Des bibliothèques partagées qui font partie du système d’exploitation.

-  Bibliothèques statiques qui vous sont fournis avec votre application.


Pour accéder aux méthodes définies dans l’un d'entre eux, vous utilisez [fonctionnalité de P/Invoke de Mono](http://www.mono-project.com/docs/advanced/pinvoke/) qui est la même technologie que vous utiliseriez dans .NET, qui est à peu près :

-  Déterminer la fonction C que vous souhaitez appeler
-  Déterminer sa signature
-  Déterminer la bibliothèque dans laquelle il réside dans
-  Écrire la déclaration P/Invoke appropriée


Lorsque vous utilisez P/Invoke, vous devez spécifier le chemin d’accès de la bibliothèque que vous établissez une liaison. Quand utiliser iOS bibliothèques partagées, vous pouvez soit coder en dur le chemin d’accès, ou vous pouvez utiliser les constantes de commodité que nous avons défini dans notre [classe Constants](https://developer.xamarin.com/api/type/Constants/), ces constantes doivent couvrir les bibliothèques iOS partagés.

Par exemple, si vous souhaitez appeler la méthode UIRectFrameUsingBlendMode à partir de la bibliothèque de UIKit d’Apple qui possède cette signature dans C:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Votre déclaration P/Invoke ressemble à ceci :

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Le Constants.UIKitLibrary est simplement une constante définie en tant que « / System/Library/Frameworks/UIKit.framework/UIKit », le point d’entrée permet de spécifier éventuellement le nom externe (UIRectFramUsingBlendMode) lors de l’exposition d’un autre nom dans c#, le plus court RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>L’accès à des Dylibs de C

Si vous devez utiliser pour consommer un Dylib C dans votre application Xamarin.iOS, il est un peu de configuration supplémentaire requise avant d’appeler le `DllImport` attribut.

Par exemple, si nous avons un `Animal.dylib` avec un `Animal_Version` méthode que nous appellerons dans notre application, nous devons informer Xamarin.iOS de l’emplacement de la bibliothèque avant de tenter de l’utiliser.

Pour ce faire, modifiez le `Main.CS` de fichier et le rendre l’aspect suivant :

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Où `/full/path/to/` est le chemin complet vers le Dylib consommé. Avec ce code en place, nous pouvons ensuite associer la `Animal_Version` méthode comme suit :

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Bibliothèques statiques

Étant donné que vous ne pouvez utiliser des bibliothèques statiques sur iOS, n’est aucune bibliothèque partagée externe pour lier avec, donc le paramètre de chemin d’accès dans l’attribut DllImport doit utiliser le nom spécial `__Internal` (Notez les caractères double trait de soulignement au début du nom) par opposition à le nom de chemin d’accès.

Cette opération force DllImport pour rechercher le symbole de la méthode que vous référencez dans le programme en cours, au lieu de tenter de charger à partir d’une bibliothèque partagée.

