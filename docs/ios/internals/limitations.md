---
title: Limitations de Xamarin.iOS
description: Ce document décrit les limitations de Xamarin.iOS, abordant les génériques, les sous-classes génériques de NSObjects, P/Invoke dans des objets génériques et bien plus encore.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: e154e4e1688b8a3d03459956934409fa9d5aef35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998083"
---
# <a name="limitations-of-xamarinios"></a>Limitations de Xamarin.iOS

Dans la mesure où les applications sur l’iPhone à l’aide de Xamarin.iOS sont compilées en code statique, il n’est pas possible d’utiliser d’équipements qui requièrent la génération de code lors de l’exécution.

Voici les limitations de Xamarin.iOS par rapport au bureau Mono :

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Prise en charge limitée des génériques

Contrairement à Mono/.NET classiques, le code sur l’iPhone est compilé statiquement avance au lieu d’en cours de compilation à la demande par un compilateur JIT.

Mono [AOT complète](http://www.mono-project.com/docs/advanced/aot/#full-aot) technologie présente quelques limitations en ce qui concerne les génériques, ces messages sont dus car pas chaque instanciation générique possible peut être déterminée à l’avance au moment de la compilation. Cela n’est pas un problème pour les runtimes .NET ou Mono régulières comme le code est toujours compilé lors de l’exécution à l’aide de la juste dans le compilateur de temps. Mais cela pose un défi pour un compilateur statique comme Xamarin.iOS.

Certains des problèmes courants que vous rencontrez les développeurs sont les suivantes :

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Sous-classes génériques de NSObjects sont limitées

Xamarin.iOS limitée prise en charge pour la création de sous-classes génériques de la classe de NSObject, comme aucune prise en charge pour les méthodes génériques. À compter de 7.2.1, à l’aide de sous-classes génériques de NSObjects est possible, similaire à celle-ci :

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Bien que les sous-classes génériques de NSObjects soient possibles, il existe quelques limitations. Lire le [sous-classes génériques NSObject](~/ios/internals/api-design/nsobject-generics.md) document pour plus d’informations



### <a name="pinvokes-in-generic-types"></a>P/Invoke dans des Types génériques

P/Invoke dans les classes génériques ne sont pas prises en charge :

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo sur un Type Nullable n’est pas pris en charge.

À l’aide Property.SetInfo de la réflexion pour définir la valeur sur un type Nullable&lt;T&gt; n’est pas pris en charge actuellement.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Types valeur en tant que clés de dictionnaire

À l’aide d’un type valeur en tant que dictionnaire&lt;TKey, TValue&gt; clé est problématique, en tant que la valeur par défaut, constructeur de dictionnaire tente d’utiliser des EqualityComparer&lt;TKey&gt;. Par défaut. EqualityComparer&lt;TKey&gt;. Par défaut, à son tour, tente d’utiliser la réflexion pour instancier un nouveau type qui implémente l’IEqualityComparer&lt;TKey&gt; interface.

Cela fonctionne pour les types référence (comme la réflexion + créer une nouvelle étape de type est ignorée), mais, pour la valeur de types de pannes et grave plutôt rapidement une fois que vous tentez d’utiliser sur l’appareil.

 **Solution de contournement**: implémenter manuellement le [IEqualityComparer&lt;TKey&gt; ](xref:System.Collections.Generic.IEqualityComparer`1) dans un nouveau type d’interface et de fournir une instance de ce type pour le [dictionnaire&lt;TKey, TValue&gt; ](xref:System.Collections.Generic.Dictionary`2) [(IEqualityComparer&lt;TKey&gt;)](xref:System.Collections.Generic.IEqualityComparer`1) constructeur.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Aucune génération de Code dynamique

Étant donné que le noyau de l’iPhone empêche une application à partir de la génération de code dynamique Mono sur l’iPhone ne prend pas en charge toutes les formes de génération de code dynamique. Elles incluent notamment :

-  Le System.Reflection.Emit n’est pas disponible.
-  Aucune prise en charge pour System.Runtime.Remoting.
-  Aucune prise en charge pour créer dynamiquement des types (aucun Type.GetType (« MyType « 1 »)), bien que la recherche de types existants (Type.GetType (« System.String »), par exemple, qui fonctionne bien). 
-  Rappels inverses doivent être enregistrés avec le runtime au moment de la compilation.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

L’absence de System.Reflection. **Émettre** signifie qu’aucun code qui dépend de la génération de code runtime ne fonctionnera. Cela inclut des éléments tels que :

-  Le Dynamic Language Runtime.
-  Toutes les langues reposant sur le Dynamic Language Runtime.
-  Remoting TransparentProxy ou tout autre élément qui provoque l’exécution pour générer dynamiquement du code. 


 **Important :** ne confondez pas **Reflection.Emit** avec **réflexion**. Reflection.Emit est sur la génération de code dynamique et avoir ce code JITed ainsi que compilées en code natif. En raison des limitations sur l’iPhone (aucune compilation JIT) cela n’est pas possible.

Cependant, l’API de réflexion entière, y compris Type.GetType (« someClass »), qui répertorie les méthodes, affichage des propriétés, l’extraction des attributs et valeurs fonctionne parfaitement.

### <a name="using-delegates-to-call-native-functions"></a>L’utilisation de délégués pour appeler des fonctions natives

Pour appeler une fonction native via un délégué c#, les déclaration du délégué doivent être décorée avec l’un des attributs suivants :

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (de préférence, s’agissant d’inter-plateformes et compatible avec .NET Standard 1.1 +)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Ne parvient pas à fournir un de ces attributs entraîne une erreur d’exécution telles que :

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Inverser les rappels

Dans Mono standard, il est possible de passer des instances de délégué c# en code non managé à la place d’un pointeur de fonction. Le runtime est généralement transformer ces pointeurs fonction en un petit thunk qui permet au code non managé effectuer un rappel dans du code managé.

Dans Mono ces ponts sont implémentées par juste-à-temps compilateur. Lorsque l’utilisation du compilateur ahead of time requis par l’iPhone il existe deux limitations importantes à ce stade :

-  Vous devez marquer toutes vos méthodes de rappel avec la [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Les méthodes doivent être des méthodes statiques, il n’est pas pris en charge par exemple méthodes. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Aucune communication à distance

La pile de communication à distance n’est pas disponible sur Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Runtime désactivé les fonctionnalités

Les fonctionnalités suivantes ont été désactivées dans iOS de Mono Runtime :

-  Profiler
-  Reflection.Emit
-  Fonctionnalités de Reflection.Emit.Save
-  Liaisons de COM
-  Le moteur JIT
-  Vérificateur de métadonnées (dans la mesure où il n’existe aucun JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Limitations de l’API .NET

L’API .NET exposée est un sous-ensemble du framework pas tout est disponible dans iOS. Consultez le Forum aux questions pour un [liste des assemblys pris en charge actuellement](~/cross-platform/internals/available-assemblies.md).



En particulier, le profil d’API utilisé par Xamarin.iOS n’inclut pas de System.Configuration, donc il n’est pas possible d’utiliser des fichiers XML externes pour configurer le comportement du runtime.
