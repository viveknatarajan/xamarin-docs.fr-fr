---
title: Marshaling d’exception
description: Xamarin.iOS contient de nouveaux événements pour aider à répondre aux exceptions, en particulier dans le code natif.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/05/2017
ms.openlocfilehash: bb9c16985d958772193093434350435ce477956a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="exception-marshaling"></a>Marshaling d’exception

_Xamarin.iOS contient de nouveaux événements pour aider à répondre aux exceptions, en particulier dans le code natif._

Le code managé et Objective-C ont prise en charge des exceptions (try/catch/finally clauses) du runtime.

Toutefois, leurs implémentations sont différentes, ce qui signifie que les bibliothèques d’exécution (runtime Mono et les bibliothèques d’exécution Objective-C) ont des problèmes lorsqu’ils doivent gérer les exceptions, puis exécutez le code écrit dans d’autres langages.

Ce document explique les problèmes qui peuvent se produire et les solutions possibles.

Il inclut également un exemple de projet, [Exception Marshaling](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), qui peut être utilisé pour tester les différents scénarios et leurs solutions.

## <a name="problem"></a>Problème

Le problème se produit lorsqu’une exception est levée et pendant le déroulement d’un frame de pile est rencontrée qui ne correspondent pas le type d’exception qui a été levée.

Un exemple typique de ce pour Xamarin.iOS ou Xamarin.Mac est lorsqu’une API native lève une exception Objective-C, et ensuite cette exception Objective-C doit une certaine manière traitée lorsque le processus de déroulement de pile atteint une image managée.

L’action par défaut est de ne rien faire. Pour l’exemple ci-dessus, cela signifie que vous permettant des frames de déroulement gérés Objective-C runtime. Cela est problématique, car le runtime Objective-C ne sait pas comment le déroulement des frames gérés ; par exemple, il ne sera pas exécuter les `catch` ou `finally` clauses dans ce frame.

### <a name="broken-code"></a>Code rompue

Prenons l’exemple de code suivant :

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Un NSInvalidArgumentException Objective-C est levée en code natif :

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

Et la trace de pile quelque chose comme ceci :

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

Les frames 0-3 sont des frames natifs et le dérouleur de pile dans le runtime Objective-C _pouvez_ les trames de déroulement. En particulier, il s’exécute tout Objective-C `@catch` ou `@finally` clauses.

Toutefois, le dérouleur de pile Objective-C est _pas_ capable de déroulement correctement les frames managés (images 4 à 6), les frames seront être déroulées, mais la logique d’exception managé ne sera pas exécutée.

Ce qui signifie qu’il n’est généralement pas possible d’intercepter ces exceptions de la manière suivante :

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

C’est parce que le dérouleur de pile Objective-C ne connaît pas managé `catch` clause et ni est le `finally` clause être exécutée.

Lorsque l’exemple de code ci-dessus _est_ effective, que c’est parce que Objective-C possède une méthode de la notification de l’exception non Objective-C, [ `NSSetUncaughtExceptionHandler` ] [ 2], qui Xamarin.iOS et Xamarin.Mac d’utilisation et à ce stade essaie de convertir toutes les exceptions Objective-C sur les exceptions gérées.

## <a name="scenarios"></a>Scénarios

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Scénario 1 : interception d’exceptions Objective-C avec un gestionnaire catch managé

Dans le scénario suivant, il est possible d’intercepter des exceptions Objective-C à l’aide de managed `catch` gestionnaires :

1. Une exception Objective-C est levée.
2. Objective-C, le runtime parcourt la pile (mais n’est pas dérouler), recherchez natif `@catch` gestionnaire qui peut gérer l’exception.
3. Le runtime Objective-C ne trouve aucun `@catch` gestionnaires, appelle `NSGetUncaughtExceptionHandler`et appelle le gestionnaire installé par Xamarin.iOS/Xamarin.Mac.
4. Gestionnaire de Xamarin.iOS/Xamarin.Mac's convertit l’exception Objective-C dans une exception managée et lever. Depuis le Objective-C runtime n’a pas de dérouler la pile (uniquement parcourue il), le frame actuel est identique à celui où l’exception Objective-C a été levée.

Un autre problème se produit, car le runtime Mono ne sait pas comment dérouler correctement les frames de Objective-C.

Lorsque le rappel d’exception des Xamarin.iOS non interceptée Objective-C est appelée, la pile est comme suit :

     0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
     1 CoreFoundation           __handleUncaughtException + 809
     2 libobjc.A.dylib          _objc_terminate() + 100
     3 libc++abi.dylib          std::__terminate(void (*)()) + 14
     4 libc++abi.dylib          __cxa_throw + 122
     5 libobjc.A.dylib          objc_exception_throw + 337
     6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
     7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
     8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
     9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
    10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]

Ici, les frames managés uniquement sont des images 8-10, mais l’exception managée est levée dans le cadre de 0. Cela signifie que le runtime Mono doit se dérouler les frames natifs 0-7, ce qui provoque un problème équivalent au problème décrit ci-dessus : bien que le runtime Mono déroule frames natifs, il ne sera pas exécuter n’importe quel Objective-C `@catch` ou `@finally` clauses .

Exemple de code :

``` objective-c
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

Et le `@finally` clause ne sera pas exécutée, car le runtime Mono qui se déroule ce frame ne connaît pas il.

Une variante consiste à lever une exception gérée dans le code managé et le déroulement de puis par le biais des frames natifs pour obtenir la première managé `catch` clause :

``` csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

Managé `UIApplication:Main` méthode appellera natif `UIApplicationMain` (méthode), puis sur iOS effectuera une grande quantité de l’exécution du code natif avant d’appeler managé `AppDelegate:FinishedLaunching` méthode, avec toujours un grand nombre de frames natifs sur la pile lorsque l’exception managée est levée :

     0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
     1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
     2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
     5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
     6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
     7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
     8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
     9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
    10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
    11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
    12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
    13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
    14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
    15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
    16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
    17: FrontBoardServices      -[FBSSerialQueue _performNext]
    18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
    19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
    20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
    21: CoreFoundation          __CFRunLoopDoSources0
    22: CoreFoundation          __CFRunLoopRun
    23: CoreFoundation          CFRunLoopRunSpecific
    24: CoreFoundation          CFRunLoopRunInMode
    25: UIKit                   -[UIApplication _run]
    26: UIKit                   UIApplicationMain
    27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
    28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
    29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
    30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])

Frames 0-1 et 27-30 sont gérés, tandis que toutes celles entre les deux sont natifs. Si Mono se déroule via ces images, aucun Objective-C `@catch` ou `@finally` clauses seront exécutées.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Scénario 2 : n’a pas pu intercepter des exceptions Objective-C

Dans le scénario suivant, il est _pas_ intercepter les exceptions de Objective-C à l’aide de managed `catch` gestionnaires, car l’exception Objective-C a été gérée dans une autre façon :

1. Une exception Objective-C est levée.
2. Objective-C, le runtime parcourt la pile (mais n’est pas dérouler), recherchez natif `@catch` gestionnaire qui peut gérer l’exception.
3. Objective-C runtime recherche une `@catch` gestionnaire se déroule la pile et commence à s’exécuter le `@catch` gestionnaire.

Ce scénario est couramment trouvé dans les applications Xamarin.iOS, car sur le thread principal il existe généralement code similaire à celui-ci :

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

Cela signifie que sur le thread principal est jamais vraiment une exception non gérée de Objective-C et notre rappel qui convertit les exceptions de Objective-C sur les exceptions gérées n’est jamais appelée.

Cela est également très courant lors du débogage d’applications Xamarin.Mac sur une version antérieure de macOS que Xamarin.Mac prend en charge, car en inspectant la plupart des objets d’interface utilisateur dans le débogueur tente de récupérer les propriétés qui correspondent à des sélecteurs qui n’existent pas sur le (plateforme en cours d’exécution Étant donné que Xamarin.Mac prend en charge une version ultérieure de Mac OS). L’appel de ces sélecteurs lèvera une `NSInvalidArgumentException` (« non reconnu sélecteur envoyé vers... »), qui finit par provoquer le blocage du processus.

Pour résumer, ayant le runtime Objective-C ou sur les frames de déroulement runtime Mono pas programmé pour handle peut provoquer des comportements indéfinis, tels que des incidents, des fuites de mémoire et des autres types de comportements d’imprévisibles (mis).

## <a name="solution"></a>Solution

Dans les 10 Xamarin.iOS et Xamarin.Mac 2.10, nous avons ajouté la prise en charge pour l’interception d’exceptions gérées et Objective-C sur la limite de managé à natif et pour la conversion de cette exception à l’autre type.

En pseudo-code, il ressemble à ceci :

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Le P/Invoke pour objc_msgSend est intercepté et parle à la place :

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

Et quelque chose de similaire est effectuée dans le cas inverse (marshaling les exceptions managées aux exceptions de Objective-C).

Intercepter des exceptions sur la limite de managé à natif n’est pas gratuitement, par conséquent, il n'est pas toujours activé par défaut :

- Xamarin.iOS/tvOS : l’interception des exceptions de Objective-C est activée dans le simulateur.
- Xamarin.watchOS : interception est appliquée dans tous les cas, étant donné que ce qui permet le déroulement de runtime Objective-C géré frames seront confondre le garbage collector et soit rendre blocage ou un incident.
- Xamarin.Mac : l’interception des exceptions de Objective-C est activée pour les versions debug.

Le [indicateurs du moment de la génération](#build_time_flags) section explique comment activer l’interception lorsqu’il n’est pas activé par défaut.

## <a name="events"></a>Événements

Il existe deux nouveaux événements qui sont déclenchés une fois qu’une exception est interceptée : `Runtime.MarshalManagedException` et `Runtime.MarshalObjectiveCException`.

Les deux événements sont transmis un `EventArgs` objet qui contient l’exception d’origine qui a été levée (le `Exception` propriété) et un `ExceptionMode` propriété pour définir comment l’exception doit être marshalée.

Le `ExceptionMode` propriété peut être modifiée dans l’événement gestionnaire pour modifier le comportement en fonction de tout traitement personnalisé dans le gestionnaire. Un exemple serait d’annuler le processus si une certaine exception se produit.

Modification de la `ExceptionMode` propriété s’applique à l’événement unique, il n’affecte pas les exceptions interceptées dans le futur.

Les modes suivants sont disponibles :

- `Default`: La valeur par défaut varie selon la plate-forme. Il s’agit de `ThrowObjectiveCException` si le garbage collector est en mode coopératif (watchOS), et `UnwindNativeCode` sinon (iOS / watchOS / macOS). La valeur par défaut peut changer à l’avenir.
- `UnwindNativeCode`: Il s’agit du comportement précédent de (non défini). Cela n’est pas disponible lorsque vous utilisez le garbage collector en mode coopératif (qui est la seule option sur watchOS ; par conséquent, il n’est pas une option valide sur watchOS), mais c’est l’option par défaut pour toutes les autres plates-formes.
- `ThrowObjectiveCException`: Convertir l’exception managée une exception Objective-C et lever l’exception Objective-C. Il s’agit de la valeur par défaut sur watchOS.
- `Abort`: Le processus d’abandon.
- `Disable`: Désactive l’interception d’exception, il n’a aucune signification pour définir cette valeur dans le Gestionnaire d’événements, mais une fois que l’événement est déclenché. il est trop tard pour la désactiver. Dans tous les cas, si la valeur, il se comporte comme `UnwindNativeCode`.

Pour le marshaling des exceptions Objective-C au code managé, les modes suivants sont disponibles :

- `Default`: La valeur par défaut varie selon la plate-forme. Il s’agit de `ThrowManagedException` si le garbage collector est en mode coopératif (watchOS), et `UnwindManagedCode` sinon (iOS / tvOS / macOS). La valeur par défaut peut changer à l’avenir.
- `UnwindManagedCode`: Il s’agit du comportement précédent de (non défini). Cela n’est pas disponible lorsque vous utilisez le garbage collector en mode coopératif (qui est le mode de catalogue global n’est valide que sur watchOS ; par conséquent, il n’est pas une option valide sur watchOS), mais il est la valeur par défaut pour toutes les autres plates-formes.
- `ThrowManagedException`: Convertir l’exception Objective-C une exception managée et lever l’exception managée. Il s’agit de la valeur par défaut sur watchOS.
- `Abort`: Le processus d’abandon.
- `Disable`: Désactive l’interception d’exception, afin qu’il n’a aucune signification pour définir cette valeur dans le gestionnaire, mais une fois l’événement est déclenché des événements, il est trop tard pour la désactiver. Dans tous les cas si la valeur, il va être abandonné.

Par conséquent, pour afficher chaque fois qu’une exception est marshalée, vous pouvez effectuer ceci :

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>Indicateurs de création

Il est possible de passer des options suivantes pour **mtouch** (pour les applications Xamarin.iOS) et **mmp** (pour les applications Xamarin.Mac), qui détermine si l’interception d’exception est activée et définir l’action par défaut doit se produire :

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

À l’exception de `disable`, ces valeurs sont identiques à la `ExceptionMode` valeurs qui sont passées à la `MarshalManagedException` et `MarshalObjectiveCException` les événements.

Le `disable` option sera _principalement_ désactiver l’interception, mais nous allons toujours intercepter les exceptions lorsqu’il n’ajoute pas de surcharge d’exécution. Les événements de marshaling sont toujours déclenchés pour ces exceptions, avec le mode par défaut est le mode par défaut pour la plateforme en cours d’exécution.

## <a name="limitations"></a>Limitations

Nous intercepter uniquement les P/Invoke vers le `objc_msgSend` famille de fonctions lors de la tentative d’intercepter des exceptions Objective-C. Cela signifie qu’un appel P/Invoke vers une autre fonction C, qui lève des exceptions Objective-C, exécuteront toujours le comportement de l’ancien et non définies (Cela peut être améliorée dans le futur).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>Liens associés

- [Exception Marshaling (exemple)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
