---
title: .NET standard 2.0 prise en charge dans Xamarin.Forms
description: Cet article explique comment convertir une application Xamarin.Forms à utiliser .NET Standard 2.0. .NET standard est une spécification d’API .NET qui sont destinées à être disponibles sur toutes les implémentations de .NET.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: c3e46592bb8760ff85eaeb5dce119897a97dfe89
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667852"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>.NET standard 2.0 prise en charge dans Xamarin.Forms

_Cet article explique comment convertir une application Xamarin.Forms à utiliser .NET Standard 2.0._

.NET standard est une spécification d’API .NET qui sont destinées à être disponibles sur toutes les implémentations de .NET. Il est plus facile de partager du code entre les applications de bureau, des applications mobiles et des jeux et services cloud, en apportant des API identiques pour les différentes plateformes. Pour plus d’informations sur les plateformes prises en charge par .NET Standard, consultez [prise en charge de la mise en œuvre .NET](/dotnet/standard/net-standard#net-implementation-support).

Les bibliothèques .NET standard sont le remplacement pour les bibliothèques de classes portables (PCL). Toutefois, une bibliothèque qui cible .NET Standard est toujours une bibliothèque de classes portable et est appelée un portable .NET Standard. Certains profils de bibliothèque de classes portable sont mappées aux versions de .NET Standard, et pour les profils qui ont un mappage, les types de deux bibliothèques sera en mesure de se référencent mutuellement. Pour plus d’informations, consultez [compatibilité de la bibliothèque de classes portable](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 permet aux applications Xamarin.Forms cible .NET Standard 2.0 en remplaçant la bibliothèque PCL avec une bibliothèque .NET Standard 2.0. Vous pouvez le faire comme suit :

- Vérifiez [.NET Core 2.0](https://www.microsoft.com/net/download/core) est installé.
- Mettre à jour de la solution Xamarin.Forms pour utiliser Xamarin.Forms 2.4 ou supérieur.
- Ajouter une bibliothèque .NET Standard à la solution, qui cible .NET Standard 2.0.
- Supprimer la classe qui est ajoutée à la bibliothèque .NET Standard.
- Ajouter le package de NuGet Xamarin.Forms 2,4 (ou version ultérieure) à la bibliothèque .NET Standard.
- Dans les projets de plateforme, ajoutez une référence à la bibliothèque .NET Standard et supprimez la référence au projet bibliothèque de classes portable qui contient la logique d’interface utilisateur Xamarin.Forms.
- Copiez les fichiers à partir du projet de bibliothèque de classes portable à la bibliothèque .NET Standard.
- Supprimer le projet de bibliothèque de classes portable qui contient la logique d’interface utilisateur Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Options de partage de code](~/cross-platform/app-fundamentals/code-sharing.md)
