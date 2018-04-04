---
title: Bureau d’enregistrement de type
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 2504ab36ed9d387cdfccd5ac4d01c85d9bec7cf3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="type-registrar"></a>Bureau d’enregistrement de type

Ce document décrit le système d’enregistrement de type utilisé par Xamarin.iOS.

## <a name="registration-of-managed-classes-and-methods"></a>Inscription de classes managées et de méthodes

Lors du démarrage, Xamarin.iOS inscrira :

  - Classes avec une [[Enregistrer]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) attribut en tant que classes d’Objective-C.
  - Classes avec une [[catégorie]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) attribut en tant que catégories de Objective-C.
  - L’interface avec un [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) attribut en tant que les protocoles Objective-C.

et dans les membres de chaque cas avec un [[Exporter]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) attribut sont exportés vers l’objectif-C. Cela permet à des classes managées à être créés et gérés de méthodes à appeler à partir de Objective-C est la façon que les méthodes et propriétés sont liées entre le monde c# et Objective-C une.

Un exemple très simple est la classe de AppDelegate dont toutes les applications. Souvenez-vous que la méthode Main gérée a une ligne telle que celle-ci :

    UIApplication.Main (args, null, "AppDelegate");

Cela indique au runtime de Objective-C pour créer le type appelé « AppDelegate » en tant que classe de délégué de l’application.  Pour le runtime Objective-C savoir comment créer une instance de la classe « AppDelegate » écrite en c#, cette classe doit être enregistré.

Xamarin.iOS runtime s’occupe de l’inscription pour vous et en interne, cette inscription peut être effectuée entièrement lors de l’exécution (enregistrement dynamique) ou elle peut être effectuée au moment de la compilation (inscription statique).  L’approche dynamique implique l’utilisation de réflexion au démarrage pour rechercher toutes les classes et méthodes pour inscrire et transfèrent ces informations au runtime Objective-C.  L’approche statique inspecte les assemblys utilisés par l’application au moment de la compilation.  Il détermine les classes et méthodes pour inscrire avec Objective-C et génère un plan qui est incorporé dans votre fichier binaire.  Ensuite, au démarrage, nous enregistrons le mappage avec le runtime Objective-C.

### <a name="categories"></a>Categories

Compter Xamarin.iOS 8.10, il sera possible de créer des catégories de Objective-C à l’aide de la syntaxe c#.

Cette opération est effectuée à l’aide de l’attribut [Category], spécification du type à étendre en tant qu’argument à l’attribut.
L’exemple suivant étend pour l’instance NSString :

    [Category (typeof (NSString))]

Chaque méthode de catégorie utilise le mécanisme normal pour l’exportation des méthodes à l’aide de l’attribut [Exporter] Objective-C :

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Toutes les méthodes d’extension managée doivent être statiques, mais il est possible de créer des méthodes d’instance Objective-C à l’aide de la syntaxe standard pour les méthodes d’extension dans c# :

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

et le premier argument de la méthode d’extension sera l’instance sur laquelle la méthode a été appelée.

Exemple complet :

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

Cet exemple ajoutera une méthode d’instance toUpper native à la classe NSString, qui peut être appelée à partir de l’objectif-C.

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>Protocoles

En commençant par Xamarin.iOS 8.10 interfaces avec l’attribut [Protocol] seront exportés vers Objective-C comme des protocoles.

Exemple :

    [Protocol ("MyProtocol")]
    interface IMyProtocol
    {
        [Export ("method")]
        void Method ();
    }

    class MyClass : IMyProtocol
    {
        void Method ()
        {
        }
    }

Cela est exporté vers Objective-C comme un protocole (MyProtocol) et une classe qui implémente le protocole (MyClass).

 **Enregistrement dynamique**

est utilisé pour les builds de simulateur, car elle accélère le cycle de build/debug.  Voici le résultat d’éliminer les étapes qui génère le mappage de classe et la compilation de ce tableau de mappage dans votre service de lancement d’application chaque fois que vous démarrez votre application, et à la place un lanceur à usage général est utilisé chaque fois.  Votre ordinateur de bureau dispose de beaucoup de puissance de l’exécution pour l’analyse des classes rapidement, donc les performances ne sont jamais un problème.

 **Inscription statique**

est conçue pour les builds d’appareil des appareils mobiles sont plus lentes que les ordinateurs de bureau, et d’exécution runtime analyse lente.  Étant donné qu’appareil génère toujours devez créer un nouveau fichier binaire, le cycle de build/debug n’est pas affecté par la création de la mise en correspondance d’inscription.

## <a name="new-registration-system"></a>Nouveau système d’enregistrement

En commençant par le 6.2.6 stable version et la version bêta 6.3.4 que nous avons ajouté un nouveau bureau d’enregistrement statique. Dans la 7.2.1). version nous avons apporté le bureau d’enregistrement de nouveau la valeur par défaut.

Ce nouveau système d’enregistrement offre les nouvelles fonctionnalités suivantes :

- Compilez la détection d’erreurs de programmeur de temps :
    - Deux classes sont en cours d’inscription avec le même nom.
    - Plusieurs méthodes exportées pour répondre à la même sélecteur.



- Peut supprimer inutilisés en code natif
    - Le nouveau système d’enregistrement ajoutera des références fortes au code utilisé dans les bibliothèques statiques, ce qui permet de l’éditeur de liens native à retirer le code natif non utilisé du fichier binaire qui en résulte.
      Sur les liaisons d’exemple de Xamarin, la plupart des applications deviennent plus petits au moins égale à 300 Ko.

- Prise en charge pour les sous-classes génériques de NSObject. Consultez [NSObject génériques](~/ios/internals/api-design/nsobject-generics.md) pour plus d’informations. En outre, le nouveau système d’enregistrement interceptera non pris en charge des constructions génériques qui aurait entraîné précédemment un comportement aléatoire lors de l’exécution.

Voici quelques exemples d’erreurs interceptées par le nouveau registar :

Exportez le même sélecteur plusieurs fois dans la même classe.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

Exportation de plus d’une classe managée portant le même nom Objective-C.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

Exportation des méthodes génériques.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



Certaines considérations à garder à l’esprit que le bureau d’enregistrement de nouveau :
- Certains tiers bibliothèques doivent être mis à jour pour fonctionner avec le nouveau système d’inscription, consultez la section [requis modifications ci-dessous](#required_modifications) pour plus d’informations.
- Un inconvénient à court terme est également que Clang doit être utilisé si l’infrastructure de comptes est utilisé (c’est parce que l’en-tête de accounts.h d’Apple peut uniquement être compilée par Clang). Ajoutez <code>--compiler:clang</code> aux arguments mtouch supplémentaires à utiliser Clang si votre utilisez Xcode 4.6 ou version antérieure (Xamarin.iOS sélectionne automatiquement Clang dans Xcode 5.0 ou version ultérieure.)

    <li>Si Xcode 4.6 (ou une version antérieure) est utilisé, GCC / G ++ doit être sélectionnée si le type exporté noms contiennent des caractères non-ascii (c’est parce que la version de Clang livré avec Xcode 4.6 ne prend pas en charge les caractères non-ascii à l’intérieur des identificateurs dans le code Objective-C). Ajoutez <code>--compiler:gcc</code> aux arguments mtouch supplémentaires à utiliser GCC.


## <a name="selecting-a-registrar"></a>Sélection d’un bureau d’enregistrement

Vous pouvez sélectionner un autre bureau d’enregistrement en ajoutant une des options suivantes pour les arguments supplémentaires mtouch dans les e/s du projet options de Build :

-  `--registrar:static` : par défaut pour les versions de l’appareil
-  `--registrar:dynamic` : par défaut pour les versions de simulateur
-  `--registrar:legacystatic` : par défaut pour les versions de périphérique tant que Xamarin.iOS 7.2.1).
-  `--registrar:legacydynamic` : par défaut pour les versions de simulateur jusqu'à Xamarin.iOS 7.2.1).


## <a name="shortcomings-in-the-old-registration-system"></a>Défauts de l’ancien système d’enregistrement

L’ancien système d’inscription présente les inconvénients suivants :

-  Il n’a aucune référence statique (natif) pour Objective-C classes et méthodes dans les bibliothèques natives tiers, ce qui signifiait que nous n’avons pas pu demander de l’éditeur de liens native pour supprimer le code natif tiers qui n’a pas été réellement utilisé (parce que tous les éléments seront supprimés). C’est la raison pour le «-force_load libNative.a » que chaque liaison tiers dû faire (ou l’équivalent ForceLoad = true dans l’attribut [LinkWith]).
-  Vous pouvez exporter des deux types managés avec le même nom Objective-C, sans avertissement. Un scénario rare a été se retrouver avec deux classes AppDelegate (dans les espaces de noms différents). Lors de l’exécution serait aléatoire complètement l’application a été sélectionnée (en fait qu'il variée entre chaque exécution d’une application qui n’a pas été reconstruits même - qu’effectuées pour une expérience de débogage très doivent et frustrante).
-  Vous pouvez exporter des deux méthodes avec la même signature Objective-C. Une nouvelle fois celle qui est appelée à partir de Objective-C a été aléatoire (mais ce problème n’a pas été fréquent que la précédente, principalement car la seule façon réellement rencontrer ce bogue a été pour substituer la méthode managée dans d’autres).
-  L’ensemble des méthodes qui a été exporté a été légèrement entre les builds statiques et dynamiques.
-  Il ne fonctionne pas correctement lors de l’exportation des classes génériques (quelle implémentation générique exacte exécutée lors de l’exécution est aléatoire, ce qui entraîne un comportement indéterminé).


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>Bureau d’enregistrement de nouveau : Les modifications requises aux liaisons


Les liaisons existantes Objective-C peut-être être mise à jour pour fonctionner avec la nouvelle registar.

Voici une liste des modifications qui doivent être effectuées.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protocoles doivent avoir l’attribut [Protocol]

Les implémentations de protocoles doivent avoir désormais l’attribut [Protocol].  Si vous ne le faites pas, vous allez une erreur de l’éditeur de liens natif comme celle-ci :

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Sélecteurs doivent avoir un nombre valid de paramètres

Tous les sélecteurs doivent indiquer un nombre de paramètres correctement.  Auparavant, ces erreurs ont été ignorés et peut entraîner des problèmes de runtime.

En bref, le nombre de signes deux-points doit correspondre au nombre de paramètres.

Par exemple :

-  Aucun paramètre : « foo »
-  Un seul paramètre : ' foo : »
-  Deux paramètres : ' foo:parameterName2 : »


Les utilisations incorrectes sont les suivantes :

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Utilisez IsVariadic paramètre dans l’exportation

Les fonctions Variadiques doivent indiquer, dans leur attribut exportation (il s’agit, car le sélecteur est incorrect sur le nombre d’arguments, autrement dit, point 2. à partir du haut est violé) :

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Doit être liée à des symboles existants

Il est impossible de lier des classes qui n’existent pas dans la bibliothèque native.

Vous obtiendrez une erreur de l’éditeur de liens native si vous essayez de lier les classes non existante.

Cela se produit généralement lorsqu’une liaison qui a existé pendant un certain temps, et le code natif a été modifié pendant ce temps, afin qu’une classe native particulier a été supprimée ou renommée, tandis que la liaison n’a pas été mis à jour.

