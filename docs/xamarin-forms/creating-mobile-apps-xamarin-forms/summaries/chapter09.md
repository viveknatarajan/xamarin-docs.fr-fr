---
title: "Résumé du chapitre 9. Appels d’API spécifiques à la plateforme"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 84650c930445172d27520129123d493253851642
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Résumé du chapitre 9. Appels d’API spécifiques à la plateforme

Il est parfois nécessaire d’exécuter du code qui varie selon la plateforme. Ce chapitre explore les techniques.

## <a name="preprocessing-in-the-shared-asset-project"></a>Dans le projet d’actifs partagés de prétraitement

Un projet d’actifs partagés Xamarin.Forms peuvent exécuter un code différent pour chaque plate-forme en utilisant les directives de préprocesseur c# `#if`, `#elif`, et `endif`. Cela est illustré dans [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Capture d’écran de triple de la variable au format paragraphe](images/ch09fg01-small.png "modèle d’appareil et le système d’exploitation")](images/ch09fg01-large.png#lightbox "modèle d’appareil et le système d’exploitation")

Toutefois, le code résultant peut être laid et difficiles à lire.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Classes parallèles dans le projet d’actifs partagés

Une approche plus structurée à l’exécution de code spécifique à la plateforme dans SAP est illustrée dans le [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) exemple. Les projets de plateforme contient une classe portant le même nommée et des méthodes, mais implémenté pour cette plate-forme. SAP puis simplement instancie la classe et appelle la méthode.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService et la bibliothèque de classes Portable

Une bibliothèque ne peut pas accéder normalement aux classes de projets d’application. Cette restriction semble pour empêcher la technique décrite dans **PlatInfoSap2** d’être utilisé dans une bibliothèque de classes portables. Toutefois, Xamarin.Forms contient une classe nommée [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) qui utilise la réflexion .NET pour accéder aux classes publiques dans le projet d’application à partir de la bibliothèque PCL.

La bibliothèque PCL doit définir un `interface` avec les membres qu’il a besoin pour chaque plateforme. Puis, chacune des plateformes contient une implémentation de cette interface. La classe qui implémente l’interface doit être identifiée avec un [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) sur le niveau de l’assembly.

La bibliothèque PCL utilise ensuite l’objet générique [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) méthode `DependencyService` pour obtenir une instance de la classe de plateforme qui implémente l’interface.

Cela est illustré dans le [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) exemple.

## <a name="platform-specific-sound-generation"></a>Génération de sonore propre à la plate-forme

Le [ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) exemple ajoute des signaux sonores à le **MonkeyTap** programme en accédant à des fonctionnalités de génération de son dans chaque plateforme.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 9 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Exemples de chapitre 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Service de dépendance](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
