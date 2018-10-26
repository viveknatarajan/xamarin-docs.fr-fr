---
title: Présentation de l’intégration Java
description: L’écosystème Java comprend une collection de diverses et considérable de composants. Plusieurs de ces composants peuvent être utilisés pour réduire le temps que nécessaire pour développer une application Android. Ce document présente et fournissent une vue d’ensemble de quelques-unes des méthodes que les développeurs peuvent utiliser ces composants Java existants afin d’améliorer leur expérience de développement d’application Xamarin.Android.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: 3ab31fb7cac97fbae3315f51daf3dd4b1edbcc1d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112388"
---
# <a name="java-integration-overview"></a>Présentation de l’intégration Java

_L’écosystème Java comprend une collection de diverses et considérable de composants. Plusieurs de ces composants peuvent être utilisés pour réduire le temps que nécessaire pour développer une application Android. Ce document présente et fournissent une vue d’ensemble de quelques-unes des méthodes que les développeurs peuvent utiliser ces composants Java existants afin d’améliorer leur expérience de développement d’application Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

Étant donné l’étendue de l’écosystème Java, il est très probable que toutes les fonctionnalités requises pour une application Xamarin.Android donnée a déjà été codée en Java. Pour cette raison, il est intéressante pour tenter de réutiliser ces bibliothèques existantes lors de la création d’une application Xamarin.Android. 

Il existe trois façons possibles de réutiliser les bibliothèques Java dans une application Xamarin.Android : 

-   **Créer une bibliothèque de liaisons Java** &ndash; avec cette technique, un projet Xamarin.Android sert à créer C# des wrappers autour des types Java. Une application Xamarin.Android peut ensuite référencer le C# wrappers créé par ce projet, puis utiliser le `.jar` fichier. 

-   **Java Native Interface** &ndash; le *Java Native* *Interface* (JNI) est une infrastructure qui permet au code de non Java (tels que C++ ou C#) pour appeler ou être appelé par du code Java en cours d’exécution à l’intérieur d’une machine virtuelle Java. 

-   **Déplacer le Code** &ndash; cette méthode consiste à prendre le code source Java et puis le convertir en C#. Cela est possible, manuellement ou à l’aide d’un outil automatisé tels que NET. 

Au cœur des deux premières techniques est le *Java Native Interface* (JNI). JNI est une infrastructure qui permet aux applications ne pas écrites en Java interagir avec le code Java en cours d’exécution dans une Machine virtuelle Java. Xamarin.Android utilise JNI pour créer *liaisons* pour C# code. 

La première technique est une approche plus automatisée et déclarative pour la liaison de bibliothèques de Java. Il implique l’utilisation de Visual Studio pour Mac ou un type de projet Visual Studio qui est fourni par Xamarin.Android &ndash; la bibliothèque de liaisons Java. Pour créer ces liaisons, une bibliothèque de liaisons Java peuvent toujours nécessiter que certaines modifications manuelles, mais pas autant que serait une approche JNI pure. Consultez [liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md) pour plus d’informations sur les bibliothèques Java de liaison. 

La deuxième technique, à l’aide de JNI, fonctionne à un niveau bien inférieur, mais peut fournir pour un contrôle plus précis et accéder aux méthodes Java qui ne seraient généralement pas accessibles via une bibliothèque de liaison de Java. 

La troisième technique est radicalement différente provenant des deux précédentes : le portage du code à partir de Java à C#. Portage du code d’une langue à l’autre peut être un processus très fastidieuse, mais il est possible de réduire que les efforts à l’aide d’un outil appelé *netteté*. Aiguiser est un outil open source qui est un Java-à-C# convertisseur. 



## <a name="summary"></a>Récapitulatif

Ce document fourni une vue d’ensemble de certaines des différentes méthodes que les bibliothèques à partir de Java peuvent être réutilisés dans une application Xamarin.Android. Il a introduit les concepts de liaisons et gérées callable wrappers et décrit les options pour le portage de code Java C#. 


## <a name="related-links"></a>Liens associés

- [Architecture](~/android/internals/architecture.md)
- [Liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md)
- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Améliorer la netteté](https://github.com/slluis/sharpen)
- [Interface Native Java](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
