---
title: Techniques de Backgrounding iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b623e9a13c7b4fd3854ee6a144d6401720d9ba8c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-backgrounding-techniques"></a>Techniques de Backgrounding iOS

Dans les sections suivantes, nous allons examiner les fonctionnalités suivantes d’iOS en même temps que les options backgrounding existantes :

-  [Les tâches en arrière-plan opportuniste](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -préserver l’autonomie en exécutant des tâches en arrière-plan en segments opportunistes lorsque l’appareil est mis en éveil pour le traitement des autres.
-  [Service de transfert en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - fiable télécharger des fichiers, quelle que soit la taille du réseau de l’état ou du fichier.
-  [Extraction d’arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -actualiser une application à partir de l’arrière-plan des intervalles déterminés par le système.
-  [Notifications à distance](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -utilisez les notifications push pour déclencher des mises à jour en arrière-plan avant que l’utilisateur ouvre l’application, avec une option pour avertir l’utilisateur ou de mettre à jour en mode silencieux.
-  **Mises à jour de l’interface utilisateur d’arrière-plan** - préparer l’application de l’interface utilisateur pour l’utilisateur et mettre à jour d’instantané de l’application, à partir de l’arrière-plan.
