---
title: Résumé du chapitre 9. Appels d’API spécifiques à la plateforme
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 9. Appels d’API spécifiques à la plateforme'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 89056bac86116161512ad32721c5d877850d75f2
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563223"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Résumé du chapitre 9. Appels d’API spécifiques à la plateforme

> [!NOTE] 
> Notes sur cette page indiquent des zones où Xamarin.Forms est différente de la matière présentée dans le livre.

Il est parfois nécessaire d’exécuter du code qui varie par plateforme. Ce chapitre explore les techniques.

## <a name="preprocessing-in-the-shared-asset-project"></a>Prétraitement dans le projet actif partagé

Un projet de ressource partagée Xamarin.Forms peut exécuter un code différent pour chaque plateforme en utilisant les directives de préprocesseur c# `#if`, `#elif`, et `endif`. Cela est illustré dans [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Capture d’écran triple de variable mise en forme de paragraphe](images/ch09fg01-small.png "modèle d’appareil et système d’exploitation")](images/ch09fg01-large.png#lightbox "modèle d’appareil et système d’exploitation")

Toutefois, le code résultant peut être horrible et difficile à lire.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Classes parallèles dans le projet de ressources partagées

Une approche plus structurée à l’exécution de code spécifique à la plateforme dans SAP est illustrée dans le [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) exemple. Chacun des projets de plateforme contient une classe portant le même nommée et des méthodes, mais implémentée pour cette plate-forme particulière. SAP ensuite simplement instancie la classe et appelle la méthode.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService et la bibliothèque de classes Portable

> [!NOTE] 
> Bibliothèques de classes portables ont été remplacés par des bibliothèques .NET Standard. Exemples de code à partir de l’ouvrage a été converti pour utiliser les bibliothèques .NET standard.

Une bibliothèque ne peut pas accéder normalement les classes dans les projets d’application. Cette restriction semble pour empêcher la technique démontrée dans **PlatInfoSap2** d’être utilisé dans une bibliothèque. Toutefois, Xamarin.Forms contient une classe nommée [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) qui utilise la réflexion .NET pour accéder à des classes publiques dans le projet d’application à partir de la bibliothèque.

La bibliothèque doit définir un `interface` avec les membres qu’il a besoin pour chaque plateforme. Ensuite, chacune des plateformes contient une implémentation de cette interface. La classe qui implémente l’interface doit être identifiée avec un [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) sur le niveau de l’assembly.

La bibliothèque utilise ensuite le générique [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) méthode de `DependencyService` pour obtenir une instance de la classe de plateforme qui implémente l’interface.

Cela est illustré dans le [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) exemple.

## <a name="platform-specific-sound-generation"></a>Génération de sons spécifiques à la plateforme

Le [ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) exemple ajoute des sonneries à la **MonkeyTap** programme en accédant à des installations de sons dans chaque plateforme.

## <a name="related-links"></a>Liens connexes

- [Chapitre 9 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Exemples du chapitre 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Service de dépendance](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
