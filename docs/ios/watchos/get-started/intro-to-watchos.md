---
title: Introduction aux watchOS
description: Vue d’ensemble de la structure de la solution watchOS et limitations
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 0a0adbab54fc134eaf2e69cc8088713e54b15d3b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos"></a>Introduction aux watchOS

> [!NOTE]
> Extraire le [présentation watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) pour une vue d’ensemble des fonctionnalités plus récentes.

## <a name="about-watchos"></a>À propos de watchOS

Une solution d’application watchOS a 3 projets :

- **Regardez Extension** – un projet qui contient le code de l’application de surveillance.
- **Surveiller l’application** – contient les ressources et l’animation de l’interface utilisateur.
- **iOS application parente** – cette application est une application iPhone normal. L’application de surveillance et l’extension sont regroupés dans l’application iPhone pour la remise à la fenêtre Espion de l’utilisateur.

Dans les applications watchOS 1, le code dans l’extension s’exécute sur l’iPhone : l’Apple Watch est effectivement un affichage externe. les applications watchOS 2 et 3 s’exécutent entièrement sur l’Apple Watch. Cette différence est indiquée dans le diagramme ci-dessous :

[ ![](intro-to-watchos-images/arch-sml.png "La différence entre watchOS 1 et watchOS 2 (et supérieur) est représentée dans ce diagramme")](intro-to-watchos-images/arch.png#lightbox)

Quelle que soit la version de watchOS est ciblée, dans Visual Studio pour Solution remplissage Mac une solution complète ressemble à ceci :

[![](intro-to-watchos-images/projectstructure-sml.png "Le remplissage de la Solution")](intro-to-watchos-images/projectstructure.png#lightbox)

Le *Parent application* dans un watchOS solution est une application iOS régulière. Il s’agit du seul projet dans la solution est visible **sur le téléphone**. En cas d’utilisation de cette application incluront des didacticiels, des écrans d’administration et le filtrage, mise en mémoire cache, etc. de couche intermédiaire. Toutefois, il est possible de l’utilisateur installer et exécuter l’application/extension watch sans **jamais** avoir ouvert l’application parente, par conséquent, si vous avez besoin de l’application parente à exécuter pour une initialisation unique ou l’administration, vous devez programmer votre montre / extension d’application pour indiquer à l’utilisateur qui.

Bien que l’application parente remet l’application de surveillance et l’extension, ils s’exécuter dans différents conteneurs.

Sur watchOS 1 ils peuvent partager des données via un groupe d’application partagé ou la fonction statique `WKInterfaceController.OpenParentApplication`, ce qui déclenchera le `UIApplicationDelegate.HandleWatchKitExtensionRequest` méthode dans votre application de parent `AppDelegate` (consultez [fonctionne avec l’application parente](~/ios/watchos/app-fundamentals/parent-app.md)).

Sur watchOS 2 ou version ultérieure, l’infrastructure de connectivité de la surveillance est utilisé pour communiquer avec l’application parente, à l’aide de la `WCSession` classe.

## <a name="application-lifecycle"></a>Cycle de vie de l'application

Dans l’extension de surveillance, une sous-classe de la `WKInterfaceController` classe est créée pour chaque séquence d’animation.

Ces `WKInterfaceController` classes sont analogues au `UIViewController` objets dans la programmation d’iOS, mais n’ont pas le même niveau d’accès à la vue.
Par exemple, vous ne peut pas ajouter des contrôles à dynamiquement ou restructurer votre interface utilisateur.
Vous pouvez, toutefois, masquer et afficher des contrôles et, avec certains contrôles, modifier leur taille, la transparence et options d’apparence.

Le cycle de vie d’un `WKInterfaceController` objet implique les appels suivants :

- [Éveillés](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : vous devez effectuer la majeure partie de l’initialisation de votre dans cette méthode.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : appelé juste avant l’application de surveillance s’affiche à l’utilisateur. Utilisez cette méthode pour effectuer l’initialisation du dernier jour, des animations, etc. de début.
- À ce stade, l’application de surveillance s’affiche et l’Extension commence à répondre aux entrées utilisateur et affichage de l’application espion par votre logique d’application de la mise à jour.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) après l’application de surveillance a été fermée par l’utilisateur, cette méthode est appelée. Une fois que cette méthode est retournée, les contrôles d’interface utilisateur ne sont pas modifiables jusqu'à la prochaine `WillActivate` est appelée. Cette méthode sera également être appelée si la connexion pour l’iPhone est interrompue.
- Une fois que l’extension a été désactivée, il n’est pas accessible à votre programme. Fonctions asynchrones en attente **ne seront pas** être appelée. Extensions du Kit de surveillance ne peut pas utiliser de modes de traitement en arrière-plan. Si le programme est réactivé par l’utilisateur, mais l’application n’a pas été terminée par le système d’exploitation, la première méthode appelée sera `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Vue d’ensemble du cycle de vie des applications")

## <a name="types-of-user-interface"></a>Types d’Interface utilisateur

Il existe trois types d’interaction de que l’utilisateur peut avoir avec votre application de surveillance.
Tous sont programmées à l’aide de sous-classes personnalisés de `WKInterfaceController`, de sorte que la séquence de cycle de vie décrit précédemment s’applique globalement (notifications sont programmées avec des sous-classes de `WKUserNotificationController`, qui est lui-même une sous-classe de `WKInterfaceController`) :

### <a name="normal-interaction"></a>Interaction normale

La majorité d’interaction d’extension d’application espion sera effectuée avec des sous-classes de `WKInterfaceController` que vous écrivez afin qu’ils correspondent à des scènes de votre application espion **Interface.storyboard**. Celle-ci est décrite en détail dans les [Installation](~/ios/watchos/get-started/installation.md) et [mise en route](~/ios/watchos/get-started/index.md) articles.
L’illustration suivante montre une partie de la [espion Kit catalogue](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) plan conceptuel de l’exemple. Pour chaque séquence a montré ici, il existe une personnalisée correspondante `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`, etc.) dans le projet d’extension.

![](intro-to-watchos-images/scenes.png "Exemples d’Interaction de normal")

### <a name="notifications"></a>Notifications

[Notifications](~/ios/watchos/platform/notifications.md) sont un cas d’usage principaux pour l’Apple Watch. Notifications à la fois locale et distantes sont pris en charge. Interaction avec des notifications se produit en deux étapes, appelées aspect court et Long.

Courte s’affichent brièvement et afficher le titre, son nom et l’icône d’application espion (tel que spécifié par `WKInterfaceController.SetTitle`).

La recherche Long combine fournie par le système **châssis** zone et le bouton de faire disparaître avec votre contenu personnalisé basé sur le plan conceptuel.

`WKUserNotificationInterfaceController` étend `WKInterfaceController` avec les méthodes `DidReceiveLocalNotification` et `DidReceiveRemoteNotification`.
Substituer ces méthodes pour réagir aux événements de notification.

Pour plus d’informations sur la conception de l’interface utilisateur de Notification, reportez-vous à la [indications de l’Interface humaine Apple Watch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Notifications de l’exemple")

## <a name="screen-sizes"></a>Tailles d’écran

L’Apple Watch a deux tailles de police : 38 et 42mm, à la fois avec un ratio d’affichage 5:4 et un écran retina. Leur taille utilisable est :

- nombre de pixels logiques 38 mm : 136 x 170 (272 x 340 physiques pixels)
- 42 mm : 156 x 195 pixels logiques (312 x 390 physiques pixels).

Utilisez `WKInterfaceDevice.ScreenBounds` pour déterminer sur l’affichage qui s’exécute votre application de surveillance.

En règle générale, il est plus facile à développer votre conception de texte et la disposition avec l’affichage de 38mm plus limité et puis montée en puissance.
Si vous démarrez avec l’environnement, mise à l’échelle vers le bas peut entraîner laid chevauchement ou de troncation de texte.

En savoir plus sur [utilisation de tailles d’écran](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>Limitations de watchOS

Il existe certaines limitations de watchOS à connaître lors du développement d’applications de watchOS :

- Appareils d’Apple Watch ont une mémoire limitée, tenez compte de l’espace disponible avant de télécharger des fichiers volumineux (par exemple). fichiers audio ou vidéo).

- Nombreux watchOS [contrôles](~/ios/watchos/user-interface/index.md) se retrouvent dans UIKit, mais différentes classes (`WKInterfaceButton` plutôt que `UIButton`, `WKInterfaceSwitch` pour `UISwitch`, etc.) et ont un ensemble limité de méthodes par rapport à leur UIKit équivalents. En outre, watchOS a des contrôles tels que `WKInterfaceDate` (pour afficher une date et heure) que UIKit n’a pas.

  - Vous ne peut pas envoyer des notifications à l’observation uniquement ou iPhone uniquement (le type de contrôle de l’utilisateur est sur le routage n’a pas été annoncé par Apple).

Certaines limitations connues / de forum aux questions :

- Apple n’autorisera pas espion personnalisé de 3 rd-party faces.

- Les API qui permettent la surveillance contrôler iTunes sur le téléphone connecté sont privés.


## <a name="further-reading"></a>informations supplémentaires

Consultez la documentation d’Apple :

* [Développement pour le Kit d’espion](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Regardez le Guide de programmation](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Indications de l’Interface humaine Apple Watch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>Liens associés

- [watchOS 3 catalogue (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 catalogue (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Le programme d’installation et installation](~/ios/watchos/get-started/installation.md)
- [Première vidéo de l’application de surveillance](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple du développement pour le guide du Kit de surveillance](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Conseils de WatchKit d’Apple](https://developer.apple.com/watchkit/tips/)
- [Introduction à watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
