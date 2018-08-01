---
title: Architecture de Xamarin.Mac
description: Ce guide explore Xamarin.Mac et sa relation à Objective-C de bas niveau. Il explique les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement des applications et le générateur.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785554"
---
# <a name="xamarinmac-architecture"></a>Architecture de Xamarin.Mac

_Ce guide explore Xamarin.Mac et sa relation à Objective-C de bas niveau. Il explique les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement des applications et le générateur._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Mac applications s’exécutent dans l’environnement d’exécution Mono et permet de compiler jusqu'à (Microsoft Intermediate Language), qui est ensuite juste-à-temps (JIT) compilées en code natif au moment de l’exécution du compilateur de Xamarin. Il s’exécute côte à côte avec le Runtime Objective-C. Les deux environnements d’exécution s’exécutent sur un noyau de type UNIX, en particulier XNU et exposent des diverses API au code utilisateur, ce qui permet aux développeurs d’accéder au système natif ou managé sous-jacent.

Le diagramme suivant montre une vue d’ensemble de cette architecture :

[![Diagramme affichant une vue d’ensemble de l’architecture](architecture-images/mac-arch.png "diagramme montrant une vue d’ensemble de l’architecture")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Code natif et managé

Lors du développement de Xamarin, les termes du contrat *natif* et *gérés* code sont souvent utilisés. Le code managé est le code qui a son exécution gérée par le .NET Framework Common Language Runtime, ou dans les cas de Xamarin : le Runtime Mono.

Le code natif est le code qui s’exécute en mode natif sur la plateforme spécifique (par exemple, Objective-C ou même du code AOA compilé, sur un processeur ARM). Ce guide explore la façon dont votre code managé est compilé en code natif et explique comment une application de Xamarin.Mac fonctionne, tirant pleinement parti des API de Mac d’Apple via l’utilisation de liaisons, tout en ayant accès à. BCL du NET et un langage sophistiqué tels que c#.

## <a name="requirements"></a>Spécifications

Les éléments suivants sont requis pour développer une application macOS avec Xamarin.Mac :

- Un macOS d’en cours d’exécution sur le Mac Sierra (10.12) ou supérieur.
- La version la plus récente de Xcode (installé à partir de la [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- La dernière version de Xamarin.Mac et de Visual Studio pour Mac

L’exécution d’applications créées Mac avec Xamarin.Mac a la configuration requise suivante :

- Un Mac exécutant Mac OS X 10,7 ou supérieur.

## <a name="compilation"></a>Compilation

Lorsque vous compilez une application de plateforme de Xamarin, le compilateur c# Mono (ou) (F #) est exécutées et compilera votre code c# et F # dans Microsoft Intermediate Language (MSIL ou IL). Xamarin.Mac utilise ensuite un *juste dans le temps (JIT)* compilateur lors de l’exécution pour compiler le code natif, ce qui permet l’exécution sur l’architecture appropriée en fonction des besoins.

Ce comportement diffère de Xamarin.iOS qui utilise la compilation AOA. Lorsque vous utilisez le compilateur AOA, tous les assemblys et toutes les méthodes qu’ils contiennent sont compilées au moment de la génération. Avec JIT, compilation se produit à la demande uniquement pour les méthodes qui sont exécutées.

Avec les applications Xamarin.Mac, Mono est généralement incorporé dans le lot d’applications (et appelés **Mono incorporé**). Lorsque vous utilisez l’API Xamarin.Mac classique, l’application utilise à la place **système Mono**, toutefois, cela est pris en charge dans l’API unifiée. Système Mono fait référence à Mono qui a été installé dans le système d’exploitation. Sur le lancement de l’application, l’application Xamarin.Mac utiliseront.

## <a name="selectors"></a>Sélecteurs

Avec Xamarin, nous avons deux écosystèmes distincts, .NET et Apple, que nous devons mettre ensemble semblent simplifiée que possible, pour vous assurer que l’objectif final est une expérience utilisateur fluide. Nous l’avons vu dans la section ci-dessus de la façon dont les deux runtimes communiquer et vous pouvez très bien avoir entendu le terme « liaisons », ce qui permet des API natives Mac à utiliser dans Xamarin. Les liaisons sont expliquées en détail dans le [documentation de liaison Objective-C](~/mac/platform/binding.md), de sorte que pour l’instant, nous allons étudier comment Xamarin.Mac fonctionne sous le capot.

Tout d’abord, il doit être une méthode pour exposer les Objective-C pour c#, qui est effectuée via les sélecteurs. Un sélecteur est un message qui est envoyé à un objet ou une classe. Avec Objective-C cette opération est effectuée le [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) fonctions. Pour plus d’informations sur l’utilisation des sélecteurs, reportez-vous à iOS [Objective-C sélecteurs](~/ios/internals/objective-c-selectors.md) guide. Il doit également être un moyen d’exposer le code managé pour Objective-C, qui est plus compliqué du fait que Objective-C ne sait rien sur le code managé. Pour contourner ce problème, nous utilisons un [bureau d’enregistrement](~/mac/internals/registrar.md). Cela est expliqué en détail dans la section suivante.

## <a name="registrar"></a>Inscription

Comme indiqué ci-dessus, le bureau d’enregistrement est le code qui expose le code managé à objectif-C. Pour cela, créez une liste de chaque classe managée qui dérive NSObject :

- Pour toutes les classes qui ne sont pas encapsulant une classe Objective-C existante, il crée une nouvelle classe Objective-C avec les membres Objective-C mise en miroir de tous les membres managés qui ont un `[Export]` attribut.
- Dans les implémentations pour chaque membre Objective-C, le code est ajouté automatiquement à appeler le membre managé mise en miroir.

Le pseudo-code ci-dessous montre comment procéder :

**Langage c# (code managé) :**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective-C (code natif) :**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

Le code managé peut contenir les attributs, `[Register]` et `[Export]`, qui utilise de bureau d’enregistrement de savoir que l’objet doit être exposée à objectif-C. L’attribut [Registre] est utilisé pour spécifier le nom de la classe générée Objective-C au cas où le nom par défaut généré n’est pas approprié. Toutes les classes dérivées de NSObject sont inscrits automatiquement auprès d’objectif-C. L’attribut requis [Exporter] contient une chaîne, qui est le sélecteur utilisé dans la classe générée Objective-C.

Il existe deux types de bureaux d’enregistrement utilisé dans Xamarin.Mac – dynamique et statique :

- Les bureaux d’enregistrement dynamiques – il s’agit du bureau d’enregistrement par défaut pour toutes les builds Xamarin.Mac. Le bureau d’enregistrement dynamique effectue l’inscription de tous les types dans votre assembly lors de l’exécution. Pour cela, à l’aide des fonctions fournies par l’API de runtime de Objective-C. Le bureau d’enregistrement dynamique a par conséquent un démarrage plus lent, mais plus rapide d’heure de création. Les fonctions natives (généralement en C), appelées trampolines, sont utilisées comme des implémentations de méthode lorsque vous utilisez les bureaux d’enregistrement dynamique. Ils varient entre différentes architectures.
- Bureaux d’enregistrement statique – le bureau d’enregistrement statique génère code Objective-C pendant la génération, qui est ensuite compilée dans une bibliothèque statique et liée dans le fichier exécutable. Cela permet un démarrage plus rapide, mais prend plus de temps au moment de la build.

## <a name="application-launch"></a>Lancement de l’application

Logique de démarrage Xamarin.Mac varient selon si incorporé ou système Mono est utilisé. Pour afficher le code et les étapes de lancement de l’application Xamarin.Mac, reportez-vous à la [lancement en-tête](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) fichier dans le référentiel public macios de xamarin.

## <a name="generator"></a>Générateur

Xamarin.Mac contient les définitions pour chaque API Mac. Vous pouvez parcourir ces sur le [MaciOS du référentiel github](https://github.com/xamarin/xamarin-macios/tree/master/src). Ces définitions contiennent des interfaces avec des attributs, ainsi que toutes les méthodes nécessaires et propriétés. Par exemple, le code suivant est utilisé pour définir une NSBox dans les [espace de noms AppKit](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Notez qu’il s’agit d’une interface avec un nombre de méthodes et propriétés :

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

        …

        [Export ("borderRect")]
        CGRect BorderRect { get; }

        [Export ("titleRect")]
        CGRect TitleRect { get; }

        [Export ("titleCell")]
        NSObject TitleCell { get; }

        [Export ("sizeToFit")]
        void SizeToFit ();

        [Export ("contentViewMargins")]
        CGSize ContentViewMargins { get; set; }

        [Export ("setFrameFromContentFrame:")]
        void SetFrameFromContentFrame (CGRect contentFrame);

        …

}
```

Le générateur, appelé `bmac` dans Xamarin.Mac, accepte ces fichiers de définition et utilise des outils .NET pour les compiler dans un assembly temporaire. Toutefois, cet assembly temporaire n’est pas utilisable pour appeler le code Objective-C. Le générateur puis lit l’assembly temporaire et génère le code c# qui peut être utilisé lors de l’exécution. C’est pourquoi, par exemple, si vous ajoutez un attribut aléatoire à votre fichier de définition de .cs, il n’apparaît pas dans le code de ces résultats. Le générateur ne connaît pas et par conséquent `bmac` ne sait pas à rechercher dans l’assembly temporaire pour le résultat.

Une fois que le Xamarin.Mac.dll a été créé, l’outil d’empaquetage, `mmp`, sera regrouper tous les composants.

À un niveau élevé, il y parvient en exécutant les tâches suivantes :

- Créer une structure de groupe d’application.
- Copier dans des assemblys managés.
- Si la liaison est activée, exécutez l’éditeur de liens managé pour optimiser vos assemblys en supprimant les composants inutilisés.
- Créer une application de lancement, la liaison dans le code de lanceur, nous avons vu, ainsi que le code registar en mode statique.

Il s’agit alors d’exécuter en tant que partie de l’utilisateur les processus qui compile le code utilisateur dans un assembly de génération de cette référence Xamarin.Mac.dll et s’exécute `mmp` pour rendre un package

Pour plus d’informations sur l’éditeur de liens et son utilisation, reportez-vous à l’e/s [l’éditeur de liens](~/ios/deploy-test/linker.md) guide.

## <a name="summary"></a>Récapitulatif

Ce guide examiné la compilation des Xamarin.Mac applications et Explorer Xamarin.Mac et sa relation avec l’objectif-C.
