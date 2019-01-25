---
title: Opérations à virgule flottante dans Xamarin.iOS
description: Ce document décrit comment Xamarin.iOS gère 32 bits et 64 bits de précision opérations à virgule flottante et traite des associés d’impact sur les performances.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 28002ddbab80445757be907b5d9f1cc02fdefd9d
ms.sourcegitcommit: ee626f215de02707b7a94ba1d0fa1d75b22ab84f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54879353"
---
# <a name="floating-point-operations-in-xamarinios"></a>Opérations à virgule flottante dans Xamarin.iOS

Xamarin.iOS effectue par défaut 32 bits et 64 bits opérations à virgule flottante à l’aide de la précision 64 bits sur ARM.  

Bien que cette précision plus élevée est plus proche de ce que les développeurs attendent d’opérations à virgule flottante dans C# sur le bureau, sur mobile, l’impact sur les performances peut être significatif.

Il est possible de compiler votre code de point flottant 32 bits pour utiliser des opérations à virgule flottante 32 bits.  Pour ce faire, vous devez utiliser au moins Xamarin.iOS 8.10 et défini dans votre iOS build du panneau d’options la « mtouch arguments supplémentaires « entrée de ligne de la valeur suivante :

     --aot-options=-O=float32

Cela permettra d’informer les compilateurs statiques (compilateur statique intégré de Mono, ou celui optimisés pour LLVM) pour effectuer des opérations à virgule flottante à l’aide de valeurs en virgule flottante 32 bits.
