---
title: "Xamarin.Mac avant la compilation à l'"
description: "En vue de considérations et les compromis de compilation de temps (AOA)"
ms.topic: article
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f9ace41380987472b6957c94719e6ae9b6f7995d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac avant la compilation à l'

## <a name="overview"></a>Vue d'ensemble

Avance de temps (AOA) la compilation constitue une technique d’optimisation puissant pour améliorer les performances de démarrage. Toutefois, elle affecte également le moment de la génération, taille de l’application et l’exécution du programme de manières importantes. Pour comprendre les compromis que leur impose, nous allons approfondir un peu la compilation et l’exécution d’une application.

Le code écrit dans des langages managés tels que c# et F # est compilé en une représentation intermédiaire appelée langage intermédiaire. Ce langage intermédiaire, stocké dans vos assemblys de bibliothèque et un programme, est relativement compact et portable entre les architectures de processeur. Langage intermédiaire, toutefois, est uniquement un intermédiaire de définir des instructions et à un moment donné IL devra être traduite en code machine spécifique au processeur.

Il existe deux points dans lequel ce traitement peut être effectué :

- **Juste-à-temps (JIT)** : pendant le démarrage et l’exécution de votre application, IL est compilé en mémoire dans le code machine.
- **Transfert de temps (AOA)** : lors de la génération, le langage intermédiaire est compilé et écrit dans des bibliothèques natives et stocké dans votre offre groupée d’application.

Chaque option présente plusieurs avantages et compromis :

- **JIT**
  - **Temps de démarrage** : compilation JIT doit être effectuée au démarrage. Pour la plupart des applications, il s’agit de l’ordre 100 ms, mais pour les grandes applications cette durée peut être beaucoup plus.
  - **L’exécution de** – code en tant que le JIT peut être optimisée pour le processeur spécifique utilisé, code légèrement meilleur peut être généré. Dans la plupart des applications Voici quelques points de pourcentage plus rapidement au maximum.
- **AOT**
  - **Temps de démarrage** – le chargement des dylibs précompilées est nettement plus rapide que les assemblys JIT.
  - **Espace disque** – ces dylibs peut prendre une quantité importante d’espace disque toutefois. Selon les assemblys qui sont AOTed, il peut doubler ou plus la taille de la partie du code de votre application.
  - **Heure de création** – la compilation AOA est sensiblement plus lente que JIT et ralentit les builds à l’utiliser. Ce ralentissement peut varier de quelques secondes à une minute ou plus, selon la taille et le nombre des assemblys compilés.
  - **Obscurcissement** -en tant que le langage intermédiaire, ce qui est beaucoup plus facile rétroconcevoir que du code machine, n’est pas forcément nécessaire qu’il peut être supprimée pour aider à obscurcir code sensibles. Cela nécessite l’option décrit ci-dessous « hybride ».

## <a name="enabling-aot"></a>L’activation d’AOA

Options d’AOA seront ajoutées au volet Mac générer dans une prochaine mise à jour. Jusqu'à cette date, l’activation AOA requiert en passant un argument de ligne de commande via le champ « arguments supplémentaires mmp » dans la génération de Mac. Les options sont les suivantes :


      --aot[=VALUE]          Specify assemblies that should be AOT compiled
                               - none - No AOT (default)
                               - all - Every assembly in MonoBundle
                               - core - Xamarin.Mac, System, mscorlib
                               - sdk - Xamarin.Mac.dll and BCL assemblies
                               - |hybrid after option enables hybrid AOT which
                               allows IL stripping but is slower (only valid
                               for 'all')
                                - Individual files can be included for AOT via +
                               FileName.dll and excluded via -FileName.dll

                               Examples:
                                 --aot:all,-MyAssembly.dll
                                 --aot:core,+MyOtherAssembly.dll,-mscorlib.dll



## <a name="hybrid-aot"></a>Hybride AOA

Lors de l’exécution d’une application macOS le runtime par défaut, à l’aide de code machine est chargé à partir des bibliothèques natives produits par la compilation AOA. Il existe, toutefois, certaines parties du code tel que trampolines, où la compilation JIT peut produire des résultats considérablement plus optimisées. Cela requiert le langage intermédiaire d’assemblys managés soit disponible. Sur iOS, les applications sont limitées à partir de toute utilisation de la compilation JIT ; Ces section de code sont AOA également.

L’option hybride indique au compilateur les deux compilation ces section (par exemple, iOS), mais également à supposer que le langage intermédiaire ne sera pas disponible lors de l’exécution. Ce langage intermédiaire peut ensuite être éliminé valider la build. Comme indiqué ci-dessus, le runtime devra utiliser moins les routines optimisés dans certains emplacements.

## <a name="further-considerations"></a>Autres considérations

Les conséquences négatives d’échelle AOA avec les tailles et le nombre d’assemblys traités. La version complète [framework cible](~/mac/platform/target-framework.md) pour exemple contient une bibliothèque de classes Base beaucoup plus important (BCL) que moderne, et ainsi AOA prend beaucoup plus de temps et produire des lots supérieure. Cela est aggravé par l’incompatibilité de l’infrastructure cible complète avec liaison des données, ce qui élimine code non utilisé. Envisagez de déplacer votre application moderne et permettre la liaison pour de meilleurs résultats.

Un avantage supplémentaire d’AOA est fourni avec des interactions améliorées avec le débogage natif et le profilage toolchains. Dans la mesure où une grande majorité de la base de code est compilée à l’avance, il aura des noms de fonction et des symboles qui sont plus faciles à lire à l’intérieur des rapports d’incidents natif, profilage et de débogage. Les fonctions JIT généré n’ont ces noms et souvent apparaissent en tant qu’offsets hexadécimales sans nom qui sont très difficiles à résoudre.
