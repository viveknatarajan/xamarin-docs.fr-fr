---
title: "Fonctionnalités de la plateforme"
description: "Fonctionnalités spécifiques d’Apple Watch à inclure dans les applications watchOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 16e779761aa164ea9890547e3baca527a3592021
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Fonctionnalités de la plateforme

_Fonctionnalités spécifiques d’Apple Watch à inclure dans les applications watchOS._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Améliorations de paie Apple](~/ios/watchos/platform/apple-pay.md)

WatchOS 3, le framework PassKit a été développé pour permettre la prise en charge pour les paiements sécurisées, dans l’application (des biens et services) pour les applications en cours d’exécution sur l’Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md)

watchOS 3 introduit plusieurs tâches d’arrière-plan d’une application peut utiliser pour mettre à jour ses informations s’assurer qu’il possède le contenu de l’utilisateur a besoin avant de l’ouvrir.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Introduction à watchOS 4](introduction-to-watchos4.md)

Nouvelles fonctionnalités dans watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Introduction à watchOS 3](introduction-to-watchos3/index.md)

Cet article présente les nouvelles et modifiées des API disponibles dans watchOS 3 pour les développeurs Xamarin.

##  <a name="notificationsnotificationsmd"></a>[Notifications](notifications.md)

Découvrez comment fournir de gestion dans votre application de surveillance de la notification personnalisée.

##  <a name="complicationscomplicationsmd"></a>[Complications](complications.md)

Ajouter la prise en charge de la complication pour afficher des données à jour sur le cadran de la montre.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Suggestions proactives](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 permet à l’application présenter des informations de façon proactive à l’utilisateur au sein de donnée de contextes. Pour prendre en charge cette fonctionnalité, le [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) inclut désormais la `MapItem` propriété qui permet à l’application de fournir des informations d’emplacement pour une utilisation ultérieure par d’autres applications.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniques d’interaction rapide](~/ios/watchos/platform/quick-interaction-techniques.md)

Les interactions utilisateur rapide en fournissant sont essentielles à la création d’applications Apple Watch exceptionnelles et les Complications. Nouveau watchOS 3, Apple a ajouté la prise en charge pour les modules de reconnaissance de mouvement, accéder aux nouvelles techniques de Notification utilisateur et la navigation et couronne numérique. Cette opération, ainsi que la prise en charge ajoutée pour SceneKit et SpriteKit, permettent au développeur de créer facilement des interfaces riches, parcourez-les rapides et réactives.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Améliorations apportées à l’app Exercice](~/ios/watchos/platform/workout-apps.md)

Nouveau watchOS 3, exercices relatifs applications ont la possibilité d’exécuter en arrière-plan sur l’Apple Watch. Pour activer cette fonctionnalité (et accéder aux données de HealthKit), l’application doit inclure le `WKBackgroundModes` clé dans le `Info.plist` fichier avec la valeur `workout-processing`.
