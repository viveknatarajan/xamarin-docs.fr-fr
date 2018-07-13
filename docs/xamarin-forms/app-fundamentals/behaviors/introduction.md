---
title: Introduction aux comportements
description: Comportements vous permettent d’ajouter des fonctionnalités sans devoir sous-classe les contrôles d’interface utilisateur. Au lieu de cela, la fonctionnalité est implémentée dans une classe de comportement et rattachée au contrôle comme s’il faisait partie du contrôle lui-même. Cet article fournit une introduction aux comportements.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995812"
---
# <a name="introduction-to-behaviors"></a>Introduction aux comportements

_Comportements vous permettent d’ajouter des fonctionnalités sans devoir sous-classe les contrôles d’interface utilisateur. Au lieu de cela, la fonctionnalité est implémentée dans une classe de comportement et rattachée au contrôle comme s’il faisait partie du contrôle lui-même. Cet article fournit une introduction aux comportements._

Comportements permettent d’implémenter le code que vous devriez normalement écrire sous forme de code-behind, car il interagit directement avec l’API du contrôle de sorte qu’il peut être plus concise liée au contrôle et empaquetée pour une réutilisation sur plusieurs applications. Ils peuvent être utilisés pour fournir une gamme complète des fonctionnalités aux contrôles, tels que :

- Ajout d’un validateur de courrier électronique à un [ `Entry` ](xref:Xamarin.Forms.Entry).
- Création d’un contrôle d’évaluation à l’aide d’un module de reconnaissance de mouvement tap.
- Contrôle d’une animation.
- Ajout d’un effet à un contrôle.

Comportements permettent également des scénarios plus avancés. Dans le contexte de *commandes*, les comportements sont une approche utile pour la connexion d’un contrôle à une commande. En outre, ils peuvent servir à associer des commandes à des contrôles qui n’ont pas été conçues pour interagir avec les commandes. Par exemple, ils peuvent servir à appeler une commande en réponse à un événement déclenché.

Xamarin.Forms prend en charge deux styles différents de comportements :

- **Comportements de Xamarin.Forms** – classes qui dérivent de la [ `Behavior` ](xref:Xamarin.Forms.Behavior) ou [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) (classe), où `T` est le type du contrôle auquel le comportement doit s’appliquer. Pour plus d’informations sur les comportements de Xamarin.Forms, consultez [les comportements de Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) et [réutilisable comportements](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Joint des comportements** – `static` classes avec un ou plusieurs des propriétés jointes. Pour plus d’informations sur les comportements joints, consultez [comportements attaché](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Ce guide se concentre sur les comportements de Xamarin.Forms, car elles sont la meilleure approche pour la construction de comportement.



## <a name="related-links"></a>Liens associés

- [Comportement](xref:Xamarin.Forms.Behavior)
- [Comportement&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
