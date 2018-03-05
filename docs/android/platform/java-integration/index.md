---
title: "Vue d’ensemble de l’intégration de Java"
description: "L’écosystème de Java inclut une collection considérable les différents composants. Plusieurs de ces composants peuvent être utilisés pour réduire le temps que nécessaire pour développer une application Android. Ce document présente et fournir une vue d’ensemble de certaines des méthodes que les développeurs peuvent utiliser ces composants Java existants afin d’améliorer leur expérience de développement d’application Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: 88e8c66d36956649f0a996046f038d89a7267cf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="java-integration-overview"></a>Vue d’ensemble de l’intégration de Java

_L’écosystème de Java inclut une collection considérable les différents composants. Plusieurs de ces composants peuvent être utilisés pour réduire le temps que nécessaire pour développer une application Android. Ce document présente et fournir une vue d’ensemble de certaines des méthodes que les développeurs peuvent utiliser ces composants Java existants afin d’améliorer leur expérience de développement d’application Xamarin.Android._

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Étant donné l’étendue de l’écosystème de Java, il est très probable que toutes les fonctionnalités requises pour une application Xamarin.Android donnée a déjà été codée dans Java. Pour cette raison, il est attrayante et essayez de réutiliser ces bibliothèques existantes lors de la création d’une application de Xamarin.Android. 

Il existe trois moyens possibles de réutiliser les bibliothèques Java dans une application de Xamarin.Android : 

-   **Créer une bibliothèque de liaisons Java** &ndash; avec cette technique, un projet Xamarin.Android est utilisé pour créer des wrappers c# autour des types Java. Une application de Xamarin.Android pouvez ensuite référencer les wrappers c# créés par ce projet, puis utiliser le `.jar` fichier. 

-   **Java Native Interface** &ndash; le *Java natif* *Interface* (JNI) est une infrastructure qui permet de code non Java (par exemple, C++ ou c#) pour appeler ou être appelé par le code Java en cours d’exécution à l’intérieur d’une machine virtuelle Java. . 

-   **Le Code de port** &ndash; cette méthode implique de prendre le code source Java, puis en les convertissant en c#. Cela est possible, manuellement ou à l’aide d’un outil automatisé telles que l’accentuation. 

Au cœur des deux premières techniques le *Java Native Interface* (JNI). JNI est une infrastructure qui permet aux applications non écrites en Java interagir avec le code Java en cours d’exécution dans une Machine virtuelle Java. Xamarin.Android utilise JNI pour créer *liaisons* pour le code c#. 

La première technique est une approche déclarative plus automatisée à des bibliothèques Java de liaison. Il implique l’utilisation de Visual Studio pour Mac ou un type de projet Visual Studio qui est fourni par Xamarin.Android &ndash; la bibliothèque de liaisons de Java. Pour créer ces liaisons, une bibliothèque de liaisons de Java peut nécessiter toujours certaines modifications manuelles, mais pas autant que serait une approche JNI pure. Consultez [liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md) pour plus d’informations sur les bibliothèques de liaison de Java. 

La seconde technique, à l’aide de JNI, fonctionne à un niveau beaucoup plus faible, mais peut en fournir pour un contrôle plus précis et l’accès aux méthodes Java qui ne devrait pas être accessibles via une bibliothèque de liaison de Java. 

La troisième technique est radicalement différente dans les deux précédents : le code à partir de Java en c#. Portage du code d’une langue à l’autre peut être un processus très laborieuse, mais il est possible de réduire qu’effort à l’aide d’un outil appelé *l’accentuation*. Netteté est un outil open source qui est un Java-à-convertisseur c#. 


<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Ce document fourni une vue d’ensemble de certaines des différentes méthodes que les bibliothèques à partir de Java peuvent être réutilisés dans une application Xamarin.Android. Il a introduit les concepts de liaisons et des callable wrappers managés et décrit les options pour le portage de code Java en c#. 


## <a name="related-links"></a>Liens associés

- [Architecture](~/android/internals/architecture.md)
- [Liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md)
- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Améliorer la netteté](https://github.com/slluis/sharpen)
- [Java Native Interface](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
