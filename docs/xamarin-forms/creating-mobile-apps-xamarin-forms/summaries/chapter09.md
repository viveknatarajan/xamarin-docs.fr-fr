---
title: Résumé du chapitre 9. Appels d’API spécifiques à la plateforme
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 9. Appels d’API spécifiques à la plateforme'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8a035da3dec468df291a19849ca89964c6707589
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994755"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Résumé du chapitre 9. Appels d’API spécifiques à la plateforme

Il est parfois nécessaire d’exécuter du code qui varie par plateforme. Ce chapitre explore les techniques.

## <a name="preprocessing-in-the-shared-asset-project"></a>Prétraitement dans le projet actif partagé

Un projet de ressource partagée Xamarin.Forms peut exécuter un code différent pour chaque plateforme en utilisant les directives de préprocesseur c# `#if`, `#elif`, et `endif`. Cela est illustré dans [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Capture d’écran triple de variable mise en forme de paragraphe](images/ch09fg01-small.png "modèle d’appareil et système d’exploitation")](images/ch09fg01-large.png#lightbox "modèle d’appareil et système d’exploitation")

Toutefois, le code résultant peut être horrible et difficile à lire.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Classes parallèles dans le projet de ressources partagées

Une approche plus structurée à l’exécution de code spécifique à la plateforme dans SAP est illustrée dans le [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) exemple. Chacun des projets de plateforme contient une classe portant le même nommée et des méthodes, mais implémentée pour cette plate-forme particulière. SAP ensuite simplement instancie la classe et appelle la méthode.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService et la bibliothèque de classes Portable

Une bibliothèque ne peut pas accéder normalement les classes dans les projets d’application. Cette restriction semble pour empêcher la technique démontrée dans **PlatInfoSap2** d’être utilisé dans une bibliothèque de classes portable. Toutefois, Xamarin.Forms contient une classe nommée [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) qui utilise la réflexion .NET pour accéder à des classes publiques dans le projet d’application à partir de la bibliothèque PCL.

La bibliothèque PCL doit définir un `interface` avec les membres qu’il a besoin pour chaque plateforme. Ensuite, chacune des plateformes contient une implémentation de cette interface. La classe qui implémente l’interface doit être identifiée avec un [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) sur le niveau de l’assembly.

La bibliothèque PCL utilise ensuite le générique [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) méthode de `DependencyService` pour obtenir une instance de la classe de plateforme qui implémente l’interface.

Cela est illustré dans le [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) exemple.

## <a name="platform-specific-sound-generation"></a>Génération de sons spécifiques à la plateforme

Le [ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) exemple ajoute des sonneries à la **MonkeyTap** programme en accédant à des installations de sons dans chaque plateforme.



## <a name="related-links"></a>Liens associés

- [Chapitre 9 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Exemples du chapitre 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Service de dépendance](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
