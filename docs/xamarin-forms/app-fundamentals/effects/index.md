---
title: Effets de Xamarin.Forms
description: Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées sans avoir recours à une implémentation de convertisseur personnalisé.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994456"
---
# <a name="xamarinforms-effects"></a>Effets de Xamarin.Forms

_Interfaces utilisateur de Xamarin.Forms sont rendus à l’aide de contrôles natifs de la plateforme cible, qui permet aux applications Xamarin.Forms de conserver l’apparence appropriée pour chaque plateforme. Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées sans avoir recours à une implémentation de convertisseur personnalisé._

## <a name="introduction-to-effectsintroductionmd"></a>[Introduction aux effets](introduction.md)

Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour les styles de petites modifications. Cet article fournit une introduction aux effets, décrit la limite entre les convertisseurs personnalisés et les effets et décrit la `PlatformEffect` classe.

## <a name="creating-an-effectcreatingmd"></a>[Création d’un effet](creating.md)

Effets de simplifient la personnalisation d’un contrôle. Cet article montre comment créer un effet qui modifie la couleur d’arrière-plan de la [ `Entry` ](xref:Xamarin.Forms.Entry) contrôler quand le contrôle obtient le focus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Passage de paramètres à un effet](passing-parameters/index.md)

Création d’un effet qui est configuré au moyen de paramètres permet à l’effet d’être réutilisé. Ces articles expliquent comment utiliser les propriétés pour transmettre des paramètres à un effet et en modifiant un paramètre lors de l’exécution.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Appel d’événements à partir d’un effet](touch-tracking.md)

Effets peuvent appeler des événements. Cet article explique comment créer un événement qui implémente le suivi des doigts de bas niveau multipoint et signale une application pour les appuis tactile, les déplacements et les versions.
