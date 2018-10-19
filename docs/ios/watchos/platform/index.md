---
title: fonctionnalités de la plateforme watchOS
description: Ce document contient des liens vers des guides différents qui décrivent les fonctionnalités de la plateforme watchOS tels que Apple Pay, notifications, complications, suggestions proactives, applications de séances d’entraînement et bien plus encore.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 09200ba5968838edf829b30a50a8ad0f4a3ab3aa
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39030676"
---
# <a name="watchos-platform-features"></a>fonctionnalités de la plateforme watchOS

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[Introduction à watchOS 5](introduction-to-watchos5/index.md)

Ce document fournit une vue d’ensemble des fonctionnalités nouvelles et mises à jour de watchOS 5 qui sont disponibles pour une utilisation lors de la création d’applications watchOS avec Xamarin.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Introduction à watchOS 4](introduction-to-watchos4.md)

Ce document fournit une vue d’ensemble de fonctionnalités ajoutés et mis à jour dans watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Introduction à watchOS 3](introduction-to-watchos3/index.md)

Cet article décrit les API nouvelles et mises à jour dans watchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Améliorations d’Apple Pay](~/ios/watchos/platform/apple-pay.md)

Dans watchOS 3, le framework PassKit a été étendu pour permettre au support technique pour les paiements sécurisés, dans l’application (de biens matériels et services) pour les applications en cours d’exécution sur l’Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md)

watchOS 3 introduit plusieurs tâches en arrière-plan qui une application peut utiliser pour mettre à jour ses informations en vous assurant qu’il a le contenu de l’utilisateur a besoin avant de l’ouvrir.

## <a name="notificationsnotificationsmd"></a>[Notifications](notifications.md)

Découvrez comment fournir une notification personnalisée gestion dans votre application watch.

## <a name="complicationscomplicationsmd"></a>[Complications](complications.md)

Ajouter la prise en charge de la complication pour afficher des données à jour sur le cadran de montre.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Suggestions proactives](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 permet à l’application présenter des informations à l’utilisateur au sein de façon proactive étant donné les contextes. Pour prendre en charge cette fonctionnalité, le [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) inclut désormais le `MapItem` propriété qui permet à l’application de fournir des informations d’emplacement pour une utilisation ultérieure par d’autres applications.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniques d’interaction rapide](~/ios/watchos/platform/quick-interaction-techniques.md)

Les interactions utilisateur rapide en fournissant sont essentielles pour créer des applications attractives Apple Watch et les Complications. Nouveau watchOS 3, Apple a ajouté la prise en charge pour les modules de reconnaissance de mouvement, accès à la couronne numériques et de nouvelles techniques de Notification utilisateur et la navigation. Ceci, ainsi que la prise en charge ajoutée pour SceneKit et SpriteKit, permettent au développeur de créer facilement des interfaces riches et glanceable qui sont à la fois rapide et réactif.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Améliorations d’application séances d’entraînement](~/ios/watchos/platform/workout-apps.md)

Nouveau watchOS 3, séances d’entraînement associé à des applications ont la possibilité d’exécuter en arrière-plan sur l’Apple Watch. Pour activer cette fonctionnalité (et accéder aux données d’HealthKit), l’application doit inclure le `WKBackgroundModes` clé dans le `Info.plist` fichier avec la valeur `workout-processing`.
