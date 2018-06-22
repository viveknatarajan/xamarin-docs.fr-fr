---
title: Concepts avancés et des éléments internes
description: Architecture sous-jacente derrière Xamarin.Android et sa conception de l’API.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/21/2018
ms.openlocfilehash: 79e61db4c27a2d29b4ee0a9d39f2d25ea5d93303
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
ms.locfileid: "34458352"
---
# <a name="advanced-concepts-and-internals"></a>Concepts avancés et des éléments internes

_Cette section contient des rubriques qui expliquent l’architecture, conception de l’API et les limitations de Xamarin.Android. En outre, il inclut des rubriques qui expliquent son implémentation de garbage collection et les assemblys qui sont disponibles dans Xamarin.Android. Étant Xamarin.Android [open source](https://github.com/xamarin/xamarin-android), il est également possible de comprendre le fonctionnement interne de Xamarin.Android en examinant son code source._


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Architecture](~/android/internals/architecture.md)

Cet article explique l’architecture sous-jacente derrière une application Xamarin.Android. Il explique l’exécution d’applications de Xamarin.Android dans un environnement d’exécution Mono en même temps qu’avec le runtime Android Machine virtuelle et explique ces concepts clés comme Android Callable Wrappers et gérés par. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[Conception d’API](~/android/internals/api-design.md)

Outre le noyau de bibliothèques de classes de Base qui font partie de Mono, Xamarin.Android est fourni avec des liaisons pour les différentes API Android permettre aux développeurs de créer des applications Android natives avec Mono.

Au cœur de Xamarin.Android est un moteur d’interopérabilité que world ponts c# avec le monde Java et permet aux développeurs d’accéder aux API Java à partir de c# ou d’autres langages .NET.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Assemblys](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android est fourni avec plusieurs assemblys. Tout comme Silverlight est un sous-ensemble étendu des assemblys .NET bureau, Xamarin.Android est également un sous-ensemble étendu de plusieurs Silverlight et les assemblys .NET du bureau. 

