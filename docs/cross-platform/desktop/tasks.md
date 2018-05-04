---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Comparaison de tâches courantes
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 84b3edc1a8cbc9ad642d94b457321786fae5fe70
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="common-tasks-comparison"></a>Comparaison de tâches courantes

| Tâche | WPF | Xamarin.Forms |
|--- |--- |--- |
|Afficher un message sur l’écran avec des boutons|`MessageBox`|`Page.DisplayAlert`|
|Créer une minuterie|Classe `DispatcherTimer`|`Device.StartTimer` méthode statique|
|Obtenir la taille de police par défaut|`SystemFonts` classe statique|`Device.GetNamedSize` méthode statique|
|Ouvrez une URL/URI|`Process.Start`|`Device.OpenUri`|
|Afficher la feuille de l’action (liste de boutons)|N/A|`Page.DisplayActionSheet`|
