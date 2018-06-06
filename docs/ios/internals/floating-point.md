---
title: Opérations en virgule flottante dans Xamarin.iOS
description: Ce document décrit comment Xamarin.iOS gère la précision 32 bits et 64 bits, opérations en virgule flottante et traite des associés d’impact sur les performances.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea5d69b52cbd4c76abb236bd1a272633dde440b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786159"
---
# <a name="floating-point-operations-in-xamarinios"></a>Opérations en virgule flottante dans Xamarin.iOS

Xamarin.iOS effectue par défaut 32 bits et 64 bits opérations à virgule flottante à l’aide de la précision 64 bits sur ARM.  

Cette précision plus élevée est proche de ce que les développeurs attendent opérations en virgule flottante en c# sur le bureau, sur mobile, l’impact sur les performances sont importantes.

Il est possible de compiler votre code de point flottant 32 bits pour utiliser des opérations à virgule flottante 32 bits.  Pour ce faire, vous devez utiliser au moins Xamarin.iOS 8.10 et définie dans votre iOS générer panneau d’options sur le « mtouch arguments supplémentaires « entrée de ligne la valeur suivante :

     --aot-options=-O=float32

Cela informe les compilateurs statiques (compilateur statique intégrée à de Mono, ou celui fonctionnant sous LLVM) pour effectuer des opérations à virgule flottante à l’aide de valeurs float 32 bits.
