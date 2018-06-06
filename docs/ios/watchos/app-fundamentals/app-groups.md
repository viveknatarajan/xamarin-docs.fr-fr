---
title: Utilisation de watchOS groupes d’applications dans Xamarin
description: Ce document décrit les groupes d’applications et leur utilisation dans une application watchOS. Elle explique comment configurer un groupe d’application, configuration requise, les considérations de Entitlements.plist et le déploiement de configuration.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 5736b25af3993e2da794422a1a6f040461532497
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790677"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Utilisation de watchOS groupes d’applications dans Xamarin


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
