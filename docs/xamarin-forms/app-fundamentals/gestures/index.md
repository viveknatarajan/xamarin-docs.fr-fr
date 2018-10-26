---
title: Mouvements de Xamarin.Forms
description: Ce guide explique comment les modules de reconnaissance de mouvement Xamarin.Forms peuvent être utilisées pour détecter l’interaction utilisateur avec des vues dans une application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
ms.openlocfilehash: 33968fb935e8b69736ac338bfa0479e4f278e64a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106180"
---
# <a name="xamarinforms-gestures"></a>Mouvements de Xamarin.Forms

_Modules de reconnaissance de mouvement peuvent être utilisés pour détecter l’interaction utilisateur avec des vues dans une application Xamarin.Forms._

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) classe prend en charge tap, pincer, panoramique et les mouvements de balayage sur [ `View` ](xref:Xamarin.Forms.View) instances.

## <a name="adding-a-tap-gesture-recognizertapmd"></a>[Ajout d’un module de reconnaissance de mouvement tap](tap.md)

Une action d’appuyer est utilisée pour la détection tap et est reconnue avec le [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) classe.

## <a name="adding-a-pinch-gesture-recognizerpinchmd"></a>[Ajout d’un module de reconnaissance de geste de pincement](pinch.md)

Un mouvement de pincement est utilisé pour effectuer le zoom interactif et est reconnu avec le [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) classe.

## <a name="adding-a-pan-gesture-recognizerpanmd"></a>[Ajout d’un module de reconnaissance de mouvement panoramique](pan.md)

Un mouvement panoramique est utilisé pour détecter le mouvement des doigts sur l’écran et en appliquant ces déplacements au contenu et est reconnu avec le [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) classe.

## <a name="adding-a-swipe-gesture-recognizerswipemd"></a>[Ajout d’un module de reconnaissance de mouvement de balayage](swipe.md)

Un mouvement de balayage se produit lorsqu’un doigt est déplacé sur l’écran dans le sens horizontal ou vertical et est souvent utilisé pour lancer la navigation via le contenu. Les mouvements de balayage sont reconnus avec les [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) classe.
