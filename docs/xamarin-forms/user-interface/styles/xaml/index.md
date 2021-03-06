---
title: Styler des applications Xamarin.Forms avec des styles XAML
description: Ce guide explique comment personnaliser l’apparence d’une application Xamarin.Forms à l’aide de styles XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: adafcb85f4e3d3bd93f7b2afe69c0ca0bb37f3d0
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926610"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Styler des applications Xamarin.Forms avec des styles XAML

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Les applications Xamarin.Forms contiennent souvent plusieurs contrôles qui ont une apparence identique. Définition de l’apparence de chaque contrôle individuel peut être répétitive et sujet aux erreurs. Au lieu de cela, vous peuvent créer les styles qui personnaliser l’apparence du contrôle en regroupant et en définissant des propriétés disponibles sur le type de contrôle.

## <a name="explicit-stylesexplicitmd"></a>[Styles explicites](explicit.md)

Un *explicite* style est celui qui est appliquées de façon sélective aux contrôles en définissant leur [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) propriétés.

## <a name="implicit-stylesimplicitmd"></a>[Styles implicites](implicit.md)

Un *implicite* style est celui qui est utilisé par tous les contrôles du même [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), sans nécessiter de chaque contrôle pour faire référence au style.

## <a name="global-stylesapplicationmd"></a>[Styles globaux](application.md)

Styles peuvent être rendues disponibles dans le monde entier en les ajoutant à l’application [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Cela permet d’éviter la duplication des styles de pages ou des contrôles.

## <a name="style-inheritanceinheritancemd"></a>[Héritage de style](inheritance.md)

Styles peuvent hériter d’autres styles pour réduire la duplication et permettre la réutilisation.

## <a name="dynamic-stylesdynamicmd"></a>[Styles dynamiques](dynamic.md)

Styles de ne pas répondre aux modifications apportées aux propriétés et restent inchangées pendant la durée d’une application. Toutefois, les applications peuvent répondre aux modifications de style dynamiquement lors de l’exécution à l’aide de ressources dynamiques.

## <a name="device-stylesdevicemd"></a>[Styles d’appareils](device.md)

Xamarin.Forms inclut six *dynamique* styles, connu sous le nom *appareil* définit le style, dans le [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) classe. Tous les six styles peuvent être appliquées à [ `Label` ](xref:Xamarin.Forms.Label) instances uniquement.

## <a name="style-classesstyle-classmd"></a>[Classes de style](style-class.md)

Classes de style de Xamarin.Forms permettent plusieurs styles à appliquer à un contrôle, sans avoir recours à l’héritage de style.
