---
title: Méthodes conseillées pour ObjC Embeddinator-4000
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ca5face9865c60fabe8359c2bf356d5d5555f517
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="embeddinator-4000-best-practices-for-objc"></a>Méthodes conseillées pour ObjC Embeddinator-4000

Il s’agit d’un brouillon et ne peut pas être synchronisé avec les fonctionnalités actuellement prises en charge par l’outil. Nous espérons que ce document sera évoluer séparément et finalement correspond à l’outil final, par exemple, nous vous suggérons les meilleures approches à long terme - solutions de contournement pas immédiatement.

Une grande partie de ce document s’applique également à d’autres langages pris en charge. Toutefois, tous les fournies sont des exemples en c# et objectif-C.

## <a name="exposing-a-subset-of-the-managed-code"></a>Exposition d’un sous-ensemble du code managé

La bibliothèque/framework natif généré contient le code Objective-C pour appeler chacune des API managées qui sont exposés. L’API plus vous surface (rendre public) puis supérieure natif _collage_ bibliothèque deviendra.

Il peut être judicieux de créer un assembly différent, plus petit, afin d’exposer uniquement les API requises pour le développeur natif. Cette façade également vous permettra de mieux contrôler la visibilité, d’affectation de noms, vérification des erreurs... du code généré.

## <a name="exposing-a-chunkier-api"></a>Exposition d’une API chunkier

Il existe un prix à payer pour effectuer la transition du code natif managé (et arrière). Par conséquent, il est préférable d’exposer _messages plutôt que bavard_ API pour les développeurs natives, par exemple,

**Bavard**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Télémesure volumineuse**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Étant donné que le nombre de transitions est plus petit, les performances seront améliorées. Elle requiert également moins de code à générer, donc cela génère une bibliothèque native plus petite également.

## <a name="naming"></a>Attribution des noms

Opérations d’affectation de noms est un des deux problèmes plus difficiles en informatique, les autres étant cache erreurs invalidation et off-par-1. Nous espérons que .NET incorporation peut empêche tout sauf d’affectation de noms.

### <a name="types"></a>Types

Objective-C ne prend pas en charge les espaces de noms. En général, ses types sont préfixés avec 2 (pour Apple) ou 3 (pour les parties 3e) caractère de préfixe, tel que `UIView` pour affichage d’UIKit, qui désigne le framework.

Pour les types .NET ignorer l’espace de noms n’est pas possible car il peut introduire des noms dupliqués ou prête à confusion. Cela rend les types .NET existants très longs, par exemple

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

serait utilisée comme :

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Toutefois vous pouvez ré-exposer le type en tant que :

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

rend plus Objective-C compatible à utiliser, par exemple :

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Méthodes

Les noms de .NET même bons n’est peut-être pas idéales d’API Objective-C.

Conventions d’affectation de noms dans Objective-C sont différentes des .NET (casse mixte au lieu de la casse pascal, plus détaillé).
Veuillez lire les [indications de codage pour/Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

À partir du point de vue d’un développeur Objective-C, une méthode avec un `Get` préfixe implique que vous ne possédez pas de l’instance, c'est-à-dire le [obtenir une règle](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Cette règle d’affectation de noms a pas de correspondance dans le monde du garbage collector .NET ; une méthode .NET avec un `Create` préfixe se comporte de façon identique dans .NET. Toutefois, pour les développeurs Objective-C, cela signifie normalement vous possédez l’instance retournée, c'est-à-dire le [créer règle](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Exceptions

Il est tout à fait commont dans .NET pour utiliser des exceptions largement pour signaler des erreurs. Toutefois, ils sont lents et pas tout à fait identiques dans ObjC. Chaque fois que possible les doit masquer du développeur Objective-C.

Par exemple, le .NET `Try` modèle sera beaucoup plus facile à utiliser à partir de code Objective-C :

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

par rapport à

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>Exceptions à l’intérieur `init*`

Dans .NET un constructeur doit soit réussir et de retourner un (_espère_) instance valide ou lève une exception.

En revanche, permet de Objective-C `init*` pour retourner `nil` lorsqu’une instance ne peut pas être créée. Il s’agit d’un modèle commun, mais pas général, utilisé dans la plupart des infrastructures de d’Apple. Dans d’autres cas un `assert` peut se produire (et terminez le processus actuel).

Le Générateur de suivre le même `return nil` de modèle de générée `init*` méthodes. Si une exception managée est levée, il sera imprimé (à l’aide de `NSLog`) et `nil` s’affichera à l’appelant.

## <a name="operators"></a>Opérateurs

Objective-C n’autorise pas les opérateurs pour être surchargés comme c#, ceux-ci sont convertis en les sélecteurs de classe.

[« Convivial »](/dotnet/standard/design-guidelines/operator-overloads/) méthode nommée sont générés, plutôt que les surcharges d’opérateur lorsque trouvé et peuvent produire un plus faciles à consommer des API.

Les classes qui substituent les opérateurs `==` et/ou `!=` doit substituer la méthode Equals (objet) standard ainsi.
