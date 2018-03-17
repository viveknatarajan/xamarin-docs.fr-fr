---
title: Prise en charge objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 047f7d7497a114bf4b7c94e50bdf09862b882794
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
---
# <a name="objective-c-support"></a>Prise en charge objective-C

## <a name="specific-features"></a>Fonctionnalités spécifiques

La génération de ObjC a un quelques fonctionnalités spéciales qui sont à noter.

### <a name="automatic-reference-counting"></a>Comptage de références automatique

L’utilisation de comptage de référence automatique (ARC) est **requis** pour appeler les liaisons générés. Projet à l’aide d’une bibliothèque d’embeddinator doit être compilé avec `-fobjc-arc`.

### <a name="nsstring-support"></a>Prise en charge NSString

Les API qui exposent `System.String` types sont convertis en `NSString`. Gestion de la mémoire est ainsi plus facile que de gérer `char*`.

### <a name="protocols-support"></a>Prise en charge des protocoles

Interfaces managées sont convertis en protocoles ObjC où tous les membres sont `@required`.

### <a name="nsobject-protocol-support"></a>Prise en charge du protocole de NSObject

Par défaut, nous supposons le hachage de la valeur par défaut et l’égalité de .net et le runtime ObjC sont précis et interchangeables qu’ils partagent une sémantique très similaire.

Lorsqu’un type managé substitue `Equals(Object)` ou `GetHashCode` cela signifie généralement que le comportement par défaut (.NET) n’était pas celui meilleures. Nous pouvons supposer que le comportement de Objective-C par défaut ne serait pas soit.

Dans ce cas, le générateur remplace le [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) (méthode) et [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) propriété définie dans le [ `NSObject` protocole](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Cela permet l’implémentation managée personnalisée à utiliser à partir du code ObjC en toute transparence.

### <a name="comparison"></a>Comparaison

Des types qui implémentent gérés `IComparable` ou il s’agit d’une version générique `IComparable<T>` produira ObjC méthodes conviviales qui retourne un `NSComparisonResult` et accepter un `nil` argument. Cela rend l’API générée plus conviviale pour les développeurs ObjC, par exemple

```csharp
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Categories

Les méthodes sont convertis en catégories des extensions managées. Par exemple les méthodes d’extension suivantes sur `Collection`:

```csharp
    public static class SomeExtensions {

        public static int CountNonNull (this Collection collection) { ... }

        public static int CountNull (this Collection collection) { ... }
    }
```

Créez une catégorie Objective-C comme celle-ci :

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

Lorsqu’un seul géré type étend plusieurs types puis plusieurs catégories Objective-C sont générés.

### <a name="subscripting"></a>Indices

Les propriétés indexées gérées sont converties en mise en indice de l’objet. Exemple :

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

Créez Objective-C similaire à :

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

qui peut être utilisé via la syntaxe en indice Objective-C :

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

Selon le type de l’indexeur, mise en indice indexée ou une clé est générée lorsque cela est approprié.

Cela [article](http://nshipster.com/object-subscripting/) est une bonne introduction à la mise en indice.

## <a name="main-differences-with-net"></a>Principales différences avec .NET

### <a name="constructors-vs-initializers"></a>Visual Studio initialiseurs de constructeurs

Dans Objective-C, vous pouvez appeler les de l’initialiseur de prototypes de toutes les classes du parent dans la chaîne d’héritage, sauf si elle est marquée comme non disponible (NS_UNAVAILABLE).

En c#, vous devez déclarer explicitement un membre de constructeur à l’intérieur d’une classe, cela signifie que les constructeurs ne sont pas hérités.

Pour exposer la représentation sous forme de droite de l’API c# pour Objective-C, nous ajoutons `NS_UNAVAILABLE` à n’importe quel initialiseur n’est pas présent dans la classe enfant à partir de la classe parente.

API C# :

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

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Ici, nous pouvons voir que `initWithId:` a été marqué comme non disponible.

### <a name="operator"></a>Opérateur

ObjC ne prend pas en charge l’opérateur comme c#, la surcharge pour les opérateurs sont convertis en les sélecteurs de classe :

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

par celle-ci :

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Toutefois, certains langages .NET ne prennent pas en charge surcharge d’opérateur, il est courant d’inclure également un [« convivial »](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) appelée méthode en plus de la surcharge d’opérateur.

Si la version de l’opérateur et la version « conviviale » sont détectées, seule la version conviviale sera générée, car ils génèrent le même nom de l’objectif-c.

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

Devient :

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Opérateur d’égalité

En général opérateur == dans c# est traitée comme un opérateur général comme indiqué ci-dessus.

Toutefois, si l’opérateur d’égalité « convivial » est trouvé, les deux opérateur == et l’opérateur ! = sera ignorée dans la génération.

### <a name="datetime-vs-nsdate"></a>DateTime et NSDate

À partir de [de NSDate](https://developer.apple.com/reference/foundation/nsdate?language=objc) documentation :

> Les objets NSDate encapsulent un point unique dans le temps, indépendamment de tout système calendrical particulier ou de fuseau horaire. Les objets de date sont immuables, représentant un intervalle de temps invariant par rapport à une date de référence absolue (00 : 00:00 UTC le 1er janvier 2001).

En raison `NSDate` référence date, toutes les conversions entre elle et `DateTime` doit être effectué au format UTC.

#### <a name="datetime-to-nsdate"></a>DateTime en NSDate

Lors de la conversion à partir de `DateTime` à `NSDate` la date/heure `Kind` propriété est prise en compte.

|Genre|Résultats                                                                                            |
|---|---|
|Utc|Conversion est effectuée à l’aide de le `DateTime` en l’état de l’objet.|
|Local|Le résultat de l’appel de `ToUniversalTime()` dans les `DateTime` objet est utilisé pour la conversion.|
|Non spécifié|Fourni `DateTime` objet est supposé pour être l’heure UTC, par conséquent, même comportement que le type == Utc.|

La conversion s’effectue à l’aide de la formule suivante :

> [!NOTE]
> **TimeInterval** = DateTimeObjectTicks - NSDateReferenceDateTicks [dt] / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

Une fois la TimeInterval nous utilisons de NSDate [dateWithTimeIntervalSinceReferenceDate :](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) sélecteur pour le créer.

#### <a name="nsdate-to-datetime"></a>NSDate en DateTime

Allant de NSDate à DateTime, nous supposons que nous recevons un NSDate instance qui est la date de référence est **UTC 00:00:00 le 1er janvier 2001** et utiliser la formule suivante :

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks [dt]

Une fois que nous avons calculé la **DateTimeTicks** , nous utilisons la valeur DateTime suivant [constructeur](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) paramètre son `kind` à `DateTimeKind.Utc`.

Il existe certaines considérations que vous devez prendre en compte, NSDate peut être `nil` , mais une valeur DateTime est une structure .NET et par définition, il ne peut pas être `null`. Si vous donnez un `nil` NSDate nous sera le traduire la valeur de date/heure par défaut qui est mappée à `DateTime.MinValue`.

MinValue et MaxValue sont également différentes, NSDate peut prendre en charge les limites supérieure et inférieures à le de heure sorte que chaque fois que vous affectez la valeur supérieure ou inférieure à nous sera attribuer de DateTime [MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) ou [MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) respectivement.

**DT**: date de référence NSDate **UTC 00:00:00 le 1er janvier 2001** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
