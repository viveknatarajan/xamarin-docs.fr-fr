---
title: "Limitations de l’incorporation de .NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 110eb4169103f99c1538a1846fd231069863530c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="net-embedding-limitations"></a>Limitations de l’incorporation de .NET


Ce document explique les limitations de l’incorporation de .NET (Embeddinator-4000) et, dès que possible, fournit des solutions de contournement pour eux.

## <a name="general"></a>Général

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Utilisez plusieurs bibliothèques incorporé dans un projet

Il n’est pas possible d’avoir deux runtimes mono coexistence à l’intérieur de la même application. Cela signifie que vous ne pouvez pas utiliser les deux bibliothèques embeddinator 4000-générées par différents à l’intérieur de la même application.

**Solution de contournement :** vous pouvez utiliser le générateur pour créer une bibliothèque unique qui inclut plusieurs assemblys (à partir de différents projets).

### <a name="subclassing"></a>Sous-classement

L’embeddinator facilite l’intégration du runtime mono dans des applications en exposant un ensemble d’API prêt à l’emploi pour la plateforme et la langue cible.

Mais il ne s’agit pas d’une intégration bidirectionnelle, par exemple, vous ne peut pas sous-classer un type managé et attendez le code managé pour rappeler à l’intérieur de votre code natif, étant donné que le code managé n’a pas connaissance de cette coexistence.

Selon vos besoins, il est possible à des parties de solution de contournement de cette limitation, par exemple

* votre code managé peut p/invoke dans votre code natif. Cela nécessite la personnalisation de votre code managé pour permettre une personnalisation à partir du code natif ;

* utiliser des produits tels que Xamarin.iOS et exposer une bibliothèque managée ObjC (dans ce cas) pour autoriser sous-classe certaines sous-classes NSObject managés.


## <a name="objc-generated-code"></a>Code de ObjC généré

### <a name="nullability"></a>Possibilité de valeur null

Il n’y a pas de métadonnées, dans .NET, qui nous dire si une référence null est acceptable ou non pour une API. La plupart des API lèveront `ArgumentNullException` si elles ne peuvent pas faire face à une `null` argument. Cela peut être problématique car la gestion de ObjC des exceptions est quelque chose de mieux éviter.

Étant donné que nous ne pouvons pas générer des annotations de possibilité de valeur null précise dans les fichiers d’en-tête et souhaitent limiter les exceptions managées nous par défaut pour les arguments non null (`NS_ASSUME_NONNULL_BEGIN`) et ajoutez quelques spécifique, si possible la précision, les annotations de possibilité de valeur null.
