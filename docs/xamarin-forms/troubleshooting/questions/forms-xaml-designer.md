---
title: Pourquoi le concepteur Visual Studio XAML ne fonctionne-t-il pas pour les fichiers XAML de Xamarin.Forms ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 43088beba6c6a86330cac164856be98d88f07fe2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61247794"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Pourquoi le concepteur Visual Studio XAML ne fonctionne-t-il pas pour les fichiers XAML de Xamarin.Forms ?

Xamarin.Forms ne prend actuellement en charge les concepteurs visuels pour les fichiers XAML. Pour cette raison, lorsque vous tentez d’ouvrir un fichier XAML de formulaires dans soit Visual Studio *Concepteur d’interface utilisateur XAML* ou *Concepteur d’interface utilisateur XAML avec encodage*, le message d’erreur suivant est levé :

> « Le fichier ne peut pas être ouvert avec l’éditeur sélectionné. Veuillez choisir un autre éditeur ».

Cette limitation est décrite dans le [vue d’ensemble](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) section de la [principes de base XAML Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md) guide :

> « Il n’est pas encore un concepteur visuel pour la génération de XAML dans les applications Xamarin.Forms, afin que tous les XAML doit être écrit manuellement. »

Toutefois, le Générateur d’aperçu du XAML de Xamarin.Forms peuvent être affichée en sélectionnant le **vue > Other Windows > Générateur d’aperçu Xamarin.Forms** option de menu.
