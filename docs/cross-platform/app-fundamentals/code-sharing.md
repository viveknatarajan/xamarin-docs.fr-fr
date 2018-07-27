---
title: Présentation du partage de Code
description: 'Ce document compare les différentes méthodes de partage de code entre les projets multiplateformes : les projets partagés, bibliothèques de classes portables et .NET Standard, y compris les avantages et les inconvénients de chacun d’eux.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 82a73619e4c0507e8857cc91d88ababa870013de
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270470"
---
# <a name="sharing-code-overview"></a>Présentation du partage de code

_Ce document compare les différentes méthodes de partage de code entre les projets multiplateformes : .NET Standard, les projets partagés et des bibliothèques de classes portables, y compris les avantages et les inconvénients de chacun d’eux._

Il existe trois méthodes pour partager du code entre des applications multiplateformes :

- [**Bibliothèques .NET standard** ](#Net_Standard) – projets .NET Standard peut implémenter le code pour être partagés entre plusieurs plateformes et peut accéder à un grand nombre d’API .NET (selon la version). Implémentez de standard 1.0 à 1.6 .NET enrichit progressivement ensembles d’API, tandis que .NET Standard 2.0 fournit la meilleure couverture des.
- [**Projets partagés** ](#Shared_Projects) : utiliser le type de projet de ressources partagées pour organiser votre code source et utiliser `#if` directives de compilateur comme requis pour gérer la configuration requise spécifique à la plateforme.
- [**Bibliothèques de classes portables** ](#Portable_Class_Libraries) (déconseillé) : les bibliothèques de classes portables (PCL) peut cibler plusieurs plateformes avec une surface d’API commune et utiliser des Interfaces pour fournir des fonctionnalités spécifiques à la plateforme. Bibliothèques de classes portables sont déconseillés dans les dernières versions de Visual Studio &ndash; utilisez .NET Standard à la place.

L’objectif d’une stratégie de partage de code consiste à prendre en charge l’architecture illustrée dans ce diagramme, où une seule base de code peut être utilisée par plusieurs plateformes.

 ![Architecture d’application de code partagée](code-sharing-images/conceptualarchitecture.png "code application architecture partagée")

Cet article compare les méthodes disponibles pour vous aider à choisir le type de projet approprié pour vos applications.

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>Bibliothèques .NET standard

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md) bibliothèques fournissent un ensemble bien défini des bibliothèques de classes de base qui peuvent être référencés dans les différents types de projets, y compris les projets multiplateformes telles que Xamarin.Android et Xamarin.iOS. .NET standard 2.0 est recommandé pour optimiser la compatibilité avec le code .NET Framework existant.

![Diagramme de .NET standard](code-sharing-images/netstandard.png "diagramme .NET Standard")

### <a name="benefits"></a>Avantages

- Vous permet de partager du code entre plusieurs projets.
- Opérations de refactorisation toujours à jour affectés toutes les références.
- Une plus grande surface d’exposition de la bibliothèque de classes de Base (BCL) .NET est disponible à celle des profils de bibliothèque de classes portable. En particulier, .NET Standard 2.0 a pratiquement la même surface d’API que le .NET Framework et est recommandée pour les nouvelles applications et des bibliothèques de classes portables existantes portage.

### <a name="disadvantages"></a>Inconvénients

- Impossible d’utiliser des directives de compilateur comme `#if __IOS__`.

### <a name="remarks"></a>Notes

.NET standard est [similaire à la bibliothèque de classes portable](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), mais avec un modèle plus simple pour la prise en charge de la plateforme et un plus grand nombre de classes à partir de la BCL.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Projets partagés

[Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md) contiennent des fichiers de code et les ressources qui sont inclus dans les projets qui y fait référence. Projets de partage ne produisent pas la sortie compilée sur leurs propres.

Cette capture d’écran montre un fichier de solution contenant trois projets d’application (pour Android, iOS et Windows), avec un **partagé** projet qui contient les fichiers de code communes C# source :

![Partagé projet solution](code-sharing-images/sharedsolution.png "partagé solution du projet")

L’architecture conceptuelle est illustrée dans le diagramme suivant, où chaque projet inclut tous les fichiers de code source partagé :

![Diagramme du projet partagé](code-sharing-images/sharedassetproject.png "diagramme du projet partagé")

### <a name="example"></a>Exemple

Une application multiplateforme qui prend en charge d’iOS, Android et Windows nécessiterait un projet d’application pour chaque plateforme. Le code commun se trouve dans le projet partagé.

Un exemple de solution contiendrait les dossiers et les projets (noms de projet ont été choisis pour expressivité, vos projets n’ont pas de suivre ces instructions d’affectation de noms) suivants :

- **Partagé** – projet partagé qui contient le code commun à tous les projets.
- **AppAndroid** – projet d’application Xamarin.Android.
- **AppiOS** – projet d’application Xamarin.iOS.
- **AppWindows** – projet d’application Windows.

De cette façon les projets d’application de trois partagent le même code source (les fichiers c# dans partagé). Toutes les modifications au code partagé seront partagées entre toutes les trois projets.

### <a name="benefits"></a>Avantages

- Vous permet de partager du code entre plusieurs projets.
- Code partagé peut comprendre des branches en fonction de la plateforme à l’aide de directives de compilateur (par exemple). à l’aide de `#if __ANDROID__` , comme indiqué dans le [création d’Applications Cross-Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) document).
- Projets d’application peuvent inclure des références spécifiques à la plateforme le code partagé pouvant utiliser (par exemple à l’aide de `Community.CsharpSqlite.WP7` dans l’exemple Tasky pour Windows Phone).

### <a name="disadvantages"></a>Inconvénients

- Refactorisations qui affectent le code à l’intérieur de directives de compilateur 'inactive' ne met pas à jour le code à l’intérieur de ces directives.
- Contrairement à la plupart des autres types de projet, un projet partagé ne possède aucun assembly « sortie ». Pendant la compilation, les fichiers sont traités comme faisant partie du projet de référence et compilées dans cet assembly. Si vous souhaitez partager votre code en tant qu’assembly .NET Standard ou bibliothèques de classes portables sont une meilleure solution.

<a name="Shared_Remarks" />

### <a name="remarks"></a>Notes

Une bonne solution pour les développeurs d’applications écrire du code qui est uniquement destinée à partager dans leur application (et ne pas distribuer à d’autres développeurs).

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables

> [!TIP]
> Bibliothèques .NET standard 2.0 sont préférables aux bibliothèques de classes portables.

Bibliothèques de classes portables sont [abordés en détail ici](~/cross-platform/app-fundamentals/pcl.md).

![Diagramme de bibliothèque de classes portable](code-sharing-images/portableclasslibrary.png "diagramme de bibliothèque de classes Portable")

### <a name="benefits"></a>Avantages

- Vous permet de partager du code entre plusieurs projets.
- Opérations de refactorisation toujours à jour affectés toutes les références.

### <a name="disadvantages"></a>Inconvénients

- Déconseillée dans les dernières versions de Visual Studio, les bibliothèques .NET Standard sont recommandées à la place. Reportez-vous à ce [explication des différences](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) entre la bibliothèque de classes portable et .NET Standard.
- Impossible d’utiliser les directives de compilateur.
- Seul un sous-ensemble du .NET framework est disponible à utiliser, déterminée par le profil sélectionné (voir la [Introduction à la bibliothèque de classes portable](~/cross-platform/app-fundamentals/pcl.md) pour plus d’informations).

### <a name="remarks"></a>Notes

Modèle de bibliothèque de classes portable est considéré comme déconseillé dans les dernières versions de Visual Studio.

## <a name="summary"></a>Récapitulatif

Le choix de la stratégie de partage de code sera piloté par les plateformes que vous ciblez. Choisissez une méthode qui convient le mieux à votre projet.

.NET standard est le meilleur choix pour la création de bibliothèques de code partageable (en particulier la publication sur NuGet). Projets partagés fonctionnent bien pour les développeurs d’applications envisagez d’utiliser un grand nombre de fonctionnalités spécifiques à la plateforme dans leurs applications inter-plateformes.

Tandis que les projets de bibliothèque de classes portable continueront à être pris en charge dans Visual Studio, .NET Standard est recommandé pour les nouveaux projets.

## <a name="related-links"></a>Liens connexes

- [Building Cross-Platform Applications (document principal)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md)
- [Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Étude de cas : Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Exemple tasky (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Exemple tasky à l’aide de la bibliothèque de classes portable (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
