---
title: Concepts avancés et éléments internes
description: Ce guide présente les concepts avancés et éléments internes de Xamarin.Forms. Cela inclut actuellement des articles sur les renderers rapides et .NET Standard.
ms.prod: xamarin
ms.assetid: 2273a31c-4022-42ba-befe-0d23ce2ff3b5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
ms.openlocfilehash: 77b32d823e1fd89f6b45dca73e89322e57ddc199
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293519"
---
# <a name="advanced-concepts--internals"></a>Concepts avancés et éléments internes

## <a name="dependency-resolutiondependency-resolutionmd"></a>[Résolution des dépendances](dependency-resolution.md)

Cet article explique comment injecter une méthode de résolution de dépendance dans Xamarin.Forms afin que le conteneur d’injection de dépendance d’une application peut contrôler la création et la durée de vie de convertisseurs personnalisés, des effets, et `DependencyService` implémentations.

## <a name="fast-renderersfast-renderersmd"></a>[Convertisseurs rapides](fast-renderers.md)

Cet article présente des renderers rapides qui réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms sur Android en aplanissant la hiérarchie des contrôles natifs obtenue.

## <a name="net-standardnet-standardmd"></a>[.NET Standard](net-standard.md)

Cet article explique comment convertir une application Xamarin.Forms à utiliser .NET Standard 2.0.
