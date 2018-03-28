---
title: Sélecteurs objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7b7f64288695ecc0f9f57ec670c4e9ff2e44804c
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="objective-c-selectors"></a>Sélecteurs objective-C

Basé sur le langage Objective-C *sélecteurs*. Un sélecteur est un message qui peut être envoyé à un objet ou un *classe*. [Xamarin.iOS](~/ios/internals/api-design/index.md) cartes sélecteurs de méthodes d’instance de l’instance et les sélecteurs de méthodes statiques de classe.

Contrairement aux fonctions C normales (et, comme les fonctions membres C++), vous ne pouvez pas appeler directement une à l’aide du sélecteur [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Mettre de côté*: en théorie, vous pouvez utiliser P/Invoke pour les fonctions de membre non virtuelle C++, mais vous devez à vous soucier de troncage per-du compilateur, qui est un monde de faibles mieux ignoré.) Au lieu de cela, les sélecteurs sont envoyés à une classe Objective-C ou à l’aide de l’instance du [ `objc_msgSend` fonction](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Vous souhaiterez peut-être [ce guide sur la messagerie Objective-C](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) utile.

<a name="Example" />

## <a name="example"></a>Exemple

Supposons que vous souhaitez appeler le [-[NSString sizeWithFont:forWidth:lineBreakMode :]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) sélecteur.
La déclaration (à partir de documentation d’Apple) est :

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Le type de retour est *CGSize* pour l’API unifiée.
-  Le *police* paramètre est un [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (et un type dérivé (indirectement) [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) et par conséquent mappé à [System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/) .
-  Le *largeur* paramètre, un *CGFloat* , est mappé à *nfloat*.
-  Le *lineBreakMode* paramètre, un *UILineBreakMode* , a déjà été lié dans Xamarin.iOS comme le [UILineBreakMode énumération](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Combiner toutes les options et nous souhaitons une déclaration objc_msgSend qui correspond à :

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Nous devons déclarer :

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Une fois déclarée, nous pouvons appeler une fois les paramètres appropriés :

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

La valeur retournée était une structure qui a été de taille inférieure à 8 octets (telles que l’ancien `SizeF` utilisé avant de passer à l’API unifiée) vous auriez le code ci-dessus sur le simulateur, mais il est bloqué sur l’appareil. Pour appeler un sélecteur qui retourne une valeur inférieure à 8 bits dans la taille, par conséquent nous *également* avez besoin de déclarer le `objc_msgSend_stret()` fonction :

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Appel serait alors :

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>Appel d’un sélecteur

Appel d’un sélecteur comporte trois étapes :

1.  Obtenir la cible du sélecteur.
1.  Obtenir le nom du sélecteur.
1.  Appelez objc_msgSend() avec les arguments appropriés.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Sélecteur cibles

Une cible de sélecteur est une instance d’objet ou une classe Objective-C. Si la cible est une instance et provenait d’un type de Xamarin.iOS lié, utilisez la [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) propriété.

Si la cible est une classe, utilisez [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) pour obtenir une référence à l’instance de classe, puis utiliser le [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) propriété.


<a name="Selector_Names" />

### <a name="selector-names"></a>Noms de sélecteur

Noms de sélecteur sont répertoriés dans la documentation d’Apple. Par exemple, le [les méthodes d’extension UIKit NSString](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) incluent [sizeWithFont :](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) et [sizeWithFont:forWidth:lineBreakMode :](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Les deux-points incorporées et de fin sont importants et font partie du nom du sélecteur.

Une fois que vous avez un nom de sélecteur, vous pouvez créer un [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instance.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Appel de objc_msgSend()

 `objc_msgSend()` est utilisé pour envoyer un message (sélecteur) à un objet. Cette famille de fonctions prend au moins deux arguments requis : la sélecteur (une instance ou classe gérer), le sélecteur de lui-même et la cible des arguments requis pour le sélecteur particulier. Les arguments de sélection et d’instance doivent être `System.IntPtr`, tous les arguments restants doivent correspondre au type attend le sélecteur, par exemple, un `nint` pour un `int`, ou un `System.IntPtr` pour toutes les `NSObject`-des types dérivés. Utilisez le [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) propriété pour obtenir un `IntPtr` pour une instance de type Objective-C.

Malheureusement, il existe plusieurs `objc_msgSend()` (fonction).

Utilisez [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) pour des sélecteurs qui retournent une structure.
Pour conserver une « intéressante », sur ARM Cela inclut des types de retour de tous les *pas* une énumération ou les types intégrés C (char, short, int, long, float, double). Sur x86 (le simulateur), il doit être utilisé pour toutes les structures supérieurs à la taille de 8 octets. (CGSize--utilisé dans l’exemple ci-dessus est de 8 octets exactement et donc n’utilise pas `objc_msgSend_stret()` dans le simulateur.)

Utilisez [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) des sélecteurs qui retournent flottante point valeur sur x86 uniquement. Cette fonction ne doive pas utilisable sur ARM ; au lieu de cela, utilisez `objc_msgSend()`.

Le principal [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) fonction est utilisée pour tous les autres sélecteurs.

Une fois que vous avez décidé qui `objc_msgSend()` ou les fonctions vous avez besoin d’appeler (et il peut être plusieurs, par exemple, pour le simulateur et l’appareil), vous pouvez utiliser un normal [ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/) méthode pour déclarer la fonction d’appel de méthode ultérieur.

Un ensemble de rolecapability `objc_msgSend()` déclarations se trouvent dans [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Inconvénients

Objective-C possède trois types de `objc_msgSend` méthodes : une pour les appels régulières, un pour les appels qui retournent des valeurs à virgule flottante (x86 uniquement) et un pour les appels qui retournent des valeurs de struct. Celui-ci comprend le suffixe `_stret` dans `ObjCRuntime.Messaging`.

Si vous appelez une méthode qui retourne certaines structures (les règles sont décrites ci-dessous), vous devez appeler la méthode avec la valeur de retour comme premier paramètre comme une valeur de sortie, comme suit :

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Éléments pénible ici et parce que la règle pour lorsque vous devez utiliser _stret_ est différente sur X86 et ARM, si vous souhaitez que vos liaisons pour travailler sur le simulateur et l’appareil, vous devez ajouter le code qui ressemble à ceci :

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>À l’aide de l’objc\_msgSend\_stret (méthode)

La règle quand utiliser le [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) sont comme suit pour **ARM**:

-  Un type valeur qui n’est pas une énumération ou un des types de base pour une énumération (int, byte, short, long, double, float).


La règle quand utiliser le [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) sont comme suit pour **X86**:

-  Un type valeur qui n’est pas une énumération ou l’un des types de base pour une énumération (int, byte, short, long, double, float) et native dont la taille est supérieure à 8 octets.


### <a name="creating-your-own-signatures"></a>Création de vos propres signatures.

Les éléments suivants [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) peut être utilisé pour créer vos propres signatures, si nécessaire.



## <a name="related-links"></a>Liens associés

- [Exemple de sélecteurs](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
