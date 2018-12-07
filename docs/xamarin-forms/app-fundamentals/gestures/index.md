---
title: Mouvements Xamarin.Forms
description: Ce guide explique comment les modules de reconnaissance de mouvement Xamarin.Forms peuvent être utilisés pour détecter l’interaction utilisateur avec des vues dans une application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
ms.openlocfilehash: 33968fb935e8b69736ac338bfa0479e4f278e64a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106180"
---
# <a name="xamarinforms-gestures"></a>Mouvements Xamarin.Forms

_Modules de reconnaissance de mouvement Xamarin.Forms pouvant être utilisés pour détecter l’interaction utilisateur avec des vues dans une application Xamarin.Forms._

La classe [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) Xamarin.Forms prend en charge les appuis, les pincements et les mouvements de balayage sur les instances de [`View`](xref:Xamarin.Forms.View).

## <a name="adding-a-tap-gesture-recognizertapmd"></a>[Ajout d’un module de reconnaissance des appuis](tap.md)

Un appui est utilisé pour la détection des appuis et est reconnu avec la classe [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).

## <a name="adding-a-pinch-gesture-recognizerpinchmd"></a>[Ajout d’un module de reconnaissance des pincements](pinch.md)

Un pincement est utilisé pour effectuer un zoom interactif et est reconnu avec la classe [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer).

## <a name="adding-a-pan-gesture-recognizerpanmd"></a>[Ajout d’un module de reconnaissance des mouvements panoramiques](pan.md)

Un mouvement panoramique est utilisé pour détecter le mouvement des doigts sur l’écran et appliquer ces déplacements au contenu. Il est reconnu avec la classe [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer).

## <a name="adding-a-swipe-gesture-recognizerswipemd"></a>[Ajout d’un module de reconnaissance des mouvements de balayage](swipe.md)

Un mouvement de balayage se produit quand un doigt est déplacé verticalement ou horizontalement sur l’écran. Il est souvent utilisé pour lancer l’exploration du contenu. Les mouvements de balayage sont reconnus avec la classe [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer).
