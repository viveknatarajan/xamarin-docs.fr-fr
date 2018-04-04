---
title: Advanced concrète (manuel)
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 96a8f77124fcd2a011e499e1088650ff7664fa71
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-manual-real-world-example"></a>Advanced concrète (manuel)


**Cet exemple utilise le [bibliothèque POP à partir de Facebook](https://github.com/facebook/pop).**


Cette section décrit une approche plus avancée pour la liaison, où nous utiliserons d’Apple `xcodebuild` outil pour tout d’abord créer le projet POP, puis puis déduire manuellement une entrée pour l’objectif Sharpie. Il s’agit essentiellement ce que fait objectif Sharpie sous le capot de la section précédente.

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Étant donné que la bibliothèque de POP dispose d’un projet Xcode (`pop.xcodeproj`), nous pouvons utiliser simplement `xcodebuild` pour générer des POP. Ce processus à son tour peut générer des fichiers d’en-tête qui objectif Sharpie peut devoir analyser. C’est pourquoi la génération avant que la liaison est importante. Lors de la génération `xcodebuild` Vérifiez vous passez le même identificateur de kit de développement logiciel et la même architecture que vous souhaitez passer à l’objectif Sharpie (et n’oubliez pas, objectif Sharpie 3.0 généralement cela pour vous !) :

```csharp
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

Il y aura un grand nombre de sortie de la génération d’informations dans la console dans le cadre de `xcodebuild`. Comme indiqué ci-dessus, nous pouvons voir que la cible « CpHeader » a été exécutée dans lequel les fichiers d’en-tête ont été copiés vers un répertoire de sortie. Cela est souvent le cas et facilite la liaison : dans le cadre de la génération de la bibliothèque native, les fichiers d’en-tête sont souvent copiés dans un emplacement « public » consommable qui peut rendre l’analyse plus facile pour la liaison. Dans ce cas, nous savons que les fichiers d’en-tête de POP sont dans le `build/Headers` active.

Nous sommes maintenant prêts à lier POP. Nous savons que nous voulons générer pour le Kit de développement logiciel `iphoneos8.1` avec la `arm64` architecture, et que les fichiers d’en-tête que nous intéressent sont dans `build/Headers` sous l’extraction de git POP. Si vous examinez le `build/Headers` répertoire, nous allons voir un nombre de fichiers d’en-tête :

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Si nous examinons `POP.h`, nous constatons qu’il est le fichier d’en-tête de niveau supérieur principale de la bibliothèque qui `#import`s autres fichiers. Pour cette raison, nous avons besoin uniquement passer `POP.h` pour objectif Sharpie, et clang s’occupe du reste en arrière-plan :

```csharp
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Vous remarquerez que nous avons passé un `-scope build/Headers` argument Sharpie d’objectif. Étant donné que les bibliothèques C et Objective-C doivent `#import` ou `#include` autres fichiers d’en-tête qui sont des détails d’implémentation de la bibliothèque et pas les API que vous souhaitez lier, le `-scope` argument indique Sharpie objectif pour ignorer toute API qui n’est pas défini dans un fichier quelque part dans le `-scope` active.

Vous trouverez le `-scope` argument est souvent facultatif pour les bibliothèques implémentés correctement, toutefois, il n’y a aucun risque en fournissant explicitement un.

En outre, nous avons spécifié `-c -Ibuild/headers`. Tout d’abord, le `-c` argument indique Sharpie objectif pour arrêter l’interprétation des arguments de ligne de commande et de passer des arguments suivants _directement au compilateur clang_. Par conséquent, `-Ibuild/Headers` est un argument du compilateur clang qui fait en sorte que clang à rechercher inclut sous `build/Headers`, c'est-à-dire où les en-têtes de POP live. Sans cet argument, clang ne sait pas où trouver les fichiers qui `POP.h` est `#import`effectue une opération. _Presque toutes les « problèmes » à l’aide d’objectif Sharpie résument pour déterminer ce qu’il faut passer pour clang_.

### <a name="completing-the-binding"></a>Fin de la liaison

Objectif Sharpie a maintenant généré `Binding/ApiDefinitions.cs` et `Binding/StructsAndEnums.cs` fichiers.

Il s’agit base première passe d’objectif Sharpie de la liaison, et dans certains cas, il peut être tout ce que vous devez. Comme indiqué ci-dessus Toutefois, le développeur doit généralement modifier manuellement les fichiers générés après objectif Sharpie à [résoudre les problèmes](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) qui ne pourrait pas être gérées automatiquement par l’outil.

Une fois que les mises à jour sont terminées, ces deux fichiers peuvent désormais être ajoutés à un projet de liaison dans Visual Studio pour Mac ou être passés directement à la `btouch` ou `bmac` outils pour produire la liaison finale.

Pour obtenir une description détaillée du processus de liaison, consultez notre [les instructions de procédure pas à pas complète](~/ios/platform/binding-objective-c/walkthrough.md).

