---
title: Fonctionnement de Xamarin.Mac
description: Ce document décrit le fonctionnement interne de Xamarin.Mac. En particulier, il examine les constructeurs, gestion de la mémoire, avant la compilation de temps et le bureau d’enregistrement.
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/25/2017
ms.openlocfilehash: 0635e110cb2aa7bc00234d3d06df57e0fd6f966e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033849"
---
# <a name="how-xamarinmac-works"></a>Fonctionnement de Xamarin.Mac

La plupart du temps le développeur n’aura jamais à vous soucier de l’interne « magiques » de Xamarin.Mac, toutefois, avoir une compréhension approximative de comment fonctionne choses sous le capot aidera dans les deux l’interprétation documentation existante avec un C# lentille et débogage problèmes lorsqu’ils surviennent.

Dans Xamarin.Mac, une application établit un pont entre deux mondes : Il est le runtime Objective-C en contenant des instances de classes natives (`NSString`, `NSApplication`, etc.) et qu’il existe le C# runtime contenant des instances des classes managées (`System.String`, `HttpClient`, etc.). Entre ces deux mondes, Xamarin.Mac crée un pont à double sens pour une application peut appeler des méthodes (sélecteurs) en Objective-C (tel que `NSApplication.Init`) et Objective-C peut appeler l’application C# méthodes sauvegarder (telles que les méthodes sur un délégué de l’application). En règle générale, les appels dans Objective-C sont gérées de façon transparente **P/Invoke** et du code de runtime fournit de Xamarin.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>Exposition C# classes / méthodes vers Objective-C

Toutefois, pour Objective-C effectuer un rappel dans une application C# dont ils ont besoin d’être exposée de façon Objective-C peut comprendre des objets. Cette opération est effectuée le `Register` et `Export` attributs. Prenons l’exemple suivant :

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

Dans cet exemple, le runtime Objective-C savent maintenant sur une classe appelée `MyClass` avec sélecteurs appelés `init` et `run`.

Dans la plupart des cas, il s’agit un détail d’implémentation que le développeur peut ignorer, car la plupart des rappels reçoit une application via les méthodes substituées sera sur `base` classes (tel que `AppDelegate`, `Delegates`, `DataSources`) ou sur  **Actions** passé dans l’API. Dans tous ces cas, `Export` attributs ne sont pas nécessaires dans le C# code.

## <a name="constructor-runthrough"></a>Révision de constructeur

Dans de nombreux cas, le développeur doit exposer l’application C# construction de classes API au runtime Objective-C afin de pouvoir être instancié à partir d’emplacements, par exemple quand appelée dans la table de montage séquentiel ou XIB fichiers. Voici les cinq constructeurs courants utilisés dans les applications Xamarin.Mac :

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

En règle générale, le développeur doit laisser le `IntPtr` et `NSCoder` constructeurs qui sont générés lors de la création de certains types tels que personnalisé `NSViews` autonome. Si Xamarin.Mac doit appeler l’une de ces constructeurs en réponse à une demande de runtime Objective-C et que vous l’avez supprimée, l’application se bloque à l’intérieur du code natif et il peut être difficile de déterminer exactement le problème.

## <a name="memory-management-and-cycles"></a>Cycles et gestion de la mémoire

Gestion de la mémoire dans Xamarin.Mac est bien des égards semblable à Xamarin.iOS. Il est également un sujet complexe, un dépasse le cadre de ce document. Veuillez lire le [mémoire et les meilleures pratiques de performances](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Longueur d’avance sur au moins une compilation

En règle générale, les applications .NET ne se compilent pas code machine lorsqu’elles sont générées, au lieu de cela, elles compilent à une couche intermédiaire appelée code de langage intermédiaire qui obtient _juste-à-temps_ (JIT) est compilé en code machine lorsque l’application est lancée.

L’heure à laquelle le runtime mono nécessaire à la compilation JIT ce code machine peut ralentir le lancement d’une application Xamarin.Mac jusqu'à 20 %, comme le temps requis pour le code machine nécessaire doit être généré.

En raison des limitations imposées par Apple iOS, la compilation JIT du code de langage intermédiaire n’est pas disponible pour Xamarin.iOS. Par conséquent, toutes les applications Xamarin.iOS sont saturées _Ahead-Of-Time_ (AOT) compilé en code machine au cours du cycle de build.

Nouvelle vers Xamarin.Mac consiste à AOT le code de langage intermédiaire au cours du cycle de build d’application, tout comme vous pouvez Xamarin.iOS. Xamarin.Mac utilise un _hybride_ approche AOT qui compile une majorité du code machine nécessaire, mais permet au runtime de compiler trampolines nécessaires et la possibilité de continuer à prendre en charge de Reflection.Emit (et autres cas, qui actuellement, fonctionne sur Xamarin.Mac).

Il existe deux principaux domaines où AOT peut vous aider à une application Xamarin.Mac :

- **Mieux les journaux d’incidents « native »** : si une application Xamarin.Mac tombe en panne en code natif, ce qui est fréquemment lors des appels non valides dans les API Cocoa (comme l’envoi d’un `null` dans une méthode qui n’accepte pas) native journaux avec JIT d’incident trames sont difficiles à analyser. Dans la mesure où les frames JIT n’ont pas les informations de débogage, il y aura plusieurs lignes avec des décalages hexadécimales et aucun doute ce qui se passait. AOT génère des images nommées « réal » et les traces sont beaucoup plus faciles à lire. Cela signifie également l’application Xamarin.Mac interagissent mieux avec des outils natifs tels que **lldb** et **Instruments**.
- **Lancez mieux ponctualité** - pour les grandes applications Xamarin.Mac, avec un plusieurs deuxième temps de démarrage, tout le code de compilation JIT peuvent prendre beaucoup de temps. AOT effectue ce travail en amont.

### <a name="enabling-aot-compilation"></a>L’activation de la compilation AOT

AOT est activé dans Xamarin.Mac en double-cliquant sur le **nom_projet** dans le **l’Explorateur de solutions**, la navigation vers **Build Mac** et l’ajout de `--aot:[options]` à la  **Arguments mmp supplémentaires :** champ (où `[options]` est une ou plusieurs options pour contrôler le type de compilation AOT, voir ci-dessous). Exemple :

![Ajout d’AOT à arguments mmp supplémentaires](how-it-works-images/aot01.png "Ajout AOT à arguments mmp supplémentaires")

> [!IMPORTANT]
> L’activation de AOT compilation augmente considérablement le temps de génération, parfois plusieurs minutes, mais elle peut améliorer les temps de lancement d’application de 20 % en moyenne. Par conséquent, la compilation AOT doit uniquement être activée sur **version** builds d’une application Xamarin.Mac.

### <a name="aot-compilation-options"></a>Options de compilation AOT

Il existe plusieurs options qui peuvent être ajustées lors de l’activation de la compilation AOT sur une application Xamarin.Mac :

- `none` -Aucune compilation AOT. Il s'agit du paramètre par défaut.
- `all` -AOT compile chaque assembly dans le MonoBundle.
- `core` -AOT compile le `Xamarin.Mac`, `System` et `mscorlib` assemblys.
- `sdk` -AOT compile le `Xamarin.Mac` et assemblys de bibliothèques de classes de Base (BCL).
- `|hybrid` -Ajout ainsi à une des options ci-dessus hybride AOT, ce qui permet la suppression du code IL, mais sera le résultat de compilation plus plus longs.
- `+` : Inclut un seul fichier pour la compilation AOT.
- `-` -Supprime un fichier unique à partir de la compilation AOT.

Par exemple, `--aot:all,-MyAssembly.dll` permettrait la compilation AOT sur tous les assemblys dans le MonoBundle _sauf_ `MyAssembly.dll` et `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` permettrait hybride, code AOT incluent la `MyOtherAssembly.dll` et à l’exclusion de la `mscorlib.dll`.

## <a name="partial-static-registrar"></a>Bureau d’enregistrement statique partielle

Lorsque vous développez une application Xamarin.Mac, réduisant ainsi le temps entre la fin d’une modification et de test qu’il peut devenir important de respecter les délais de développement. Stratégies telles que la modularité des codes base et de tests unitaires peuvent aider à réduire le temps de compilation, comme elles réduisent le nombre de fois qu’une application nécessite une régénération complète coûteuse.

En outre et de nouveau vers Xamarin.Mac, _partielle bureau d’enregistrement statique_ (tel qu’il est développé par Xamarin.iOS) permet de réduire considérablement les temps de lancement d’une application Xamarin.Mac dans le **déboguer** configuration. Comprendre comment à l’aide du bureau d’enregistrement statique partielle peut compressés un presque une amélioration de 5 x de lancement du débogage prendra un peu d’arrière-plan sur le bureau d’enregistrement, ce qui est la différence entre statiques et dynamiques, ainsi que ce que fait cette version de « static partielle ».

### <a name="about-the-registrar"></a>Sur le bureau d’enregistrement

Sous le capot de n’importe quel Xamarin.Mac application trouve le framework Cocoa à partir d’Apple et le runtime Objective-C. Création d’un pont entre cette « world natif » et le « monde géré » de C# est la responsabilité principale de Xamarin.Mac. Partie de cette tâche est gérée par le bureau d’enregistrement, qui est exécutée à l’intérieur de `NSApplication.Init ()` (méthode). Il s’agit d’une des raisons qui requiert de toute utilisation de l’API Cocoa dans Xamarin.Mac `NSApplication.Init` à être appelée en premier.

Travail du bureau d’enregistrement est d’informer le runtime Objective-C de l’existence de l’application C# classes qui dérivent de classes telles que `NSApplicationDelegate`, `NSView`, `NSWindow`, et `NSObject`. Cela nécessite une analyse de tous les types dans l’application pour déterminer ce qui nécessite l’inscription et les éléments sur chaque type de rapport.

Cette analyse peut être effectuée **dynamiquement**, au démarrage de l’application avec la réflexion, ou **statiquement**, comme une étape de temps de génération. Lors de la sélection d’un type d’enregistrement, le développeur doit être conscient des éléments suivants :

- L’enregistrement statique peut réduire considérablement le temps de lancement, mais peut ralentir les temps de builds considérablement (généralement à double que le temps de build debug). Il s’agit de la valeur par défaut pour **version** configuration génère.
- L’enregistrement dynamique retarde ce travail jusqu'à ce que l’application de lancement et ignore la génération de code, mais ce travail supplémentaire peut créer un temps de pause (au moins deux secondes) dans le lancement de l’application. Cela est particulièrement notable dans les versions de configuration debug, qui est par défaut pour l’enregistrement dynamique et dont la réflexion est plus lente.

L’enregistrement statique partielle, introduite dans Xamarin.iOS 8.13, donne au développeur le meilleur des deux options. Les informations d’inscription de tous les éléments en précalculant `Xamarin.Mac.dll` et copie ces informations avec Xamarin.Mac dans une bibliothèque statique (qui ne doit être lié au moment de la génération), Microsoft a supprimé la plupart du temps de réflexion de dynamique bureau d’enregistrement sans impacter les temps de génération.

### <a name="enabling-the-partial-static-registrar"></a>L’activation du bureau d’enregistrement statique partielle

Le bureau d’enregistrement statique partielle est activée dans Xamarin.Mac en double-cliquant sur le **nom_projet** dans le **l’Explorateur de solutions**, la navigation vers **Build Mac** et l’ajout de `--registrar:static` à la **arguments mmp supplémentaires :** champ. Exemple :

![Ajout du bureau d’enregistrement statique partielle à arguments mmp supplémentaires](how-it-works-images/psr01.png "Ajout du bureau d’enregistrement statique partielle à arguments mmp supplémentaires")

## <a name="additional-resources"></a>Ressources supplémentaires

Voici certains des explications plus détaillées du fonctionnement en interne :

- [Sélecteurs objective-C](~/ios/internals/objective-c-selectors.md)
- [Inscription](~/ios/internals/registrar.md)
- [API unifiée de Xamarin pour iOS et OS X](~/cross-platform/macios/unified/index.md)
- [Notions de base Theading](~/ios/app-fundamentals/threading.md)
- [Les délégués, des protocoles et des événements](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Sur `newrefcount`](~/ios/internals/newrefcount.md)

