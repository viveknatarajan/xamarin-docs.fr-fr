---
title: Virgule flottante
ms.topic: article
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 73681a37f15f3dd93c85bafb6bb9d71ab30af85c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="floating-point"></a>Virgule flottante

Xamarin.iOS effectue par défaut 32 bits et 64 bits opérations à virgule flottante à l’aide de la précision 64 bits sur ARM.  

Cette précision plus élevée est proche de ce que les développeurs attendent opérations en virgule flottante en c# sur le bureau, sur mobile, l’impact sur les performances sont importantes.

Il est possible de compiler votre code de point flottant 32 bits pour utiliser des opérations à virgule flottante 32 bits.  Pour ce faire, vous devez utiliser au moins Xamarin.iOS 8.10 et définie dans votre iOS générer panneau d’options sur le « mtouch arguments supplémentaires « entrée de ligne la valeur suivante :

     --aot-options=-O=float32

Cela informe les compilateurs statiques (compilateur statique intégrée à de Mono, ou celui fonctionnant sous LLVM) pour effectuer des opérations à virgule flottante à l’aide de valeurs float 32 bits.
