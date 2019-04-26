---
title: Création d’Applications présentation de la plateforme
description: Ce document fournit une vue d’ensemble de la création d’applications multiplateformes. Il aborde la valeur de C#, concevoir des modèles tels que MVC/MVVM et des interfaces utilisateur natives.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 1eb308e0095c29d8ab0d0bdf1f74b807fd2ab97f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275625"
---
# <a name="building-cross-platform-applications-overview"></a>Création d’Applications présentation de la plateforme

Ce guide présente la plateforme Xamarin et comment créer une application multiplateforme pour optimiser la réutilisation de code et de fournir une expérience native de haute qualité sur toutes les principales plateformes mobiles : iOS, Android et Windows Phone.

L’approche utilisée dans ce document est généralement applicable aux applications de productivité et les applications de jeu, cependant, le focus est sur la productivité et l’utilitaire (applications hors jeu). Consultez le [Introduction à MonoGame document](~/graphics-games/monogame/introduction/index.md) ou découvrir [Visual Studio Tools pour Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) pour obtenir des conseils de développement de jeux inter-plateformes.

L’expression « écrire-une seule fois, exécuter partout » est souvent utilisée pour chantent les louanges d’une seule codebase que s’exécute sans modification sur plusieurs plateformes. Alors qu’il a l’avantage de réutiliser le code, cette approche conduit souvent à des applications qui ont un ensemble de fonctionnalités le plus bas qu’un dénominateur commun et une interface utilisateur générique-recherche qui ne tient pas bien dans une des plateformes cibles.

Xamarin n’est pas simplement une « écriture-une seule fois, exécuter partout » plateforme, car un de ses forces est la capacité à implémenter des interfaces utilisateur natives spécifiquement pour chaque plateforme. Toutefois, avec une conception réfléchie il est toujours possible de partager la majeure partie de l’utilisateur non-code d’interface et bénéficiez du meilleur des deux mondes : écrire votre code de la logique de données de stockage et une seule fois et présenter des interfaces utilisateur natives sur chaque plateforme. Ce document décrit une approche architecturale générale pour atteindre cet objectif.

Voici un résumé des points clés pour la création d’applications multiplateforme de Xamarin :

-   **Utilisez C#**  -écrire vos applications C#. Code existant écrit dans C# peut être déplacée vers iOS et Android à l’aide de Xamarin très facilement et évidemment utilisé dans les applications Windows.
-   **Utiliser des modèles de conception MVC ou MVVM** -développer d’Interface d’utilisateur de votre application à l’aide du modèle de modèle / / contrôleur d’affichage. L’architecture de votre application à l’aide d’un modèle/affichage/contrôleur ou du modèle/affichage/ViewModel où il existe une séparation claire entre le « modèle » et le reste. Déterminer quelles parties de votre application sera à l’aide des éléments d’interface utilisateur native de chaque plateforme (iOS, Android, Windows, Mac) et utilisez-le comme indication pour diviser votre application en deux composants : « Core » et « Interface utilisateur ».
-   **Créer des interfaces utilisateur natives** -chaque application spécifique à un système d’exploitation fournit une couche d’interface utilisateur différentes (implémenté dans C# outils de conception avec l’aide de l’interface utilisateur native) :

1.  Sur iOS, utilisez les APIs UIKit pour créer des applications natives attrayantes, éventuellement qui utilisent le Concepteur de Xamarin iOS pour créer votre interface utilisateur visuellement.
1.  Sur Android, utilisez Android.Views pour créer des applications de qualité natif, en tirant parti de concepteur d’interface utilisateur de Xamarin.
1.  Sur Windows vous utiliserez XAML pour la couche de présentation, créé dans Visual Studio ou Blend Concepteur d’interface utilisateur.
1.  Sur Mac, vous allez utiliser des tables de montage séquentiel pour la couche de présentation, créée dans Xcode.

Les projets Xamarin.Forms sont pris en charge sur toutes les plateformes et vous permettent de que créer des interfaces utilisateur qui peuvent être partagées entre les plateformes à l’aide de XAML de Xamarin.Forms. 

La quantité de réutiliser le code dépend en grande partie la quantité de code est conservé dans le cœur partagé et la quantité de code est l’interface utilisateur spécifique. Le code de base est tout ce qui n’interagit pas directement avec l’utilisateur, mais fournit à la place des services pour les parties de l’application qui les recueillera et afficher ces informations.

Pour augmenter la quantité de réutiliser le code, vous pouvez adopter les composants inter-plateformes qui fournissent des services communs dans tous ces systèmes tels que :

1.   [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) pour le stockage local SQL,
1.   [Xamarin Plugins](https://xamarin.com/plugins) permettant d’accéder aux fonctionnalités spécifiques au périphérique, y compris de l’appareil photo, de contacts et de géolocalisation,
1.   [Les packages NuGet](https://nuget.org) qui sont compatibles avec les projets Xamarin, tel que [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  À l’aide des fonctionnalités de .NET framework pour la mise en réseau, les services web, les e/s et bien plus encore.


Certains de ces composants sont implémentées dans le *Tasky* étude de cas.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Séparer le Code réutilisable dans une bibliothèque principale

En suivant le principe de séparation des responsabilités en superposition d’architecture de votre application, puis en déplaçant les fonctionnalités principales qui est indépendante de la plateforme dans une bibliothèque réutilisable principale, vous pouvez optimiser le partage de code entre plateformes, comme la figure ci-dessous illustre :

 ![](overview-images/layers2.png "En suivant le principe de séparation des responsabilités en superposition d’architecture de votre application, puis en déplaçant les fonctionnalités principales qui est indépendante de la plateforme dans une bibliothèque réutilisable principale, vous pouvez optimiser le partage de code entre plateformes")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Études de cas

Il existe une étude de cas qui accompagne ce document – *Tasky Pro*. Chaque étude de cas décrit l’implémentation des concepts décrites dans ce document dans un exemple réel. Le code est open source et disponible sur [github](https://github.com/xamarin/mobile-samples/).
