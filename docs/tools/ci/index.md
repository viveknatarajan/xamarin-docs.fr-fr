---
title: Introduction à l’intégration continue avec Xamarin
description: Ce document contient des liens vers des guides qui décrivent l’intégration continue avec Xamarin. Contenu lié fournit une vue d’ensemble de l’intégration continue et traite des Application Center Build TeamCity et Jenkins.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: lobrien
ms.author: laobri
ms.date: 10/23/2018
ms.openlocfilehash: 073fc5abace2e0cb923394a359437528f703f338
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61371077"
---
# <a name="continuous-integration-with-xamarin"></a>Intégration continue avec Xamarin

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Introduction à l’intégration continue](~/tools/ci/intro-to-ci.md)

Cette section décrit les différents composants impliqués dans l’intégration continue et leurs relations. Il décrit les environnements d’intégration continue qui sont abordées dans les sections spécifiques ci-dessous.

## <a name="devops-with-xamarintoolscidevopsmd"></a>[DevOps avec Xamarin](~/tools/ci/devops.md)

Cette section identifie les fonctionnalités de DevOps dans Azure et Visual Studio, vous pouvez vous attendre pour fonctionner avec un projet Xamarin.

## <a name="working-with-continuous-integration-environments"></a>Utilisation d’environnements d’intégration continue

### <a name="build-xamarin-apps-with-azure-pipelineshttpsdocsmicrosoftcomazuredevopspipelineslanguagesxamarin"></a>[Créer des applications Xamarin avec les Pipelines Azure](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Les Pipelines Azure permet de générer automatiquement des applications Xamarin pour Android et iOS.

### <a name="build-xamarin-apps-using-app-centerhttpsdocsmicrosoftcomappcenterbuildxamarin"></a>[Créer des applications Xamarin à l’aide d’App Center](https://docs.microsoft.com/appcenter/build/xamarin/)

Créer des solutions de Xamarin.iOS et Xamarin.Android avec App Center, directement à partir de GitHub, Azure DevOps ou Bitbucket.

### <a name="build-xamarin-apps-with-teamcitytoolsciteamcitymd"></a>[Créer des applications Xamarin avec TeamCity](~/tools/ci/teamcity.md)

Ce guide décrit les étapes nécessaires à l’utilisation de TeamCity pour compiler des applications mobiles et envoyez-les à App Center Test.

### <a name="build-xamarin-apps-with-jenkinstoolscijenkins-walkthroughmd"></a>[Créer des applications Xamarin avec Jenkins](~/tools/ci/jenkins-walkthrough.md)

Ce guide explique comment configurer Jenkins en tant qu’un serveur d’intégration continue et d’automatiser la compilation d’applications mobiles créées avec Xamarin. Il décrit comment installer Jenkins sur OS X, configurez-le et paramétrer des tâches pour compiler des applications Xamarin.iOS et Xamarin.Android lorsque les modifications sont validées dans le système de contrôle de version.
