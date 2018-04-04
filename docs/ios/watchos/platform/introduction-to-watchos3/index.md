---
title: Introduction à watchOS 3
description: Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans watchOS 3 pour les développeurs Xamarin.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: 506e3795538ceffc77301a608c504fc6ec2045a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos-3"></a>Introduction à watchOS 3

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans watchOS 3 pour les développeurs Xamarin._

Ce document couvre les rubriques suivantes :

- [Nouveautés dans watchOS 3](#Whats-New-in-watchOS-3)
    - [Améliorations de payer Apple](#Apple-Pay-Enhancements) prend en charge les paiements dans l’application sur l’Apple Watch.
    - [Tâches en arrière-plan](#Background-Tasks) donnez à l’application la possibilité de mettre à jour ses informations dans l’arrière-plan, par conséquent, il est prêt lorsque l’utilisateur a besoin.
    - [Améliorations de complications](#Complications-Enhancements) ont été apportées pour watchOS 3 qui fournit de nouvelles fonctionnalités pour les applications.
    - [Qui vient d’être disponible infrastructures](#Newly-Available-Frameworks) ont exposées pour les applications watchOS.
    - [Suggestions proactive](#Proactive-Suggestions) permet à l’application afficher les informations de façon proactive à l’utilisateur.
    * Plusieurs [sécurité et confidentialité améliorations](#Security-and-Privacy-Enhancements) ont été apportées à watchOS 3.
    - [Les instantanés et ancrer](#Snapshots-and-Dock) lui fournir un accès rapide aux applications watchOS application.
    - [Notifications à l’utilisateur](#User-Notifications) fournit des notifications à la fois locale et distantes à l’utilisateur.
    * Plusieurs [Framework améliorations de la connectivité espion](#Watch-Connectivity-Framework-Enhancements) ont été apportées dans watchOS 3.
    * Plusieurs [WatchKit Framework améliorations](#WatchKit-Framework-Enhancements) ont été apportées dans watchOS 3.
    - [Améliorations d’application entraînement](#Workout-App-Enhancements) offre de nouvelles possibilités pour effectuer liés les applications Apple Watch.
- [Des modifications supplémentaires Framework](#Additional-Framework-Changes) ont été apportées dans l’ensemble de watchOS 3.
- [API déconseillées](#Deprecated-APIs) dans watchOS 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Nouveautés dans watchOS 3

Apple a ajouté plusieurs nouvelles API et sur les services dans watchOS 3 ainsi que de nombreuses améliorations apportées aux fonctionnalités existantes, y compris :

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Améliorations de paie Apple

WatchOS 3, le framework PassKit a été développé pour permettre la prise en charge pour les paiements sécurisées, dans l’application (des biens et services) pour les applications en cours d’exécution sur l’Apple Watch.

Utilisez la nouvelle [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) et [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) classes de présenter et répondre à une interface où l’utilisateur peut autoriser les demandes de paiement.

Pour plus d’informations, consultez notre [améliorations de payer Apple](~/ios/watchos/platform/apple-pay.md) guide.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Tâches d’arrière-plan

watchOS 3 introduit plusieurs tâches d’arrière-plan d’une application peut utiliser pour mettre à jour ses informations s’assurer qu’il possède le contenu de l’utilisateur a besoin avant de l’ouvrir.

Les nouvelles tâches en arrière-plan suivantes sont disponibles :

- **Actualisation de l’application d’arrière-plan** - le [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) tâche permet à l’application mettre à jour son état en arrière-plan. En général, cela inclut une autre tâche, telles que le téléchargement de contenu à partir d’internet en utilisant un [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Actualisation de la capture instantanée d’arrière-plan** - le [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) tâche permet à l’application mettre à jour son contenu et l’interface utilisateur avant que le système prend un instantané qui sera utilisé pour remplir la station d’accueil.
- **Connectivité d’espion d’arrière-plan** - le [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) tâche est démarrée pour l’application lorsqu’il reçoit des données d’arrière-plan de l’iPhone apparié.
- **URL de Session d’arrière-plan** - le [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) tâche est démarrée pour l’application lorsqu’un transfert en arrière-plan nécessite une autorisation ou qu’il se termine (correctement ou erreur).

Pour plus d’informations, consultez notre [les tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md) guide.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Améliorations de complications

Complications sont de petits éléments visuels qui fournissent des informations utiles en un coup de œil. En fonction de la face espion est sélectionnée, l’utilisateur a la possibilité de personnaliser un visage Espion avec un ou plusieurs des complications.

watchOS 3 permet à l’application de créer un ou plusieurs des complications pour l’application Espion afin que l’utilisateur peut accéder à ses informations en un coup de œil à partir d’un type d’espion.

En outre, les Complications présentent les avantages suivants :

- L’utilisateur peut rapidement lancer l’application en appuyant sur la Complication directement à partir d’un visage espion.
- Ayant une des Complications de l’application sur les causes de face espion pour conserver l’application dans un état prêt à lancer lorsqu’il tente de lancer l’application en arrière-plan, le système de conserver en mémoire et laisse l’il temps supplémentaire pour mettre à jour.
- Complications sont garanti que les mises à jour au moins 50 par émission de données par jour.
- Lorsque l’application comprend des Complications, elle sera utilisée dans la galerie de Face d’Apple Watch (voir d’Apple [Ajout Complications à la galerie](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) documentation pour plus d’informations).

Dans watchOS 3, le framework ClockKit inclut désormais plusieurs nouveaux modèles de très nombreuses complications tels que [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) et [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). En outre, pour créer un texte localisable, utilisez nouvelles méthodes de la [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) classe.

Pour plus d’informations, consultez notre [rapide Techniques d’Interaction pour watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Infrastructures qui vient d’être disponibles.

watchOS 3 comprend plusieurs infrastructures existantes Apple qui étaient précédemment indisponibles telles que :

- **SceneKit** -SceneKit utilisation à inclure 3D modèles dans l’interface utilisateur de l’application de surveillance, y compris la plupart des fonctionnalités disponibles sur d’autres plateformes, comme l’éclairage, trame, animation, physique et les systèmes de particules. Audio spatial 3D, les nuanceurs complète ou OpenGL personnalisés, filtres d’Image de base et matériaux physiquement basés n’est pas pris en charge.
- **SpriteKit** -SpriteKit utiliser pour effectuer le rendu et animer les composants de l’interface utilisateur de l’application application espion, y compris la plupart des fonctionnalités disponibles sur d’autres plateformes, comme les systèmes de particules, physique, d’éclairage et actions. Audio spatial 3D, la lecture vidéo et filtres d’Image de base ne sont pas pris en charge.
- **AVFoundation** - pour gérer et lire des fichiers audio.
- **CloudKit** - pour déplacer des données entre les conteneurs d’application et iCloud espion.
- **Base de données Audio** - pour gérer des types de données pour représenter le flux audio, les mémoires tampons complexes et valeurs d’heure.
- **GameKit** - pour créer des jeux de réseaux sociaux.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Suggestions proactive

watchOS 3 permet à l’application présenter des informations de façon proactive à l’utilisateur au sein de donnée de contextes. Pour prendre en charge cette fonctionnalité, le [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) inclut désormais la `MapItem` propriété qui permet à l’application de fournir des informations d’emplacement pour une utilisation ultérieure par d’autres applications.

Pour plus d’informations, consultez notre [présentation Proactive des Suggestions](~/ios/watchos/platform/proactive-suggestions.md) guide.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Améliorations de la confidentialité et de sécurité

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans watchOS 3 qui aideront le développeur d’améliorer la sécurité de leurs applications et de garantir la confidentialité de l’utilisateur final.

Par conséquent, les applications exécutées sur watchOS 3 (ou version ultérieure) doivent déclarer statiquement intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés spécifiques confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pour lesquelles l’application souhaite accéder.

Étant donné que watchOS 3 partage ces modifications avec iOS 10, consultez notre iOS 10 [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) guide pour plus d’informations.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Instantanés et la station d’accueil

Dans watchOS 3, Apple a ajouté la station d’accueil où les utilisateurs peuvent épingler leurs applications favorites et y accéder rapidement. Lorsque l’utilisateur appuie sur le bouton sur l’Apple Watch, une galerie d’instantanés de l’application épinglé s’affiche. L’utilisateur peut passez gauche ou droite pour rechercher l’application souhaitée, puis cliquez sur l’application pour le lancer en remplaçant l’instantané avec l’interface de l’application en cours d’exécution.

Périodiquement, le système de prend des instantanés de l’interface utilisateur de l’application et utilise ces instantanés pour remplir les documents. watchOS permet à l’application pour mettre à jour son contenu et l’interface utilisateur avant la prise de cet instantané.

Pour plus d’informations, consultez notre [les tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md) guide et Apple [WKSnapshotRefreshBackgroundTask référence](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Notifications à l’utilisateur

L’infrastructure de Notification utilisateur introduite dans watchOS 3 prend en charge la remise de notifications à la fois locale et distantes à l’Apple Watch. Utilisez cette infrastructure planifier des notifications en fonction des conditions spécifiques telles que le délai du jour ou l’emplacement et à recevoir et gérer les notifications.

Pour plus d’informations, consultez notre [rapide Techniques d’Interaction pour watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Regardez les améliorations de la connectivité Framework

La nouvelle `HasContentPending` propriété de la [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) classe indique que la session a reçu des données en arrière-plan qui doit être traitée. Et le `RemainingComplicationUserInfoTransfers` propriété retourne les autres moment que l’application iOS peuvent mettre à jour son watchOS Complication.

Pour plus d’informations, consultez notre [les tâches en arrière-plan](~/ios/watchos/platform/background-tasks.md) guide.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>Améliorations WatchKit Framework

watchOS 3 comprend plusieurs améliorations à l’infrastructure WatchKit, y compris les éléments suivants :

- L’application peut obtenir l’état de la couronne numérique à l’aide de la nouvelle [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) classe et de recevoir des mises à jour lorsque l’utilisateur fait pivoter la couronne à l’aide de la [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) classe.
- Le [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) classe inclut désormais la `ApplicationState` (méthode) et [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) constante que l’application peut utiliser pour effectuer le suivi de l’état d’exécution de l’application. `WKExtension` fournit également deux nouvelles méthodes qui peuvent être utilisés pour planifier des tâches en arrière-plan.
- Le [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) inclut désormais la nouvelle `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` et `HandleBackgroundTasks` méthodes pour surveiller les modifications de l’état de l’application et de gérer les mises à jour de la tâche en arrière-plan.
- Un nouveau [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) classe a été ajoutée pour fournir les types suivants de la reconnaissance de mouvement pour les applications espion : [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer ](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) et [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- La nouvelle [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) classe fournit une interface pour n’importe quel HomeKit jointe caméra IP.
- La nouvelle [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) classe permet à l’application afficher un film « affiche « qui est remplacé par la séquence en cours d’exécution lorsque l’utilisateur appuie sur elle.
- La nouvelle [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) classe permet à l’application présenter un bouton Apple Pay dans son interface utilisateur qui exécutera une demande de paiement lorsque tapées.
- La nouvelle [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) classe présente une interface pour l’affichage d’une scène SceneKit sur l’Apple Watch.
- La nouvelle [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) classe présente une interface pour l’affichage d’une scène SpriteKit sur l’Apple Watch.

Pour plus d’informations, consultez notre [rapide Techniques d’Interaction pour watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) guide.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Améliorations d’application entraînement

Nouveau watchOS 3, exercices relatifs applications ont la possibilité d’exécuter en arrière-plan sur l’Apple Watch. Pour activer cette fonctionnalité (et accéder aux données de HealthKit), l’application doit inclure le `WKBackgroundModes` clé dans le `Info.plist` fichier avec la valeur `workout-processing`.

En outre, le développeur a désormais la possibilité de lancer l’application d’entraînement watchOS à partir de la version d’application iOS sur iPhone apparié.

Pour plus d’informations, consultez notre [entraînement application améliorations](~/ios/watchos/platform/workout-apps.md) guide.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

Outre les modifications majeures framework et les ajouts répertoriés ci-dessus, Apple a fait de nombreuses modifications supplémentaires framework mineure watchOS 3.

Pour plus d’informations, consultez notre [des modifications supplémentaires Framework](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) guide.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>API déconseillées

Les API suivantes ont été déconseillées dans watchOS 3 :

- La `UILocalNotification` classe de UIKit a été déconseillée et doit être remplacé par l’infrastructure de Notification de l’utilisateur.

Consultez d’Apple [watchOS 2.2 aux différences d’API watchOS 3.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) documentation pour obtenir la liste complète des deprecations et des modifications.


## <a name="related-links"></a>Liens associés

- [Exemples watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Quelles sont les nouveautés dans watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
