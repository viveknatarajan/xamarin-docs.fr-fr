---
title: Pourquoi le concepteur Visual Studio XAML ne fonctionne pas pour les fichiers XAML de Xamarin.Forms ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 1f82f16429ca23a4ba6806f775310dd90126096e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Pourquoi le concepteur Visual Studio XAML ne fonctionne pas pour les fichiers XAML de Xamarin.Forms ?

Xamarin.Forms ne prend actuellement en charge les concepteurs visuels pour les fichiers XAML. Pour cette raison, lorsque vous tentez d’ouvrir un fichier XAML de formulaires dans soit Visual Studio *Concepteur de l’interface utilisateur XAML* ou *Concepteur de l’interface utilisateur XAML avec encodage*, le message d’erreur suivant est levé :

> « Le fichier ne peut pas être ouvert avec l’éditeur sélectionné. Veuillez choisir un autre éditeur. »

Cette limitation est décrite dans le [vue d’ensemble](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) section de la [Xamarin.Forms XAML bases](~/xamarin-forms/xaml/xaml-basics/index.md) guide :

> « Il n’est pas encore un concepteur visuel pour la génération de code XAML dans les applications de Xamarin.Forms, par conséquent, tout code XAML doit être écrit manuellement. »
