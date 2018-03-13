---
title: "Sélecteur"
description: "L’affichage du sélecteur est un contrôle pour la sélection d’un élément de texte dans une liste de données."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: fc1aaffe4e31b596d57b5de30c87217ffba3772e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="picker"></a>Sélecteur

_L’affichage du sélecteur est un contrôle pour la sélection d’un élément de texte dans une liste de données._

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) affiche une liste courte d’éléments, à partir de laquelle l’utilisateur peut sélectionner. Toutefois, un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) n’affiche pas toutes les données lors de son premier affichage. Au lieu de cela, la valeur de sa [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) propriété est affichée comme un espace réservé sur la plateforme iOS et Android utilisent :

[![](images/picker-initial.png "Affichage du sélecteur d’initiale")](images/picker-initial-large.png#lightbox "initiale d’affichage du sélecteur")

Lorsque le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) gains focus, ses données s’affiche et l’utilisateur peut sélectionner un élément :

[![](images/picker-selection.png "Sélecteur de sélection d’un élément")](images/picker-selection-large.png#lightbox "sélecteur de sélection d’un élément")

Après sélection, l’élément sélectionné est affiché par le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Sélecteur d’après la sélection")

Il existe deux techniques pour remplir un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) avec des données :

- Définition de la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété aux données à afficher. Il s’agit de la technique recommandée, ce qui a été introduite dans Xamarin.Forms 2.3.4. Pour plus d’informations, consultez [définition ItemsSource propriété d’un sélecteur](populating-itemssource.md).
- Ajout de données à afficher pour le [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) collection. Cette technique est le processus d’origine utilisé pour remplir un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) avec des données. Pour plus d’informations, consultez [Ajout de données à la Collection d’éléments d’un sélecteur](populating-items.md).


## <a name="related-links"></a>Liens associés

- [Picker](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
