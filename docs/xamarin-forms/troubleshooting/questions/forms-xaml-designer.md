---
title: Pourquoi le concepteur Visual Studio XAML ne fonctionne pas pour les fichiers XAML de Xamarin.Forms ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 43088beba6c6a86330cac164856be98d88f07fe2
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546149"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Pourquoi le concepteur Visual Studio XAML ne fonctionne pas pour les fichiers XAML de Xamarin.Forms ?

Xamarin.Forms ne prend actuellement en charge les concepteurs visuels pour les fichiers XAML. Pour cette raison, lorsque vous tentez d’ouvrir un fichier XAML de formulaires dans soit Visual Studio *Concepteur de l’interface utilisateur XAML* ou *Concepteur de l’interface utilisateur XAML avec encodage*, le message d’erreur suivant est levé :

> « Le fichier ne peut pas être ouvert avec l’éditeur sélectionné. Veuillez choisir un autre éditeur. »

Cette limitation est décrite dans le [vue d’ensemble](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) section de la [Xamarin.Forms XAML bases](~/xamarin-forms/xaml/xaml-basics/index.md) guide :

> « Il n’est pas encore un concepteur visuel pour la génération de code XAML dans les applications de Xamarin.Forms, par conséquent, tout code XAML doit être écrit manuellement. »

Toutefois, le Générateur d’aperçu de Xamarin.Forms XAML peut être affiché en sélectionnant le **vue > autres fenêtres > Aperçu de Xamarin.Forms** option de menu.
