---
title: "Sous-classes génériques de NSObject"
ms.topic: article
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 76c35cfb993bade324b25f86e75db7eb028a7399
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="generic-subclasses-of-nsobject"></a>Sous-classes génériques de NSObject

## <a name="using-generics-with-nsobjects"></a>L’utilisation de génériques avec NSObjects

Depuis Xamarin.iOS 7.2.1)., vous pouvez utiliser des génériques dans les sous-classes de `NSObject` (par exemple [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

Vous pouvez maintenant créer des classes génériques, comme celle-ci :

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Étant donné que des objets qui sous-classe `NSObject` sont inscrits avec le runtime Objective-C, il existe certaines limitations concernant ce qui est possible avec les sous-classes génériques de `NSObject` types.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Considérations pour les sous-classes génériques de NSObject

Ce document décrit en détail les limitations de la prise en charge limitée des sous-classes génériques de `NSObjects` introduite avec Xamarin.iOS 7.2.1).
    
### <a name="generic-type-arguments-in-member-signatures"></a>Arguments de Type générique dans les Signatures de membre

Tous les arguments de type générique dans une signature de membre exposé à Objective-C doivent avoir un `NSObject` contrainte.

**Bonne**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Raison**: le paramètre de type générique est un `NSObject`, de sorte que la signature de sélecteur pour `myMethod:` peuvent être exposés en toute sécurité à Objective-C (il sera toujours `NSObject` ou une de ses sous-classes).

**Mauvais**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Raison**: il n’est pas possible de créer une signature Objective-C pour les membres exportés code Objective-C peut appeler, étant donné que la signature est diffèrent selon le type exact du type générique `T`.

**Bonne**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Raison**: il est possible d’avoir des arguments de type générique sans contrainte tant qu’ils ne prennent pas de partie de la signature de membre exporté.

**Bonne**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Raison**: le `T` paramètre dans le Objective-C exporté `MyMethod` est contraint à être un `NSObject`, le type sans contrainte `U` ne fait pas partie de la signature.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Instanciations de Types génériques à partir de Objective-C

L’instanciation des types génériques à partir de Objective-C n’est pas autorisée. Cela se produit généralement lorsqu’un type managé est utilisé dans un xib.

Envisagez cette définition de classe qui expose un constructeur qui accepte un `IntPtr` (la façon de Xamarin.iOS de construction d’un objet c# à partir d’une instance native de Objective-C) :
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Pendant la construction ci-dessus est bien, lors de l’exécution, cela lève une exception au si Objective-C tente de créer une instance de celui-ci.

Il s’agit d’effet Objective-C n’a aucun concept de types génériques, et il ne peut pas spécifier le type générique exact à créer.

Ce problème peut être contourné en créant une sous-classe spécialisée de type générique.   Exemple :
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Maintenant il n’existe aucune ambiguïté, la classe `GenericUIView` peut être utilisé dans xibs.

## <a name="no-support-for-generic-methods"></a>Aucune prise en charge pour les méthodes génériques

### <a name="generic-methods-are-not-allowed"></a>Les méthodes génériques ne sont pas autorisés.

Le code suivant ne compilera pas :

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Raison**: cela n’est pas autorisé, car Xamarin.iOS ne connaît pas le type à utiliser pour l’argument de type `T` lorsque la méthode est appelée à partir de Objective-C.

Une alternative consiste à créer une méthode spécialisée et qui exporter à la place :

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Aucun membre statique exporté autorisé

Vous ne pouvez pas exposer des membres statiques pour Objective-C si elle est hébergée dans une sous-classe générique de `NSObject`.

Exemple d’un scénario non pris en charge :

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Raison :** tout comme les méthodes génériques, le Xamarin.iOS runtime doit être en mesure de connaître le type à utiliser pour l’argument de type générique T.

Membres pour l’instance de l’instance elle-même est utilisé (dans la mesure où il y aura jamais d’une instance générique<T>, il sera toujours générique<SomeSpecificClass>), mais pour les membres statiques ne sont pas présents.

Notez que cela s’applique même si le membre en question n’utilise pas l’argument de type T en aucune façon.

L’alternative est dans ce cas consiste à créer une sous-classe spécialisée :

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

### <a name="requires-new-static-registrar"></a>Requiert le bureau d’enregistrement de nouveau statique

La prise en charge des génériques requiert la nouvelle [système d’enregistrement](~/ios/internals/registrar.md).

Si vous essayez d’utiliser l’ancien système hérité d’inscription affiche des avertissements lorsqu’il rencontre des types génériques (en plus de ne pas générer de code correct, ce qui provoque un comportement non défini).
    
## <a name="performance"></a>Performances

Le bureau d’enregistrement statique ne peut pas résoudre un membre exporté dans un type générique au moment de la génération, comme il le fait en général, il doit être recherché lors de l’exécution. Cela signifie que l’appel d’une telle méthode à partir de Objective-C est légèrement plus lente que l’appel de membres à partir de classes non génériques.

