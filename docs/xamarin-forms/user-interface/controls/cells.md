---
title: Cellules de Xamarin.Forms
description: "Xamarin.Forms cellules peuvent être ajoutés à ListViews et TableViews."
ms.topic: article
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0f8886546004702adbdbca7d991c67d5700e453e
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinforms-cells"></a>Cellules de Xamarin.Forms

_Xamarin.Forms cellules peuvent être ajoutés à ListViews et TableViews._

A *cellule* est un élément spécialisé utilisé pour les éléments dans une table et décrit la façon dont chaque élément d’une liste doit être restitué. Le [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) dérive de la classe [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), à partir de laquelle [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) dérive également. Une cellule n’est pas lui-même un élément visuel ; Il est à la place d’un modèle pour la création d’un élément visuel. 

`Cell` est utilisé exclusivement avec [ `ListView` ](views.md#listView) et [ `TableView` ](views.md#tableView) contrôles. Pour apprendre à utiliser et personnaliser des cellules, reportez-vous à la [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) et [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) documentation.

## <a name="cells"></a>Cellules

Xamarin.Forms prend en charge les types suivants de la cellule :

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) affiche un ou deux chaînes de texte. Définir le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) propriété et, éventuellement, le [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) propriété ces chaînes de texte.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [Guide](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![Exemple de TextCell](cells-images/TextCell.png "TextCell exemple")](cells-images/TextCell-Large.png#lightbox "TextCell exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| Le [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) affiche les mêmes informations que [ `TextCell` ](#textCell) , mais inclut une image bitmap que vous définissez avec la [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) propriété.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [Guide](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![Exemple de ImageCell](cells-images/ImageCell.png "ImageCell exemple")](cells-images/ImageCell-Large.png#lightbox "ImageCell exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| Le [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) contient du texte est définie avec la [ `Text`'](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) propriété et et marche/arrêt initialement définie avec la valeur booléenne [ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) propriété. Gérer les [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) événement d’être averti lorsque le `On` de propriété est modifiée.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [Guide](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Exemple de SwitchCell](cells-images/SwitchCell.png "SwitchCell exemple")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| Le [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) définit un [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) propriété qui identifie la cellule et une ligne unique de texte modifiable dans le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) propriété. Gérer les [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) événement pour être averti lorsque l’utilisateur a terminé l’entrée de texte.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [Guide](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Exemple de EntryCell](cells-images/EntryCell.png "EntryCell exemple")](cells-images/EntryCell-Large.png#lightbox "EntryCell exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Exemple de Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
