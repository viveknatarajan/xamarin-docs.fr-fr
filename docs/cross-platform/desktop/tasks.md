---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Comparaison de tâches courantes
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>Comparaison de tâches courantes

| Tâche | WPF | Xamarin.Forms |
|--- |--- |--- |
|Afficher un message sur l’écran avec des boutons|`MessageBox`|`Page.DisplayAlert`|
|Créer une minuterie|Classe `DispatcherTimer`|`Device.StartTimer` méthode statique|
|Obtenir la taille de police par défaut|`SystemFonts` classe statique|`Device.GetNamedSize` méthode statique|
|Ouvrez une URL/URI|`Process.Start`|`Device.OpenUri`|
|Afficher la feuille de l’action (liste de boutons)|N/A|`Page.DisplayActionSheet`|
