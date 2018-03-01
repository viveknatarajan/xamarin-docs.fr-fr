---
title: Styles
description: "À l’aide de styles pour personnaliser l’apparence"
ms.topic: article
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 934948579e5f3fb19c7afe49f4e86a1ef255b77f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="styles"></a>Styles

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Xamarin.Forms applications contiennent souvent plusieurs contrôles qui ont une apparence identiques. Définir l’apparence de chaque contrôle individuel peut être répétitive et sujet aux erreurs. Au lieu de cela, les styles peuvent être créés que personnaliser l’apparence du contrôle par les propriétés de regroupement et les paramètres disponibles sur le type de contrôle.

## <a name="explicit-stylesexplicitmd"></a>[Styles explicites](explicit.md)

Un *explicite* style est celui qui est appliquées de façon sélective aux contrôles en définissant leur [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriétés.

## <a name="implicit-stylesimplicitmd"></a>[Styles implicites](implicit.md)

Un *implicite* style est celui qui est utilisé par tous les contrôles du même [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), sans nécessiter de chaque contrôle pour faire référence au style.

## <a name="global-stylesapplicationmd"></a>[Styles globaux](application.md)

Styles peuvent être rendues disponibles globalement en les ajoutant à l’application [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Cela permet d’éviter la duplication de styles de pages ou des contrôles.

## <a name="style-inheritanceinheritancemd"></a>[Héritage de style](inheritance.md)

Styles peuvent hériter d’autres styles pour réduire la duplication et permettre la réutilisation.

## <a name="dynamic-stylesdynamicmd"></a>[Dynamic Styles](dynamic.md)

Styles de ne pas répondre aux modifications apportées aux propriétés et restent inchangées pendant la durée d’une application. Toutefois, les applications peuvent répondre aux modifications de style dynamiquement au moment de l’exécution à l’aide de ressources dynamiques.

## <a name="device-stylesdevicemd"></a>[Styles de périphérique](device.md)

Xamarin.Forms inclut six *dynamique* styles, appelés *périphérique* styles, dans le [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) classe. Tous les styles de six peuvent être appliqués à [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) uniquement des instances.
