---
title: Modèles de données de Xamarin.Forms
description: Un modèle de données est utilisée pour spécifier l’apparence des données sur les contrôles pris en charge et généralement est lié aux données à afficher.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994969"
---
# <a name="xamarinforms-data-templates"></a>Modèles de données de Xamarin.Forms

_Un modèle de données est utilisée pour spécifier l’apparence des données sur les contrôles pris en charge et généralement est lié aux données à afficher._

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Modèles de données de Xamarin.Forms permettent de définir la présentation des données sur les contrôles pris en charge. Cet article fournit une introduction aux modèles de données, examiner la raison pour laquelle ils sont nécessaires.

## <a name="creating-a-datatemplatecreatingmd"></a>[Création d’un DataTemplate](creating.md)

Modèles de données peuvent être créés en ligne, dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), ou d’un type personnalisé ou un type de cellule Xamarin.Forms approprié. Un modèle inline doit être utilisé s’il est inutile de réutiliser le modèle de données ailleurs. Vous pouvez également un modèle de données peut être réutilisé en la définissant comme un type personnalisé, ou comme une ressource de niveau de contrôle, au niveau des pages, ou au niveau de l’application.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Création d’un DataTemplateSelector](selector.md)

Un [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) peut être utilisé pour choisir un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) lors de l’exécution selon la valeur d’une propriété liée aux données. Cela permet à plusieurs `DataTemplate` instances à appliquer au même type d’objet, pour personnaliser l’apparence d’objets particuliers. Cet article montre comment créer et consommer un `DataTemplateSelector`.


## <a name="related-links"></a>Liens associés

- [Modèles de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
