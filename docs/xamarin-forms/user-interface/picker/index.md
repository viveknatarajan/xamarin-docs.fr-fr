---
title: Sélecteur de Xamarin.Forms
description: Le sélecteur de Xamarin.Forms affiche une courte liste d’éléments, à partir de laquelle l’utilisateur peut sélectionner un élément. Cet article explique comment utiliser la classe de sélecteur pour sélectionner un élément de texte dans une liste de données.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: dc39fd9c129fb63fa4a3a73b15aea4204a38cdbd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61231008"
---
# <a name="xamarinforms-picker"></a>Sélecteur de Xamarin.Forms

_La vue de sélecteur est un contrôle pour la sélection d’un élément de texte à partir d’une liste de données._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) affiche une courte liste d’éléments, à partir de laquelle l’utilisateur peut sélectionner un élément. `Picker` définit les propriétés suivantes :

- [`Title`](xref:Xamarin.Forms.Picker.Title) de type `string`, qui utilise par défaut `null`.
- `TitleColor` de type [ `Color` ](xref:Xamarin.Forms.Color), la couleur utilisée pour afficher la `Title` texte.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) de type `IList`, la liste de source d’éléments à afficher, qui utilise par défaut `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) de type `int`, l’index de l’élément sélectionné par défaut, -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) de type `object`, l’élément sélectionné, qui utilise par défaut `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) de type [ `Color` ](xref:Xamarin.Forms.Color), la couleur utilisée pour afficher le texte qui utilise par défaut [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) de type [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), qui utilise par défaut [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) de type `string`, qui utilise par défaut `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) de type `double`, qui est par défaut de -1,0.

Toutes les propriétés sont soutenues par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que des styles peuvent, et les propriétés peuvent être des cibles de liaisons de données. Le [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) et [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) propriétés ont un mode de liaison par défaut de [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), ce qui signifie qu’ils peuvent être des cibles de liaisons de données dans une application qui utilise le [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architecture. Pour plus d’informations sur la définition des propriétés de police, consultez [polices](~/xamarin-forms/user-interface/text/fonts.md).

Un [ `Picker` ](xref:Xamarin.Forms.Picker) n’affiche pas toutes les données lorsqu’il est tout d’abord affiché. Au lieu de cela, la valeur de son [ `Title` ](xref:Xamarin.Forms.Picker.Title) propriété est affichée comme un espace réservé sur les plateformes iOS et Android :

[![](images/picker-initial.png "Affichage du sélecteur d’initiale")](images/picker-initial-large.png#lightbox "initiale d’affichage du sélecteur")

Lorsque le [ `Picker` ](xref:Xamarin.Forms.Picker) gains le focus, ses données s’affiche et l’utilisateur peut sélectionner un élément :

[![](images/picker-selection.png "Sélecteur de sélection d’un élément")](images/picker-selection-large.png#lightbox "sélecteur de sélection d’un élément")

Le [ `Picker` ](xref:Xamarin.Forms.Picker) se déclenche un [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) événement lorsque l’utilisateur sélectionne un élément. Après sélection, l’élément sélectionné est affiché par le `Picker`:

![](images/picker-after-selection.png "Sélecteur d’après la sélection")

Il existe deux techniques pour remplir un [ `Picker` ](xref:Xamarin.Forms.Picker) avec des données :

- Définition de la [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) propriété aux données à afficher. Il s'agit de la technique recommandée. Pour plus d’informations, consultez [définition ItemsSource propriété d’un sélecteur](populating-itemssource.md).
- Ajout de données à afficher pour le [ `Items` ](xref:Xamarin.Forms.Picker.Items) collection. Cette technique a été le processus d’origine pour remplir un [ `Picker` ](xref:Xamarin.Forms.Picker) avec des données. Pour plus d’informations, consultez [Ajout de données à la Collection d’éléments d’un sélecteur](populating-items.md).

## <a name="related-links"></a>Liens connexes

- [Picker](xref:Xamarin.Forms.Picker)
