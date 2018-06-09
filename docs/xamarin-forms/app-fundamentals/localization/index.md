---
title: Localisation de Xamarin.Forms
description: L’infrastructure de localisation intégré .NET permet de créer des applications multilingues inter-plateformes avec Xamarin.Forms. Texte et des images peuvent être localisées et les applications peuvent prendre en charge un sens de flux de droite à gauche.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 78731924324a1ddd34c0d197070699e2998c1513
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240591"
---
# <a name="xamarinforms-localization"></a>Localisation de Xamarin.Forms

_L’infrastructure de localisation intégré .NET permet de créer des applications multilingues inter-plateformes avec Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Localisation de chaînes et d’images](text.md)

Le mécanisme intégré pour la localisation utilise des applications .NET [fichiers RESX](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) et les classes dans le `System.Resources` et `System.Globalization` espaces de noms. Fichiers RESX contenant les chaînes traduites sont incorporés dans l’assembly Xamarin.Forms, ainsi que d’une classe générée par le compilateur qui fournit un accès fortement typé pour les traductions. Le texte traduit peut ensuite être récupéré dans le code.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Localisation de droite à gauche](right-to-left.md)

Sens du flux est la direction dans laquelle les éléments d’interface utilisateur sur la page sont analysés par le œil. Localisation de droite à gauche ajoute la prise en charge de la direction du flux de droite à gauche aux applications de Xamarin.Forms.
