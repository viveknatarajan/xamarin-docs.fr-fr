---
title: "Utilisation des groupes d’applications"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9365bc2707876816419bb5d136a6a1011cf129d7
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="working-with-app-groups"></a>Utilisation des groupes d’applications


Un App Group est un groupe qui permet à différentes applications (ou à une application et ses extensions) d’accéder à un emplacement partagé du stockage de fichiers. Vous pouvez utiliser des App Groups pour les données suivantes, par exemple :

- Apple Watch [paramètres](~/ios/watchos/app-fundamentals/settings.md).
- Partagé [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Partagé [fichiers](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Configuration d’un groupe d’application

L’emplacement partagé est configuré à l’aide un [application groupe](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), qui est configuré dans le **certificats, les identificateurs et les profils** section sur [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Cette valeur doit également être référencée de chaque projet **Entitlements.plist**.

### <a name="provisioning"></a>Provisionnement

Le groupe de l’application possède un identificateur, ce qui correspond généralement à votre ID d’offre groupée avec un `group.` préfixe. Par exemple, nous pouvons utiliser l’ID d’offre groupée `com.xamarin.WatchSettings` et le groupe de l’application `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Utilisez l’ID d’offre groupée com.xamarin.WatchSettings et le group.com.xamarin.WatchSettings de groupe d’application")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

Ainsi que la configuration du profil de configuration, **activer App Groups** dans les **Entitlements.plist** et entrez l’ID que vous avez choisie :

[![](app-groups-images/entitlements-sml.png "Configurer le fichier plist et entrez l’ID")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Déploiement

Assurez-vous de vous configurez le groupe d’application correctement dans votre [déploiement](~/ios/watchos/deploy-test/index.md#App_Groups) de configuration.


Pour plus d’informations, consultez la [fonctionnalités des applications du groupe](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.


## <a name="related-links"></a>Liens associés

- [Partage de données avec votre application conteneur Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Document de groupe de l’application d’Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
