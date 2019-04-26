---
title: CollectionView de Xamarin.Forms
description: Le CollectionView est une vue flexible et plus performant pour la présentation des listes de données à l’aide des spécifications de mise en page différente.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: b22449659d2d4b7791328d53ed2d2b29d405ffc8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019994"
---
# <a name="xamarinforms-collectionview"></a>CollectionView de Xamarin.Forms

![Preview](~/media/shared/preview.png)

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version d’évaluation et ne dispose pas de certaines de ses fonctionnalités planifiée. En outre, l’API peut changer que l’implémentation est terminée.

`CollectionView` est une vue pour la présentation des listes de données à l’aide de spécifications de mise en page différente. Il vise à fournir la plus flexible et performante devoir [ `ListView` ](xref:Xamarin.Forms.ListView). Bien que le `CollectionView` et `ListView` API sont similaires, il existe des différences notables :

- `CollectionView` a un modèle de disposition flexible qui permet d’être présentés verticalement ou horizontalement, dans une liste ou d’une grille de données.
- `CollectionView` n’a aucun concept de cellules. Au lieu de cela, un modèle de données est utilisé pour définir l’apparence de chaque élément de données dans la liste.
- `CollectionView` utilise automatiquement la virtualisation fournie par les contrôles natifs sous-jacent.
- `CollectionView` réduit la surface d’API de [ `ListView` ](xref:Xamarin.Forms.ListView). De nombreuses propriétés et événements de `ListView` ne sont pas présents dans `CollectionView`.
- `CollectionView` n’inclut pas de séparateurs intégrés.

`CollectionView` est disponible dans les versions préliminaires de Xamarin.Forms 4.0. Toutefois, il est actuellement en phase expérimentale et peut uniquement être utilisé en ajoutant la ligne suivante de code à votre `AppDelegate` classe sur iOS ou à votre `MainActivity` classe sur Android, avant d’appeler `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` est disponible uniquement sur iOS et Android.

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[Remplir un CollectionView de données](populate-data.md)

Un `CollectionView` est rempli avec des données en définissant son `ItemsSource` propriété à toute collection qui implémente `IEnumerable`. L’apparence de chaque élément dans la liste peut être définie en configurant le `ItemTemplate` propriété à un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="specify-collectionview-layoutlayoutmd"></a>[Spécifiez la disposition de CollectionView](layout.md)

Par défaut, un `CollectionView` affiche ses éléments dans une liste verticale. Toutefois, grilles et répertorie vertical et horizontal peuvent être spécifiés.

## <a name="set-collectionview-selection-modeselectionmd"></a>[Définir le mode de sélection CollectionView](selection.md)

Par défaut, `CollectionView` la sélection est désactivée. Toutefois, la sélection unique et multiples peut être activée.

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[Afficher un EmptyView lorsque les données ne sont pas disponibles](emptyview.md)

Dans `CollectionView`, une vue vide peut être spécifiée, qui fournit des commentaires à l’utilisateur lorsque aucune donnée n’est disponible pour l’affichage. La vue vide peut être une chaîne, une vue ou plusieurs vues.

## <a name="scroll-an-item-into-viewscrollingmd"></a>[Faire défiler un élément dans la vue](scrolling.md)

Quand un où les balayages utilisateur pour lancer un défilement, la position de fin du défilement peut être contrôlée afin que les éléments sont affichées. En outre, `CollectionView` définit deux `ScrollTo` méthodes défilement par programmation des éléments dans l’affichage. Une des surcharges fait défiler l’élément à l’index spécifié dans la vue, tandis que l’autre fait défiler l’élément spécifié dans la vue.
