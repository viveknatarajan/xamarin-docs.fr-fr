---
title: Concepts avancés et des éléments internes
description: Architecture sous-jacente derrière Xamarin.Android et sa conception de l’API.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/13/2017
ms.openlocfilehash: 517e21f2decd0dabbd03d752f13831a891ad7138
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-concepts-and-internals"></a>Concepts avancés et des éléments internes


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Architecture](~/android/internals/architecture.md)

Cet article explique l’architecture sous-jacente derrière une application Xamarin.Android. Il explique l’exécution d’applications de Xamarin.Android dans un environnement d’exécution Mono en même temps qu’avec le runtime Android Machine virtuelle et explique ces concepts clés comme Android Callable Wrappers et gérés par. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[Conception d’API](~/android/internals/api-design.md)

Outre le noyau de bibliothèques de classes de Base qui font partie de Mono, Xamarin.Android est fourni avec des liaisons pour les différentes API Android permettre aux développeurs de créer des applications Android natives avec Mono.

Au cœur de Xamarin.Android est un moteur d’interopérabilité que world ponts c# avec le monde Java et permet aux développeurs d’accéder aux API Java à partir de c# ou d’autres langages .NET.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Assemblys](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android est fourni avec plusieurs assemblys. Tout comme Silverlight est un sous-ensemble étendu des assemblys .NET bureau, Xamarin.Android est également un sous-ensemble étendu de plusieurs Silverlight et les assemblys .NET du bureau. 

