---
title: Introduction à watchOS 3
description: Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans watchOS 3 pour les développeurs Xamarin.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/07/2017
ms.openlocfilehash: 0428a0df157e359ab34a6a71dbba31bdeb6962fa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224060"
---
# <a name="introduction-to-watchos-3"></a>Introduction à watchOS 3

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans watchOS 3 pour les développeurs Xamarin._

Ce document couvre les rubriques suivantes :

- [Nouveautés dans watchOS 3](#Whats-New-in-watchOS-3)
    - [Améliorations de payer Apple](#Apple-Pay-Enhancements) prend en charge les paiements dans l’application sur l’Apple Watch.
    - [Tâches en arrière-plan](#Background-Tasks) donnez à l’application la possibilité de mettre à jour ses informations dans l’arrière-plan, par conséquent, il est prêt lorsque l’utilisateur a besoin.
    - [Améliorations de complications](#Complications-Enhancements) ont été apportées pour watchOS 3 qui fournissent de nouvelles fonctionnalités pour les applications.
    - [Qui vient d’être disponible infrastructures](#Newly-Available-Frameworks) ont exposés pour les applications watchOS.
    - [Suggestions proactives](#Proactive-Suggestions) permet à l’application afficher des informations à l’utilisateur de manière proactive.
    * Plusieurs [sécurité et confidentialité améliorations](#Security-and-Privacy-Enhancements) ont été apportées à watchOS 3.
    - [Captures instantanées et ancrage](#Snapshots-and-Dock) permettre aux utilisateurs un accès rapide aux applications application watchOS.
    - [Notifications à l’utilisateur](#User-Notifications) fournit des notifications locales et distantes à l’utilisateur.
    * Plusieurs [Framework amélioration de la connectivité espion](#Watch-Connectivity-Framework-Enhancements) ont été apportées dans watchOS 3.
    * Plusieurs [WatchKit Framework améliorations](#WatchKit-Framework-Enhancements) ont été apportées dans watchOS 3.
    - [Améliorations d’application séances d’entraînement](#Workout-App-Enhancements) offre de nouvelles fonctionnalités à l’entraînement liés les applications Apple Watch.
- [Des modifications supplémentaires Framework](#Additional-Framework-Changes) ont été formulées dans watchOS 3.
- [API déconseillées](#Deprecated-APIs) dans watchOS 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Nouveautés dans watchOS 3

Apple a ajouté plusieurs nouvelles API et services dans watchOS 3 ainsi que de nombreuses améliorations aux fonctionnalités existantes, y compris :

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay améliorations

Dans watchOS 3, le framework PassKit a été étendu pour permettre au support technique pour les paiements sécurisés, dans l’application (de biens matériels et services) pour les applications en cours d’exécution sur l’Apple Watch.

Utilisez la nouvelle [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) et [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) classes de présenter et répondre à une interface où l’utilisateur peut autoriser les demandes de paiement.

Pour en savoir plus, consultez notre [Apple payer améliorations](~/ios/watchos/platform/apple-pay.md) guide.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Tâches d’arrière-plan

watchOS 3 introduit plusieurs tâches en arrière-plan qui une application peut utiliser pour mettre à jour ses informations en vous assurant qu’il a le contenu de l’utilisateur a besoin avant de l’ouvrir.

Les nouvelles tâches en arrière-plan suivantes sont disponibles :

- **Actualisation de l’application d’arrière-plan** - le [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) tâche autorise l’application à mettre à jour son état en arrière-plan. En général, cela inclut une autre tâche, telles que le téléchargement de nouveau contenu à partir d’internet en utilisant un [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Actualisation de la capture instantanée d’arrière-plan** - le [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) tâche autorise l’application à mettre à jour son contenu et l’interface utilisateur avant que le système prend un instantané qui sera utilisé pour remplir l’écran d’ancrage.
- **En arrière-plan espion connectivité** - le [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) tâche est démarrée pour l’application lorsqu’il reçoit les données d’arrière-plan de l’iPhone apparié.
- **Session de l’URL d’arrière-plan** - le [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) tâche est démarrée pour l’application lorsqu’un transfert en arrière-plan nécessite une autorisation ou qu’il se termine (correctement ou erreur).

Pour en savoir plus, consultez notre [tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md) guide.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Améliorations de complications

Complications sont de petits éléments visuels qui fournissent des informations utiles en un clin de œil. Selon le cadran de montre sélectionné, l’utilisateur a la possibilité de personnaliser un cadran de montre avec un ou plusieurs Complication.

watchOS 3 donne la possibilité de créer un ou plusieurs des complications pour l’application watch afin que l’utilisateur peut accéder à ses informations at-a-glance à partir d’un cadran de montre à l’application.

En outre, les Complications offrent les avantages suivants :

- L’utilisateur peut lancer rapidement l’application en appuyant sur la Complication directement à partir d’un cadran de montre.
- Ayant une des Complications de l’application sur les causes de visage espion le système afin de maintenir l’application dans un état prêt à lancer où il tente de lancer l’application en arrière-plan, conservez-la en mémoire et laisse l’il temps supplémentaire pour mettre à jour.
- Complications sont garanties au moins 50 mises à jour de push par jour.
- Lors de l’application comprend des Complications, seront proposé dans la galerie de visage Apple Watch (consultez d’Apple [Ajout de complications liées à la galerie](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) documentation pour plus d’informations).

Dans watchOS 3, le framework ClockKit inclut désormais plusieurs nouveaux modèles de complications de très grande taille tels que [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) et [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). En outre, pour créer le texte localisable, utiliser les nouvelles méthodes de la [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) classe.

Pour en savoir plus, consultez notre [rapide Techniques d’Interaction pour watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Infrastructures qui vient d’être disponibles

watchOS 3 comprend plusieurs infrastructures Apple existants qui n’étaient précédemment pas disponibles telles que :

- **SceneKit** -des modèles dans l’interface utilisateur de l’application Apple watch, y compris la plupart des fonctionnalités disponibles sur d’autres plateformes telles que physique d’éclairage, trame, animation, les systèmes de particules SceneKit utilisation à inclure 3D. Audio spatial 3D, des nuanceurs de métal ou OpenGL personnalisés, filtres d’Image Core et supports physiquement en fonction du ne sont pas pris en charge.
- **SpriteKit** -SpriteKit d’utilisation pour effectuer le rendu et animer des sprites dans l’interface utilisateur de l’application application espion, y compris la plupart des fonctionnalités disponibles sur d’autres plateformes telles que les systèmes de particules, actions, physique et d’éclairage. Audio spatial 3D, la lecture vidéo et filtres d’Image de base ne sont pas pris en charge.
- **AVFoundation** , pour gérer et de lire un fichier audio.
- **CloudKit** - pour déplacer des données entre les conteneurs d’application et iCloud espion.
- **Core Audio** : pour gérer des types de données pour représenter les flux audio, les mémoires tampons complexes et les valeurs de temps.
- **GameKit** - pour créer des jeux sociaux.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Suggestions proactives

watchOS 3 permet à l’application présenter des informations à l’utilisateur au sein de façon proactive étant donné les contextes. Pour prendre en charge cette fonctionnalité, le [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) inclut désormais le `MapItem` propriété qui permet à l’application de fournir des informations d’emplacement pour une utilisation ultérieure par d’autres applications.

Pour en savoir plus, consultez notre [Introduction aux Suggestions proactives](~/ios/watchos/platform/proactive-suggestions.md) guide.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Améliorations de la confidentialité et de sécurité

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans watchOS 3 qui aideront les développeurs à améliorer la sécurité de leurs applications et garantir la confidentialité de l’utilisateur final.

Par conséquent, les applications qui s’exécutent sur watchOS 3 (ou version ultérieure) doivent déclarer statiquement leur intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés spécifiques confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pourquoi l’application souhaite accéder.

Étant donné que watchOS 3 partage ces modifications avec iOS 10, consultez notre iOS 10 [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) guide pour plus d’informations.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Captures instantanées et la station d’accueil

Dans watchOS 3, Apple a ajouté la station d’accueil où les utilisateurs peuvent épingler leurs applications favorites et y accéder rapidement. Lorsque l’utilisateur appuie sur le bouton sur l’Apple Watch, une galerie de captures instantanées application épinglée s’affichera. L’utilisateur peut balayer gauche ou droite pour rechercher l’application souhaitée, puis appuyez sur l’application pour le lancer en remplaçant l’instantané avec l’interface de l’application en cours d’exécution.

Périodiquement, le système prend des captures instantanées de l’interface utilisateur de l’application et utilise ces captures instantanées pour remplir les documents. watchOS donne la possibilité de mettre à jour son contenu ni l’interface utilisateur avant que cet instantané est mis à l’application.

Pour plus d’informations, consultez notre [tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md) guide et Apple [WKSnapshotRefreshBackgroundTask référence](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Notifications à l’utilisateur

L’infrastructure de Notification utilisateur introduite dans watchOS 3 prend en charge la remise des notifications locales et distantes à l’Apple Watch. Utilisez cette infrastructure planifier des notifications en fonction des conditions spécifiques comme la durée de la journée ou emplacement et à recevoir et gérer les notifications.

Pour en savoir plus, consultez notre [rapide Techniques d’Interaction pour watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Regardez les améliorations de connectivité Framework

La nouvelle `HasContentPending` propriété de la [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) classe indique que la session a reçu des données en arrière-plan qui doivent être traitées. Et le `RemainingComplicationUserInfoTransfers` propriété retourne les autres fois que l’application iOS peuvent mettre à jour son watchOS Complication.

Pour en savoir plus, consultez notre [tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md) guide.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>Améliorations WatchKit Framework

watchOS 3 comprend plusieurs améliorations à l’infrastructure WatchKit, y compris les éléments suivants :

- L’application peut obtenir l’état de la couronne numérique à l’aide de la nouvelle [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) classe et de recevoir des mises à jour lorsque l’utilisateur fait pivoter l’à l’aide de la couronne le [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) classe.
- Le [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) classe inclut désormais la `ApplicationState` (méthode) et [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) constante que l’application peut utiliser pour effectuer le suivi de l’état d’exécution de l’application. `WKExtension` fournit également deux nouvelles méthodes qui peuvent être utilisées pour planifier des tâches en arrière-plan.
- Le [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) inclut désormais le nouveau `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` et `HandleBackgroundTasks` méthodes pour surveiller les modifications de l’état de l’application et de gérer les mises à jour de la tâche en arrière-plan.
- Un nouveau [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) classe a été ajoutée pour fournir les types suivants de la reconnaissance de mouvement pour les applications de surveillance : [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) et [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- La nouvelle [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) classe fournit une interface pour n’importe quel HomeKit attachés caméra IP.
- La nouvelle [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) classe permet à l’application afficher un film « affiche « qui est remplacée par la séquence en cours d’exécution lorsque l’utilisateur appuie sur elle.
- La nouvelle [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) classe autorise l’application à présenter un bouton Apple Pay dans son interface utilisateur qui lance une demande de paiement lorsque j’appuie.
- La nouvelle [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) classe présente une interface permettant d’afficher une scène SceneKit sur l’Apple Watch.
- La nouvelle [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) classe présente une interface permettant d’afficher une scène SpriteKit sur l’Apple Watch.

Pour en savoir plus, consultez notre [rapide Techniques d’Interaction pour watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Améliorations apportées à l’application Exercice

Nouveau watchOS 3, séances d’entraînement associé à des applications ont la possibilité d’exécuter en arrière-plan sur l’Apple Watch. Pour activer cette fonctionnalité (et accéder aux données d’HealthKit), l’application doit inclure le `WKBackgroundModes` clé dans le `Info.plist` fichier avec la valeur `workout-processing`.

En outre, le développeur a désormais la possibilité de lancer l’application d’entraînement watchOS à partir de la version de l’application iOS sur l’iPhone apparié.

Pour en savoir plus, consultez notre [séances d’entraînement application améliorations](~/ios/watchos/platform/workout-apps.md) guide.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

Outre les modifications de l’infrastructure principale et les ajouts répertoriés ci-dessus, Apple a effectué de nombreuses modifications de framework mineures supplémentaires dans watchOS 3.

Pour en savoir plus, consultez notre [des modifications supplémentaires Framework](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) guide.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>API déconseillées

Les API suivantes ont été déconseillées dans watchOS 3 :

- Le `UILocalNotification` classe de UIKit a été déconseillée et doit être remplacé par l’infrastructure de Notification à l’utilisateur.

Consultez d’Apple [watchOS 2.2 aux différences d’API watchOS 3.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) documentation pour obtenir la liste complète des dépréciations et les modifications.


## <a name="related-links"></a>Liens associés

- [Exemples watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Quelles sont les nouveautés dans watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
