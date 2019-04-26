---
title: Bureau d’enregistrement de type pour Xamarin.iOS
description: Ce document décrit le Registre de type Xamarin.iOS, ce qui rend C# classes disponibles pour le runtime Objective-C.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/29/2018
ms.openlocfilehash: 83340ce2d5db145c29166d90d3a5180b1767d7ca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036393"
---
# <a name="type-registrar-for-xamarinios"></a>Bureau d’enregistrement de type pour Xamarin.iOS

Ce document décrit le système d’inscription de type utilisé par Xamarin.iOS.

## <a name="registration-of-managed-classes-and-methods"></a>Inscription de classes managées et méthodes

Lors du démarrage, Xamarin.iOS s’inscrira :

- Classes avec un [[inscrire]](xref:Foundation.RegisterAttribute) attribut en tant que classes d’Objective-C.
- Classes avec un [[Category]](xref:ObjCRuntime.CategoryAttribute) attribut comme des catégories d’Objective-C.
- Interfaces avec un [[Protocol]](xref:Foundation.ProtocolAttribute) attribut en tant que les protocoles Objective-C.
- Les membres avec une [[Exporter]](xref:Foundation.ExportAttribute), rendant possible pour Objective-C pour y accéder.

Par exemple, considérez le managé `Main` méthode courant dans les applications Xamarin.iOS :

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

Ce code indique au runtime de Objective-C à utiliser le type appelé `AppDelegate` en tant que classe de délégué de l’application. Pour le runtime Objective-C être en mesure de créer une instance de la C# `AppDelegate` classe, que la classe doit être inscrit.

Xamarin.iOS effectue l’inscription automatiquement, lors de l’exécution (inscription dynamique) ou au moment de la compilation (inscription statique).

L’enregistrement dynamique utilise la réflexion au démarrage pour rechercher toutes les classes et méthodes pour s’inscrire, en les passant au runtime Objective-C. L’enregistrement dynamique est utilisé par défaut pour les builds de simulateur.

L’enregistrement statique inspecte, au moment de la compilation, les assemblys utilisés par l’application. Il détermine les classes et les méthodes pour s’inscrire avec Objective-C et génère une carte, qui est incorporée dans votre fichier binaire.
Ensuite, au démarrage, il enregistre le mappage avec le runtime Objective-C. L’enregistrement statique est utilisé pour les builds d’appareil.

### <a name="categories"></a>Categories

À partir de Xamarin.iOS 8.10, il est possible de créer des catégories d’Objective-C à l’aide de C# syntaxe.

Pour créer une catégorie, utilisez la `[Category]` d’attribut et spécifier le type à étendre. Par exemple, le code suivant étend `NSString`:

```csharp
[Category (typeof (NSString))]
```

Chacune des méthodes d’une catégorie a un `[Export]` attribut, rendant ainsi disponible pour le runtime Objective-C :

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

Toutes les méthodes d’extension managée doivent être statiques, mais il est possible de créer des méthodes d’instance Objective-C à l’aide de la norme C# syntaxe pour les méthodes d’extension :

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

Le premier argument de la méthode d’extension est l’instance sur laquelle la méthode a été appelée :

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
 }
 ```

Cet exemple ajoute un natif `toUpper` l’instance de méthode à la `NSString` classe. Cette méthode peut être appelée à partir d’Objective-c :

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

### <a name="protocols"></a>Protocoles

À partir de Xamarin.iOS 8.10, interagit avec le `[Protocol]` attribut est exporté vers Objective-C en tant que protocoles :

```csharp
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
```

Ce code exporte `IMyProtocol` et Objective-C en tant que protocole appelé `MyProtocol` et une classe appelée `MyClass` qui implémente le protocole.

## <a name="new-registration-system"></a>Nouveau système d’inscription

En commençant par le 6.2.6 stable version et la version bêta 6.3.4, nous avons ajouté un nouveau bureau d’enregistrement statique. Dans le 7.2.1 version, nous avons apporté le bureau d’enregistrement de nouveau la valeur par défaut.

Ce nouveau système d’enregistrement offre les nouvelles fonctionnalités suivantes :

- Lors de la compilation de la détection d’erreurs de programmeur :
    - Deux classes sont en cours d’inscription avec le même nom.
    - Plusieurs méthodes exportées pour répondre à la même sélecteur.
- Suppression du code natif non utilisé :
    - Le nouveau système d’inscription ajoutera les références fortes au code utilisé dans les bibliothèques statiques, ce qui permet de l’éditeur de liens natif de retirer le code natif non utilisé du fichier binaire qui en résulte. Sur les liaisons d’exemple de Xamarin, la plupart des applications deviennent plus petits d’au moins 300k.

- Prise en charge des sous-classes génériques de `NSObject`; consultez [NSObject génériques](~/ios/internals/api-design/nsobject-generics.md) pour plus d’informations. En outre, le nouveau système d’inscription interceptera les constructions génériques non pris en charge qui aurait auparavant entraîné un comportement aléatoire lors de l’exécution.

### <a name="errors-caught-by-the-new-registrar"></a>Erreurs interceptées par le bureau d’enregistrement nouveau

Voici quelques exemples des erreurs interceptées par le bureau d’enregistrement de nouveau.

- Exportation de plusieurs fois le même sélecteur dans la même classe :

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo:")]
        void Foo (NSString str);
        [Export ("foo:")]
        void Foo (string str)
    }
    ```

- Exportation de plus d’une classe managée portant le même nom Objective-C :

    ```csharp
    [Register ("Class")]
    class MyClass : NSObject {}

    [Register ("Class")]
    class YourClass : NSObject {}
    ```

- Exportation des méthodes génériques :

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo")]
        void Foo<T> () {}
    }
    ```

### <a name="limitations-of-the-new-registrar"></a>Limitations de la nouvelle bureau d’enregistrement

Voici quelques éléments à prendre en compte sur le nouveau bureau d’enregistrement :

- Certaines des bibliothèques tierces doivent être mis à jour pour fonctionner avec le nouveau système d’inscription. Consultez [requis modifications](#required-modifications) ci-dessous pour plus d’informations.

- Un inconvénient à court terme est également que Clang doit être utilisé si l’infrastructure de comptes est utilisée (il s’agit, car Apple **accounts.h** en-tête peut uniquement être compilé par Clang). Ajouter `--compiler:clang` aux arguments mtouch supplémentaires à utiliser Clang si vous utilisez Xcode 4.6 ou une version antérieure (Xamarin.iOS sélectionne automatiquement Clang dans Xcode 5.0 ou version ultérieure.)

- Si Xcode 4.6 (ou une version antérieure) est utilisé, GCC / G ++ doit être sélectionnée si le type exporté noms contiennent des caractères non-ASCII (c’est parce que la version de Clang livré avec Xcode 4.6 ne prend pas en charge les caractères non-ASCII dans les identificateurs dans le code Objective-C). Ajouter `--compiler:gcc` aux arguments mtouch supplémentaires à utiliser GCC.

## <a name="selecting-a-registrar"></a>Sélection d’un bureau d’enregistrement

Vous pouvez sélectionner un autre bureau d’enregistrement en ajoutant une des options suivantes aux arguments mtouch supplémentaires dans le projet **Build iOS** paramètres :

- `--registrar:static` – par défaut pour les builds d’appareil
- `--registrar:dynamic` – par défaut pour les builds de simulateur

> [!NOTE]
> API classique de Xamarin en charge d’autres options telles que `--registrar:legacystatic` et `--registrar:legacydynamic`. Toutefois, ces options ne sont pas pris en charge par l’API unifiée.

## <a name="shortcomings-in-the-old-registration-system"></a>Défauts de l’ancien système d’inscription

L’ancien système d’inscription présente les inconvénients suivants :

- Il n’a aucune référence statique (natif) pour Objective-C classes et méthodes dans les bibliothèques natives par des tiers, ce qui signifiait que nous n’avons pas pu demander de l’éditeur de liens natif pour supprimer le code natif par des tiers qui n’a pas été réellement utilisé (étant donné que tous les éléments sont supprimées). C’est la raison pour le `-force_load libNative.a` ayant effectuer chaque liaison de tiers (ou l’équivalent `ForceLoad=true` dans le `[LinkWith]` attribut).
- Vous pouviez exporter deux types managés avec le même nom Objective-C sans avertissement. Un scénario rare était de vous retrouver avec deux `AppDelegate` classes dans différents espaces de noms. Lors de l’exécution il serait complètement aléatoire celle a été prélevé (en réalité, il variait d’exécutions d’une application qui n’a pas été reconstruits même - qu’effectuées pour une expérience de débogage très étranges et frustrante).
- Vous pouvez exporter des deux méthodes avec la même signature Objective-C. Une nouvelle fois celui est appelée à partir d’Objective-C était-elle aléatoire (mais ce problème n’était pas aussi courante que le précédent, principalement parce que la seule façon de rencontrer en fait ce bogue a été de substituer la méthode managée dans d’autres).
- L’ensemble de méthodes qui ont été exportée était légèrement différente entre les builds dynamiques et statiques.
- Il ne fonctionne pas correctement lors de l’exportation des classes génériques (quelle implémentation générique exacte exécutée lors de l’exécution serait aléatoire, ce qui entraîne un comportement indéterminé).

<a name="required-modifications" />

## <a name="new-registrar-required-changes-to-bindings"></a>Nouveau bureau d’enregistrement : modifications aux liaisons requises

Cette section décrit les changements de liaisons qui doivent être effectuées pour pouvoir fonctionner avec le bureau d’enregistrement de nouveau.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protocoles doivent avoir l’attribut [Protocol]

Protocoles doivent maintenant avoir le `[Protocol]` attribut. Si vous ne le faites pas, vous allez comme une erreur de l’éditeur de liens natif :

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Sélecteurs doivent avoir un nombre valid de paramètres

Tous les sélecteurs doivent indiquer le nombre de paramètres correctement. Auparavant, ces erreurs ont été ignorées et peut entraîner des problèmes de runtime.

En bref, le nombre de signes deux-points doit correspondre au nombre de paramètres :

- Aucun paramètre : `foo`
- Un seul paramètre : `foo:`
- Deux paramètres : `foo:parameterName2:`

Utilisations incorrectes sont les suivantes :

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Utiliser IsVariadic paramètre dans l’exportation

Les fonctions Variadiques doivent utiliser le `IsVariadic` l’argument de la `[Export]` attribut :

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Doit être lié à des symboles existants

Il est impossible de lier les classes qui n’existent pas dans la bibliothèque native.
Si une classe a été retirée ou renommée dans la bibliothèque native, veillez à mettre à jour les liaisons pour faire correspondre.
