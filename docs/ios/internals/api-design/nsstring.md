---
title: Chaîne NSString dans Xamarin.iOS et Xamarin.Mac
description: Ce document décrit comment Xamarin.iOS convertit en toute transparence les objets de chaîne NSString à C# lorsque cela n’arrive pas les objets, de chaîne.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cc9e3f992642f3cdc3d16fe6f829b6a6c06b50fc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277812"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Chaîne NSString dans Xamarin.iOS et Xamarin.Mac

La conception de Xamarin.iOS et Xamarin.Mac appelle pour l’utiliser l’API pour exposer du type de chaîne de .NET natif, `string`, pour la manipulation de chaîne dans C# et d’autres langages de programmation de .NET et pour exposer la chaîne en tant que le type de données exposé par l’API au lieu de le `NSString` type de données.

Cela signifie que les développeurs ne doivent pas obligé de conserver les chaînes qui sont destinées à être utilisées pour appeler dans Xamarin.iOS et Xamarin.Mac API (unifiée) dans un type spécial (`Foundation.NSString`), ils peuvent continuer à utiliser de Mono `System.String` pour toutes les opérations et chaque fois que une API dans Xamarin.iOS ou Xamarin.Mac nécessite une chaîne, notre liaison API prend en charge les informations de marshaling.

Par exemple, la propriété « text » de Objective-C sur une `UILabel` de type `NSString`, est déclarée comme suit :

```objc
@property(nonatomic, copy) NSString *text
```

Cela est exposé dans Xamarin.iOS en tant que :

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Dans les coulisses, l’implémentation de cette propriété marshale le C# chaîne en un `NSString` et appelle le `objc_msgSend` méthode dans la même manière qu’Objective-C.

Un certain nombre d’API Objective-C-tiers qui n’utilisent pas un `NSString`, mais à la place utiliser une chaîne C (une «*char*»). Dans ce cas, vous pouvez toujours utiliser le C# la chaîne de type de données, mais vous devez utiliser le [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) attribut pour informer le Générateur de liaison cette chaîne ne doit pas être marshalée en tant qu’un `NSString`, mais sous forme de chaîne C à la place.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>Exceptions à la règle

Dans Xamarin.iOS et Xamarin.Mac, nous avons apporté une exception à cette règle. La décision entre lorsque nous exposons `string`s, et lorsque nous rendre un except et exposer `NSString`s, est effectuée si le `NSString` méthode pourrait faire une comparaison de pointeur au lieu d’une comparaison de contenu.

Cela peut se produire lorsqu’une API Objective-C utilise un public `NSString` constante en tant que jeton qui représente une action, au lieu de comparer le contenu réel de la chaîne.

Dans ce cas, `NSString`  API est exposées, et il existe une minorité d’API que vous le faire. Vous remarquerez également que les propriétés de chaîne NSString sont exposées dans des classes. Ceux `NSString` propriétés sont exposées pour les éléments tels que les notifications. Ce sont les propriétés généralement ressemblent à ceci :

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
Les notifications sont des clés qui sont utilisées pour la `NSNotification` classe lorsque vous souhaitez inscrire pour un événement particulier en cours de diffusion par le runtime.

Clés ressemblent généralement quelque chose comme ceci :

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Un autre emplacement où `NSString`s sont exposés dans l’API est sous forme de jetons qui sont utilisés comme paramètres à certaines API dans iOS ou OS X qui acceptent `NSDictionary` objets en tant que paramètres. Le dictionnaire contient généralement `NSString` clés. Xamarin.iOS, par convention, ceux statique `NSString` propriétés en ajoutant le nom de « Clé ».
