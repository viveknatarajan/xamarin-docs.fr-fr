---
title: Techniques de backgrounding iOS
description: 'Ce document des liens vers des guides qui décrivent les différentes techniques de backgrounding dans iOS : tâches en arrière-plan, de service de transfert en arrière-plan, de récupération en arrière-plan et de notifications à distance.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 00cca0c75cc79858f6edda5d6fb954611d81161b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169604"
---
# <a name="ios-backgrounding-techniques"></a>Techniques de backgrounding iOS

Dans les sections suivantes, nous explorerons les fonctionnalités suivantes d’iOS en même temps que les options de backgrounding existantes :

-  [Les tâches en arrière-plan opportuniste](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -préserver l’autonomie en exécutant des tâches en arrière-plan dans des segments opportunistes quand l’appareil est éveillé pour d’autres traitements.
-  [Service de transfert en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - fiable télécharger des fichiers, quel que soit la taille de fichier ou d’état du réseau.
-  [En arrière-plan Fetch](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -actualiser une application à partir de l’arrière-plan à intervalles déterminées par le système.
-  [Notifications à distance](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -utilisez les notifications push pour déclencher des mises à jour en arrière-plan avant que l’utilisateur ouvre l’application, avec une option pour informer l’utilisateur ou de mettre à jour en mode silencieux.
-  **En arrière-plan mises à jour de l’interface utilisateur** - préparer l’application de l’interface utilisateur pour l’utilisateur et mettre à jour de la capture instantanée de l’application, tout à partir de l’arrière-plan.
