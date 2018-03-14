---
title: "Partie 3 : configuration d’une Solution de plateforme Xamarin croisée"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: 852c11eeccf347c3175cc5d8d42f55616aedfb84
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Partie 3 : configuration d’une Solution de plateforme Xamarin croisée

Quelle que soit la Quelles plateformes sont utilisées, les projets de Xamarin tous les utilisent le même format de fichier de solution (Visual Studio **.sln** format de fichier). Les solutions peuvent être partagées entre les environnements de développement, même lorsque des projets individuels ne peut pas être chargés (par exemple, un projet Windows dans Visual Studio pour Mac).



Lorsque vous créez un nouveau entre l’application de la plateforme, la première étape consiste à créer une solution vide. Cette section les étapes suivantes : configuration des projets pour générer des applications mobiles inter-plateformes.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Partage de Code

Reportez-vous à la [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md) document pour obtenir une description détaillée de l’implémentation de partage de code entre plateformes.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Projets partagés

L’approche la plus simple pour le partage de fichiers de code est d’utiliser un [projet partagé](~/cross-platform/app-fundamentals/shared-projects.md).

Cette méthode vous permet de partager le même code pour les projets de plateforme différente et utiliser des directives de compilateur pour inclure les chemins de code différentes, spécifique à la plateforme.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Bibliothèques de classes portables (PCL)

Historiquement un fichier de projet .NET (et l’assembly résultant) sont destinés à une version de framework spécifique. Cela empêche le projet ou l’assembly partagé par différentes infrastructures.

Une bibliothèque de classes Portable (PCL) est un type spécial de projet qui peut être utilisé sur différentes plates-formes CLI comme Xamarin.iOS et Xamarin.Android, ainsi que WPF, la plateforme Windows universelle et Xbox. La bibliothèque ne peut utiliser qu’un sous-ensemble du .NET framework complet, limité par les plateformes actuellement ciblées.

Vous pouvez en savoir plus sur Xamarin [prise en charge des bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md) et suivez les instructions pour voir comment la [TaskyPortable exemple](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) fonctionne.


### <a name="net-standard"></a>.NET Standard

Introduite dans 2016, [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) projets fournissent un moyen simple de partager le code entre plateformes, les assemblys qui peut être utilisé sur Windows et Linux Xamarin plateformes (iOS, Android, Mac).

Les bibliothèques .NET standard peuvent être créés et utilisés comme PCLs, à ceci près que les API disponibles dans chaque version (1.0 vers la version 1.6) ne sont plus facilement découverts et chaque version est à compatibilité descendante avec les numéros de version inférieure.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Remplissage de la Solution

Quelle que soit la méthode est utilisée pour partager du code, la structure globale de la solution doit implémenter une architecture multiniveau qui encourage le partage de code.
L’approche de Xamarin est au code de groupe dans les deux types de projets :

-   **Projet de base** : écrire du code réutilisable dans un seul endroit, à partager entre différentes plateformes. Utiliser le principe d’encapsulation pour masquer les détails d’implémentation chaque fois que possible.
-   **Les projets d’application de plateforme spécifique** – consommer le code réutilisable avec comme couplage peu que possible. Les fonctionnalités spécifiques à la plateforme sont ajoutées à ce niveau, de composants exposées dans le projet principal.


 <a name="Core_Project" />


### <a name="core-project"></a>Projet de base

Projets de code partagé doivent uniquement faire référence les assemblys qui sont disponibles sur toutes les plateformes – Internet Explorer. comme les espaces de noms framework communs `System`, `System.Core` et `System.Xml`.

Les projets partagés doivent implémenter autant de fonctionnalités non IU est possible, ce qui peut inclure les couches suivantes :

-   **Couche données** – Code qui prend en charge le stockage de données physique, par exemple.  [SQLite-NET](https://github.com/praeclarum/sqlite-net), une autre base de données comme [Realm.io](https://realm.io/products/realm-mobile-database/) ou même des fichiers XML. Les classes de la couche données sont normalement utilisée uniquement par la couche d’accès aux données.
-   **Couche d’accès aux données** – définit une API qui prend en charge les opérations de données requises pour les fonctionnalités de l’application, telles que les méthodes pour accéder aux listes de données, des éléments de données et pour créer, modifier et les supprimer.
-   **Couche d’accès aux services** – une couche facultative pour le cloud de fournir des services à l’application. Contient le code qui accède aux ressources réseau à distance (services web, téléchargements d’images, etc.) et éventuellement la mise en cache des résultats.
-   **Couche métier** : définition des classes de modèle et les Façade ou le Gestionnaire de classes qui exposent les fonctionnalités pour les applications spécifiques à la plateforme.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Projets d’Application de plateforme spécifique

Projets spécifiques à la plate-forme doivent référencer les assemblys requis pour lier à Kit de développement logiciel du chaque plateforme (Xamarin.iOS, Xamarin.Android, Xamarin.Mac ou Windows), ainsi que le projet de code partagé Core.

Les projets spécifiques à une plateforme doivent implémenter :

-   **Couche d’application** – fonctionnalités spécifiques de plateforme et de la liaison ou de conversion entre les objets de la couche métier et de l’interface utilisateur.
-   **Couche d’Interface utilisateur** – écrans, les contrôles d’interface utilisateur personnalisée, présentation de la logique de validation.


<a name="Example" />


### <a name="example"></a>Exemple

L’architecture d’application est illustrée dans ce diagramme :

 [ ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "L’architecture d’application est illustrée dans ce diagramme")](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Cette capture d’écran montre une configuration de la solution de projet cœur partagé, iOS et les projets d’application Android. Le projet partagé contient un code relatif à chacune des couches de l’architecture (code d’accès aux données, données, Service et entreprise) :

 ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Le projet partagé contient un code relatif à chacune des couches de l’architecture (code d’accès aux données, données, Service et entreprise)")


 <a name="Project_References" />


## <a name="project-references"></a>Références de projets

Références de projet reflètent les dépendances pour un projet. Les projets limitent leurs références à des assemblys afin que le code est facile à partager.
Les projets d’application de plateforme spécifique référencent le code partagé, ainsi que tous les autres assemblys spécifiques à la plateforme qu’ils doivent tirer parti de la plateforme cible.

Les projets d’application chaque référencent de projet partagé et contiennent le code de l’interface utilisateur nécessaire aux fonctionnalités présente à l’utilisateur, comme indiqué dans les captures d’écran :

![](part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Les projets de l’application de chaque référence de projet partagé") ![ ] (part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "l’application projette chaque référence de projet partagé")


Des exemples spécifiques de structuration des projets sont donnés dans les études de cas.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Ajout de fichiers

 <a name="Build_Action" />


### <a name="build-action"></a>Action de génération

Il est important de définir l’action de génération correcte pour certains types de fichiers. Cette liste affiche l’action de génération pour certains types de fichiers courants :

-  **Tous les fichiers c#** – Action de génération : compiler
-   **Images dans Xamarin.iOS & Windows** – Action de génération : contenu
-   **Les fichiers XIB et animation dans Xamarin.iOS** – Action de génération : interface
-   **Images et dispositions AXML dans Android** – Action de génération : AndroidResource
-  **Les fichiers XAML dans les projets Windows** – Action de génération : Page
-  **Fichiers XAML de Xamarin.Forms** – Action de génération : EmbeddedResource


En général l’IDE détecte le type de fichier et proposer l’action de génération correcte.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Respect de la casse

Enfin, n’oubliez pas que certaines plates-formes de systèmes de fichiers respectant la casse (par exemple).
iOS et Android), veillez à utiliser une norme d’affectation de noms de fichier cohérent et vous assurer que les noms de fichiers que vous utilisez dans le code correspond exactement au système de fichiers. Cela est particulièrement important pour les images et d’autres ressources que vous référencez dans le code.
