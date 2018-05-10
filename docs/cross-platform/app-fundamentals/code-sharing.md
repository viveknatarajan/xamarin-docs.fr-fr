---
title: Options de partage de Code
description: 'Ce document compare les différentes méthodes de partage de code entre les projets interplateforme : les projets partagés, bibliothèques de classes portables et .NET Standard, y compris les avantages et les inconvénients de chacune.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: de2e24b1746568510c84fb163efa8562ab47cf00
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="sharing-code-overview"></a>Vue d’ensemble du Code de partage

_Ce document compare les différentes méthodes de partage de code entre les projets interplateforme : les projets partagés, bibliothèques de classes portables et .NET Standard, y compris les avantages et les inconvénients de chacune._

Il existe trois méthodes alternatives pour partager du code entre des applications multiplateformes :

-   [**Projets partagés** ](#Shared_Projects) : le type de projet d’actifs partagés permet d’organiser votre code source et utiliser des directives de compilateur #if comme requis pour gérer les exigences spécifiques à la plateforme.
-   [**Bibliothèques de classes portables** ](#Portable_Class_Libraries) : créer une cible de la bibliothèque de classes Portable (PCL) les plateformes que vous souhaitez prendre en charge et utilisez les Interfaces pour fournir des fonctionnalités spécifiques à la plateforme.
-   [**Les bibliothèques standard .NET** ](#Net_Standard) – projets .NET Standard fonctionnent de façon similaire à PCLs, exigeant l’utilisation d’Interfaces d’injecter des fonctionnalités spécifiques à la plateforme.

L’objectif d’une stratégie de partage de code est à prendre en charge l’architecture illustrée dans ce diagramme, où une seule base de code peut être utilisée par plusieurs plateformes.

 ![](code-sharing-images/conceptualarchitecture.png "Architecture d’application de code partagé")

Cet article compare les trois méthodes pour vous aider à choisir le type de projet approprié pour vos applications.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Projets partagés

L’approche la plus simple pour le partage de fichiers de code consiste à utiliser un [projet partagé](~/cross-platform/app-fundamentals/shared-projects.md).

Cette capture d’écran montre un fichier de solution contenant trois projets d’application (pour Android, iOS et Windows Phone), avec un **Shared** projet qui contient les fichiers de code communes C# source :

 ![](code-sharing-images/sharedsolution.png "Solution de projet partagé")

L’architecture conceptuelle est indiqué dans le diagramme suivant, où chaque projet inclut tous les fichiers sources partagés :

 ![](code-sharing-images/sharedassetproject.png "Diagramme de projet partagées")


### <a name="example"></a>Exemple

Une application multiplateforme qui prend en charge d’iOS, Android et Windows Phone nécessiterait un projet d’application pour chaque plateforme. Le code commun réside dans le projet partagé.

Un exemple de solution contient les dossiers et les projets (noms de projet ont été choisis pour simplicité, vos projets n’ont pas de suivre ces règles d’affectation de noms) suivants :

-   **Shared** : le projet partagé qui contient le code commun à tous les projets.
-   **AppAndroid** – Xamarin.Android projet d’application.
-   **AppiOS** – Xamarin.iOS projet d’application.
-   **AppWinPhone** -projet d’application Windows Phone.


De cette façon les projets de trois application partagent le même code source (fichiers c# dans partagé). Toutes les modifications au code partagé seront partagées entre tous les trois projets.


### <a name="benefits"></a>Avantages

-  Vous permet de partager du code sur plusieurs projets.
-  Code partagé peut être reliée par une branche en fonction de la plateforme à l’aide de directives de compilateur (par exemple). à l’aide de `#if __ANDROID__` , comme indiqué dans le [génération d’Applications Cross-Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) document).
-  Les projets d’application peuvent inclure des références de plateforme spécifique que le code partagé peut utiliser (par exemple à l’aide de `Community.CsharpSqlite.WP7` dans l’exemple Tasky pour Windows Phone).



### <a name="disadvantages"></a>Inconvénients

-  Contrairement à la plupart des autres types de projet, un projet partagé n’a aucun assembly de « sortie ». Lors de la compilation, les fichiers sont traités comme faisant partie du projet de référence et compilées dans cet assembly. Si vous souhaitez partager votre code en tant qu’assembly .NET Standard ou bibliothèques de classes portables sont une meilleure solution.
-  Refactorisations qui affectent le code à l’intérieur des directives de compilateur 'inactive' ne mettra pas à jour le code.


 <a name="Shared_Remarks" />

### <a name="remarks"></a>Notes

Une bonne solution pour les développeurs d’applications l’écriture de code qui est uniquement destinée à leur application de partage (et ne pas distribuer à d’autres développeurs).

 <a name="Portable_Class_Libraries" />


## <a name="portable-class-libraries"></a>Bibliothèques de classes portables


Bibliothèques de classes portables sont [présentées en détail ici](~/cross-platform/app-fundamentals/pcl.md).

 ![](code-sharing-images/portableclasslibrary.png "Diagramme de bibliothèque de classes portable")


### <a name="benefits"></a>Avantages

-  Vous permet de partager du code sur plusieurs projets.
-  Opérations de refactorisation, toujours à jour affectés toutes les références.


### <a name="disadvantages"></a>Inconvénients

-  Impossible d’utiliser les directives de compilateur.
-  Seul un sous-ensemble du .NET framework est disponible à utiliser, déterminée par le profil sélectionné (voir la [Introduction à la bibliothèque PCL](~/cross-platform/app-fundamentals/pcl.md) pour plus d’informations).


### <a name="remarks"></a>Notes

Une bonne solution si vous envisagez de partager l’assembly résultant avec d’autres développeurs.



<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>Bibliothèques .NET standards

.NET standard se [présentées en détail ici](~/cross-platform/app-fundamentals/net-standard.md).

![](code-sharing-images/netstandard.png "Diagramme .NET standard")

### <a name="benefits"></a>Avantages

-  Vous permet de partager du code sur plusieurs projets.
-  Opérations de refactorisation, toujours à jour affectés toutes les références.
-  Une plus grande surface d’exposition de la bibliothèque de classes de Base (BCL) .NET est disponible à des profils de la bibliothèque de classes portables.

### <a name="disadvantages"></a>Inconvénients

 -  Impossible d’utiliser les directives de compilateur.

### <a name="remarks"></a>Notes

.NET standard est similaire à la bibliothèque de classes portables, mais avec un modèle plus simple pour la prise en charge de la plateforme et un plus grand nombre de classes à partir de la BCL.



## <a name="summary"></a>Récapitulatif

Le choix de la stratégie de partage de code sera déterminée par la plateforme ciblée. Choisissez une méthode qui convient le mieux à votre projet.

Bibliothèque de classes portables ou .NET Standard sont de bons choix pour la création de bibliothèques de code partageable (notamment la publication sur NuGet). Les projets partagés fonctionnent bien pour les développeurs d’applications envisagez d’utiliser un grand nombre de fonctionnalités entre plates-formes spécifiques dans leurs applications inter-plateformes.


## <a name="related-links"></a>Liens associés

- [Développement d’Applications de plateforme Cross (document principal)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md)
- [Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Étude de cas : Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Exemple tasky (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Exemple tasky à l’aide de la bibliothèque de classes portables (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
