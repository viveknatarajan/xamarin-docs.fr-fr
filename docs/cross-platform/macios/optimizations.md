---
title: Optimisations de build
description: Ce document explique les différentes optimisations qui sont appliquées au moment de la génération pour les applications Xamarin.iOS et Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: conceptdev
ms.author: crdun
ms.date: 04/16/2018
ms.openlocfilehash: f1aa805b9b7a16ad1e8af573cf4170f885eb0197
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261199"
---
# <a name="build-optimizations"></a>Optimisations de build

Ce document explique les différentes optimisations qui sont appliquées au moment de la génération pour les applications Xamarin.iOS et Xamarin.Mac.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Supprimer UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Supprime les appels à [UIApplication.EnsureUIThread] [ 1] (pour Xamarin.iOS) ou `NSApplication.EnsureUIThread` (pour Xamarin.Mac).

Cette optimisation change le type de code suivant :

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

dans les éléments suivants :

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Par défaut, qu'elle est activée pour la mise en production génère.

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]remove-uithread-checks` à mtouch/mmp.

[1]: https://docs.microsoft.com/dotnet/api/UIKit.UIApplication.EnsureUIThread

## <a name="inline-intptrsize"></a>Inline IntPtr.Size

La valeur de constante d’incorporations de `IntPtr.Size` en fonction de la plateforme cible.

Cette optimisation change le type de code suivant :

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

dans les éléments suivants (lors de la création d’une plateforme 64 bits) :

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Par défaut, il est activé si vous ciblez une seule architecture ou de l’assembly de plateforme (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** ou  **Xamarin.Mac.dll**).

Si vous ciblez plusieurs architectures, cette optimisation crée des assemblys différents pour la version 32 bits et la version 64 bits de l’application, et les deux versions devra être inclus dans l’application, efficace en augmentant la taille de l’application finale au lieu de diminuer Il.

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]inline-intptr-size` à mtouch/mmp.

## <a name="inline-nsobjectisdirectbinding"></a>Inline NSObject.IsDirectBinding

`NSObject.IsDirectBinding` est une propriété d’instance qui détermine si une instance particulière est d’un type de wrapper ou non (un type de wrapper est un type managé qui est mappé à un type natif ; pour une instance managée `UIKit.UIView` type est mappé à natif `UIView` type - l’inverse est un type d’utilisateur Dans ce cas `class MyUIView : UIKit.UIView` serait un type d’utilisateur).

Il est nécessaire de connaître la valeur de `IsDirectBinding` lors de l’appel dans Objective-C, car la valeur détermine la version de `objc_msgSend` à utiliser.

Étant donné seulement le code suivant :

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

Nous pouvons déterminer que dans `UIView.SomeProperty` la valeur de `IsDirectBinding` n’est pas une constante et ne peut pas être inline :

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Toutefois, il est possible de consulter tous les types dans l’application et de déterminer qu’il n’y a aucun type héritant de `NSUrl`, et il est donc déconseillé d’inline la `IsDirectBinding` valeur à une constante `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

En particulier, cette optimisation change le type de code suivant (il s’agit du code de liaison de `NSUrl.AbsoluteUrl`) :

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

dans les éléments suivants (quand il peut être déterminé qu’il n’y a aucune sous-classes de `NSUrl` dans l’application) :

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Il est toujours activée par défaut pour Xamarin.iOS et toujours désactivé par défaut pour Xamarin.Mac (car il est possible de charger dynamiquement des assemblys dans Xamarin.Mac, il n’est pas possible de déterminer qu’une classe particulière n’est jamais sous-classé).

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]inline-isdirectbinding` à mtouch/mmp.

## <a name="inline-runtimearch"></a>Inline Runtime.Arch

Cette optimisation change le type de code suivant :

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

dans les éléments suivants (lors de la création de périphérique) :

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Il est toujours activé par défaut pour Xamarin.iOS (il n’est pas disponible pour Xamarin.Mac).

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]inline-runtime-arch` à mtouch.

## <a name="dead-code-elimination"></a>Élimination de code mort

Cette optimisation change le type de code suivant :

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

dans :

```csharp
Console.WriteLine ("Doing this");
```

Il évalue également les comparaisons de constante, comme suit :

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

et de déterminer si l’expression `8 == 8` est un toujours true et réduire à :

```csharp
Console.WriteLine ("Doing this");
```

Il s’agit une optimisation performante lorsqu’il est utilisé avec les optimisations d’incorporation (inlining), car il peut transformer le type de code suivant (il s’agit du code de liaison de `NFCIso15693ReadMultipleBlocksConfiguration.Range`) :

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

dans la collection (lors de la création d’un appareil de 64 bits et lorsque cela est également possible pour vous assurer qu’aucun `NFCIso15693ReadMultipleBlocksConfiguration` sous-classes dans l’application) :

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Le compilateur AOT est déjà à éliminer code mort similaire à celui-ci, mais cette optimisation est effectuée à l’intérieur de l’éditeur de liens, ce qui signifie que l’éditeur de liens en mesure de voir qu’il n’y a plusieurs méthodes qui ne sont pas utilisées plus et donc peuvent être supprimés (à moins d’utiliser un autre emplacement) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Il est toujours activé par défaut (lorsque l’éditeur de liens est activé).

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]dead-code-elimination` à mtouch/mmp.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Optimiser les appels à BlockLiteral.SetupBlock

Le runtime Xamarin.iOS/Mac a besoin de connaître la signature du bloc lors de la création d’un bloc d’Objective-C pour managé déléguer. Cela peut être une opération relativement coûteuse. Cette optimisation calculer la signature de bloc au moment de la génération et modifier le langage intermédiaire pour appeler un `SetupBlock` méthode prenant la signature en tant qu’argument à la place. Cela évite la nécessité pour le calcul de la signature lors de l’exécution.

Tests d’évaluation montrent que cela accélère l’appel d’un bloc d’un facteur de 10 à 15.

Il va transformer les éléments suivants [code](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

dans :

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Il est activé par défaut lors de l’utilisation du bureau d’enregistrement statique (dans Xamarin.iOS le bureau d’enregistrement statique est activée par défaut pour les builds d’appareil et dans le bureau d’enregistrement statique est activé par défaut pour la version de Xamarin.Mac génère).

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]blockliteral-setupblock` à mtouch/mmp.

## <a name="optimize-support-for-protocols"></a>Optimiser la prise en charge des protocoles

Le runtime Xamarin.iOS/Mac a besoin d’informations sur les protocoles d’implémente Objective-C types gérés. Ces informations sont stockées dans les interfaces (et les attributs de ces interfaces), qui n’est pas un format très efficace, ni est-il compatible avec l’éditeur de liens.

Par exemple, que ces interfaces stockent des informations sur tous les membres de protocole dans un `[ProtocolMember]` attribut, qui, entre autres choses, contenir des références aux types de paramètres de ces membres. Cela signifie que tout simplement implémenter une telle interface fera l’éditeur de liens conserver tous les types utilisés dans cette interface, même pour les membres facultatifs l’application jamais appelle ou implémente.

Cette optimisation rendra le bureau d’enregistrement statique stocker toutes les informations requises dans un format efficace qui utilise peu de mémoire qui est facile et rapide à rechercher lors de l’exécution.

Il apprendra également l’éditeur de liens qu’il n’a pas nécessairement besoin de conserver ces interfaces, ni aucun des attributs associés.

Cette optimisation nécessite l’éditeur de liens et le bureau d’enregistrement statique doit être activé.

Sur Xamarin.iOS, cette optimisation est activée par défaut lorsque l’éditeur de liens et le bureau d’enregistrement statique sont activées.

Sur Xamarin.Mac cette optimisation n’est jamais activée par défaut, car Xamarin.Mac prend en charge la chargement dynamique des assemblys et les assemblys ne peuvent pas sont connues au moment de la génération (et par conséquent ne pas optimisée).

Le comportement par défaut peut être substitué en passant `--optimize=-register-protocols` à mtouch/mmp.

## <a name="remove-the-dynamic-registrar"></a>Supprimer du bureau d’enregistrement dynamique

Le Xamarin.iOS et Xamarin.Mac runtime prennent en charge [l’inscription des types managés](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) avec le runtime Objective-C. Elle peut être soit effectuée au moment de la génération ou lors de l’exécution (ou partiellement au moment de la génération et le reste lors de l’exécution), mais si elle est complètement effectuée au moment de la génération, cela signifie que le code de prise en charge pour effectuer l’opération lors de l’exécution peut être supprimé. Cela entraîne une diminution significative de la taille de l’application, en particulier pour les applications plus petites telles que les extensions ou des applications watchOS.

Cette optimisation requiert le bureau d’enregistrement statique et l’éditeur de liens doit être activé.

L’éditeur de liens tente de déterminer si elle est fiable supprimer le bureau d’enregistrement dynamique et le cas, essayez de le supprimer.

Étant donné que Xamarin.Mac prend en charge dynamiquement le chargement d’assemblys lors de l’exécution (ce qui n’était pas connu au moment de la génération), il est impossible de déterminer au moment de la génération s’il s’agit d’une optimisation sécurisée. Cela signifie que cette optimisation n’est jamais activée par défaut pour les applications Xamarin.Mac.

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]remove-dynamic-registrar` à mtouch/mmp.

Si la valeur par défaut est substituée pour supprimer du bureau d’enregistrement dynamique, l’éditeur de liens émet des avertissements s’il détecte qu’il n’est pas sécurisé (mais le bureau d’enregistrement dynamique va être supprimé).

## <a name="inline-runtimedynamicregistrationsupported"></a>Inline Runtime.DynamicRegistrationSupported

La valeur d’incorporations de `Runtime.DynamicRegistrationSupported` comme déterminé au moment de la génération.

Si le bureau d’enregistrement dynamique est supprimé (voir la [supprimer du bureau d’enregistrement dynamique](#remove-the-dynamic-registrar) optimisation), il s’agit d’une constante `false` valeur, sinon il s’agit d’une constante `true` valeur.

Cette optimisation change le type de code suivant :

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

dans les éléments suivants lorsque le bureau d’enregistrement dynamique est supprimé :

```csharp
throw new Exception ("dynamic registration is not supported");
```

dans les éléments suivants lorsque le bureau d’enregistrement dynamique n’est pas supprimé :

```csharp
Console.WriteLine ("do something");
```

Cette optimisation nécessite l’éditeur de liens doit être activé et ne s’applique aux méthodes avec la `[BindingImpl (BindingImplOptions.Optimizable)]` attribut.

Il est toujours activé par défaut (lorsque l’éditeur de liens est activé).

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]inline-dynamic-registration-supported` à mtouch/mmp.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Précalculer les méthodes pour créer des délégués managés pour les blocs Objective-C

Lorsque Objective-C appelle un sélecteur qui prend un bloc en tant que paramètre, et ensuite le code managé a remplacé cette méthode, le Xamarin.iOS / Xamarin.Mac runtime a besoin créer un délégué pour ce bloc.

Le code de liaison généré par le Générateur de liaison inclura un `[BlockProxy]` attribut, qui spécifie le type avec un `Create` méthode qui peut le faire.

Étant donné le code Objective-C suivant :

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

et le code de liaison suivant :

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

le Générateur de produit :

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }
    
    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...
        
    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;
        
        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }
    
    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Lorsque Objective-C appelle `[ObjCBlockTester callClassCallback]`, le Xamarin.iOS / Xamarin.Mac runtime examinera le `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` attribut sur le paramètre. Il recherche ensuite la `Create` méthode sur ce type et appeler cette méthode pour créer le délégué.

Cette optimisation trouveront le `Create` méthode au moment de la génération et le bureau d’enregistrement statique générera du code qui recherche la méthode lors de l’exécution à l’aide des jetons de métadonnées à la place à l’aide de l’attribut et la réflexion (cela est beaucoup plus rapide et permet également de l’éditeur de liens Pour supprimer le code de runtime correspondant, réduire la taille de l’application).

Si mmp/mtouch est impossible de trouver le `Create` (méthode), puis s’affichera un avertissement MT4174/MM4174, et la recherche sera effectuée lors de l’exécution à la place.
La cause la plus probable est écrit manuellement le code de liaison sans requis `[BlockProxy]` attributs.

Cette optimisation requiert le bureau d’enregistrement statique doit être activé.

Il est toujours activé par défaut (à condition que le bureau d’enregistrement statique est activée).

Le comportement par défaut peut être substitué en passant `--optimize=[+|-]static-delegate-to-block-lookup` à mtouch/mmp.
