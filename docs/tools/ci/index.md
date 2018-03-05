---
title: "Introduction à l’intégration continue avec Xamarin"
description: "Intégration continue est une pratique de l’ingénierie logicielle dans laquelle une génération automatique compile et teste éventuellement une application lorsque le code est ajouté ou modifié par les développeurs dans le référentiel de contrôle de version du projet. Cet article présente les concepts généraux de l’intégration continue et certaines des options disponibles pour l’intégration continue avec les projets de Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: c28389479148829ee87eeda307915aacf7007b21
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Introduction à l’intégration continue avec Xamarin

_Intégration continue est une pratique de l’ingénierie logicielle dans laquelle une génération automatique compile et teste éventuellement une application lorsque le code est ajouté ou modifié par les développeurs dans le référentiel de contrôle de version du projet. Cet article présente les concepts généraux de l’intégration continue et certaines des options disponibles pour l’intégration continue avec les projets de Xamarin._

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]


##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Introduction à l’intégration continue](~/tools/ci/intro-to-ci.md)

Cette section aborde les différents composants impliqués dans l’intégration continue et leurs relations. Elle décrit les environnements d’intégration continue qui sont décrites dans les sections spécifiques ci-dessous.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>Utilisation des environnements d’intégration continue


### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[Utilisation de build App Center avec Xamarin](/appcenter/build/xamarin/)

Créer des solutions de Xamarin.iOS et Xamarin.Android avec Application Center, directement à partir de GitHub ou Bitbucket VSTS.

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[Utilisation de TeamCity avec Xamarin](~/tools/ci/teamcity.md)

Ce guide décrit les étapes à l’aide de TeamCity pour compiler des applications mobiles et puis de les envoyer à Xamarin Test Cloud.

###  <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[Utilisation de Jenkins avec Xamarin](~/tools/ci/jenkins-walkthrough.md)

Ce guide illustre comment configurer Jenkins comme un serveur d’intégration continue et automatiser la compilation des applications mobiles créées avec Xamarin. Il décrit comment installer Jenkins sur OS X, configurez-le et configurer les travaux de compilation des applications Xamarin.iOS et Xamarin.Android lorsque des modifications sont validées dans le système de contrôle de version.

