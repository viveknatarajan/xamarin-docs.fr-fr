---
title: Effets Xamarin.Forms
description: Les effets permettent de personnaliser les contrôles natifs de chaque plateforme sans avoir à implémenter de convertisseur personnalisé.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994456"
---
# <a name="xamarinforms-effects"></a>Effets Xamarin.Forms

_Les interfaces utilisateur Xamarin.Forms sont affichées à l’aide des contrôles natifs de la plateforme cible, ce qui permet aux applications Xamarin.Forms de conserver l’apparence appropriée pour chaque plateforme. Les effets permettent de personnaliser les contrôles natifs de chaque plateforme sans avoir à implémenter de convertisseur personnalisé._

## <a name="introduction-to-effectsintroductionmd"></a>[Présentation des effets](introduction.md)

Les effets permettent de personnaliser les contrôles natifs de chaque plateforme et sont généralement utilisés pour les petits changements de style. Cet article présente les effets, décrit la limite entre les effets et les convertisseurs personnalisés, et décrit la classe `PlatformEffect`.

## <a name="creating-an-effectcreatingmd"></a>[Création d’un effet](creating.md)

Les effets simplifient la personnalisation d’un contrôle. Cet article montre comment créer un effet qui change la couleur d’arrière-plan du contrôle [`Entry`](xref:Xamarin.Forms.Entry) quand le contrôle obtient le focus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Passage de paramètres à un effet](passing-parameters/index.md)

La configuration d’un effet avec des paramètres permet de le réutiliser. Ces articles décrivent l’utilisation de propriétés pour passer des paramètres à un effet et changer un paramètre au moment de l’exécution.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Appel d’événements à partir d’un effet](touch-tracking.md)

Les effets peuvent appeler des événements. Cet article explique comment créer un événement qui implémente le suivi du doigt multipoint de bas niveau et signale à une application les appuis tactiles, les déplacements et les libérations.
