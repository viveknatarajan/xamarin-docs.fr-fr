---
title: Modèles d’Application d’entreprise à l’aide de livre électronique de Xamarin.Forms
description: Directives architecturales pour développer des applications d’entreprise Xamarin.Forms adaptables, testables et facile à gérer
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: b7f69e987eb4f628dfaf5422bc1ce52108d90634
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Modèles d’Application d’entreprise à l’aide de livre électronique de Xamarin.Forms

_Directives architecturales pour développer des applications d’entreprise Xamarin.Forms adaptables, testables et facile à gérer_

![](images/cover-sml.png "Modèles d’Application d’entreprise à l’aide de livre électronique de Xamarin.Forms")

Ce livre électronique fournit des conseils sur la façon d’implémenter le modèle Model-View-ViewModel (MVVM), l’injection de dépendances, la navigation, validation et gestion de la configuration, tout en conservant un couplage faible. En outre, il existe également des conseils sur l’exécution de l’authentification et autorisation avec IdentityServer, l’accès aux données à partir de microservices en conteneur et le test unitaire.

## <a name="prefaceprefacemd"></a>[Préface](preface.md)

Ce chapitre explique l’objectif et la portée de ce guide, et qui il est destiné à.

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Les développeurs d’applications d’entreprise sont confrontés à plusieurs problèmes qui peuvent modifier l’architecture de l’application pendant le développement. Par conséquent, il est important de créer une application afin qu’il peut être modifié ou étendu au fil du temps. La conception pour une telle capacité d’adaptation peut être difficile, mais implique généralement le partitionnement d’une application en composants discrètes et faiblement couplées qui peuvent être facilement intégrés ensemble dans une application.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Le modèle Model-View-ViewModel (MVVM) permet de séparer clairement la logique métier et de présentation d’une application à partir de son interface utilisateur (IU). Maintenir une séparation nette entre la logique d’application et l’interface utilisateur permettant de résoudre de nombreux problèmes de développement et peut rendre plus faciles à tester, gérer et faire évoluer une application. Elle peut améliorer considérablement les possibilités de réutilisation de code et permet aux développeurs et concepteurs de l’interface utilisateur à effectuer davantage de collaborent aisément lors du développement de leurs éléments respectifs d’une application.

## <a name="dependency-injectiondependency-injectionmd"></a>[Injection de dépendances](dependency-injection.md)

Injection de dépendances permet de découpler de types concrets à partir du code qui dépend de ces types. Il utilise généralement un conteneur qui contient une liste des inscriptions et des mappages entre les interfaces et les types abstraits et les types concrets qui implémentent ou étendent ces types.

Conteneurs d’injection de dépendance de réduire le couplage entre les objets en fournissant une fonctionnalité pour instancier des instances de classe et de gérer leur durée de vie basée sur la configuration du conteneur. Lors de la création d’objets, le conteneur injecte toutes les dépendances de l’objet requiert dans celui-ci. Si ces dépendances n’ont pas encore été créés, le conteneur crée et résout tout d’abord de leurs dépendances.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Communication entre les composants faiblement couplés](communicating-between-loosely-coupled-components.md)

Le Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe implémente la publication-modèle, autorisant basée sur message la communication entre les composants qui n’est pas pratique lier par objet et références de type abonnement. Ce mécanisme permet les éditeurs et les abonnés communiquer sans avoir une référence à d’autres, afin de réduire les dépendances entre les composants, en tenant également compte des composants développés et testés indépendamment.

## <a name="navigationnavigationmd"></a>[Navigation](navigation.md)

Xamarin.Forms inclut la prise en charge de la navigation entre les pages, ce qui se produit généralement à partir de l’interaction utilisateur avec l’interface utilisateur, ou à partir de l’application elle-même, à la suite de modifications de l’état interne de piloté par la logique. Toutefois, la navigation peut être complexe à implémenter dans les applications qui utilisent le modèle MVVM.

Ce chapitre présente un `NavigationService` (classe), qui est utilisé pour exécuter le modèle d’abord la navigation à partir de modèles d’affichage. Placer la logique de la navigation dans l’affichage de classes de modèle signifie que la logique peut être levée par le biais des tests automatisés. En outre, le modèle d’affichage peut ensuite implémenter la logique pour contrôler la navigation pour vous assurer que certaines règles d’entreprise sont appliquées.

## <a name="validationvalidationmd"></a>[Validation](validation.md)

N’importe quelle application qui accepte les entrées d’utilisateurs doit s’assurer que l’entrée est valide. Sans validation, un utilisateur peut fournir des données qui entraîne l’échec de l’application. Validation applique des règles d’entreprise et empêche une personne malveillante d’injecter des données malveillantes.

Dans le contexte de modèle ViewModel (MVVM) de modèle, un modèle d’affichage ou modèle sera souvent requise pour effectuer la validation des données et de signaler des erreurs de validation à la vue afin que l’utilisateur de les corriger.

## <a name="configuration-managementconfiguration-managementmd"></a>[Gestion de la configuration](configuration-management.md)

Les paramètres permettent la séparation des données qui configure le comportement d’une application à partir du code, ce qui permet le comportement d’être modifiée sans la reconstruction de l’application. Paramètres d’application sont des données qu’une application crée et gère et paramètres utilisateur sont les paramètres personnalisables d’une application qui affectent le comportement de l’application et n’ont pas besoin de fréquent ajustement de nouveau.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Microservices en conteneur](containerized-microservices.md)

Microservices offrent une approche pour le développement d’applications et de déploiement qui convient aux exigences de flexibilité, l’échelle et la fiabilité des applications cloud modernes. Un des principaux avantages de microservices est qu’ils peuvent être montés en indépendamment, ce qui signifie qu’un domaine fonctionnel spécifique peut être monté en nécessitant plus le traitement d’alimentation de bande passante réseau pour prendre en charge à la demande, sans mise à échelle inutilement des zones de l’application qui ne rencontre pas d’augmentation de la demande.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Authentification et autorisation](authentication-and-authorization.md)

Il existe plusieurs approches pour l’intégration de l’authentification et autorisation dans une application de Xamarin.Forms qui communique avec une application de web ASP.NET MVC. Ici, l’authentification et autorisation sont effectuées avec une microservice d’identité en conteneur qui utilise IdentityServer 4. IdentityServer est une infrastructure open source qui OpenID Connect et OAuth 2.0 pour ASP.NET Core qui s’intègre avec ASP.NET Core Identity pour effectuer une authentification de jeton de support.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Accès aux données distantes](accessing-remote-data.md)

Assurez-vous de nombreuses solutions modernes basée sur le web utilisation des services web, hébergées par les serveurs web, pour fournir des fonctionnalités de client à distance des applications. Les opérations exposées par un service web constituent une API web et les applications clientes doivent être en mesure d’utiliser l’API web sans savoir comment les données ou les opérations qui expose de l’API sont implémentées.

## <a name="unit-testingunit-testingmd"></a>[Tests unitaires](unit-testing.md)

Test de modèles et afficher les modèles à partir d’applications de MVVM est identique à d’autres classes de test et les mêmes outils et techniques qui peuvent être utilisés. Toutefois, il existe certains modèles sont classiques au modèle et les classes de modèle de vue, qui peuvent tirer parti des techniques de test unitaire spécifique.

## <a name="feedback"></a>Commentaires

Ce projet est un site de la Communauté, sur lequel vous pouvez poser des questions et fournir des commentaires. Le site de la Communauté se trouve sur [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Vous pouvez également des commentaires sur le livre électronique peuvent être envoyé par courrier électronique [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
