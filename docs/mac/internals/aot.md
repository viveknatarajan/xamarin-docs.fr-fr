---
title: Xamarin.Mac avant au moins une compilation
description: Ce document décrit avant la compilation de temps dans Xamarin.Mac. Il compare la compilation AOT à la compilation JIT, explique comment activer AOT et examine la compilation AOT hybride.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: e155a394afd68d9970ee32785f6d0aeda6e2d129
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117253"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac avant au moins une compilation

## <a name="overview"></a>Vue d'ensemble

À l’avance de time (AOT), la compilation est une technique d’optimisation performante pour améliorer les performances de démarrage. Toutefois, elle affecte également votre temps de génération, taille de l’application et l’exécution du programme de manières profondes. Pour comprendre les compromis qu'il impose, nous allons approfondir un peu la compilation et l’exécution d’une application.

Le code écrit les langages, managés tels que C# et F#, est compilé en une représentation intermédiaire appelée langage intermédiaire. Ce langage intermédiaire, stocké dans vos assemblys de bibliothèque et un programme, est relativement compact et plus portable entre les architectures de processeur. Langage intermédiaire, cependant, est uniquement un intermédiaire de définir des instructions et à un moment donné IL devra être traduit en code machine spécifique au processeur.

Il existe deux points dans lequel ce traitement peut être effectué :

- **Juste à temps (JIT)** : pendant le démarrage et l’exécution de votre application, le langage intermédiaire est compilé en mémoire en code machine.
- **Anticipée de time (AOT)** : lors de la génération, le langage intermédiaire est compilé et écrit dans les bibliothèques natives et stocké au sein de votre offre groupée d’applications.

Chaque option a un nombre d’avantages et les inconvénients :

- **JIT**
  - **Temps de démarrage** – compilation JIT doit être effectuée au démarrage. Pour la plupart des applications, c’est l’ordre de 100 ms, mais pour les grandes applications cette heure peut être beaucoup plus.
  - **L’exécution** – code en tant que le JIT peut être optimisé pour le processeur spécifique utilisé, légèrement meilleur code peut être généré. Dans la plupart des applications, c’est plus rapidement les points de pourcentage quelques au maximum.
- **AOA**
  - **Temps de démarrage** – le chargement les dylibs précompilés est nettement plus rapide que les assemblys JIT.
  - **Espace disque** – ces les dylibs peut prendre une quantité significative d’espace disque toutefois. Selon les assemblys qui sont AOTed, il peut doubler ou plus la taille de la partie du code de votre application.
  - **Heure de création** – compilation AOT est sensiblement plus lente que JIT et ralentir les builds à l’utiliser. Ce ralentissement peut varier de secondes jusqu'à une minute ou plus, en fonction de la taille et le nombre d’assemblys compilés.
  - **Obscurcissement** : en tant que le langage intermédiaire, ce qui est beaucoup plus facile à rétroconcevoir que du code machine, n’est pas nécessairement obligatoire peuvent être supprimé pour aider à obscurcir le code sensible. Cela nécessite l’option décrit ci-dessous « hybride ».

## <a name="enabling-aot"></a>L’activation AOA

Options de compilation AOT seront ajoutées au volet Build Mac dans une prochaine mise à jour. En attendant, d’activer AOT nécessite en passant un argument de ligne de commande via le champ « arguments mmp supplémentaires » dans la Build Mac. Les options sont les suivantes :


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



## <a name="hybrid-aot"></a>Compilation AOT hybride

Pendant l’exécution d’une application macOS le runtime par défaut, à l’aide de code machine chargés depuis les bibliothèques natives produites par la compilation AOT. Il existe, toutefois, certaines zones du code tel que trampolines, où la compilation JIT peut produire des résultats considérablement plus optimisées. Cela nécessite l’IL d’assemblys managés soit disponible. Sur iOS, les applications sont limitées à partir de toute utilisation de la compilation JIT ; Ces section de code sont AOT compilé également.

L’option hybride indique au compilateur pour les deux compilation de ces section (comme iOS), mais également à supposer que le langage intermédiaire ne sera pas disponible lors de l’exécution. Ce langage intermédiaire peut ensuite être supprimé post-build. Comme indiqué ci-dessus, le runtime est contraints d’utiliser le moins optimisés routines à certains endroits.

## <a name="further-considerations"></a>Autres considérations

Les conséquences négatives d’AOT à l’échelle avec les tailles et le nombre d’assemblys traités. La version complète [framework cible](~/mac/platform/target-framework.md) pour l’exemple contient une bibliothèque de classes Base beaucoup plus important (BCL) que moderne, et donc AOT doit prendre beaucoup plus de temps et produire des lots plus volumineux. Cela est aggravé par incompatibilité du framework cible complet avec des liens, qui supprime le code inutilisé. Envisagez de déplacer votre application moderne et l’activation de l’édition des liens pour de meilleurs résultats.

L’un des avantages supplémentaires d’AOT est fourni avec des interactions améliorées avec débogage natif et les chaînes d’outils de profilage. Dans la mesure où une grande majorité de la base de code sera compilée avance, elle aura des noms de fonction et de symboles qui sont plus faciles à lire à l’intérieur des rapports d’incidents native, profilage et de débogage. Fonctions JIT généré n’ont ces noms pas et ne sont pas souvent apparaissent comme des décalages hexadécimal sans nom qui sont très difficiles à résoudre.
