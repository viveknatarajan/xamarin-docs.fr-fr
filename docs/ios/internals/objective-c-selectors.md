---
title: Sélecteurs objective-C dans Xamarin.iOS
description: Ce document explique comment interagir avec les sélecteurs Objective-C à partir de c#. Il décrit comment appeler les sélecteurs et les considérations techniques qui doivent être prises en compte lors de cette opération.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/12/2017
ms.openlocfilehash: 15db59945f482728f760006095e294bc5628c8bd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036351"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Sélecteurs objective-C dans Xamarin.iOS

Le langage d’Objective-C est basé sur *sélecteurs*. Un sélecteur est un message qui peut être envoyé à un objet ou un *classe*. [Xamarin.iOS](~/ios/internals/api-design/index.md) cartes sélecteurs aux méthodes d’instance de l’instance et classe les sélecteurs de méthodes statiques.

Contrairement aux fonctions C normales (et, comme les fonctions membres C++), vous ne pouvez pas appeler directement un à l’aide du sélecteur [P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/) au lieu de cela, les sélecteurs sont envoyés à une classe Objective-C ou à l’aide de l’instance du [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
fonction.

Pour plus d’informations sur les messages en Objective-C, examinons d’Apple [utilisation d’objets](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) guide.

## <a name="example"></a>Exemple

Supposons que vous souhaitez appeler le [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
sélecteur de [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring).
La déclaration (à partir de documentation d’Apple) est :

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

Cette API a les caractéristiques suivantes :

- Le type de retour est `CGSize` pour l’API unifiée.
- Le `font` paramètre est un [UIFont](xref:UIKit.UIFont) (et un type dérivé (indirectement) [NSObject](xref:Foundation.NSObject)et est mappé à [System.IntPtr](xref:System.IntPtr).
- Le `width` paramètre, un `CGFloat`, est mappé à `nfloat`.
- Le `lineBreakMode` paramètre, un [ `UILineBreakMode` ](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc), a déjà été lié dans Xamarin.iOS en tant que le [`UILineBreakMode`](xref:UIKit.UILineBreakMode)
Énumération.

En résumé, le `objc_msgSend` déclaration doit correspondre à :

```csharp
CGSize objc_msgSend(
    IntPtr target, 
    IntPtr selector, 
    IntPtr font, 
    nfloat width, 
    UILineBreakMode mode
);
```

Déclarer comme suit :

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Pour appeler cette méthode, utilisez le code suivant :

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, 
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

La valeur retournée était une structure qui a été de taille inférieure à 8 octets (telles que l’ancien `SizeF` utilisé avant de passer à l’API unifiée), le code ci-dessus auriez rencontré sur le simulateur, mais s’est bloquée sur l’appareil. Pour appeler un sélecteur qui retourne une valeur inférieure à 8 bits en taille, déclarez le `objc_msgSend_stret` (fonction) :

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Pour appeler cette méthode, utilisez le code suivant :

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, 
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## <a name="invoking-a-selector"></a>Appel d’un sélecteur

Appel d’un sélecteur comporte trois étapes :

1. Obtenir la cible du sélecteur.
2. Obtenir le nom de sélecteur.
3. Appelez `objc_msgSend` avec les arguments appropriés.

### <a name="selector-targets"></a>Cibles de sélecteur

Une cible de sélecteur est une instance d’objet ou une classe Objective-C. Si la cible est une instance et provenance d’un type de Xamarin.iOS lié, utilisez la [ `ObjCRuntime.INativeObject.Handle` ](xref:ObjCRuntime.INativeObject.Handle) propriété.

Si la cible est une classe, utilisez [ `ObjCRuntime.Class` ](xref:ObjCRuntime.Class) pour obtenir une référence à l’instance de classe, puis utilisez le [ `Class.Handle` ](xref:ObjCRuntime.Class.Handle) propriété.

### <a name="selector-names"></a>Noms de sélecteur

Les noms de sélecteur sont répertoriés dans la documentation d’Apple. Par exemple, [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring?language=objc) inclut [ `sizeWithFont:` ](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) et [ `sizeWithFont:forWidth:lineBreakMode:` ](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) sélecteurs. Le signe deux-points embedded et de fin font partie du nom de sélecteur et ne peut pas être omis.

Une fois que vous avez un nom de sélecteur, vous pouvez créer un [ `ObjCRuntime.Selector` ](xref:ObjCRuntime.Selector) instance pour celui-ci.

### <a name="calling-objcmsgsend"></a>Appel objc_msgSend

`objc_msgSend` envoie un message (sélecteur) à un objet. Cette famille de fonctions prend au moins deux arguments requis : la cible de sélecteur (une instance ou une classe gérer), le sélecteur de lui-même et tous les arguments requis pour le sélecteur. Les arguments de sélection et d’instance doivent être `System.IntPtr`, et tous les autres arguments doivent correspondre au type attend le sélecteur, par exemple un `nint` pour un `int`, ou un `System.IntPtr` pour tous les `NSObject`-les types dérivés. Utilisez le [`NSObject.Handle`](xref:Foundation.NSObject.Handle)
propriété à obtenir un `IntPtr` pour une instance de type Objective-C.

Il existe plusieurs `objc_msgSend` fonction :

- Utilisez [ `objc_msgSend_stret` ](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) des sélecteurs qui retournent un struct. Sur ARM, cela inclut le retour de tous les types qui ne sont pas une énumération ou l’un des types intégrés C (`char`, `short`, `int`, `long`, `float`, `double`). Sur x86 (le simulateur), cette méthode doit être utilisée pour toutes les structures supérieurs à la taille de 8 octets (`CGSize` est de 8 octets et n’utilise pas `objc_msgSend_stret` dans le simulateur). 
- Utilisez [ `objc_msgSend_fpret` ](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) pour valeur de point de sélecteurs qui retournent flottante sur x86 uniquement. Cette fonction n’a pas besoin être utilisé sur ARM ; au lieu de cela, utilisez `objc_msgSend`. 
- La principale [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) fonction est utilisée pour tous les autres sélecteurs.

Une fois que vous avez décidé qui `objc_msgSend` ou vous devez appeler les fonctions (simulateur ou un appareil peuvent chacun nécessiter une méthode différente), vous pouvez utiliser un élément normal [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) méthode pour déclarer la fonction pour un appel ultérieur.

Un ensemble de préfabriqués `objc_msgSend` déclarations se trouve dans `ObjCRuntime.Messaging`.

## <a name="different-invocations-on-simulator-and-device"></a>Différents appels sur le simulateur ou un appareil

Comme décrit ci-dessus, Objective-C a trois types de `objc_msgSend` méthodes : une pour les appels régulières, un pour les appels qui retournent des valeurs à virgule flottante (x86 uniquement) et un pour les appels qui retournent des valeurs de struct. Ce dernier comprend le suffixe `_stret` dans `ObjCRuntime.Messaging`.

Si vous appelez une méthode qui renvoie certaines structures (règles décrites ci-dessous), vous devez appeler la méthode avec la valeur de retour comme premier paramètre comme un `out` valeur :

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

La règle pour le moment auquel utiliser le `_stret_` méthode diffère sur x86 et ARM.
Si vous souhaitez que vos liaisons pour travailler sur le simulateur et l’appareil, ajoutez le code suivant :

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
} 
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### <a name="using-the-objcmsgsendstret-method"></a>À l’aide de la méthode objc_msgSend_stret

Quand vous créez pour ARM, utilisez le [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
pour n’importe quel type de valeur qui n’est pas une énumération ou l’un des types de base pour une énumération (`int`, `byte`, `short`, `long`, `double`, `float`).

Quand vous créez pour x86, utilisez [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
pour n’importe quel type de valeur qui n’est pas une énumération ou l’un des types de base pour une énumération (`int`, `byte`, `short`, `long`, `double`, `float`) et dont la taille native est supérieure à 8 octets.

### <a name="creating-your-own-signatures"></a>Création de vos propres signatures

Ce qui suit [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) peut être utilisé pour créer vos propres signatures, si nécessaire.

## <a name="related-links"></a>Liens connexes

- [Sélecteurs objective-C](https://developer.xamarin.com/samples/mac-ios/Objective-C/) exemple
