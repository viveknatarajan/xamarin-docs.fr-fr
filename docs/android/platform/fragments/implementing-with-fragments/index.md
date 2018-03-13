---
title: "Mise en œuvre avec des Fragments"
description: "Android 3.0 introduit des Fragments. Les fragments sont des composants autonomes et modulaires qui aident à gérer la complexité de l’écriture d’applications exécutées sur des écrans de différentes tailles. Cet article montre comment l’utilisation de fragments de développer des applications de Xamarin.Android et la prise en charge des fragments sur les appareils Android préalable 3.0."
ms.topic: article
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2ed67eac51f6edcfda16caf73e4667c49124082c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-with-fragments"></a>Mise en œuvre avec des Fragments

_Android 3.0 introduit des Fragments. Les fragments sont des composants autonomes et modulaires qui aident à gérer la complexité de l’écriture d’applications exécutées sur des écrans de différentes tailles. Cet article montre comment l’utilisation de fragments de développer des applications de Xamarin.Android et la prise en charge des fragments sur les appareils Android préalable 3.0._


## <a name="overview"></a>Vue d'ensemble

Dans cette section, nous examinerons comment créer une application qui affiche une liste de la lecture de Shakespeare et un guillemet à partir de chaque jeu sélectionné. Notre application utilisera fragments afin que nous puissions définir des composants d’interface utilisateur dans un seul emplacement, mais puis de les utiliser sur différents facteurs de forme. Par exemple, les captures d’écran ci-après affichent l’application en cours d’exécution sur une tablette 10 », ainsi que sur un téléphone :

[![Captures d’écran de l’exemple d’application en cours d’exécution sur une tablette et téléphone](images/intro-screenshot-sml.png)](images/intro-screenshot.png#lightbox)

Cette section couvre les rubriques suivantes :

- **Création de Fragments** &ndash; montre comment créer un fragment pour afficher une liste de la lecture de Shakespeare et un autre fragment pour afficher un guillemet à partir de chaque jeu.

- **Prise en charge de différentes tailles d’écran** &ndash; montre comment la disposition de l’application pour tirer parti des tailles d’écran plus grands.

- **L’utilisation du Package de prise en charge Android** &ndash; implémente le Package de prise en charge Android, puis effectue quelques changements mineurs aux activités dans l’application, ce qui permet de s’exécuter sur des versions antérieures d’Android.


## <a name="requirements"></a>Configuration requise

Cette procédure pas à pas requiert Xamarin.Android 4.0 ou version ultérieure. Il sera également être nécessaire installer le Package de prise en charge Android, comme indiqué dans la documentation de Fragments.


## <a name="introduction"></a>Introduction

Dans l’exemple, que nous allons créer dans cette section, les activités ne contiennent pas de logique pour le chargement de la liste, répondre à la sélection de l’utilisateur ou l’affichage de la demande pour le jeu sélectionné. Cette logique existe dans les fragments individuels.
En plaçant cette logique dans les fragments eux-mêmes, nous pouvons scinder le flux de travail de l’application pour prendre en charge des écrans de grande taille avec une activité ou les petits écrans avec plusieurs activités sans avoir à écrire une logique différente pour chaque activité. Sur un Tablet PC, les deux fragments seront trouve dans une activité. Sur un téléphone, les fragments seront hébergés dans les différentes activités.

Cette application comprend les éléments suivants :

 **MainActivity** – affiche un ou les deux des fragments, en fonction de la taille de l’écran. Il s’agit de l’activité de démarrage.

 **TitlesFragment** – affiche une liste de lecture de Shakespeare à partir de laquelle l’utilisateur peut sélectionner.

 **DetailsFragment** – affiche les guillemets de la lecture sélectionnée.

 **DetailsActivity** : héberge et affiche le DetailsFragment.
Cette activité est utilisée par les appareils avec un écran de petite taille, tels que les téléphones.



## <a name="related-links"></a>Liens associés

- [FragmentsWalkthrough (exemple)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Vue d’ensemble du Concepteur](~/android/user-interface/android-designer/index.md)
- [Exemples de Xamarin.Android : nid galerie](https://developer.xamarin.com/samples/HoneycombGallery/)
- [Implémentation des Fragments](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Package de prise en charge](http://developer.android.com/sdk/compatibility-library.html)
