---
title: Sous le capot
description: Un œil sur le fonctionnement interne de Xamarin.Mac
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 74721e880bb0d3ada3f3940a4074d06f55601c0e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="under-the-hood"></a>Sous le capot

_Un œil sur le fonctionnement interne de Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Avance de compilation de temps (AOA)](aot.md)

Avance de temps (AOA) la compilation constitue une technique d’optimisation puissant pour améliorer les performances de démarrage. Toutefois, elle affecte également le moment de la génération, taille de l’application et l’exécution du programme manières importantes, il est utile de comprendre comment il fonctionne.

## <a name="mac-architecturearchitecturemd"></a>[Architecture de Mac](architecture.md)

Relation de Xamarin.Mac pour Objective-C, y compris les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement des applications et le générateur.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac réduit l’écart entre les environnements managés et exécution de/Cocoa, ce qui permet des classes managées pour appeler les classes Objective-C non managés et être appelée lorsque les événements se produisent. Le travail requis pour effectuer cet « magique » est géré par le bureau d’enregistrement, mais comprendre ce qui se passe « sous le capot » peut parfois être utile.
