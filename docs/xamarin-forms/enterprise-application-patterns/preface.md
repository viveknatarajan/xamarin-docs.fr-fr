---
title: Introduction au développement d’applications Enterprise
description: Ce chapitre fournit une préface aux modèles d’Application d’entreprise à l’aide de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fd085d2fb12e82233f6d3be2e2773a84539f837b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242366"
---
# <a name="preface-to-enterprise-app-development"></a>Introduction au développement d’applications Enterprise

Ce livre électronique fournit des conseils sur la création d’applications inter-plateformes enterprise à l’aide de Xamarin.Forms. Xamarin.Forms est un kit de ressources de l’interface utilisateur multiplateforme qui permet aux développeurs de créer facilement des dispositions d’interface qui peuvent être partagées entre les plateformes, notamment iOS, Android et la plateforme Windows universelle (UWP) utilisateur natives. Il fournit une solution complète pour Business à employé (B2E), interentreprises (B2B) et d’entreprise pour les applications consommateur (B2C), en offrant la possibilité de partager du code sur toutes les plateformes cibles et de contribuer à réduire le coût total de possession (TCO).

Le guide fournit des directives architecturales pour le développement d’applications d’entreprise Xamarin.Forms adaptables, facile à gérer et faire l’objet. Est des conseils sur l’implémentation de MVVM, injection de dépendance, la navigation, validation et gestion de la configuration, tout en conservant un couplage faible. En outre, il existe également des conseils sur l’exécution de l’authentification et autorisation avec IdentityServer, l’accès aux données à partir de microservices en conteneur et le test unitaire.

Le guide est fourni avec le code source pour le [eShopOnContainers des applications mobiles](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)et le code source pour le [eShopOnContainers référence application](https://github.com/dotnet-architecture/eShopOnContainers). L’application mobile eShopOnContainers est une application multiplateforme entreprise développée à l’aide de Xamarin.Forms, qui se connecte à une série de microservices en conteneur appelé les eShopOnContainers référencer l’application. Toutefois, l’application mobile eShopOnContainers peut être configurée pour utiliser les données à partir des services fictives pour ceux qui souhaitent éviter de déployer le microservices en conteneur.

## <a name="whats-left-out-of-this-guides-scope"></a>Reste hors de portée de ce Guide

Ce guide est destiné aux lecteurs qui sont déjà familiarisés avec Xamarin.Forms. Pour obtenir une présentation détaillée de Xamarin.Forms, consultez la [Xamarin.Forms documentation](~/xamarin-forms/index.yml), et [créer des applications mobiles avec Xamarin.Forms](https://aka.ms/xamebook).

Le guide est complémentaire à [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook), qui se concentre sur le développement et le déploiement de microservices en conteneur. Incluent d’autres guides de valeur de la lecture [création d’une architecture et de développer des Applications Web modernes avec ASP.NET Core et Microsoft Azure](http://aka.ms/WebAppEbook), [placées dans des conteneurs Docker cycle de vie Application avec Microsoft Platform et outils](http://aka.ms/dockerlifecycleebook), et [Microsoft Platform et des outils de développement d’applications mobiles](http://aka.ms/MobAppDev/StndPDF).

## <a name="who-should-use-this-guide"></a>Qui doit utiliser ce Guide

Le public de ce guide est principalement les développeurs et les architectes qui souhaitent apprendre à concevoir et implémenter des applications multiplateformes enterprise à l’aide de Xamarin.Forms.

Un secondaire s’adresse aux décideurs techniques qui souhaitent recevoir une vue d’ensemble de l’architecture et la technologie avant de décider quelle approche à sélectionner pour le développement d’applications inter-plateformes enterprise à l’aide de Xamarin.Forms.

## <a name="how-to-use-this-guide"></a>L’utilisation de ce Guide

Ce guide se concentre sur la création d’applications inter-plateformes enterprise à l’aide de Xamarin.Forms. Par conséquent, il doit être lu dans son intégralité afin de fournir une base de présentation de ces applications et leurs considérations techniques. Le guide, ainsi que son application d’exemple, peut également servir d’un point de départ ou une référence pour la création d’une nouvelle application d’entreprise. Utilisez l’application d’exemple associés en tant que modèle pour la nouvelle application, ou pour savoir comment organiser les composants d’une application. Ensuite, font référence à ce guide pour obtenir des conseils architecturaux.

N’hésitez pas à transférer de ce guide pour les membres de l’équipe pour garantir une compréhension commune inter-plateformes enterprise du développement d’applications à l’aide de Xamarin.Forms. Tout le monde à partir d’un ensemble commun de terminologie et sous-jacent principes ayant afin de garantir une application cohérente des modèles architecturaux et pratiques.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (exemple)](https://github.com/dotnet-architecture/eShopOnContainers)
