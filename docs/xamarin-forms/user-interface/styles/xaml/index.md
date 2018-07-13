---
title: Styler des applications Xamarin.Forms à l’aide de Styles de XAML
description: Ce guide explique comment personnaliser l’apparence d’une application Xamarin.Forms à l’aide de styles XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 5145572b30302e58c36250fff40e8b637fcd221f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995073"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Styler des applications Xamarin.Forms à l’aide de Styles de XAML

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Les applications Xamarin.Forms contiennent souvent plusieurs contrôles qui ont une apparence identique. Définition de l’apparence de chaque contrôle individuel peut être répétitive et sujet aux erreurs. Au lieu de cela, les styles peuvent être créés que personnaliser l’apparence de contrôle par les propriétés de regroupement et les paramètres disponibles sur le type de contrôle.

## <a name="explicit-stylesexplicitmd"></a>[Styles explicites](explicit.md)

Un *explicite* style est celui qui est appliquées de façon sélective aux contrôles en définissant leur [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) propriétés.

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
