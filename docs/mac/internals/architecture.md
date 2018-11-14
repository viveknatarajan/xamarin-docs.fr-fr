---
title: Architecture de Xamarin.Mac
description: Ce guide explore Xamarin.Mac et sa relation à Objective-C à un niveau bas. Il explique les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement de l’application et le générateur.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 04/12/2017
ms.openlocfilehash: 1ea38b527acaa89b9f25690de4e55664a7afd9e8
ms.sourcegitcommit: d09391c315336d36496880ef465a72b8974f2ac7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579828"
---
# <a name="xamarinmac-architecture"></a>Architecture de Xamarin.Mac

_Ce guide explore Xamarin.Mac et sa relation à Objective-C à un niveau bas. Il explique les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement de l’application et le générateur._

## <a name="overview"></a>Vue d'ensemble

Les applications Xamarin.Mac s’exécutent dans l’environnement d’exécution Mono et du compilateur de Xamarin permet de compiler sur le langage intermédiaire (IL), qui est ensuite juste-à-temps (JIT) compilé en code natif au moment de l’exécution. Le logiciel s’exécute côte à côte avec le Runtime Objective-C. Les deux environnements d’exécution s’exécutent sur un noyau de type UNIX, en particulier XNU et exposent les différentes API pour le code utilisateur permettant aux développeurs d’accéder au système natif ou managé sous-jacent.

Le diagramme suivant montre une vue d’ensemble de cette architecture :

[![Diagramme montrant une vue d’ensemble de l’architecture](architecture-images/mac-arch.png "diagramme montrant une vue d’ensemble de l’architecture")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Code natif et managé

Lors du développement de Xamarin, les termes du contrat *natif* et *gérés* code sont souvent utilisés. Le code managé est le code qui a son exécution gérée par le .NET Framework Common Language Runtime, ou dans les cas de Xamarin : le Runtime Mono.

Le code natif est le code qui s’exécute en mode natif sur la plateforme spécifique (par exemple, Objective-C ou même du code compilé de AOT, sur un processeur ARM). Ce guide décrit comment votre code managé est compilé en code natif et explique comment le fonctionnement d’une application Xamarin.Mac, tirant pleinement parti des API de Mac d’Apple via l’utilisation de liaisons, tout en ayant accès à. NET BCL et un langage sophistiqué telles que C#.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour développer une application macOS avec Xamarin.Mac :

- Un Mac exécutant macOS Sierra (10.12) ou supérieur.
- La dernière version de Xcode (installé à partir de la [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- La dernière version de Xamarin.Mac et Visual Studio pour Mac

L’exécution d’applications créées Mac avec Xamarin.Mac a la configuration requise suivante :

- Un Mac exécutant Mac OS X 10.7 ou ultérieur.

## <a name="compilation"></a>Compilation

Lorsque vous compilez une application de plateforme de Xamarin, Mono C# (ou F#) compilateur s’exécutera et compilera votre C# et F# code en langage intermédiaire Microsoft (MSIL ou IL). Xamarin.Mac utilise ensuite un *Just in Time (JIT)* compilateur lors de l’exécution pour compiler le code natif, ce qui permet l’exécution sur l’architecture appropriée en fonction des besoins.

Ce comportement diffère Xamarin.iOS qui utilise la compilation AOT. Lorsque vous utilisez le compilateur AOT, tous les assemblys et toutes les méthodes qu’ils contiennent sont compilées au moment de la génération. Avec JIT, la compilation se produit à la demande uniquement pour les méthodes qui sont exécutées.

Avec les applications Xamarin.Mac, Mono est généralement incorporé dans le bundle d’applications (désignées comme **Mono Embedded**). Lorsque vous utilisez l’API de Xamarin.Mac classique, l’application peut utiliser à la place **système Mono**, toutefois, il est non pris en charge dans l’API unifiée. Mono du système fait référence à Mono qui a été installé dans le système d’exploitation. Lors du lancement de l’application, l’application Xamarin.Mac utiliserez.

## <a name="selectors"></a>Sélecteurs

Grâce à Xamarin, nous avons deux écosystèmes distincts, .NET et Apple, que nous devons mettre ensemble semblent comme rationalisée que possible, pour vous assurer que l’objectif final est une expérience utilisateur. Nous l’avons vu dans la section ci-dessus de la façon dont les deux runtimes communiquent, et vous pouvez très bien avoir entendu du terme « liaisons » qui permet de l’API Mac natives à utiliser dans Xamarin. Les liaisons sont expliquées en détail dans le [documentation de liaison Objective-C](~/mac/platform/binding.md), c’est le cas pour l’instant, examinons le fonctionnement de Xamarin.Mac sous le capot.

Tout d’abord, il doit exister un moyen d’exposer Objective-C à C#, ce qui est fait par le biais de sélecteurs. Un sélecteur est un message qui est envoyé à un objet ou une classe. Avec Objective-C cette opération est effectuée le [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) fonctions. Pour plus d’informations sur l’utilisation de sélecteurs, reportez-vous à iOS [sélecteurs Objective-C](~/ios/internals/objective-c-selectors.md) guide. Il doit également être un moyen d’exposer le code managé et Objective-C, qui est plus compliqué dû au fait que Objective-C ne sait pas quoi que ce soit sur le code managé. Pour contourner ce problème, nous utilisons un [bureau d’enregistrement](~/mac/internals/registrar.md). Cela est expliqué plus en détail dans la section suivante.

## <a name="registrar"></a>Inscription

Comme mentionné ci-dessus, le bureau d’enregistrement est de code que le code managé expose à Objective-C. Il effectue cela en créant une liste de chaque classe managée qui dérive de NSObject :

- Pour toutes les classes qui ne sont pas encapsulant une classe Objective-C existante, il crée une nouvelle classe Objective-C avec les membres Objective-C mise en miroir de tous les membres managés qui ont un `[Export]` attribut.
- Dans les implémentations pour chaque membre Objective-C, le code est ajouté automatiquement à appeler le membre géré mise en miroir.

Le pseudo-code ci-dessous montre un exemple de comment procéder :

**C#(code managé) :**

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

Le code managé peut contenir les attributs, `[Register]` et `[Export]`, que le bureau d’enregistrement utilise que l’objet doit être exposé à Objective-C. L’attribut [Registre] est utilisé pour spécifier le nom de la classe Objective-C générée au cas où le nom généré par défaut ne convient pas. Toutes les classes dérivées du NSObject sont automatiquement enregistrés avec Objective-C. L’attribut requis de [Export] contient une chaîne, qui est le sélecteur utilisé dans la classe Objective-C générée.

Il existe deux types de bureaux d’enregistrement utilisée dans Xamarin.Mac – dynamique et statique :

- Bureaux d’enregistrement dynamique – il s’agit du bureau d’enregistrement par défaut pour toutes les builds de Xamarin.Mac. Le bureau d’enregistrement dynamique effectue l’inscription de tous les types dans votre assembly lors de l’exécution. Pour cela, à l’aide des fonctions fournies par l’API d’exécution de Objective-C. Le bureau d’enregistrement dynamique a par conséquent un démarrage plus lent, mais plus rapide d’heure de création. Fonctions natives (généralement en C), appelées trampolines, sont utilisées comme des implémentations de méthode lorsque vous utilisez le bureau d’enregistrement dynamique. Ils varient entre les différentes architectures.
- Bureaux d’enregistrement statique : le bureau d’enregistrement statique génère code Objective-C pendant la génération, qui est ensuite compilée dans une bibliothèque statique et liée dans le fichier exécutable. Cela permet un démarrage plus rapide, mais prend plus de temps au moment de la build.

## <a name="application-launch"></a>Lancement de l’application

Logique de démarrage de Xamarin.Mac varient selon si incorporé ou système Mono est utilisé. Pour afficher le code et les étapes de lancement de l’application Xamarin.Mac, reportez-vous à la [lancement en-tête](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) fichier dans le référentiel public macios de xamarin.

## <a name="generator"></a>Générateur

Xamarin.Mac contient des définitions pour chaque API Mac. Vous pouvez parcourir ces sur le [MaciOS github référentiel](https://github.com/xamarin/xamarin-macios/tree/master/src). Ces définitions contiennent des interfaces avec des attributs, ainsi que les méthodes nécessaires et propriétés. Par exemple, le code suivant est utilisé pour définir un NSBox dans le [espace de noms AppKit](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Notez qu’il s’agit d’une interface avec un nombre de méthodes et propriétés :

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

Le générateur, appelé `bmac` dans Xamarin.Mac, prend ces fichiers de définition et utilise des outils .NET pour les compiler dans un assembly temporaire. Toutefois, cet assembly temporaire n’est pas utilisable pour appeler le code Objective-C. Le générateur, puis lit l’assembly temporaire et génère C# code qui peut être utilisé lors de l’exécution. C’est pourquoi, par exemple, si vous ajoutez un attribut aléatoire à votre fichier .cs de définition, il n’apparaît pas dans le code généré. Le générateur ne le sait et par conséquent `bmac` ne sait pas à rechercher dans l’assembly temporaire, la sortie.

Une fois que le Xamarin.Mac.dll a été créé, l’outil d’empaquetage, `mmp`, sera regrouper tous les composants.

À un niveau élevé, pour cela, en exécutant les tâches suivantes :

- Créer une structure de bundle d’application.
- Copiez vos assemblys managés.
- Si la liaison est activée, exécutez l’éditeur de liens géré pour optimiser vos assemblys en supprimant les composants inutilisés.
- Créer une application de lancement, la liaison dans le code de lanceur parlé, ainsi que le code d’inscription en mode statique.

Il s’agit, exécuter en tant que partie de l’utilisateur les processus qui compile le code utilisateur dans un assembly de génération de cette référence Xamarin.Mac.dll et exécutions `mmp` pour le rendre un package

Pour plus d’informations sur l’éditeur de liens et son utilisation, reportez-vous à iOS [l’éditeur de liens](~/ios/deploy-test/linker.md) guide.

## <a name="summary"></a>Récapitulatif

Ce guide a examiné de compilation d’applications Xamarin.Mac et exploré Xamarin.Mac et sa relation avec Objective-C.
