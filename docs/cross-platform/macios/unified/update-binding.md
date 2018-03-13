---
title: "Migration d’une liaison vers l’API unifiée"
description: "Cet article décrit les étapes requises pour mettre à jour un projet de liaison de Xamarin existant pour prendre en charge les API unifiée pour les applications Xamarin.IOS et Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2a04dc047674b67b8f21571ed9e7890ddf773f64
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Migration d’une liaison vers l’API unifiée

_Cet article décrit les étapes requises pour mettre à jour un projet de liaison de Xamarin existant pour prendre en charge les API unifiée pour les applications Xamarin.IOS et Xamarin.Mac._

## <a name="overview"></a>Vue d'ensemble

En commençant le 1er février 2015 Apple exige que tous les envois de nouveau à l’iTunes et Mac App Store les applications 64 bits. Par conséquent, toute nouvelle application Xamarin.iOS ou Xamarin.Mac devrez être à l’aide de la nouvelle API unifiée au lieu des MonoTouch classique et MonoMac APIs existant pour prendre en charge 64 bits.

En outre, un projet de liaison Xamarin doit prennent également en charge les nouvelles API unifiée à inclure dans un projet de Xamarin.Mac ou le Xamarin.iOS de 64 bits. Cet article décrit les étapes requises pour mettre à jour un projet existant de la liaison pour utiliser l’API unifiée.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont nécessaire pour terminer les étapes présentées dans cet article :

 -  **Visual Studio pour Mac** -la version la plus récente de Visual Studio pour Mac installé et configuré sur l’ordinateur de développement.
 -  **Apple Mac** : une Apple mac est nécessaire pour générer des projets de liaison pour iOS et Mac.

Les projets de liaison ne sont pas pris en charge dans Visual studio sur un ordinateur Windows.

## <a name="modify-the-using-statements"></a>Modifier les instructions Using

Les API unifiée rend plus facile que jamais à partager du code entre Mac et iOS, ainsi que ce qui permet de prendre en charge 32 et 64 bits des applications avec le même binaire. En supprimant le _MonoMac_ et _MonoTouch_ préfixes des espaces de noms, partage plus simple s’effectue entre les projets d’application Xamarin.Mac et Xamarin.iOS.

Par conséquent, nous devrons modifier un de nos contrats de liaison (et d’autres `.cs` fichiers dans notre projet de liaison) pour supprimer la _MonoMac_ et _MonoTouch_ préfixes à partir de notre `using` instructions.

Par exemple, étant donné ce qui suit à l’aide d’instructions dans un contrat de liaison :

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Nous serait suppriment le `MonoTouch` préfixe résultant dans les éléments suivants :

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Là encore, nous devons faire pour toute `.cs` fichier dans notre projet de liaison. Cette modification en place, l’étape suivante consiste à mettre à jour notre projet de liaison pour utiliser les nouveaux Types de données natif.

Pour plus d’informations sur l’API unifiée, consultez le [API unifiée](~/cross-platform/macios/unified/index.md) documentation. Pour plus d’informations sur la prise en charge 32 et 64 bits les applications et informations sur les infrastructures de voir les [32 et 64 bits des considérations relatives à la plateforme](~/cross-platform/macios/32-and-64/index.md) documentation.

## <a name="update-to-native-data-types"></a>Mise à jour pour les Types de données natifs

Objective-C mappe le `NSInteger` type de données à `int32_t` sur les systèmes 32 bits et en `int64_t` sur les systèmes 64 bits. Pour faire correspondre ce comportement, la nouvelle API unifiée remplace les utilisations précédentes de `int` (qui dans .NET est définie comme étant toujours `System.Int32`) à un type de données : `System.nint`.

Avec la nouvelle `nint` introduit de type de données, l’API unifiée le `nuint` et `nfloat` types, un mappage à la `NSUInteger` et `CGFloat` également les types.

Étant donné ce qui précède, nous devons examiner nos API et assurez-vous que n’importe quelle instance de `NSInteger`, `NSUInteger` et `CGFloat` que nous avons précédemment mappé à `int`, `uint` et `float` mis à jour vers la nouvelle `nint`, `nuint` et `nfloat` types.

Par exemple, prenons une définition de méthode Objective-C de :

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Si le contrat de liaison précédente a la définition suivante :

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Nous serait mise à jour de la nouvelle liaison soit :

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Si nous sommes le mappage vers une bibliothèque de tiers 3e de version plus récente de ce que nous avions initialement lié à, vous devez examiner le `.h` pour la bibliothèque et les voir si les fichiers d’en-tête existants, explicites les appels à `int`, `int32_t`, `unsigned int`, `uint32_t` ou `float` ont été mis à niveau un `NSInteger`, `NSUInteger` ou `CGFloat`. Dans ce cas, les mêmes modifications à la `nint`, `nuint` et `nfloat` types seront doivent être apportées à leurs mappages ainsi.

Pour plus d’informations sur ces modifications de types de données, consultez la [Types natifs](~/cross-platform/macios/nativetypes.md) document.

## <a name="update-the-coregraphics-types"></a>Mettre à jour les Types de CoreGraphics

Les types de données de point, la taille et rectangle qui sont utilisés avec `CoreGraphics` utiliser 32 ou 64 bits en fonction de l’appareil qu’ils sont exécutent sur. Lorsque Xamarin liée à l’origine iOS et Mac API nous avons utilisé les structures de données existantes qui se sont produites pour faire correspondre les types de données dans `System.Drawing` (`RectangleF` par exemple).

En raison de la configuration requise pour prendre en charge 64 bits et les nouveaux types de données en mode natif, les ajustements suivants devront être effectuées au code existant lors de l’appel `CoreGraphic` méthodes :

- **CGRect** -utilisez `CGRect` au lieu de `RectangleF` lorsque la définition flottante point régions rectangulaires.
- **CGSize** -utilisez `CGSize` au lieu de `SizeF` lors de la définition des tailles en points flottante (largeur et hauteur).
- **CGPoint** -utilisez `CGPoint` au lieu de `PointF` lorsque la définition flottante point (coordonnées X et Y).

Étant donné ce qui précède, nous devons examiner nos API et assurez-vous que n’importe quelle instance de `CGRect`, `CGSize` ou `CGPoint` qui a été précédemment lié à `RectangleF`, `SizeF` ou `PointF` être converti en type natif `CGRect`, `CGSize` ou `CGPoint` directement.

Par exemple, prenons un initialiseur Objective-C de :

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Si notre liaison précédente inclus le code suivant :

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Nous serait mise à jour de ce code :

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Toutes les modifications de code maintenant en place, nous devons modifier notre projet de liaison ou vérifiez le fichier à lier à l’API unifiée.

## <a name="modify-the-binding-project"></a>Modifier le projet de liaison

Comme dernière étape de mise à jour notre projet de liaison pour utiliser les API unifiée, nous devons modifier le `MakeFile` qui nous permet de générer le projet ou le Type de projet Xamarin (si nous créons une liaison à partir de Visual Studio pour Mac) et indiquez à _btouch_  à lier à l’API unifiée au lieu de ceux qui sont standard.


### <a name="updating-a-makefile"></a>Mise à jour d’un MakeFile

Si nous utilisons un makefile pour générer votre projet de liaison dans un Xamarin. DLL, nous devons inclure le `--new-style` l’option de ligne de commande et appelez `btouch-native` au lieu de `btouch`.

Étant donné ce qui suit `MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

Nous avons besoin de changer à partir de l’appel `btouch` à `btouch-native`, donc nous serait ajuster notre définition de macro comme suit :

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Nous serait de mettre à jour l’appel à `btouch` et ajouter la `--new-style` option comme suit :

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Nous pouvons maintenant exécuter notre `MakeFile` normalement pour générer la nouvelle version 64 bits de notre API.

### <a name="updating-a-binding-project-type"></a>Mise à jour d’un Type de projet de liaison

Si nous utilisons un Mac liaison de modèle de projet de Visual Studio pour créer notre API, nous devrons mettre à jour vers la nouvelle version d’API unifiée du modèle de projet de liaison. Pour ce faire, le plus simple consiste à démarrer un nouveau projet de liaison d’API unifiée et copie sur l’ensemble du code et des paramètres existants.

Effectuez ce qui suit :

1. Démarrez Visual Studio pour Mac.
2. Sélectionnez **fichier** > **nouveau** > **Solution...**
3. Dans la boîte de dialogue Nouvelle Solution, sélectionnez **iOS** > **API unifiée** > **iOS projet Binding**: 

    [![](update-binding-images/image01new.png "Dans la boîte de dialogue Nouvelle Solution, sélectionnez iOS / API unifiée / projet de liaison iOS")](update-binding-images/image01new.png#lightbox)
4. Dans la boîte de dialogue « Configurer votre nouveau projet » Entrez un **nom** pour le nouveau projet de liaison et cliquez sur le **OK** bouton.
5. Incluez la version 64 bits de bibliothèque Objective-C que vous allez créer des liaisons pour.
6. Copiez le code source à partir de votre projet de liaison d’API classique 32 bits existant (tel que le `ApiDefinition.cs` et `StructsAndEnums.cs` fichiers).
7. Apportez les modifications d’indiqués ci-dessus pour les fichiers de code source.

Toutes ces modifications en place, vous pouvez créer la nouvelle version 64 bits de l’API comme vous le feriez pour la version 32 bits.

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons montré les modifications qui doivent être apportées à un projet de liaison Xamarin existant pour prendre en charge les nouvelles API unifiée et les appareils de 64 bits et les étapes requises pour générer la nouvelle version compatible 64 bits d’une API.



## <a name="related-links"></a>Liens associés

- [Mac et iOS](~/cross-platform/macios/index.md)
- [API unifiée](~/cross-platform/macios/nativetypes.md)
- [Considérations relatives à la plate-forme 32/64 bits](~/cross-platform/macios/32-and-64/index.md)
- [La mise à niveau des applications iOS existante](~/cross-platform/macios/unified/updating-ios-apps.md)
- [API unifiée](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
