---
title: Concepts avancés et éléments internes
description: Ce guide présente les concepts avancés et éléments internes de Xamarin.Forms. Cela inclut actuellement des articles sur les renderers rapides et .NET Standard.
ms.prod: xamarin
ms.assetid: 2273a31c-4022-42ba-befe-0d23ce2ff3b5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2017
ms.openlocfilehash: 292b0814cba446c97042ba1fe52ad9414ba74760
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203057"
---
# <a name="advanced-concepts--internals"></a>Concepts avancés et éléments internes

## <a name="fast-renderersfast-renderersmd"></a>[Convertisseurs rapides](fast-renderers.md)

Cet article présente des renderers rapides qui réduisent l’inflation et les coûts de rendu d’un contrôle Xamarin.Forms sur Android en aplanissant la hiérarchie des contrôles natifs obtenue.

## <a name="net-standardnet-standardmd"></a>[.NET Standard](net-standard.md)

Cet article explique comment convertir une application Xamarin.Forms à utiliser .NET Standard 2.0.

## <a name="dependency-resolutiondependency-resolutionmd"></a>[Résolution des dépendances](dependency-resolution.md)

Cet article explique comment injecter une méthode de résolution de dépendance dans Xamarin.Forms, afin que le conteneur d’injection de dépendance d’une application peut contrôler la construction et la durée de vie de convertisseurs personnalisés, des effets, et `DependencyService` implémentations.
