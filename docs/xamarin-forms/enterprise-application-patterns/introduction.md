---
title: Introduction
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a4f1f6ba820221be7553405f570911d3dc66a657
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction"></a>Introduction

Quelle que soit la plateforme, les développeurs d’applications d’entreprise sont confrontés à plusieurs problèmes :

-   Exigences de l’application qui peuvent changer au fil du temps.
-   Défis et nouvelles opportunités commerciales.
-   Commentaires en continu pendant le développement qui peut affecter considérablement la portée et la configuration requise de l’application.

Avec ces éléments à l’esprit, il est important créer des applications qui peuvent être modifiées ou étendues au fil du temps. La conception pour une telle capacité d’adaptation peut être difficile, car il nécessite une architecture qui permet aux parties individuelles de l’application indépendamment développés et testés de manière isolée, sans affecter le reste de l’application.

Plusieurs applications d’entreprise sont suffisamment complexes pour nécessiter plusieurs développeurs. Il peut être un défi important pour décider comment concevoir une application de sorte que plusieurs développeurs peuvent travailler efficacement sur différentes parties de l’application indépendamment, tout en garantissant que les éléments de collaborer en toute transparence lorsque intégré dans l’application.

L’approche traditionnelle de la conception et création d’une application entraîne ce qui est appelé un *monolithique* application, où les composants sont étroitement avec aucune distinction claire entre eux. En règle générale, cette approche monolithiques aux applications qui sont difficiles et inefficace pour mettre à jour, car il peut être difficile à résoudre des bogues sans interruption des autres composants dans l’application, et il peut être difficile à ajouter de nouvelles fonctionnalités ou pour remplacer les fonctionnalités existantes.

Un recours efficace pour relever ces défis consiste à partitionner une application en composants discrètes et faiblement couplées qui peuvent être facilement intégrés ensemble dans une application. Une telle approche présente plusieurs avantages :

-   Il permet des fonctionnalités individuelles à être développé, testé, étendu et gérées par différents utilisateurs ou équipes.
-   Elle favorise la réutilisation et une séparation claire des responsabilités entre les fonctionnalités horizontal de l’application, telles que l’authentification et l’accès aux données et les fonctionnalités verticales, telles que des fonctionnalités de l’application métier spécifique. Ainsi, les dépendances et les interactions entre les composants d’application à gérer plus facilement.
-   Il permet de maintenir une séparation des rôles en permettant à différents utilisateurs ou équipes, pour vous concentrer sur une tâche spécifique ou une partie des fonctionnalités en fonction de leur expertise. En particulier, il fournit une séparation claire entre l’interface utilisateur et la logique métier de l’application.

Toutefois, il existe de nombreux problèmes qui doivent être résolus lors du partitionnement d’une application en composants discrètes et faiblement couplées. Elles incluent notamment :

-   Décider comment fournir une séparation claire des responsabilités entre les contrôles d’interface utilisateur et leur logique. Une des décisions plus importantes lors de la création d’une application d’entreprise Xamarin.Forms est de placer la logique métier dans les fichiers code-behind, ou s’il faut créer une séparation claire des responsabilités entre les contrôles d’interface utilisateur et leur logique, pour rendre l’application plus facile à gérer et faire l’objet. Pour plus d’informations, consultez [Model-View-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
-   Déterminer s’il faut utiliser un conteneur d’injection de dépendance. Conteneurs d’injection de dépendance limitent la dépendance entre les objets en fournissant une fonctionnalité pour construire des instances de classes avec leurs dépendances injectés de couplage et de gérer leur durée de vie basée sur la configuration du conteneur. Pour plus d’informations, consultez [Injection de dépendance](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
-   Choix entre les événements de plateforme fourni et faiblement couplées basée sur message la communication entre les composants qui n’est pas pratique lier par objet et références de type. Pour plus d’informations, consultez Introduction aux [communiquer entre faiblement couplés composants](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
-   Décider comment naviguer entre les pages, y compris l’appel de navigation, et l’emplacement de la logique de navigation. Pour plus d’informations, consultez [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).
-   Déterminer comment valider l’entrée d’utilisateur est correcte. La décision doit inclure comment valider l’entrée d’utilisateur et comment avertir l’utilisateur sur les erreurs de validation. Pour plus d’informations, consultez [Validation](~/xamarin-forms/enterprise-application-patterns/validation.md).
-   Décider comment effectuer l’authentification et comment protéger les ressources disposant d’autorisations. Pour plus d’informations, consultez [l’authentification et l’autorisation](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
-   Détermination de l’accès aux données distantes à partir du web services, y compris comment récupérer des données de manière fiable et comment mettre en cache des données. Pour plus d’informations, consultez [l’accès à des données distantes](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
-   Décider comment tester l’application. Pour plus d’informations, consultez [exécuter des tests unitaires](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Ce guide fournit des conseils sur ces problèmes et se concentre sur les modèles de base et l’architecture pour la création d’une application multiplateforme enterprise à l’aide de Xamarin.Forms. Le guide vise à aider à produire du code adaptable, facile à gérer et faire l’objet, en traitant les scénarios de développement courants Xamarin.Forms enterprise application et en séparant les problèmes de présentation, logique de présentation et d’entités via la prise en charge de la Modèle de Model-View-ViewModel (MVVM).

## <a name="sample-application"></a>Exemple d’Application

Ce guide comprend un exemple d’application, eShopOnContainers, qui est un magasin en ligne qui inclut les fonctionnalités suivantes :

-   Authentification et d’autorisation contre un service principal.
-   Parcourir un catalogue de shirts tasses de café et autres éléments de marketing.
-   Le filtrage du catalogue.
-   Classement des éléments à partir du catalogue.
-   Affichage de l’historique des commandes de l’utilisateur.
-   Configuration des paramètres.

### <a name="sample-application-architecture"></a>Architecture de l’Application exemple

Figure 1-1 fournit une vue d’ensemble de l’architecture de l’exemple d’application.

![](introduction-images/architecture.png "architecture de haut niveau eShopOnContainers")

**Figure 1-1**: architecture de haut niveau eShopOnContainers

L’exemple d’application est fourni avec trois applications de client :

-   Une application MVC développées avec ASP.NET Core.
-   Une Application à Page unique (SPA) développées avec 2 angulaire et Typescript. Cette approche pour les applications web évite d’effectuer un aller-retour sur le serveur à chaque opération.
-   Une application mobile développé avec Xamarin.Forms, qui prend en charge d’iOS, Android et la plateforme Windows universelle (UWP).

Pour plus d’informations sur les applications web, consultez [création d’une architecture et de développer des Applications Web modernes avec ASP.NET Core et Microsoft Azure](http://aka.ms/WebAppEbook).

L’exemple d’application inclut les services principaux suivants :

-   Microservice d’identité, qui utilise ASP.NET Core Identity et IdentityServer.
-   Un catalogue microservice, qui est piloté par les données de créer, lire, mettre à jour, supprimer (CRUD) des services qui utilise une base de données SQL Server à l’aide de EntityFramework Core.
-   Un tri microservice, qui est un service de domaine qui utilise des modèles de conception de domaine.
-   Microservice du panier d’achat, qui est un service CRUD piloté par les données qui utilise le Cache Redis.

Ces services principaux sont implémentés en tant que microservices à l’aide d’ASP.NET MVC de base et sont déployés en tant que conteneurs uniques au sein d’un seul hôte Docker. Ces services principaux sont collectivement aux eShopOnContainers référence d’application. Les applications clientes communiquent avec les services principaux via une interface web de transfert REST (Representational State). Pour plus d’informations sur microservices et Docker, consultez [placées dans des conteneurs de Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Pour plus d’informations sur l’implémentation de services principaux, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Application mobile

Ce guide se concentre sur la création d’applications inter-plateformes enterprise à l’aide de Xamarin.Forms et utilise l’application mobile eShopOnContainers comme exemple. Figure 1-2 affiche les pages de l’application mobile eShopOnContainers qui fournissent les fonctionnalités décrites précédemment.

[![](introduction-images/screenshots.png "L’application mobile eShopOnContainers")](introduction-images/screenshots-large.png#lightbox "l’application mobile eShopOnContainers")

**Figure 1-2**: l’application mobile eShopOnContainers

L’application mobile utilise les services principaux fournies par l’application de référence eShopOnContainers. Toutefois, il peut être configuré pour consommer des données à partir des services fictives pour ceux qui souhaitent éviter de déployer les services principaux.

L’application mobile eShopOnContainers exerce les fonctionnalités Xamarin.Forms suivantes :

-   XAML
-   Contrôles
-   Liaisons
-   Convertisseurs de
-   Styles
-   Animations
-   Commandes
-   comportements
-   Déclencheurs
-   Effects (Effets)
-   Convertisseurs personnalisés
-   MessagingCenter
-   Contrôles personnalisés

Pour plus d’informations sur cette fonctionnalité, consultez le [Xamarin.Forms documentation](~/xamarin-forms/index.yml), et [créer des applications mobiles avec Xamarin.Forms](https://aka.ms/xamebook).

En outre, les tests unitaires sont fournies pour certaines des classes dans l’application mobile eShopOnContainers.

#### <a name="mobile-app-solution"></a>Solution d’application mobile

La solution d’application mobile eShopOnContainers organise le code source et autres ressources dans les projets. Tous les projets utiliseront des dossiers pour organiser le code source et autres ressources en catégories. Le tableau suivant présente les projets qui composent l’application mobile eShopOnContainers :

|Projet|Description|
|--- |--- |
|eShopOnContainers.Core|Ce projet est le projet de bibliothèque (PCL) de classes portables qui contient le code partagé et l’interface utilisateur partagée.|
|eShopOnContainers.Droid|Ce projet conserve du code spécifique Android et est le point d’entrée pour l’application Android.|
|eShopOnContainers.iOS|Ce projet conserve iOS code spécifique et est le point d’entrée de l’application iOS.|
|eShopOnContainers.UWP|Ce projet conserve du code spécifique de plateforme Windows universelle (UWP) et point d’entrée pour l’application Windows.|
|eShopOnContainers.TestRunner.Droid|Ce projet est Android test runner pour le projet eShopOnContainers.UnitTests.|
|eShopOnContainers.TestRunner.iOS|Ce projet est iOS test runner pour le projet eShopOnContainers.UnitTests.|
|eShopOnContainers.TestRunner.Windows|Ce projet est le testeur de plateforme Windows universelle pour le projet eShopOnContainers.UnitTests.|
|eShopOnContainers.UnitTests|Ce projet contient des tests unitaires pour le projet eShopOnContainers.Core.|

Les classes de l’application mobile eShopOnContainers peuvent être réutilisées dans n’importe quelle application Xamarin.Forms avec peu ou aucune modification.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core Project

Le projet de bibliothèque de classes portables eShopOnContainers.Core contient les dossiers suivants :

|Dossier|Description|
|--- |--- |
|Animations|Contient des classes qui permettent d’animations à être consommés en XAML.|
|comportements|Contient les comportements qui sont exposées pour l’affichage de classes.|
|Contrôles|Contient des contrôles personnalisés utilisés par l’application.|
|Convertisseurs de|Contient les convertisseurs de valeurs qui s’appliquent une logique personnalisée à une liaison.|
|Effects (Effets)|Contient le `EntryLineColorEffect` (classe), qui est utilisé pour modifier la couleur de bordure de spécifique `Entry` contrôles.|
|Exceptions|Contenant le custom `ServiceAuthenticationException`.|
|Extensions|Contient des méthodes d’extension pour le `VisualElement` et `IEnumerable` classes.|
|Programmes d’assistance|Contient des classes d’assistance pour l’application.|
|Modèles|Contient les classes de modèle pour l’application.|
|Propriétés|Contient `AssemblyInfo.cs`, un fichier de métadonnées d’assembly .NET.|
|Services|Contient des interfaces et des classes qui implémentent les services qui sont fournis à l’application.|
|Déclencheurs|Contient le `BeginAnimation` déclencheur, lequel est utilisé pour appeler une animation en XAML.|
|Validations|Contient des classes impliquées dans la validation de l’entrée de données.|
|ViewModels|Contient la logique d’application qui est exposée à des pages.|
|Affichages|Contient les pages de l’application.|

##### <a name="platform-projects"></a>Projets de plateforme

Les projets de plateforme contient les implémentations d’effet, les implémentations de convertisseur personnalisé et autres ressources spécifiques à la plateforme.

## <a name="summary"></a>Récapitulatif

Plateformes et les outils de développement d’application mobile multiplateforme Xamarin fournissent une solution complète pour les clients mobiles B2C, B2B et B2E applications, en offrant la possibilité de partager le code entre toutes les plateformes cibles (iOS, Android et Windows) et qui contribue à réduire le coût total de possession. Les applications peuvent partager leur code de la logique utilisateur interface et l’application, tout en conservant l’apparence plateforme native.

Les développeurs d’applications d’entreprise sont confrontés à plusieurs problèmes qui peuvent modifier l’architecture de l’application pendant le développement. Par conséquent, il est important de créer une application afin qu’il peut être modifié ou étendu au fil du temps. La conception pour une telle capacité d’adaptation peut être difficile, mais implique généralement le partitionnement d’une application en composants discrètes et faiblement couplées qui peuvent être facilement intégrés ensemble dans une application.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
