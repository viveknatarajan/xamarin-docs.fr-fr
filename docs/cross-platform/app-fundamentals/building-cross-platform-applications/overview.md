---
title: Building Cross-vue d’ensemble des Applications de plateforme
description: Ce document fournit une vue d’ensemble de la création des applications multiplateformes. Il présente la valeur de c#, les modèles de conception tels que MVC/MVVM et les interfaces utilisateur natives.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 1eb308e0095c29d8ab0d0bdf1f74b807fd2ab97f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780781"
---
# <a name="building-cross-platform-applications-overview"></a>Building Cross-vue d’ensemble des Applications de plateforme

Ce guide présente la plateforme de Xamarin et comment créer une application multiplateforme afin d’optimiser la réutilisation de code et de fournir une expérience de qualité native sur toutes les principales plateformes mobiles : iOS, Android et Windows Phone.

L’approche utilisée dans ce document est généralement applicable pour les applications de productivité et applications de jeu, toutefois, le focus est sur la productivité et l’utilitaire (hors jeu applications). Consultez le [MonoGame document présentation](~/graphics-games/monogame/introduction/index.md) ou extrayez [Visual Studio Tools pour Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) pour obtenir des conseils de développement de jeux multiplateformes.

La phrase « écriture-une seule fois, exécuter partout » est souvent utilisée pour chantent les avantages d’une seule base de code que s’exécute sans modification sur plusieurs plateformes. Alors qu’il a l’avantage de la réutilisation de code, cette approche permet souvent d’accéder aux applications qui ont un jeu de fonctionnalités plus petit qu’un dénominateur commun et d’une interface utilisateur générique-recherche qui ne tient pas bien dans aucune des plateformes cibles.

Xamarin n’est pas simplement une « écriture-une fois, exécuter partout » plateforme, car un de ses avantages est la possibilité d’implémenter des interfaces utilisateur natives spécifiquement pour chaque plateforme. Toutefois, avec une conception réfléchie il est toujours possible de partager la plupart de l’utilisateur non-code de l’interface et obtenir le meilleur des deux mondes : écrire votre code de la logique de données de stockage et une seule fois et de présenter des interfaces utilisateur natives sur chaque plateforme. Ce document décrit une approche architecturale générale pour atteindre cet objectif.

Voici un résumé des points clés pour la création d’applications inter-plateformes de Xamarin :

-   **Utiliser le langage c#** -écrire vos applications en c#. Code existant écrit en c# peut être déplacée vers iOS et Android très facilement à l’aide de Xamarin et évidemment utilisé dans les applications Windows.
-   **Utiliser des modèles de conception MVC ou MVVM** -développer l’Interface utilisateur de votre application à l’aide du modèle modèle/vue/Controller. L’architecture de votre application à l’aide d’un modèle/Vue/contrôleur ou du modèle/vue/ViewModel dans lequel il existe une distinction claire entre la « modèle » et le reste. Déterminer quelles parties de votre application sera à l’aide des éléments d’interface utilisateur native de chaque plateforme (iOS, Android, Windows, Mac) et utiliser cette indication pour diviser votre application en deux composants : « Core » et « Interface utilisateur ».
-   **Créer des interfaces utilisateur natives** -chaque application spécifiques du système d’exploitation fournit une couche d’interface utilisateur différentes (implémenté dans c# à l’aide des outils de conception de l’interface utilisateur natives) :

1.  Sur iOS, utilisez les APIs UIKit pour créer des applications de qualité natif, si vous le souhaitez en utilisant le concepteur iOS de Xamarin pour créer votre interface utilisateur visuellement.
1.  Sur Android, utilisez Android.Views pour créer des applications de qualité natif, en tirant parti du Concepteur de l’interface utilisateur de Xamarin.
1.  Sur Windows vous utiliserez XAML pour la couche de présentation, créé dans le Concepteur de l’interface utilisateur de Visual Studio ou Blend.
1.  Sur Mac, vous allez utiliser les animations de la couche de présentation, créée dans Xcode.

Les projets de Xamarin.Forms sont pris en charge sur toutes les plateformes et vous permettent de que créer des interfaces utilisateur qui peuvent être partagés entre les plateformes à l’aide de Xamarin.Forms XAML. 

La quantité de code réutilisation dépend en grande partie la quantité de code est conservé dans le noyau partagé et la quantité de code est l’interface utilisateur spécifique. Le code de base est tout ce qui n’interagit pas directement avec l’utilisateur, mais fournit à la place des services pour les parties de l’application qui recueille et afficher ces informations.

Pour augmenter la quantité de réutilisation de code, vous pouvez adopter inter-plateformes les composants qui fournissent des services communs dans tous ces systèmes tels que :

1.   [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) pour le stockage local de SQL,
1.   [Xamarin Plugins](https://xamarin.com/plugins) pour accéder aux fonctionnalités spécifiques à l’appareil, y compris la caméra, de contacts et de géolocalisation,
1.   [Les packages NuGet](https://nuget.org) qui sont compatibles avec les projets de Xamarin, tel que [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  À l’aide des fonctionnalités de .NET framework pour la mise en réseau, les services web, les e/s et bien plus encore.


Certaines de ces composants sont implémentées dans le *Tasky* étude de cas.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Séparer le Code réutilisable dans une bibliothèque de base

En suivant le principe de séparation de responsabilité par superposition d’architecture de votre application, puis à déplacer les fonctionnalités principales qui est indépendant des plates-formes dans une bibliothèque réutilisable core, vous pouvez optimiser le partage entre les plateformes, comme la figure ci-dessous de code décrit :

 ![](overview-images/layers2.png "En suivant le principe de séparation de responsabilité par superposition d’architecture de votre application, puis à déplacer les fonctionnalités principales qui est indépendant des plates-formes dans une bibliothèque réutilisable core, vous pouvez optimiser le partage entre les plateformes de code")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Études de cas

Il existe une étude de cas qui accompagnent ce document – *Tasky Pro*. Chaque étude de cas présente l’implémentation des concepts présentés dans ce document dans un exemple réel. Le code est open source et disponible sur [github](https://github.com/xamarin/mobile-samples/).
