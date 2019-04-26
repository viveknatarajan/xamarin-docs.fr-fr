---
title: Incorporation .NET meilleures pratiques pour Objective-C
description: Ce document décrit différentes meilleures pratiques pour l’utilisation de l’incorporation de .NET avec Objective-C. Il aborde l’exposition d’un sous-ensemble du code managé, exposer une API chunkier, d’affectation de noms et bien plus encore.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 33138b7858b8bc04a5be30f9fad1709e916f5575
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364093"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Incorporation .NET meilleures pratiques pour Objective-C

Il s’agit d’un brouillon et ne peut pas être synchronisé avec les fonctionnalités actuellement prises en charge par l’outil. Nous espérons que ce document sera évoluer séparément et finalement correspond à l’outil finale, par exemple, nous vous suggérons les meilleures approches à long terme - solutions de contournement pas immédiats.

Une grande partie de ce document s’applique également à d’autres langages pris en charge. Toutefois, toutes fournies sont des exemples dans C# et Objective-C.

## <a name="exposing-a-subset-of-the-managed-code"></a>Exposer un sous-ensemble du code managé

La bibliothèque/framework natif généré contient le code Objective-C pour appeler chacune des API managées qui sont exposés. L’API plus vous surface (rendre public) puis supérieure natif _glue_ bibliothèque deviendra.

Il peut être judicieux de créer un assembly différent et plus petit, pour exposer uniquement les API requises pour le développement natif. Cette façade vous permettra également mieux contrôler la visibilité, d’affectation de noms, vérification des erreurs... du code généré.

## <a name="exposing-a-chunkier-api"></a>Exposer une API chunkier

Il existe un prix à payer pour effectuer la transition du code natif à managé (et serveur). Par conséquent, il est préférable d’exposer _regroupée plutôt que bavard_ API pour les développeurs natifs, par exemple,

**Chatty**

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

**Chunky**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Étant donné que le nombre de transitions est plus petit, les performances seront améliorées. Elle requiert également moins de code à générer, donc cela génère une bibliothèque native plus petits également.

## <a name="naming"></a>Attribution des noms

Affectation de noms est un des deux problèmes plus difficiles en informatique, les autres étant cache erreurs invalidation et off-par-1. J’espère que l’incorporation de .NET peut vous protègent contre tout sauf d’affectation de noms.

### <a name="types"></a>Types

Objective-C ne prend pas en charge les espaces de noms. En général, ses types sont préfixés avec 2 (pour Apple) ou 3 (pour la 3e parties) comme préfixe, de caractère `UIView` pour vue de UIKit, qui dénote le framework.

Pour les types .NET ignorer l’espace de noms n’est pas possible car elle peut introduire des noms en double, ou manque de clarté. Cela rend les types .NET existants très longs, par exemple

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

rend plus conviviale Objective-C à utiliser, par exemple :

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Méthodes

Les noms de .NET même appropriés n’est peut-être pas idéales pour une API Objective-C.

Conventions de nommage dans Objective-C sont différentes de .NET (casse mixte au lieu de la casse pascal, plus détaillé).
Veuillez lire le [indications de codage pour Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

À partir du point de vue d’un développeur Objective-C, une méthode avec un `Get` préfixe implique que vous ne possédez pas de l’instance, c'est-à-dire le [obtenir la règle](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Cette règle d’affectation de noms n’a aucune correspondance dans le monde du garbage collector .NET ; une méthode .NET avec un `Create` préfixe se comporte de façon identique dans .NET. Toutefois, pour les développeurs Objective-C, il normalement signifie que vous possédez l’instance retournée, c'est-à-dire le [créer règle](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Exceptions

Il est assez courant dans .NET d’utiliser des exceptions largement pour signaler des erreurs. Toutefois, ils sont lentes et pas tout à fait identique dans Objective-C. Chaque fois que possible vous devez les masquer par rapport au développeur Objective-C.

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

Dans .NET un constructeur doit soit réussir et de retourner un (_J’espère que_) instance valide ou lever une exception.

En revanche, Objective-C permet `init*` pour retourner `nil` lorsqu’une instance ne peut pas être créée. Il s’agit d’un modèle commun, mais pas général, utilisé dans la plupart des infrastructures de d’Apple. Dans d’autres cas un `assert` peut se produire (et arrêter le processus actuel).

Le Générateur de suivre les mêmes `return nil` de modèle pour générée `init*` méthodes. Si une exception managée est levée, elle sera imprimée (à l’aide de `NSLog`) et `nil` s’affichera à l’appelant.

## <a name="operators"></a>Opérateurs

Objective-C n’autorise pas les opérateurs surchargée en tant que C# effectue, par conséquent, ils sont convertis en sélecteurs de classe.

[« Convivial »](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) méthodes nommées sont générées plutôt que les surcharges d’opérateur lorsque trouvé et peuvent produire un plus facile à consommer des API.

Les classes qui substituent les opérateurs `==` et/ou `!=` doivent substituer la méthode de Equals (Object) standard.
