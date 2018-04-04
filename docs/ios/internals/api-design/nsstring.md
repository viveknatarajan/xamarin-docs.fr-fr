---
title: NSString
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 4b084c2f1066c5cfad90911d845aa7555c669130
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="nsstring"></a>NSString

La conception de Xamarin.iOS et Xamarin.Mac appelle pour l’API de l’utilisation d’exposer du type de chaîne de .NET natif, `string`, pour la manipulation de chaînes dans c# et d’autres langages de programmation .NET et pour exposer la chaîne en tant que le type de données exposé par l’API au lieu du `NSString` type de données.


Cela signifie que les développeurs doivent ont pas de conserver les chaînes qui sont destinés à être utilisés pour appeler Xamarin.iOS et de l’API de Xamarin.Mac (unifié) dans un type spécial (`Foundation.NSString`), ils peuvent continuer à utiliser de Mono `System.String` pour toutes les opérations et chaque fois que une API dans Xamarin.iOS ou Xamarin.Mac nécessite une chaîne, notre liaison API prend en charge les informations de marshaling.

Par exemple, la propriété de « texte » Objective-C sur une `UILabel` de type `NSString`, est déclaré comme suit :

```csharp
@property(nonatomic, copy) NSString *text
```

Ces informations sont exposées dans Xamarin.iOS en tant que :

```csharp
class UILabel {
    public string Text { get; set; }
}
```

En arrière-plan, l’implémentation de cette propriété marshale la chaîne c# dans une `NSString` et appelle le `objc_msgSend` méthode dans la même manière qu’Objective-C.

Il existe un certain nombre de tiers Objective-C API qui n’utilisent pas une `NSString`, mais à la place utiliser une chaîne C (un «*char*»). Dans ce cas, vous pouvez toujours utiliser le type de données de chaîne C#, mais vous devez utiliser le [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) pour informer le Générateur de liaison cette chaîne ne doit pas être marshalée en tant qu’attribut un `NSString`, mais à la place sous forme de chaîne C.

 <a name="Exceptions_to_the_Rule" />


## <a name="exceptions-to-the-rule"></a>Exceptions à la règle

Dans Xamarin.iOS et Xamarin.Mac, nous avons apporté une exception à cette règle. La décision entre lorsque nous exposons `string`s, et lorsque nous avons un except et exposer `NSString`s, est effectuée si le `NSString` méthode pourrait faire une comparaison de pointeur au lieu d’une comparaison de contenu.


Cela peut se produire lorsqu’une API Objective-C utilise publique `NSString` constante comme un jeton qui représente une action, au lieu de comparer le contenu réel de la chaîne.


Dans ce cas, `NSString` API est exposées, et il existe une minorité d’API qui a ce paramètre. Vous remarquerez également que les propriétés NSString sont exposées dans certaines classes. Ceux `NSString` propriétés exposées pour les éléments tels que des notifications. Ceux qui sont propriétés généralement ressemblent à ceci :

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Les notifications sont des clés qui sont utilisées pour la `NSNotification` classe lorsque vous souhaitez inscrire pour un événement particulier en cours de diffusion par le runtime.

Clés généralement se présenter comme suit :

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Un autre emplacement où `NSString`s sont exposés dans l’API n’est que les jetons qui sont utilisés en tant que paramètres à certaines API dans iOS ou OS X qui prennent `NSDictionary` objets en tant que paramètres. Le dictionnaire contient généralement `NSString` clés. Xamarin.iOS, par convention, ceux statique `NSString` propriétés en ajoutant le nom de « Clé ».
