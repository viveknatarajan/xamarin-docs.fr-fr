---
title: Sous le capot dans Xamarin.Mac
description: Ce document contient des liens vers des guides différents qui décrivent le fonctionnement interne de Xamarin.Mac. Documents liés abordent avant la compilation à l’architecture de Xamarin.Mac et le bureau d’enregistrement de Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 872f26febf3abbe4d659773d2bf2d27348c64513
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033241"
---
# <a name="under-the-hood-in-xamarinmac"></a>Sous le capot dans Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[À l’avance de compilation time (AOT)](aot.md)

À l’avance de time (AOT), la compilation est une technique d’optimisation performante pour améliorer les performances de démarrage. Toutefois, elle affecte également votre temps de génération, taille de l’application et l’exécution du programme de façons profonde, donc il est utile de comprendre comment il fonctionne.

## <a name="mac-architecturearchitecturemd"></a>[Architecture Mac](architecture.md)

Relation de Xamarin.Mac et Objective-C, y compris les concepts tels que la compilation, sélecteurs, bureaux d’enregistrement, lancement de l’application et le générateur.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac comble le fossé entre le monde géré et runtime de Cocoa, ce qui permet des classes managées appeler les classes non managées de Objective-C et être rappelé lorsqu’événements se produisent. Le travail requis pour effectuer cette « magiques » est géré par le bureau d’enregistrement, mais comprendre ce qui se passe « en coulisse » peut parfois être utile.
