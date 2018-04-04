---
title: Effects (Effets)
description: Interfaces utilisateur de Xamarin.Forms sont rendus à l’aide des contrôles natifs de la plateforme cible, qui permet aux applications de Xamarin.Forms conserver l’apparence appropriée pour chaque plateforme. Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées sans avoir recours à une implémentation de convertisseur personnalisé.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 63d7750294321a28dbde833f72fe6b7277ada397
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="effects"></a>Effects (Effets)

_Interfaces utilisateur de Xamarin.Forms sont rendus à l’aide des contrôles natifs de la plateforme cible, qui permet aux applications de Xamarin.Forms conserver l’apparence appropriée pour chaque plateforme. Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées sans avoir recours à une implémentation de convertisseur personnalisé._

## <a name="introduction-to-effectsintroductionmd"></a>[Introduction aux effets](introduction.md)

Autoriser les effets des contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour le style de petites modifications. Cet article fournit une introduction aux effets, décrit la limite entre les convertisseurs personnalisés et les effets et décrit la `PlatformEffect` classe.

## <a name="creating-an-effectcreatingmd"></a>[Création d’un effet](creating.md)

Effets simplifient la personnalisation d’un contrôle. Cet article explique comment créer un effet qui modifie la couleur d’arrière-plan de la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôler quand le contrôle obtient le focus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Passage de paramètres à un effet](passing-parameters/index.md)

Création d’un effet qui est configuré via des paramètres permet l’effet à être réutilisé. Les articles suivants illustrent l’utilisation de propriétés pour transmettre des paramètres à un effet et en modifiant un paramètre lors de l’exécution.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Appel des événements à partir d’un effet](touch-tracking.md)

Effets peuvent appeler des événements. Cet article explique comment créer un événement qui implémente le suivi de bas niveau tactiles doigt et signale une application pour les appuis tactile, se déplace et les versions.