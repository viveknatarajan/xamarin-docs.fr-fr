---
title: Sélecteur
description: L’affichage du sélecteur est un contrôle pour la sélection d’un élément de texte dans une liste de données.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 7f0050351ca28d7f8afeb82a85e82e51d399824b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847496"
---
# <a name="picker"></a>Sélecteur

_L’affichage du sélecteur est un contrôle pour la sélection d’un élément de texte dans une liste de données._

Le Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) affiche une liste courte d’éléments, à partir de laquelle l’utilisateur peut sélectionner un élément. `Picker` définit des huit propriétés :

- [`Title`](xref:Xamarin.Forms.Picker.Title) de type `string`, qui utilise par défaut `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) de type `IList`, la liste source d’éléments à afficher, qui utilise par défaut `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) de type `int`, l’index de l’élément sélectionné, qui utilise par défaut -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) de type `object`, l’élément sélectionné, qui utilise par défaut `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) de type [ `Color` ](xref:Xamarin.Forms.Color), la couleur utilisée pour afficher le texte qui utilise par défaut [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) de type [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), qui utilise par défaut [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) de type `string`, qui utilise par défaut `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) de type `double`, qui utilise par défaut -1,0.

Toutes les propriétés de huit sont soutenues par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie qu’ils peuvent avoir le format et les propriétés peuvent être des cibles de liaisons de données. Le [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) et [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) propriétés ont un mode de liaison par défaut de [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), ce qui signifie qu’ils peuvent être des cibles de liaisons de données dans une application qui utilise le [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architecture. Pour plus d’informations sur la définition des propriétés de police, consultez [polices](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) n’affiche pas toutes les données lors de son premier affichage. Au lieu de cela, la valeur de sa [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) propriété est affichée comme un espace réservé sur la plateforme iOS et Android utilisent :

[![](images/picker-initial.png "Affichage du sélecteur d’initiale")](images/picker-initial-large.png#lightbox "initiale d’affichage du sélecteur")

Lorsque le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) gains focus, ses données s’affiche et l’utilisateur peut sélectionner un élément :

[![](images/picker-selection.png "Sélecteur de sélection d’un élément")](images/picker-selection-large.png#lightbox "sélecteur de sélection d’un élément")

Le [ `Picker` ](xref:Xamarin.Forms.Picker) se déclenche un [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) événement lorsque l’utilisateur sélectionne un élément. Après sélection, l’élément sélectionné est affiché par le `Picker`:

![](images/picker-after-selection.png "Sélecteur d’après la sélection")

Il existe deux techniques pour remplir un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) avec des données :

- Définition de la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété aux données à afficher. Il s’agit de la technique recommandée, ce qui a été introduite dans Xamarin.Forms 2.3.4. Pour plus d’informations, consultez [définition ItemsSource propriété d’un sélecteur](populating-itemssource.md).
- Ajout de données à afficher pour le [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) collection. Cette technique est le processus d’origine utilisé pour remplir un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) avec des données. Pour plus d’informations, consultez [Ajout de données à la Collection d’éléments d’un sélecteur](populating-items.md).

## <a name="related-links"></a>Liens associés

- [Picker](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
