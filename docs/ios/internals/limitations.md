---
title: Limitations de Xamarin.iOS
description: Ce document décrit les limitations de Xamarin.iOS, traitant des génériques, génériques sous-classes de NSObjects, P/Invoke dans les objets génériques et bien plus encore.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: 8eb2cd5a749beab6f089479f5992fe3fbc16dd0a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786227"
---
# <a name="limitations-of-xamarinios"></a>Limitations de Xamarin.iOS

Étant donné que les applications sur l’iPhone à l’aide de Xamarin.iOS sont compilées en code statique, il n’est pas possible d’utiliser toutes les installations qui nécessitent la génération de code lors de l’exécution.

Voici les limitations de Xamarin.iOS par rapport au bureau Mono :

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Prise en charge limitée des génériques

Contrairement à Mono/.NET traditionnelle, le code sur l’iPhone est statiquement compilé avance au lieu d’être compilé à la demande par un compilateur JIT.

De mono [AOA complète](http://www.mono-project.com/docs/advanced/aot/#full-aot) technologie présente quelques limitations en ce qui concerne les génériques, ces erreurs sont dues car pas chaque instanciation générique possible peut être déterminée en amont au moment de la compilation. Cela n’est pas un problème pour les runtimes .NET ou Mono régulières que le code est compilé toujours lors de l’exécution à l’aide de la juste de compilateur temps. Mais cela pose des problèmes pour un compilateur statique comme Xamarin.iOS.

Voici quelques-uns des problèmes courants que vous rencontrez les développeurs :

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Les sous-classes générique de NSObjects sont limitées

Xamarin.iOS a actuellement limité de prise en charge pour la création de sous-classes génériques de la classe NSObject, telles qu’aucune prise en charge pour les méthodes génériques. À compter de 7.2.1)., à l’aide de sous-classes génériques de NSObjects est possible, comme celle-ci :

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Alors que des sous-classes génériques de NSObjects sont possibles, il existe quelques limitations. Lecture la [génériques sous-classes de NSObject](~/ios/internals/api-design/nsobject-generics.md) document pour plus d’informations



### <a name="pinvokes-in-generic-types"></a>P/Invokes dans des Types génériques

P/Invoke dans les classes génériques ne sont pas pris en charge :

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo sur un Type Nullable n’est pas pris en charge.

À l’aide Property.SetInfo de la réflexion pour définir la valeur sur un Nullable&lt;T&gt; n’est pas pris en charge actuellement.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Types valeur en tant que clés de dictionnaire

À l’aide d’un type valeur en tant que dictionnaire&lt;TKey, TValue&gt; clé peut être problématique, comme la valeur par défaut, constructeur de dictionnaire tente d’utiliser des EqualityComparer&lt;TKey&gt;. Valeur par défaut. EqualityComparer&lt;TKey&gt;. Par défaut, à son tour, tente d’utiliser la réflexion pour instancier un nouveau type qui implémente l’IEqualityComparer&lt;TKey&gt; interface.

Cela fonctionne pour les types référence (comme la réflexion + créer une nouvelle étape de type est ignorée), mais pour valeur tape se bloque et grave plutôt rapidement une fois que vous tentez de l’utiliser sur l’appareil.

 **Solution de contournement**: implémenter manuellement le [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) dans un nouveau type d’interface et de fournir une instance de ce type pour le [dictionnaire&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) constructeur.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Aucune génération de Code dynamique

Étant donné que le noyau de l’iPhone empêche une application à partir de la génération de code dynamique ne Mono sur l’iPhone ne prend pas en charge toutes les formes de génération de code dynamique. Elles incluent notamment :

-  Le System.Reflection.Emit n’est pas disponible.
-  Aucune prise en charge pour System.Runtime.Remoting.
-  Aucune prise en charge pour créer dynamiquement des types (aucun Type.GetType (« MyType « 1 »)), bien que la recherche de types existants (Type.GetType (« System.String »), par exemple, qui fonctionne très bien). 
-  Rappels inverses doivent être inscrit avec le runtime au moment de la compilation.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Le manque de System.Reflection. **Émettre** signifie qu’aucun code qui dépend de la génération de code runtime ne fonctionnera pas. Cela inclut des éléments tels que :

-  Le Dynamic Language Runtime.
-  Toutes les langues reposant sur Dynamic Language Runtime.
-  TransparentProxy de la communication à distance ou dans tout autre élément susceptible d’empêcher l’exécution pour générer dynamiquement du code. 


 **Important :** ne confondez pas **Reflection.Emit** avec **réflexion**. Reflection.Emit est sur la génération de code dynamique et avoir ce code JITed et le code compilé en mode natif. En raison des limitations sur l’iPhone (aucune compilation JIT), cela n’est pas possible.

Mais l’API de réflexion entière, y compris Type.GetType (« someClass »), qui répertorie les méthodes, affichage des propriétés, l’extraction des attributs et valeurs fonctionne correctement.

### <a name="using-delegates-to-call-native-functions"></a>Utilisation de délégués pour appeler des fonctions natives

Pour appeler une fonction native via un délégué c#, la déclaration de celui du délégué doit être décorée avec l’un des attributs suivants :

- [UnmanagedFunctionPointerAttribute](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute/) (par défaut, car il est interplateforme et compatible avec .NET Standard 1.1 +)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Ne parvient pas à fournir un de ces attributs provoque une erreur d’exécution telles que :

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Rappels inverses

Dans Mono standard, il est possible de passer des instances de délégué c# au code non managé à la place d’un pointeur de fonction. Le runtime transforment généralement les pointeurs de fonction dans un petit thunk qui permet à du code non managé vers du code managé.

En Mono ces ponts sont implémentées par juste-à-temps compilateur. Lorsque vous utilisez le compilateur anticipée de temps requis par l’iPhone il existe deux limitations importantes à ce stade :

-  Vous devez marquer tous vos méthodes de rappel avec le [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Les méthodes doivent être des méthodes statiques, il n’est pas prise en charge pour l’instance méthodes. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Aucune communication à distance

La pile de communication à distance n’est pas disponible sur Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Runtime désactivé les fonctionnalités

Les fonctionnalités suivantes ont été désactivées dans les e/s de Mono Runtime :

-  Profiler
-  Reflection.Emit
-  Fonctionnalités de Reflection.Emit.Save
-  Liaisons de COM
-  Le moteur JIT
-  Vérificateur de métadonnées (dans la mesure où il n’existe aucun JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Limitations d’API .NET

L’API .NET exposé est un sous-ensemble du framework étant pas tous les éléments disponibles dans iOS. Consultez le Forum aux questions concernant un [la liste des assemblys pris en charge actuellement](~/cross-platform/internals/available-assemblies.md).



En particulier, le profil de l’API utilisé par Xamarin.iOS n’inclut pas System.Configuration, il n’est pas possible d’utiliser des fichiers XML externes pour configurer le comportement du runtime.
