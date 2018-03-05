---
title: Cellules de Xamarin.Forms
description: "Xamarin.Forms cellules peuvent être ajoutés à ListViews et TableViews."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Cellules de Xamarin.Forms

_Xamarin.Forms cellules peuvent être ajoutés à ListViews et TableViews._

<style>.tableimg {max-largeur : aucun ! important ;}</style>

## <a name="cells"></a>Cellules

Une cellule est un élément spécialisé utilisé pour les éléments dans une table et décrit la façon dont chaque élément d’une liste doit être dessiné. Dérive de la cellule [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), à partir de laquelle dérive également VisualElement. Mais la cellule n’est pas un élément visuel, il décrit simplement un modèle pour la création d’un élément visuel. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Fournit des fonctionnalités et la classe de base pour toutes les cellules de Xamarin.Forms. Les cellules sont conçus pour être ajoutés à des éléments [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ou [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) contrôles.

Pour apprendre à utiliser et personnaliser des cellules, reportez-vous à la [ListView](~/xamarin-forms/user-interface/listview/index.md) et [TableView](~/xamarin-forms/user-interface/tableview.md) documentation.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Type</strong>
    </th>
    <th>
      <strong>Description</strong>
    </th>
    <th style="min-width:400px">
      <strong>Screenshot</strong>
    </th>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> avec une étiquette et un champ de saisie de texte de ligne unique.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="Exemple de EntryCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> avec une étiquette et un commutateur d’activation/désactivation.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="Exemple de SwitchCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> avec du texte principal et secondaire.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="Exemple de TextCell" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">texte cellule</a> qui inclut également une image.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="Exemple de ImageCell" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [La galerie de Xamarin.Forms (exemple)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
