---
title: .NET 2.0 prise en charge standard dans Xamarin.Forms
description: Cet article explique comment convertir une application Xamarin.Forms Standard de .NET 2.0. .NET standard est une spécification de l’API .NET qui sont destinés à être disponible sur toutes les implémentations de .NET.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: dc90155c79d1d2850281744c4c9aac70cbd7ecc3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242353"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>.NET 2.0 prise en charge standard dans Xamarin.Forms

_Cet article explique comment convertir une application Xamarin.Forms Standard de .NET 2.0._

.NET standard est une spécification de l’API .NET qui sont destinés à être disponible sur toutes les implémentations de .NET. Cela rend plus facile de partager du code entre les applications de bureau, des applications mobiles et des jeux et services de cloud en plaçant des API identiques dans les différentes plateformes. Pour plus d’informations sur les plateformes prises en charge par .NET Standard, consultez [prise en charge de la mise en œuvre .NET](/dotnet/standard/net-standard#net-implementation-support/).

Les bibliothèques .NET standard sont le remplacement pour les bibliothèques de classe Portable (PCL). Toutefois, une bibliothèque qui cible .NET Standard est toujours une bibliothèque de classes portables et est appelée une bibliothèque PCL basée sur .NET Standard. Certains profils PCL sont mappées aux versions .NET Standard, et pour les profils qui ont un mappage, les types de deux bibliothèques pourront se référencent mutuellement. Pour plus d’informations, consultez [compatibilité de la bibliothèque PCL](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 permet aux applications Xamarin.Forms cible .NET Standard 2.0 en remplaçant la bibliothèque PCL avec une bibliothèque Standard de .NET 2.0. Ceci peut être réalisé comme suit :

- Vérifiez [.NET Core 2.0](https://www.microsoft.com/net/download/core) est installé.
- Mettre à jour de la solution Xamarin.Forms pour utiliser Xamarin.Forms 2.4 ou supérieur.
- Ajouter une bibliothèque .NET Standard pour la solution, qui cible .NET Standard 2.0.
- Supprimer la classe qui est ajoutée à la bibliothèque .NET Standard.
- Ajouter le package de NuGet Xamarin.Forms 2,4 (ou version ultérieure) à la bibliothèque .NET Standard.
- Dans les projets de plateforme, ajoutez une référence à la bibliothèque Standard de .NET et supprimer la référence au projet bibliothèque de classes portables qui contient la logique d’interface utilisateur Xamarin.Forms.
- Copier les fichiers à partir du projet de bibliothèque de classes portables dans la bibliothèque .NET Standard.
- Supprimer le projet de bibliothèque de classes portables qui contient la logique d’interface utilisateur Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Options de partage de code](~/cross-platform/app-fundamentals/code-sharing.md)
