---
title: Pourquoi mon projet Android échoue-t-il avec COMPILETODALVIK une erreur de niveau supérieur inattendue ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250454"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Pourquoi mon projet Android échoue-t-il avec COMPILETODALVIK une erreur de niveau supérieur inattendue ?

Cette erreur peut se produire dans le panneau de l’erreur de Visual Studio pour Mac ou dans la fenêtre de sortie de génération de Visual Studio ; dans les projets Android à l’aide de Xamarin.Forms.Maps.

Il est généralement résolu en augmentant la taille du tas Java pour votre projet Xamarin.Android. Suivez ces étapes pour augmenter la taille du tas :

## <a name="visual-studio"></a>Visual Studio

1. Cliquez sur le projet Android & Ouvrir les options du projet.
2. Accédez à **Options Android -> Avancé**
3. Dans la zone de texte de taille du tas Java entrez 1G.
4. Regénérez le projet.

![Capture d’écran des Options de projet Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Android Options de génération dans Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

1.  Cliquez sur le projet Android & Ouvrir les options du projet.
2.  Accédez à **Générer -> Build Android -> Avancé**
3.  Dans la zone de texte de taille du tas Java entrez 1G.
4.  Regénérez le projet.  

![Capture d’écran de Visual Studio pour Mac Options du projet](maps-compiletodalvik-error-images/xsjavaheap.png "Android Options de génération dans Visual Studio pour Mac")

