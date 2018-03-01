---
title: "Pourquoi mon projet Xamarin.Forms.Maps Android n’échoue avec l’erreur de niveau supérieur COMPILETODALVIK inattendue ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 41cb955ba22a83d0ae2a11ac5c5a114b465ca9b2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Pourquoi mon projet Xamarin.Forms.Maps Android n’échoue avec l’erreur de niveau supérieur COMPILETODALVIK inattendue ?

Cette erreur peut se produire dans la zone d’erreur de Visual Studio pour Mac ou dans la fenêtre de sortie de génération de Visual Studio ; dans les projets Android à l’aide de Xamarin.Forms.Maps.

Il est généralement résolu en augmentant la taille du tas Java pour votre projet Xamarin.Android. Suivez ces étapes pour augmenter la taille du tas :

## <a name="visual-studio"></a>Visual Studio

1. Cliquez sur le projet Android & Ouvrir les options de projet.
2. Accédez à **Android Options -> Avancé**
3. Dans la zone de texte de taille du tas Java entrez 1G.
4. Regénérez le projet.

![Capture d’écran des Options de projet Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Android générer des Options dans Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

1.  Cliquez sur le projet Android & Ouvrir les options de projet.
2.  Accédez à **Générer -> génération Android -> Avancé**
3.  Dans la zone de texte de taille du tas Java entrez 1G.
4.  Regénérez le projet.  

![Capture d’écran de Visual Studio pour les Options de projet Mac](maps-compiletodalvik-error-images/xsjavaheap.png "Android générer des Options dans Visual Studio pour Mac")

