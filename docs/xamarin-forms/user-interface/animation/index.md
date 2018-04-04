---
title: Animation
description: Xamarin.Forms inclut sa propre infrastructure de l’animation est simple et facile pour créer des animations simples, tout en étant suffisamment flexibles pour créer des animations complexes.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 7cff122e7ecc24f5ad93bd863ee422981871f857
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="animation"></a>Animation

_Xamarin.Forms inclut sa propre infrastructure de l’animation est simple et facile pour créer des animations simples, tout en étant suffisamment flexibles pour créer des animations complexes._

Les classes d’animation Xamarin.Forms ciblent différentes propriétés d’éléments visuels, avec une animation classique progressivement modification d’une propriété d’une valeur à un autre sur une période de temps. Notez qu’il n’existe aucune interface XAML pour les classes d’animation Xamarin.Forms. Toutefois, les animations peuvent être encapsulées dans [comportements](~/xamarin-forms/app-fundamentals/behaviors/index.md) , puis référencé à partir de XAML.

## <a name="simple-animationssimplemd"></a>[Animations simples](simple.md)

Le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe fournit des méthodes d’extension qui peuvent être utilisés pour construire des animations simples pour faire pivoter, mettre à l’échelle, traduisent et fondu [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) instances. Cet article montre comment créer et de l’annulation d’animations à l’aide de la `ViewExtensions` classe.

## <a name="easing-functionseasingmd"></a>[Fonctions d'accélération](easing.md)

Xamarin.Forms inclut une [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) classe qui vous permet de spécifier une fonction de transfert qui contrôle comment animations accélérer ou ralentir qu’ils s’exécutent. Cet article explique comment utiliser les fonctions d’accélération prédéfinies et comment créer des fonctions d’accélération personnalisées.

## <a name="custom-animationscustommd"></a>[Animations personnalisées](custom.md)

Le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe est le bloc de construction de toutes les animations de Xamarin.Forms, avec les méthodes d’extension dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe de création d’un ou plusieurs `Animation` objets. Cet article montre comment utiliser la `Animation` classe pour créer et annuler des animations, synchroniser plusieurs animations et créer des animations personnalisées animer des propriétés qui ne sont pas animées par les méthodes d’animation existante.

