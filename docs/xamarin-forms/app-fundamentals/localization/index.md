---
title: Localisation de Xamarin.Forms
description: L’infrastructure de localisation .NET intégrée peut être utilisé pour générer des applications multilingues multiplateforme avec Xamarin.Forms. Texte et images peuvent être localisés, et les applications peuvent prendre en charge un sens de déroulement de droite à gauche.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: 71033e935a2d3a4be88dbcc5d975938771484640
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285571"
---
# <a name="xamarinforms-localization"></a>Localisation de Xamarin.Forms

_L’infrastructure de localisation .NET intégrée peut être utilisé pour générer des applications multilingues multiplateforme avec Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Localisation de chaînes et d’images](text.md)

Le mécanisme intégré pour la localisation utilise des applications .NET [fichiers RESX](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) et les classes dans le `System.Resources` et `System.Globalization` espaces de noms. Les fichiers RESX contenant les chaînes traduites sont incorporés dans l’assembly de Xamarin.Forms, ainsi qu’une classe générée par le compilateur qui fournit l’accès fortement typé pour les traductions. Le texte traduit peut ensuite être récupéré dans le code.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Localisation de droite à gauche](right-to-left.md)

Direction du flux est la direction dans laquelle les éléments d’interface utilisateur sur la page sont analysés par le œil. Localisation de droite à gauche ajoute la prise en charge de la direction du flux de droite à gauche pour les applications Xamarin.Forms.
