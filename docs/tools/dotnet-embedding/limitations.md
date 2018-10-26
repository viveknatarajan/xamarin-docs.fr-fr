---
title: Limitations de l’incorporation de .NET
description: Ce document décrit les limitations de l’incorporation de .NET, l’outil qui vous permet de consommer le code .NET dans d’autres langages de programmation.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 7a162d632c98b4e412fa1b7b0c0c40ac945ff09f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114351"
---
# <a name="net-embedding-limitations"></a>Limitations de l’incorporation de .NET

Ce document explique les limitations de l’incorporation de .NET et, si possible, fournit des solutions de contournement pour eux.

## <a name="general"></a>Général

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Utiliser plusieurs bibliothèques incorporé dans un projet

Il n’est pas possible d’avoir deux runtimes Mono coexistence à l’intérieur de la même application. Cela signifie que vous ne pouvez pas utiliser deux différentes bibliothèques d’incorporation .NET générés à l’intérieur de la même application.

**Solution de contournement :** vous pouvez utiliser le générateur pour créer une bibliothèque unique qui inclut plusieurs assemblys (à partir de différents projets).

### <a name="subclassing"></a>Sous-classement

Incorporation .NET facilite l’intégration du runtime Mono dans les applications en exposant un ensemble d’API prêtes à l’emploi pour la plate-forme et le langage cible.

Toutefois, il ne s’agit pas d’une intégration bidirectionnelle, par exemple, vous ne peut pas surclasser un type managé et attendez le code managé pour effectuer un rappel à l’intérieur de votre code natif, dans la mesure où votre code managé n’a pas connaissance de cette coexistence.

Selon vos besoins, il est possible à des parties de solution de contournement de cette limitation, par exemple

* votre code managé peut p/invoke dans votre code natif. Cela nécessite la personnalisation de votre code managé pour permettre une personnalisation à partir du code natif ;

* utiliser des produits tels que Xamarin.iOS et exposer une bibliothèque managée qui permettre de Qu'objective-C (dans ce cas) à la sous-classe certains gérés NSObject sous-classes.

## <a name="objective-c-generated-code"></a>Code généré objective-C

### <a name="nullability"></a>Possibilité de valeur null

Il n’existe aucune métadonnée dans .NET qui nous indiquent si une référence null est acceptable ou non pour une API. La plupart des API lève `ArgumentNullException` si elles ne peuvent pas faire face à une `null` argument. Cela peut être problématique car gestion Objective-C des exceptions est quelque chose de mieux éviter.

Dans la mesure où nous ne pouvons pas générer d’annotations de possibilité de valeur null précise dans les fichiers d’en-tête et pour réduire les exceptions gérées au nous par défaut pour les arguments non null (`NS_ASSUME_NONNULL_BEGIN`) et ajoutez quelques spécifique, si possible la précision, les annotations de possibilité de valeur null.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Actuellement incorporation .NET ne prend pas en charge bitcode sur iOS, qui est activée pour certains modèles de projet Xcode. Cela aura doit être désactivée pour les infrastructures de lien a été généré avec succès.

![Option de Bitcode](images/ios-bitcode-option.png)
