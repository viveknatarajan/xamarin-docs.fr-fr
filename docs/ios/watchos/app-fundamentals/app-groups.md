---
title: "Utilisation des groupes d’applications"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 27ce6c48c5bca69605773eb5ef5637201b9ce6c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-app-groups"></a>Utilisation des groupes d’applications


Un App Group est un groupe qui permet à différentes applications (ou à une application et ses extensions) d’accéder à un emplacement partagé du stockage de fichiers. Vous pouvez utiliser des App Groups pour les données suivantes, par exemple :

- Apple Watch [paramètres](~/ios/watchos/app-fundamentals/settings.md).
- Partagé [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Partagé [fichiers](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Configuration d’un groupe d’application

L’emplacement partagé est configuré à l’aide un [application groupe](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), qui est configuré dans le **certificats, les identificateurs et les profils** section sur [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Cette valeur doit également être référencée de chaque projet **Entitlements.plist**.

### <a name="provisioning"></a>Mise en service

Le groupe de l’application possède un identificateur, ce qui correspond généralement à votre ID d’offre groupée avec un `group.` préfixe. Par exemple, nous pouvons utiliser l’ID d’offre groupée `com.xamarin.WatchSettings` et le groupe de l’application `group.com.xamarin.WatchSettings`.

[ ![](app-groups-images/app-group-sml.png "Utilisez l’ID d’offre groupée com.xamarin.WatchSettings et le group.com.xamarin.WatchSettings de groupe d’application")](app-groups-images/app-group.png)

### <a name="entitlementsplist"></a>Entitlements.plist

Ainsi que la configuration du profil de configuration, **activer App Groups** dans les **Entitlements.plist** et entrez l’ID que vous avez choisie :

[ ![](app-groups-images/entitlements-sml.png "Configurer le fichier plist et entrez l’ID")](app-groups-images/entitlements.png)


### <a name="deployment"></a>Déploiement

Assurez-vous de vous configurez le groupe d’application correctement dans votre [déploiement](~/ios/watchos/deploy-test/index.md#app-groups) de configuration.


Pour plus d’informations, consultez la [fonctionnalités des applications du groupe](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.


## <a name="related-links"></a>Liens associés

- [Partage de données avec votre application conteneur Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Document de groupe de l’application d’Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
