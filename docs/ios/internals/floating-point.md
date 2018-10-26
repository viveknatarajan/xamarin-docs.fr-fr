---
title: Opérations à virgule flottante dans Xamarin.iOS
description: Ce document décrit comment Xamarin.iOS gère 32 bits et 64 bits de précision opérations à virgule flottante et traite des associés d’impact sur les performances.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: c5ee1b833e309c78c7338298cbe5c8800afb1ba1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116236"
---
# <a name="floating-point-operations-in-xamarinios"></a>Opérations à virgule flottante dans Xamarin.iOS

Xamarin.iOS effectue par défaut 32 bits et 64 bits opérations à virgule flottante à l’aide de la précision 64 bits sur ARM.  

Bien que cette précision plus élevée est plus proche de ce que les développeurs attendent d’opérations à virgule flottante dans C# sur le bureau, sur mobile, l’impact sur les performances peut être significatif.

Il est possible de compiler votre code de point flottant 32 bits pour utiliser des opérations à virgule flottante 32 bits.  Pour ce faire, vous devez utiliser au moins Xamarin.iOS 8.10 et défini dans votre iOS build du panneau d’options la « mtouch arguments supplémentaires « entrée de ligne de la valeur suivante :

     --aot-options=-O=float32

Cela permettra d’informer les compilateurs statiques (compilateur statique intégré de Mono, ou celui optimisés pour LLVM) pour effectuer des opérations à virgule flottante à l’aide de valeurs en virgule flottante 32 bits.
