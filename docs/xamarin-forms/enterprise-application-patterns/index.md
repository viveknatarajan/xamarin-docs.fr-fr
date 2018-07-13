---
title: Modèles d’Application d’entreprise à l’aide de livre électronique Xamarin.Forms
description: Ce livre électronique fournit des conseils architecturaux pour le développement d’applications d’entreprise Xamarin.Forms adaptables, testables et facile à gérer.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ecfe99f66e16eafabc3117036ff065e3a35259c3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994346"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Modèles d’Application d’entreprise à l’aide de livre électronique Xamarin.Forms

_Guide architectural complet pour le développement d’applications d’entreprise Xamarin.Forms adaptables, testables et facile à gérer_

![](images/cover-sml.png "Modèles d’Application d’entreprise à l’aide de livre électronique Xamarin.Forms")

Ce livre électronique fournit des conseils sur la façon d’implémenter le modèle Model-View-ViewModel (MVVM), l’injection de dépendances, la navigation, validation et gestion de la configuration, tout en conservant un couplage faible. En outre, il existe également des conseils sur l’exécution de l’authentification et autorisation avec IdentityServer, l’accès aux données à partir de microservices en conteneur et les tests unitaires.

## <a name="prefaceprefacemd"></a>[Préface](preface.md)

Ce chapitre explique l’objectif et la portée de ce guide, et qui il est destiné à.

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Les développeurs d’applications d’entreprise sont confrontés à plusieurs difficultés pouvant modifier l’architecture de l’application pendant le développement. Par conséquent, il est important créer une application afin qu’il peut être modifié ou étendu au fil du temps. Conception pour une telle capacité d’adaptation peut s’avérer difficile, mais implique généralement le partitionnement d’une application en composants discrètes, faiblement couplés qui peuvent être facilement intégrés ensemble dans une application.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Le modèle Model-View-ViewModel (MVVM) permet de séparer clairement la logique métier et de présentation d’une application à partir de son interface utilisateur (IU). Maintenir une séparation nette entre la logique d’application et l’interface utilisateur vous aide à résoudre de nombreux problèmes liés au développement et peut rendre plus faciles à tester, gérer et faire évoluer une application. Il peut améliorer considérablement les opportunités de réutilisation de code et permet aux développeurs et concepteurs d’interfaces utilisateur plus collaborent aisément lors du développement de leurs éléments respectifs d’une application.

## <a name="dependency-injectiondependency-injectionmd"></a>[Injection de dépendances](dependency-injection.md)

L’injection de dépendances permet de découplage des types concrets à partir du code qui dépend de ces types. En règle générale, il utilise un conteneur qui contient une liste d’inscriptions et les mappages entre les types et interfaces abstraits et les types concrets qui implémentent ou étendent ces types.

Conteneurs d’injection de dépendance réduisent le couplage entre les objets en fournissant une fonctionnalité pour instancier des instances de classe et de gérer leur durée de vie basée sur la configuration du conteneur. Lors de la création d’objets, le conteneur injecte des dépendances nécessitant l’objet dedans. Si ces dépendances n’ont pas encore été créés, le conteneur crée et résout d’abord les leurs dépendances.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Communication entre les composants faiblement couplés](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) classe implémente la publier-s’abonner de modèle, autorisant basée sur message de communication entre les composants qui n’est pas pratique lier par des références d’objet et le type. Ce mécanisme permet des éditeurs et les abonnés de communiquer sans avoir une référence à d’autres, qui contribue à réduire les dépendances entre les composants, tout en autorisant également les composants soient indépendamment développées et testées.

## <a name="navigationnavigationmd"></a>[Navigation](navigation.md)

Xamarin.Forms inclut la prise en charge de la navigation entre les pages, ce qui se traduit généralement à partir de l’interaction utilisateur avec l’interface utilisateur, ou à partir de l’application elle-même, résultant des modifications d’état pilotés par logique interne. Toutefois, la navigation peut être complexe à implémenter dans les applications qui utilisent le modèle MVVM.

Ce chapitre présente un `NavigationService` (classe), qui est utilisé pour effectuer la navigation model first à partir de modèles de vue. Placer la logique de navigation dans l’affichage de classes de modèle signifie que la logique peut être levée par le biais des tests automatisés. En outre, le modèle de vue peut ensuite implémenter une logique pour contrôler la navigation pour vous assurer que certaines règles commerciales sont appliquées.

## <a name="validationvalidationmd"></a>[Validation](validation.md)

N’importe quelle application qui accepte les entrées d’utilisateurs doit garantir que l’entrée est valide. Sans validation, un utilisateur peut fournir des données d’origine de l’application en échec. Validation applique des règles d’entreprise et empêche une personne malveillante d’injecter des données malveillantes.

Dans le contexte de modèle de modèle ViewModel (MVVM) de modèle, un modèle de vue ou modèle souvent devront effectuer la validation de données et de signaler des erreurs de validation à la vue afin que l’utilisateur pour les corriger.

## <a name="configuration-managementconfiguration-managementmd"></a>[Gestion de la configuration](configuration-management.md)

Les paramètres permettent la séparation des données qui configure le comportement d’une application à partir du code, ce qui permet le comportement d’être modifiée sans reconstruction de l’application. Paramètres d’application sont des données qu’une application crée et gère et paramètres utilisateur sont les paramètres personnalisables d’une application qui affectent le comportement de l’application et ne nécessitent pas ré-ajustement fréquente.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Microservices en conteneur](containerized-microservices.md)

Les Microservices offrent une approche de développement d’applications et de déploiement qui est adaptée aux exigences d’agilité, mise à l’échelle et la fiabilité des applications cloud modernes. Un des principaux avantages de microservices est qu’elles peuvent être montées indépendamment, ce qui signifie qu’une zone fonctionnelle spécifique peut être augmenté ou nécessitant plus le traitement d’alimentation de bande passante réseau pour prendre en charge à la demande, sans augmenter inutilement des zones de l’application qui ne rencontre pas d’augmentation de la demande.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Authentification et autorisation](authentication-and-authorization.md)

Il existe plusieurs approches pour l’intégration de l’authentification et autorisation dans une application Xamarin.Forms qui communique avec une application web ASP.NET MVC. Ici, l’authentification et l’autorisation sont effectuées avec un microservice d’identité en conteneur qui utilise 4 d’IdentityServer. IdentityServer est une infrastructure open source de OAuth 2.0 et OpenID Connect pour ASP.NET Core qui s’intègre avec ASP.NET Core Identity pour effectuer l’authentification du jeton du porteur.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Accès aux données distantes](accessing-remote-data.md)

Assurez-vous de nombreuses solutions modernes basée sur le web utilisation des services web, hébergés par des serveurs web, pour fournir des fonctionnalités pour le client à distance des applications. Les opérations qui expose un service web constituent une API web et les applications clientes doivent être en mesure d’utiliser l’API web sans connaître la façon dont les données ou les opérations qui expose l’API sont implémentées.

## <a name="unit-testingunit-testingmd"></a>[Tests unitaires](unit-testing.md)

Test des modèles et des modèles de vue à partir des applications MVVM est identique au test de toutes les autres classes et les mêmes outils et techniques peuvent être utilisés. Toutefois, il existe quelques modèles qui typiques d’un modèle et les classes de modèle de vue, qui peuvent tirer parti des techniques de test d’unité spécifique.

## <a name="feedback"></a>Commentaires

Ce projet a un site communautaire, sur lequel vous pouvez poser des questions et fournir des commentaires. Le site de la Communauté se trouve sur [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Vous pouvez également des commentaires sur le livre électronique peuvent être envoyé par courrier électronique à [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF de 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (exemple)](https://github.com/dotnet-architecture/eShopOnContainers)
