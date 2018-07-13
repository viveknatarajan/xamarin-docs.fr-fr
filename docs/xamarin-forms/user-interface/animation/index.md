---
title: Animation dans Xamarin.Forms
description: Xamarin.Forms inclut sa propre infrastructure d’animation simple pour la création d’animations simples, tout en étant suffisamment flexibles pour créer des animations complexes.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998291"
---
# <a name="animation-in-xamarinforms"></a>Animation dans Xamarin.Forms

_Xamarin.Forms inclut sa propre infrastructure d’animation simple pour la création d’animations simples, tout en étant suffisamment flexibles pour créer des animations complexes._

Les classes d’animation Xamarin.Forms ciblent différentes propriétés d’éléments visuels, avec une animation typique progressivement modification d’une propriété d’une valeur à un autre sur une période de temps. Notez qu’il n’existe aucune interface XAML pour les classes d’animation de Xamarin.Forms. Toutefois, les animations peuvent être encapsulées dans [comportements](~/xamarin-forms/app-fundamentals/behaviors/index.md) , puis référencé à partir de XAML.

## <a name="simple-animationssimplemd"></a>[Animations simples](simple.md)

Le [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) classe fournit des méthodes d’extension qui peuvent être utilisées pour construire des animations simples pour faire pivoter, mettre à l’échelle, traduisent et fondu [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) instances. Cet article montre comment créer et annuler des animations à l’aide de la `ViewExtensions` classe.

## <a name="easing-functionseasingmd"></a>[Fonctions d'accélération](easing.md)

Xamarin.Forms inclut un [ `Easing` ](xref:Xamarin.Forms.Easing) classe qui vous permet de spécifier une fonction de transfert qui contrôle comment animations accélèrent ou ralentir car ils s’exécutent. Cet article montre comment utiliser les fonctions d’accélération prédéfinies et comment créer des fonctions d’accélération personnalisées.

## <a name="custom-animationscustommd"></a>[Animations personnalisées](custom.md)

Le [ `Animation` ](xref:Xamarin.Forms.Animation) classe est le bloc de construction de toutes les animations de Xamarin.Forms, avec les méthodes d’extension dans le [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) classe créant un ou plusieurs `Animation` objets. Cet article montre comment utiliser le `Animation` classe pour créer et annuler des animations, synchroniser plusieurs animations et créer des animations personnalisées qui animer des propriétés qui ne sont pas animées par les méthodes d’animation existantes.
