---
title: Fonctionnement de Xamarin.Mac
description: "Ce document décrit le fonctionnement interne de Xamarin.Mac. En particulier, il examine les constructeurs, gestion de la mémoire, avant la compilation de temps et le bureau d’enregistrement."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: a1dbff32b113bd1c3a6b2058a34c73977c59c9e5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="how-xamarinmac-works"></a>Fonctionnement de Xamarin.Mac

La plupart du temps, le développeur aura jamais à vous soucier du interne « magiques » de Xamarin.Mac, toutefois, avoir une compréhension approximative de comment fonctionne choses sous le capot aidera dans interprétation de la documentation existante avec un objectif c# et débogage des problèmes lors de la ils surviennent.

Dans Xamarin.Mac, une application établit un pont entre deux mondes : l’exécution en fonction de Objective-C contenant des instances de classes natives (`NSString`, `NSApplication`, etc.) et l’exécution c# contenant des instances de classes managées (`System.String`, `HttpClient`, etc.). Entre ces deux mondes Xamarin.Mac crée un pont bidirectionnelle de sorte qu’une application peut appeler des méthodes (sélecteurs) dans Objective-C (tel que `NSApplication.Init`) et Objective-C peut appeler des méthodes de l’application c# précédent (comme les méthodes sur un délégué de l’application). En règle générale, les appels dans Objective-C sont gérés en toute transparence **P/Invoke** et du code de runtime fournit de Xamarin.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>Exposition de classes c# / méthodes pour Objective-C

Toutefois, pour Objective-C rappeler dans les objets d’une application c#, ils doivent être exposées de manière Objective-C peut comprendre. Cette opération est effectuée le `Register` et `Export` attributs. Prenons l’exemple suivant :

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

Dans cet exemple, le runtime Objective-C maintenant connaissent une classe appelée `MyClass` avec sélecteurs appelés `init` et `run`.

Dans la plupart des cas, il s’agit d’un détail d’implémentation que le développeur peut ignorer, car la plupart des rappels d’une application reçoit via les méthodes substituées sera sur `base` classes (tel que `AppDelegate`, `Delegates`, `DataSources`) ou sur  **Actions** passé dans l’API. Dans tous ces cas, `Export` attributs ne sont pas nécessaires dans le code c#.

## <a name="constructor-runthrough"></a>Révision du constructeur

Dans de nombreux cas, le développeur doit exposer c# classes construction l’application API à l’exécution Objective-C afin de pouvoir être instancié à partir d’emplacements tels que lorsqu’elle est appelée dans les fichiers de plan conceptuel ou XIB. Voici les cinq constructeurs courants utilisés dans les applications Xamarin.Mac :

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

En général, le développeur doit laisser le `IntPtr` et `NSCoder` constructeurs générés lors de la création de certains types tels que personnalisé `NSViews` uniquement. Si Xamarin.Mac a besoin d’appeler l’une de ces constructeurs en réponse à une demande de runtime Objective-C et que vous l’avez supprimé, l’application se bloque à l’intérieur du code natif et il peut être difficile d’identifier exactement le problème.

## <a name="memory-management-and-cycles"></a>Cycles et gestion de la mémoire

Gestion de la mémoire dans Xamarin.Mac est bien des égards semblable à Xamarin.iOS. Il est également un sujet complexe, un dépasse le cadre de ce document. Veuillez lire les [mémoire et les performances les meilleures pratiques](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>De compilation à l’avance

En règle générale, les applications .NET ne compilez pas code machine lorsqu’elles sont générées, à la place de compilation pour une couche intermédiaire appelée code IL qui obtient _juste-à-temps_ (JIT) est compilé en code machine lorsque l’application est lancée.

Le temps nécessaire à la compilation en JIT le runtime mono ce code machine peut ralentir le lancement d’une application Xamarin.Mac jusqu'à 20 %, comme le temps requis pour le code machine nécessaire à générer.

En raison des limitations imposées par Apple iOS, la compilation JIT du code de langage intermédiaire n’est pas disponible pour Xamarin.iOS. Par conséquent, toutes les applications de Xamarin.iOS sont saturées _anticipée de temps_ (AOA) est compilé en code machine au cours du cycle de génération.

Nouvelle à Xamarin.Mac est la capacité à AOA le code IL au cours du cycle de génération d’application, comme Xamarin.iOS peut. Xamarin.Mac utilise un _hybride_ approche AOA qui compile la majorité du code machine nécessaire, mais permet l’exécution de compilation trampolines nécessaires et la possibilité de continuer à prendre en charge de Reflection.Emit (et autres à l’application qui travaillez sur Xamarin.Mac).

Il existe deux principales zones où AOA peut aider à une application Xamarin.Mac :

- **Une meilleure journaux « natifs »** : si une application Xamarin.Mac se bloque en code natif, ce qui est souvent lors d’appels non valide dans les API/Cocoa (telles que l’envoi d’un `null` dans une méthode qui n’accepte pas) natif journaux avec JIT d’incident les cadres sont difficiles à analyser. Étant donné que les frames JIT n’ont pas les informations de débogage, il y aura plusieurs lignes avec des décalages hexadécimales et aucun indice, ce qui était en cours. AOA génère des images nommées « réal » et les suivis sont beaucoup plus faciles à lire. Cela signifie également Xamarin.Mac application va interagir mieux avec les outils natifs comme **lldb** et **Instruments**.
- **Lancez mieux les performances au moment de** - pour les grandes applications Xamarin.Mac, avec un plusieurs deuxième temps de démarrage, tout le code de compilation JIT peuvent prendre beaucoup de temps. AOA effectue ce travail en amont.

### <a name="enabling-aot-compilation"></a>L’activation de la compilation AOA

AOA est activée dans Xamarin.Mac en double-cliquant sur le **nom du projet** dans les **l’Explorateur de solutions**, la navigation vers **Mac générer** et l’ajout de `--aot:[options]` à la  **Les arguments supplémentaires mmp :** champ (où `[options]` est une ou plusieurs options pour contrôler le type AOA, voir ci-dessous). Exemple :

![Ajout d’AOA pour les arguments supplémentaires mmp](how-it-works-images/aot01.png "Ajout AOA aux arguments de mmp supplémentaires")

> [!IMPORTANT]
> L’activation d’AOA compilation augmente considérablement le temps de génération, parfois plusieurs minutes, mais elle peut améliorer les temps de lancement d’application de 20 % en moyenne. Par conséquent, la compilation AOA doit être activée sur **version** génère d’une application Xamarin.Mac.

### <a name="aot-compilation-options"></a>Options de compilation AOA

Il existe plusieurs options qui peuvent être ajustées lors de l’activation de la compilation d’une application Xamarin.Mac AOA :

- `none` -Aucune compilation AOA. Il s'agit du paramètre par défaut.
- `all` -AOA compile chaque assembly dans le MonoBundle.
- `core` -AOA compile la `Xamarin.Mac`, `System` et `mscorlib` assemblys.
- `sdk` -AOA compile la `Xamarin.Mac` et assemblys de bibliothèques de classes de Base (BCL).
- `|hybrid` -Ajout ainsi à une des options ci-dessus hybride AOA, ce qui permet de supprimer du langage intermédiaire, mais pourra entraîner plus être compilé fois.
- `+` -Comprend un seul pour à la compilation d’AOA.
- `-` -Supprime un fichier unique à partir de la compilation d’AOA.

Par exemple, `--aot:all,-MyAssembly.dll` permettrait compilation AOA sur tous les assemblys dans le MonoBundle _sauf_ `MyAssembly.dll` et `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` permettrait hybride, le code AOA inclut le `MyOtherAssembly.dll` et en excluant les `mscorlib.dll`.

## <a name="partial-static-registrar"></a>Bureau d’enregistrement de statique partielle

Lorsque vous développez une application Xamarin.Mac, réduisant ainsi le temps entre la fin d’une modification et de test qu’il peut devenir important de respecter les délais de développement. Stratégies telles que la modularisation de passer et de tests unitaires peuvent aider à réduire le temps de compilation, car elles réduisent le nombre de fois qu’une application nécessite une régénération complète coûteuse.

En outre, nouvelle fonction Xamarin.Mac, _partielle de bureau d’enregistrement statique_ (comme développée par Xamarin.iOS) peuvent réduire considérablement les temps de lancement d’une application Xamarin.Mac dans les **déboguer** configuration. Comprendre comment le bureau d’enregistrement statique partielle permet insérée à un presque une amélioration 5 x lancement du débogage prendra quelques explications sur le bureau d’enregistrement, ce qui est la différence entre statiques et dynamiques, ainsi que ce que fait cette version de « statique partielle ».

### <a name="about-the-registrar"></a>Sur le bureau d’enregistrement

Sous le capot de n’importe quel Xamarin.Mac application trouve le framework/Cocoa par Apple et le runtime Objective-C. Création d’un pont entre cette « world natif » et « world managée » de c# est la responsabilité principale de Xamarin.Mac. Partie de cette tâche est gérée par le bureau d’enregistrement, qui est exécutée à l’intérieur de `NSApplication.Init ()` (méthode). Il s’agit d’une des raisons qui nécessite de toute utilisation de l’API/Cocoa dans Xamarin.Mac `NSApplication.Init` à être appelé en premier.

Les travaux du bureau d’enregistrement sont d’informer le runtime Objective-C de l’existence de c# classes l’application qui dérivent des classes telles que `NSApplicationDelegate`, `NSView`, `NSWindow`, et `NSObject`. Cela nécessite une analyse de tous les types dans l’application pour déterminer les besoins de l’inscription et les éléments sur chaque type de rapport.

Cette analyse peut être effectuée **dynamiquement**, au démarrage de l’application avec la réflexion, ou **statiquement**, comme une étape de génération. Lors de la sélection d’un type d’enregistrement, le développeur doit être conscient des éléments suivants :

- D’inscription statique peut réduire considérablement le temps de lancement, mais peut ralentir les builds fois considérablement (généralement plus de double temps de build debug). Il s’agit de la valeur par défaut pour **version** configuration génère.
- L’enregistrement dynamique retarde ce travail jusqu'à ce que l’application de lancement et ignore la génération de code, mais ce travail supplémentaire peut créer une pause notable (au moins deux secondes) dans le lancement de l’application. Cela est particulièrement notable dans les versions de configuration debug, qui utilise par défaut pour l’enregistrement dynamique et dont la réflexion est plus lente.

L’enregistrement statique partielle, introduite initialement dans Xamarin.iOS 8.13, donne au développeur le meilleur des deux options. En calculant avant des informations d’inscription de chaque élément dans `Xamarin.Mac.dll` et les journaux de transaction ces informations avec Xamarin.Mac dans une bibliothèque statique (qui ne doit être lié au moment de la génération), Microsoft a supprimé la plupart du temps de réflexion de la dynamique bureau d’enregistrement sans impacter le moment de la génération.

### <a name="enabling-the-partial-static-registrar"></a>Activer le bureau d’enregistrement statique partielle

Le bureau d’enregistrement statique partielle est activée dans Xamarin.Mac en double-cliquant sur le **nom du projet** dans les **l’Explorateur de solutions**, la navigation vers **Mac générer** et l’ajout de `--registrar:static` à la **les arguments supplémentaires mmp :** champ. Exemple :

![Ajout du bureau d’enregistrement statique partielle pour les arguments supplémentaires mmp](how-it-works-images/psr01.png "ajout le bureau d’enregistrement statique partielle pour les arguments supplémentaires mmp")

## <a name="additional-resources"></a>Ressources supplémentaires

Voici certaines des explications plus détaillées du fonctionnement en interne :

- [Sélecteurs objective-C](~/ios/internals/objective-c-selectors.md)
- [Inscription](~/ios/internals/registrar.md)
- [API unifiée de Xamarin pour iOS et OS X](~/cross-platform/macios/unified/index.md)
- [Notions de base Theading](~/ios/app-fundamentals/threading.md)
- [Les délégués, les protocoles et les événements](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [À propos `newrefcount`](~/ios/internals/newrefcount.md)

