---
title: Sous le capot dans Xamarin.Mac
description: Ce document liens vers des guides pas à pas qui décrivent le fonctionnement interne de Xamarin.Mac. Discuter des documents liés avant la compilation de temps, l’architecture de Xamarin.Mac et le bureau d’enregistrement Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792487"
---
# <a name="under-the-hood-in-xamarinmac"></a>Sous le capot dans Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Avance de compilation de temps (AOA)](aot.md)

Avance de temps (AOA) la compilation constitue une technique d’optimisation puissant pour améliorer les performances de démarrage. Toutefois, elle affecte également le moment de la génération, taille de l’application et l’exécution du programme manières importantes, il est utile de comprendre comment il fonctionne.

## <a name="mac-architecturearchitecturemd"></a>[Architecture de Mac](architecture.md)

Relation de Xamarin.Mac pour Objective-C, y compris les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement des applications et le générateur.

## <a name="xamarinmac-registrarregistrarmd"></a>[Bureau d’enregistrement Xamarin.Mac](registrar.md)

Xamarin.Mac réduit l’écart entre les environnements managés et exécution de/Cocoa, ce qui permet des classes managées pour appeler les classes Objective-C non managés et être appelée lorsque les événements se produisent. Le travail requis pour effectuer cet « magique » est géré par le bureau d’enregistrement, mais comprendre ce qui se passe « sous le capot » peut parfois être utile.
