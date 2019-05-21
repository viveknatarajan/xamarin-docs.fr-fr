---
title: CollectionView de Xamarin.Forms
description: Le CollectionView est une vue flexible et plus performant pour la présentation des listes de données à l’aide des spécifications de mise en page différente.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 73e68f29c61661019cfc56f8caa26c6a0b1ce4ba
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971011"
---
# <a name="xamarinforms-collectionview"></a>CollectionView de Xamarin.Forms

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Le [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) est une vue flexible et plus performant pour la présentation des listes de données à l’aide des spécifications de mise en page différente.

## <a name="datapopulate-datamd"></a>[Données](populate-data.md)

Un [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) est rempli avec des données en définissant son [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource) propriété à toute collection qui implémente `IEnumerable`. L’apparence de chaque élément dans la liste peut être définie en configurant le [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate) propriété à un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="layoutlayoutmd"></a>[Disposition](layout.md)

Par défaut, un [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) affiche ses éléments dans une liste verticale. Toutefois, grilles et répertorie vertical et horizontal peuvent être spécifiés.

## <a name="selectionselectionmd"></a>[Sélection](selection.md)

Par défaut, [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) sélection est désactivée. Toutefois, la sélection unique et multiples peut être activée.

## <a name="empty-viewsemptyviewmd"></a>[Vues vides](emptyview.md)

Dans [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), une vue vide peut être spécifiée, qui fournit des commentaires à l’utilisateur lorsque aucune donnée n’est disponible pour l’affichage. La vue vide peut être une chaîne, une vue ou plusieurs vues.

## <a name="scrollingscrollingmd"></a>[Défilement](scrolling.md)

Quand un où les balayages utilisateur pour lancer un défilement, la position de fin du défilement peut être contrôlée afin que les éléments sont affichées. En outre, [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) définit deux [ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*) méthodes défilement par programmation des éléments dans l’affichage. Une des surcharges fait défiler l’élément à l’index spécifié dans la vue, tandis que l’autre fait défiler l’élément spécifié dans la vue.
