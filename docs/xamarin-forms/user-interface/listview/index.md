---
title: Affichage de liste
description: Présenter vos données dans les listes et attrayantes et interactives.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: ddd779fc7eb1a10e74c68504367083ff0efcdfcd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="listview"></a>Affichage de liste

ListView est une vue pour la présentation des listes de données, en particulier longues listes qui requièrent le défilement. Ce guide vous explique comment utiliser ListView :

1. **[Sources de données](data-and-databinding.md)**  &ndash; remplir un ListView avec des données, avec ou sans liaison de données.
2. **[Apparence de la cellule](customizing-cell-appearance.md)**  &ndash; personnaliser l’apparence des cellules intégrés ou créer votre propre cellule personnalisé.
3. **[Liste d’apparence](customizing-list-appearance.md)**  &ndash; personnaliser l’apparence de ListView. Définir des en-têtes et pieds de page, activez les groupes et modifier la hauteur des lignes.
4. **[L’interactivité](interactivity.md)**  &ndash; gérer des clics et les sélections, implémenter tirer pour actualiser et ajouter des actions contextuelles.
5. **[Performances](performance.md)**  &ndash; éviter les problèmes de performances.

## <a name="use-cases"></a>Cas d’usage
Assurez-vous que le ListView est le contrôle approprié à vos besoins. ListView peut être utilisé dans toute situation où vous affichez les listes permettant le défilement de données. ListViews prend en charge la liaison de données et les actions du contexte.

ListView ne doit pas être confondu avec [TableView](~/xamarin-forms/user-interface/tableview.md). Le contrôle TableView constitue une meilleure option chaque fois que vous avez une liste non liées d’options ou de données. Par exemple, l’application de paramètres iOS qui a un jeu d’options principalement prédéfini, est mieux adaptée à utiliser TableView à ListView.

Notez qu’un ListView est préférable également adapté aux données homogènes &ndash; , autrement dit, toutes les données doivent être du même type. Il s’agit car seul un type de cellule peut être utilisé pour chaque ligne dans la liste. TableViews peut prendre en charge plusieurs types de cellules, afin qu’ils soient une meilleure option lorsque vous avez besoin de mélanger les vues.


## <a name="components"></a>Composants
ListView a un nombre de composants disponibles pour tester les fonctionnalités natives de chaque plateforme. Chacun de ces composants est décrit ci-dessous :

- **[En-têtes et pieds de page](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; texte ou la vue pour afficher le début et la fin d’une liste, séparer les données de la liste. En-têtes et pieds de page peuvent être liés à une source de données indépendamment à partir de la source de données de ListView.
- **[Groupes](customizing-list-appearance.md#Grouping)**  &ndash; les données dans un ListView peuvent être regroupées pour faciliter la navigation. Les groupes sont généralement liés aux données :

![](images/grouping-depth.png "ListView avec des données regroupées")

- **[Cellules](customizing-cell-appearance.md)**  &ndash; les données dans un ListView sont présentées dans les cellules. Chaque cellule correspond à une ligne de données. Intégrées cellules sélectionnables, ou vous pouvez définir votre propre cellule personnalisé. Les cellules d’intégrés et personnalisés peuvent être utilisés/définis dans XAML ou du code.
  - **[Intégrés](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; générés dans des cellules, en particulier TextCell et ImageCell, peut être très utile pour les performances, car ils correspondent à des contrôles natifs sur chaque plateforme.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; affiche une chaîne de texte, éventuellement avec texte détaillé. Texte de détail est rendu en tant qu’une deuxième ligne dans une police plus petite avec une couleur accentuée.
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; affiche une image avec du texte. Apparaît sous la forme d’un TextCell avec une image sur la gauche.
  - **[Cellules personnalisés](customizing-cell-appearance.md#customcells)**  &ndash; les cellules personnalisées sont parfaites lorsque vous avez besoin de présenter des données complexes. Par exemple, un affichage personnalisé utilisable pour présenter une liste de chansons, y compris l’album et artiste :

![](images/image-cell-default.png "ListView avec ImageCells")

Pour en savoir plus sur la personnalisation des cellules dans un ListView, consultez [apparence de la cellule de personnalisation de ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Fonctionnalité
ListView prend en charge un nombre de styles d’interaction, notamment :

- **[Tirer pour actualiser](interactivity.md#Pull_to_Refresh)**  &ndash; ListView prend en charge l’actualisation de l’extraction sur chaque plateforme.
- **[Actions du contexte](interactivity.md#Context_Actions)**  &ndash; ListView prend en charge les mesures sur les éléments individuels dans une liste. Par exemple, vous pouvez implémenter le passage à l’action sur iOS ou longuement actions sur Android.
- **[Sélection](interactivity.md#selectiontaps)**  &ndash; vous pouvez écouter les sélections et désélections non contiguës une action lorsque l’utilisateur clique sur une ligne.

![](images/context-default.png "ListView avec des Actions de contexte")

Pour en savoir plus sur les fonctionnalités d’interactivité de ListView, consultez [Actions et l’interactivité de ListView](interactivity.md).


## <a name="related-links"></a>Liens associés

- [Utilisation avec ListView (exemple)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Liaison bidirectionnelle (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Intégré dans les cellules (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Cellules personnalisés (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Regroupement (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Affichage de convertisseur personnalisé (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [Interactivité de ListView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS classeur](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Classeur Android](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
