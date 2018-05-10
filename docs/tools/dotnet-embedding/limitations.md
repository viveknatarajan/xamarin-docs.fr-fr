---
title: Limitations de l’incorporation de .NET
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8ea3e7c6ff2fc28700c46109a814fc5da6958500
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="net-embedding-limitations"></a>Limitations de l’incorporation de .NET

Ce document explique les limitations d’incorporation de .NET et, dès que possible, fournit des solutions de contournement pour eux.

## <a name="general"></a>Général

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Utilisez plusieurs bibliothèques incorporé dans un projet

Il n’est pas possible d’avoir deux runtimes Mono coexistence à l’intérieur de la même application. Cela signifie que vous ne pouvez pas utiliser les deux bibliothèques différentes générées par incorporation de .NET à l’intérieur de la même application.

**Solution de contournement :** vous pouvez utiliser le générateur pour créer une bibliothèque unique qui inclut plusieurs assemblys (à partir de différents projets).

### <a name="subclassing"></a>Sous-classement

Incorporation de .NET facilite la l’intégration du runtime Mono dans des applications en exposant un ensemble d’API prêt à l’emploi pour la plateforme et la langue cible.

Mais il ne s’agit pas d’une intégration bidirectionnelle, par exemple, vous ne peut pas sous-classer un type managé et attendez le code managé pour rappeler à l’intérieur de votre code natif, étant donné que le code managé n’a pas connaissance de cette coexistence.

Selon vos besoins, il est possible à des parties de solution de contournement de cette limitation, par exemple

* votre code managé peut p/invoke dans votre code natif. Cela nécessite la personnalisation de votre code managé pour permettre une personnalisation à partir du code natif ;

* utiliser des produits tels que Xamarin.iOS et exposer une bibliothèque managée qui permettre Qu'objective-C (dans ce cas) à la sous-classe certains gérés NSObject sous-classes.

## <a name="objective-c-generated-code"></a>Code généré objective-C

### <a name="nullability"></a>Possibilité de valeur null

Il n’existe pas dans .NET qui nous indiquent si une référence null est acceptable ou non pour une API de métadonnées. La plupart des API lèveront `ArgumentNullException` si elles ne peuvent pas faire face à une `null` argument. Cela peut être problématique car la gestion de Objective-C des exceptions est quelque chose de mieux éviter.

Étant donné que nous ne pouvons pas générer des annotations de possibilité de valeur null précise dans les fichiers d’en-tête et souhaitent limiter les exceptions managées nous par défaut pour les arguments non null (`NS_ASSUME_NONNULL_BEGIN`) et ajoutez quelques spécifique, si possible la précision, les annotations de possibilité de valeur null.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Actuellement l’incorporation de .NET ne prend pas en charge bitcode sur iOS, qui est activée pour certains modèles de projet Xcode. Cela aura doit être désactivée pour les infrastructures de liaison généré avec succès.

![Option de Bitcode](images/ios-bitcode-option.png)
