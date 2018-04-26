---
title: Architecture d’iOS
description: Exploration Xamarin.iOS de bas niveau
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 85dc675a9b18b974f21532298e4d3028bdecd0b7
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="ios-architecture"></a>Architecture d’iOS

Xamarin.iOS applications s’exécutent dans l’environnement d’exécution Mono et utilisent la compilation en avance de la durée (AOA) complet pour compiler le code c# pour le langage d’assembly ARM. Il s’exécute côte à côte avec les [Objective-C Runtime](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Les deux environnements d’exécution s’exécuter sur un noyau de type UNIX, en particulier [XNU](https://en.wikipedia.org/wiki/XNU)et d’exposer les diverses API au code utilisateur, ce qui permet aux développeurs d’accéder au système natif ou managé sous-jacent.

Le diagramme suivant montre une vue d’ensemble de cette architecture :

[ ![](architecture-images/ios-arch-small.png "Ce diagramme montre une vue d’ensemble de l’architecture de compilation de manière anticipée de la durée (AOA)")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>Code natif et managé : une explication

Lors du développement de Xamarin les termes du contrat *natif et managé* code sont souvent utilisés. [Le code managé](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) est un code qui a son exécution gérée par le [Common Language Runtime de .NET Framework](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx), ou dans les cas de Xamarin : le Runtime Mono. C’est ce que nous appelons un langage intermédiaire.

Le code natif est le code qui s’exécute en mode natif sur la plateforme spécifique (par exemple, Objective-C ou même du code AOA compilé, sur un processeur ARM). Ce guide explore comment AOA compile votre code managé en code natif et explique comment une application de Xamarin.iOS fonctionne, rendre pleinement parti des e/s d’Apple API via l’utilisation de liaisons, tout en ayant accès à. BCL du NET et un langage sophistiqué tels que c#.


## <a name="aot"></a>AOT

Lorsque vous compilez une application de plateforme de Xamarin, le compilateur c# Mono (ou) (F #) est exécutées et compilera votre code c# et F # en langage MSIL (Microsoft Intermediate). Si vous exécutez un Xamarin.Android, une application Xamarin.Mac ou même d’une application de Xamarin.iOS sur le simulateur, le [.NET Common Language Runtime (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) compile le MSIL à l’aide d’une juste dans le compilateur de temps (JIT). Lors de l’exécution de que cette est compilée en code natif, laquelle peut être exécutée sur l’architecture appropriée pour votre application.

Toutefois, il est une restriction de sécurité sur iOS, définie par Apple, qui n’autorise pas l’exécution du code généré dynamiquement sur un appareil.
Pour vous assurer que vous nous communiquerez ces protocoles de sécurité, Xamarin.iOS utilise à la place un compilateur de manière anticipée de la durée (AOA) pour compiler le code managé. Cela produit un iOS natif binaire, éventuellement optimisé avec LLVM pour les appareils, ce qui peuvent être déployés sur le processeur ARM d’Apple. Un diagramme approximative de la façon dont il s’ajuste est illustré ci-dessous :

[![](architecture-images/aot.png "Un diagramme approximative de la façon dont il s’ajuste")](architecture-images/aot-large.png#lightbox)

À l’aide d’AOA a un nombre de limitations qui sont décrites en détail dans les [Limitations](~/ios/internals/limitations.md) guide. Il fournit également un certain nombre d’améliorations sur JIT par une réduction dans le temps de démarrage et différentes optimisations des performances

Maintenant que nous avons étudié la façon dont le code est compilé à partir de la source en code natif, jetons un œil en coulisses pour voir comment Xamarin.iOS nous permet d’écrire des applications iOS entièrement native

## <a name="selectors"></a>Sélecteurs

Avec Xamarin, nous avons deux écosystèmes distincts, .NET et Apple, que nous devons mettre ensemble semblent simplifiée que possible, pour vous assurer que l’objectif final est une expérience utilisateur fluide. Nous l’avons vu dans la section ci-dessus de la façon dont les deux runtimes communiquer et vous pouvez très bien avoir entendu le terme « liaisons » qui permet d’API à utiliser dans Xamarin iOS natif. Les liaisons sont expliquées en détail dans notre [Objective-C liaison](~/cross-platform/macios/binding/overview.md) documentation, par conséquent, pour l’instant voyons comment iOS fonctionne sous le capot.

Tout d’abord, il doit être une méthode pour exposer les Objective-C pour c#, qui est effectuée via les sélecteurs. Un sélecteur est un message qui est envoyé à un objet ou une classe. Avec Objective-C cette opération est effectuée le [objc_msgSend](~/cross-platform/macios/binding/overview.md) fonctions.
Pour plus d’informations sur l’utilisation des sélecteurs, reportez-vous à la [Objective-C sélecteurs](~/ios/internals/objective-c-selectors.md) guide. Il doit également être un moyen d’exposer le code managé pour Objective-C, qui est plus compliqué du fait que Objective-C ne sait rien sur le code managé. Pour contourner ce problème, nous utilisons *bureaux*. Ils sont expliqués plus en détail dans la section suivante.

## <a name="registrars"></a>Les bureaux d’enregistrement

Comme indiqué ci-dessus, le bureau d’enregistrement est le code qui expose le code managé à objectif-C. Pour cela, créez une liste de chaque classe managée qui dérive NSObject :

- Pour toutes les classes qui ne sont pas encapsulant une classe Objective-C existante, il crée une nouvelle classe Objective-C avec les membres Objective-C mise en miroir de tous les membres managés qui ont une [`Export`] attribut.

- Dans les implémentations pour chaque membre Objective-C, le code est ajouté automatiquement à appeler le membre managé mise en miroir.

Le pseudo-code ci-dessous montre comment procéder :

**Langage c# (Code managé)**

```csharp
 class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
```

**Objectif-c :**

```objectivec
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end

@implementation MyViewController {}

    -(void) myFunc
    {
        /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

Le code managé peut contenir les attributs, `[Register]` et `[Export]`, qui utilise de bureau d’enregistrement de savoir que l’objet doit être exposée à objectif-C.
Le `[Register]` attribut est utilisé pour spécifier le nom de la classe générée Objective-C au cas où le nom par défaut généré n’est pas approprié. Toutes les classes dérivées de NSObject sont inscrits automatiquement auprès d’objectif-C.
Requis `[Export]` attribut contient une chaîne, qui est le sélecteur utilisé dans la classe générée Objective-C.

Il existe deux types de bureaux d’enregistrement utilisé dans Xamarin.iOS – dynamique et statique :


- **Les bureaux d’enregistrement dynamiques** – le bureau d’enregistrement dynamique est l’inscription de tous les types dans votre assembly lors de l’exécution. Pour ce faire à l’aide des fonctions fournies par [API de runtime de Objective-C](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Le bureau d’enregistrement dynamique a par conséquent un démarrage plus lent, mais plus rapide d’heure de création. Il s’agit de la valeur par défaut pour le simulateur iOS. Les fonctions natives (généralement en C), appelées trampolines, sont utilisées comme des implémentations de méthode lorsque vous utilisez les bureaux d’enregistrement dynamique. Ils varient entre différentes architectures.

- **Les bureaux d’enregistrement statiques** – le bureau d’enregistrement statique génère le code Objective-C pendant la génération, qui est ensuite compilée dans une bibliothèque statique et liée dans le fichier exécutable. Cela permet un démarrage plus rapide, mais prend plus de temps au moment de la build. Il est utilisé par défaut pour les versions de l’appareil. Le bureau d’enregistrement statique peut également servir au Simulateur iOS en passant `--registrar:static` comme une `mtouch` d’attribut dans les options de génération de votre projet, comme indiqué ci-dessous :

    [![](architecture-images/image1.png "Définition des arguments supplémentaires mtouch")](architecture-images/image1.png#lightbox)

Pour plus d’informations sur les spécificités du système de l’enregistrement du Type utilisé par Xamarin.iOS iOS, reportez-vous à la [bureau d’enregistrement de Type](~/ios/internals/registrar.md) guide.

## <a name="application-launch"></a>Lancement de l’application

Le point d’entrée de tous les exécutables Xamarin.iOS est fourni par une fonction appelée `xamarin_main`, qui initialise mono.

Selon le type de projet, le texte suivant est effectué :

- Pour iOS régulière et les applications de tvOS, la méthode Main managée, fournie par l’application Xamarin est appelée. La méthode Main, ceci géré appelle ensuite `UIApplication.Main`, qui est le point d’entrée pour l’objectif-C. UIApplication.Main est la liaison pour Objective-C `UIApplicationMain` (méthode).
- Pour les extensions, la fonction native – `NSExtensionMain` ou (`NSExtensionmain` pour les extensions WatchOS) – fournies par Apple bibliothèques est appelée. Étant donné que ces projets sont des bibliothèques de classes et les projets non exécutables, il n’existe aucune méthode Main managé à exécuter.

Toute cette séquence de lancement est compilé dans une bibliothèque statique, qui est ensuite liée à votre fichier exécutable final afin que votre application sache comment obtenir rien.

Notre application a démarré à ce stade, Mono est en cours d’exécution, nous sommes en code managé et nous savons comment appeler du code natif et d’être rappelé. Le suivante que nous devons faire consiste à commencer à ajouter des contrôles et rendre l’application interactive.


## <a name="generator"></a>Générateur

Xamarin.iOS contient les définitions pour chaque API d’iOS unique. Vous pouvez parcourir ces sur le [MaciOS du référentiel github](https://github.com/xamarin/xamarin-macios/tree/master/src). Ces définitions contiennent des interfaces avec des attributs, ainsi que toutes les méthodes nécessaires et propriétés. Par exemple, le code suivant est est utilisé pour définir un UIToolbar dans le UIKit [espace de noms](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327). Notez qu’il s’agit d’une interface avec un nombre de méthodes et propriétés :

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Le générateur, appelé [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) dans Xamarin.iOS, accepte ces fichiers de définition et utilise les outils de .NET pour [les compiler dans un assembly temporaire](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Toutefois, cet assembly temporaire n’est pas utilisable pour appeler le code Objective-C. Le générateur puis lit l’assembly temporaire et génère le code c# qui peut être utilisé lors de l’exécution.
C’est pourquoi, par exemple, si vous ajoutez un attribut aléatoire à votre fichier de définition de .cs, il n’apparaît pas dans le code de ces résultats. Le générateur ne connaît donc `btouch` ne sait pas à rechercher dans l’assembly temporaire pour le résultat.

Une fois que le Xamarin.iOS.dll a été créé, mtouch regrouper tous les composants ensemble.

À un niveau élevé, il y parvient en exécutant les tâches suivantes :

-   Créer une structure de groupe d’application.
-   Copier dans des assemblys managés.
-   Si la liaison est activée, exécutez l’éditeur de liens managé pour optimiser vos assemblys par extraction de pièces inutilisées.
-   Compilation d’AOA.
-   Créer un fichier exécutable natif, ce qui génère une série de bibliothèques statiques (une pour chaque assembly) qui sont liées dans le fichier exécutable natif, afin que l’exécutable natif se compose de toutes les sorties à partir de l’AOA, le code du bureau d’enregistrement (si statique) et le code de lanceur compilateur


Pour plus d’informations sur l’éditeur de liens et son utilisation, reportez-vous à la [l’éditeur de liens](~/ios/deploy-test/linker.md) guide.

## <a name="summary"></a>Récapitulatif

Ce guide étudié AOA compilation de Xamarin.iOS applications et Explorer Xamarin.iOS et sa relation à Objective-C en profondeur.

## <a name="related-links"></a>Liens associés

- [Limitations](~/ios/internals/limitations.md)
- [Liaison Objective-C](~/cross-platform/macios/binding/overview.md)
- [Sélecteurs objective-C](~/ios/internals/objective-c-selectors.md)
- [Bureau d’enregistrement de type](~/ios/internals/registrar.md)
- [Éditeur de liens](~/ios/deploy-test/linker.md)
