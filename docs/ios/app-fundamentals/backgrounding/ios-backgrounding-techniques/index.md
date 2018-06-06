---
title: Techniques de Backgrounding iOS
description: 'Ce document des liens vers des guides qui décrivent les différentes techniques backgrounding dans iOS : les tâches en arrière-plan, service de transfert en arrière-plan, extraction en arrière-plan et des notifications à distance.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ebf3c07a319a79994093f89f8e54f4cba7402533
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783753"
---
# <a name="ios-backgrounding-techniques"></a>Techniques de Backgrounding iOS

Dans les sections suivantes, nous allons examiner les fonctionnalités suivantes d’iOS en même temps que les options backgrounding existantes :

-  [Les tâches en arrière-plan opportuniste](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -préserver l’autonomie en exécutant des tâches en arrière-plan en segments opportunistes lorsque l’appareil est mis en éveil pour le traitement des autres.
-  [Service de transfert en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - fiable télécharger des fichiers, quelle que soit la taille du réseau de l’état ou du fichier.
-  [Extraction d’arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -actualiser une application à partir de l’arrière-plan des intervalles déterminés par le système.
-  [Notifications à distance](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -utilisez les notifications push pour déclencher des mises à jour en arrière-plan avant que l’utilisateur ouvre l’application, avec une option pour avertir l’utilisateur ou de mettre à jour en mode silencieux.
-  **Mises à jour de l’interface utilisateur d’arrière-plan** - préparer l’application de l’interface utilisateur pour l’utilisateur et mettre à jour d’instantané de l’application, à partir de l’arrière-plan.
