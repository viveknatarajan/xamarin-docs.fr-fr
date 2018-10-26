---
title: Prise en charge objective-C
description: Ce document fournit une description de la prise en charge pour Objective-C dans l’incorporation de .NET. Il aborde le comptage de références automatique, chaîne NSString, protocoles, NSObject protocole, exceptions et bien plus encore.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 48caa70cf2bd408f8afc673b400f7d5a4369e108
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110717"
---
# <a name="objective-c-support"></a>Prise en charge objective-C

## <a name="specific-features"></a>Fonctionnalités spécifiques

La génération de Objective-C dispose de quelques fonctionnalités spéciales qui méritent votre attention.

### <a name="automatic-reference-counting"></a>Comptage de références automatique

L’utilisation de comptage de référence automatique (ARC) est **requis** pour appeler les liaisons générés. Projet à l’aide d’une bibliothèque basée sur l’incorporation de .NET doit être compilé avec `-fobjc-arc`.

### <a name="nsstring-support"></a>Prise en charge de la chaîne NSString

Les API qui exposent `System.String` types sont convertis en `NSString`. Cela facilite la gestion de la mémoire que lorsque le traitement de `char*`.

### <a name="protocols-support"></a>Prise en charge des protocoles

Les interfaces gérées sont converties en protocoles Objective-C où tous les membres sont `@required`.

### <a name="nsobject-protocol-support"></a>Prise en charge du protocole NSObject

Par défaut, le hachage par défaut et l’égalité de .NET et le runtime Objective-C sont supposées pour être interchangeables, qu’elles partagent une sémantique similaire.

Quand un type managé remplace `Equals(Object)` ou `GetHashCode`, cela signifie généralement que le comportement par défaut (.NET) n’était pas suffisant ; cela implique que le comportement de Objective-C par défaut est probablement pas suffisamment soit.

Dans ce cas, le générateur remplace le [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) (méthode) et [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) propriété définie dans le [ `NSObject` protocole](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Cela permet l’implémentation managée personnalisée à utiliser en toute transparence à partir de code Objective-C.

### <a name="exceptions-support"></a>Prise en charge des exceptions

En passant `--nativeexception` en tant qu’argument à `objcgen` convertira les exceptions managées dans les exceptions Objective-C qui peuvent être interceptées et traitées. 

### <a name="comparison"></a>Comparaison

Des types qui implémentent gérés `IComparable` (ou sa version générique `IComparable<T>`) produira des méthodes conviviales Objective-C qui retournent un `NSComparisonResult` et accepter un `nil` argument. Cela rend l’API générée plus conviviale pour les développeurs Objective-C. Exemple :

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Categories

Les méthodes sont converties en catégories des extensions managées. Par exemple, les méthodes d’extension suivantes sur `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

Créez une catégorie d’Objective-C comme celle-ci :

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Lorsqu’un type managé unique s’étend à plusieurs types, plusieurs catégories Objective-C sont générés.

### <a name="subscripting"></a>Indices

Propriétés indexées gérées sont converties en mise en indice de l’objet. Exemple :

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

créerait Objective-C similaire à :

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

qui peut être utilisé via la syntaxe en indice Objective-C :

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

Selon le type de votre indexeur, indexée ou par clé mise en indice sera généré le cas échéant.

Cela [article](http://nshipster.com/object-subscripting/) est une excellente introduction à la mise en indice.

## <a name="main-differences-with-net"></a>Principales différences avec .NET

### <a name="constructors-vs-initializers"></a>Initialiseurs de constructeurs vs

En Objective-C, vous pouvez appeler les de l’initialiseur de prototypes de toutes les classes parent dans la chaîne d’héritage, sauf si elle est marquée comme non disponible (`NS_UNAVAILABLE`).

Dans C# vous devez déclarer explicitement un membre de constructeur à l’intérieur d’une classe, ce qui signifie que les constructeurs ne sont pas hérités.

Pour exposer la représentation sous forme de droite de la C# API et Objective-C, `NS_UNAVAILABLE` est ajouté à n’importe quel initialiseur qui n’est pas présent dans la classe enfant à partir de la classe parente.

C#API :

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Objective-C sont apparues API :

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Ici, `initWithId:` a été marqué comme non disponible.

### <a name="operator"></a>Opérateur

Objective-C ne prend pas en charge en tant que la surcharge d’opérateur C# effectue, par conséquent, les opérateurs sont convertis en les sélecteurs de classe :

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

par celle-ci :

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Toutefois, certains langages .NET ne gèrent pas la surcharge d’opérateur, il est courant d’inclure également un [« convivial »](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) nommée méthode en plus de la surcharge d’opérateur.

Si la version de l’opérateur et la version « conviviale » se trouvent, seule la version conviviale sera générée, car ils génèrent le même nom Objective-C.

```csharp
public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}

public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}
```

devient :

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Opérateur d’égalité

Dans l’opérateur général `==` dans C# est traité comme un opérateur général comme indiqué ci-dessus.

Toutefois, si l’opérateur d’égalité « convivial » est trouvé, les deux opérateur `==` et opérateur `!=` va être ignorée dans la génération.

### <a name="datetime-vs-nsdate"></a>Date/heure vs NSDate

À partir de la [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) documentation :

> `NSDate` objets encapsulent un point unique dans le temps, indépendants de tout système calendrical particulier ou d’un fuseau horaire. Objets date sont immuables, représentant un intervalle de temps invariant par rapport à une date de référence absolue (00 : 00:00 UTC 1er janvier 2001).

En raison `NSDate` référencer date, toutes les conversions entre elle et `DateTime` doit être effectuée au format UTC.

#### <a name="datetime-to-nsdate"></a>DateTime à NSDate

Lors de la conversion à partir de `DateTime` à `NSDate`, le `Kind` propriété sur `DateTime` est pris en compte :

|Genre|Résultats|
|---|---|
|`Utc`|Conversion est effectuée à l’aide de l’élément fourni `DateTime` en l’état de l’objet.|
|`Local`|Le résultat de l’appel `ToUniversalTime()` dans les `DateTime` objet est utilisé pour la conversion.|
|`Unspecified`|Fourni `DateTime` objet est censé pour être au format UTC, donc même comportement lorsque `Kind` est `Utc`.|

La conversion utilise la formule suivante :

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

Dans cette formule : 

- `NSDateReferenceDateTicks` est calculé selon le `NSDate` référencer la date de 00:00:00 UTC 1er janvier 2001 : 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) est défini sur [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Pour créer le `NSDate` objet, le `TimeInterval` est utilisé avec le `NSDate` [dateWithTimeIntervalSinceReferenceDate :](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) sélecteur.

#### <a name="nsdate-to-datetime"></a>NSDate en DateTime

La conversion de `NSDate` à `DateTime` utilise la formule suivante :

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

Dans cette formule : 

- `NSDateReferenceDateTicks` est calculé selon le `NSDate` référencer la date de 00:00:00 UTC 1er janvier 2001 : 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) est défini sur [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Après avoir calculé `DateTimeTicks`, le `DateTime` [constructeur](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) est appelé, en définissant son `kind` à `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` peut être `nil`, mais un `DateTime` est un struct dans .NET, qui, par définition, ne peut pas être `null`. Si vous donnez une `nil` `NSDate`, il sera traduit la valeur par défaut `DateTime` valeur, qui est mappé à `DateTime.MinValue`.

`NSDate` prend en charge un maximum supérieure et une valeur minimale inférieure à `DateTime`. Lors de la conversion à partir de `NSDate` à `DateTime`, ces valeurs supérieures et inférieures sont modifiés pour le `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) ou [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue), respectivement.
