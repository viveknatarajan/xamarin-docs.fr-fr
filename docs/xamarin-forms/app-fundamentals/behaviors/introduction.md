---
title: Introduction aux comportements
description: "Comportements vous permettent d’ajouter des fonctionnalités aux contrôles d’interface utilisateur sans devoir sous-classe les. Au lieu de cela, la fonctionnalité est implémentée dans une classe de comportement et liée au contrôle comme s’il faisait partie du contrôle lui-même. Cet article fournit une introduction aux comportements."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 9a92a80bae49a20b276e0d985845fbf08fe92bec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-behaviors"></a>Introduction aux comportements

_Comportements vous permettent d’ajouter des fonctionnalités aux contrôles d’interface utilisateur sans devoir sous-classe les. Au lieu de cela, la fonctionnalité est implémentée dans une classe de comportement et liée au contrôle comme s’il faisait partie du contrôle lui-même. Cet article fournit une introduction aux comportements._

Comportements permettent d’implémenter le code qui serait normalement avoir à écrire en tant que code-behind, car il interagit directement avec l’API du contrôle de sorte qu’il peut être concise liée au contrôle et empaqueté pour une réutilisation entre plusieurs applications. Ils peuvent être utilisés pour fournir une gamme complète des fonctionnalités aux contrôles, tels que :

- Ajout d’un validateur de courrier électronique à un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/).
- Création d’un contrôle d’évaluation à l’aide d’un module de reconnaissance de mouvement tap.
- Contrôle d’une animation.
- Ajout d’un effet à un contrôle.

Comportements également activer des scénarios plus avancés. Dans le contexte de *ordonnant*, les comportements sont une approche très utile pour la connexion d’un contrôle à une commande. En outre, elles peuvent servir pour associer les commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes. Par exemple, ils peuvent servir à appeler une commande en réponse à un événement de déclenchement.

Xamarin.Forms prend en charge deux différents styles de comportements :

- **Comportements de Xamarin.Forms** : les classes qui dérivent de la [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) ou [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) (classe), où `T` est le type de contrôle vers lequel le comportement doit s’appliquer. Pour plus d’informations sur les comportements de Xamarin.Forms, consultez [Xamarin.Forms comportements](~/xamarin-forms/app-fundamentals/behaviors/creating.md) et [réutilisable comportements](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Joint des comportements** – `static` classes avec une ou plusieurs propriétés jointes. Pour plus d’informations sur les comportements attachés, consultez [joint les comportements](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Ce guide se concentre sur les comportements de Xamarin.Forms, car elles sont la meilleure approche à la construction de comportement.



## <a name="related-links"></a>Liens associés

- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Behavior<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
