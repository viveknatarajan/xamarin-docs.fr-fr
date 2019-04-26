---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Comparaison de tâches courantes
description: Ce document compare comment effectuer différentes tâches courantes sur WPF et Xamarin.Forms. Il examine les boutons, les minuteries, les tailles de police, ouverture d’un URI et d’affichage d’une feuille de l’action.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269593"
---
# <a name="common-tasks-comparison"></a>Comparaison de tâches courantes

| Tâche | WPF | Xamarin.Forms |
|--- |--- |--- |
|Afficher un message sur l’écran avec des boutons|`MessageBox`|`Page.DisplayAlert`|
|Créez un minuteur|Classe `DispatcherTimer`|`Device.StartTimer` méthode statique|
|Obtenir une taille de police par défaut|`SystemFonts` classe statique|`Device.GetNamedSize` méthode statique|
|Ouvrez une URI/URL|`Process.Start`|`Device.OpenUri`|
|Afficher la feuille de l’action (liste de boutons)|N/A|`Page.DisplayActionSheet`|
