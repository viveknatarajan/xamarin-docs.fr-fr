---
title: Utilisation de watchOS App Groups dans Xamarin
description: Ce document décrit les groupes d’applications et leur utilisation dans une application watchOS. Il explique comment configurer un groupe d’application, déploiement, Entitlements.plist considérations et exigences d’approvisionnement.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 78f6c03f73f0e4d8a74f826dd7bc25bbe325d545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61411699"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Utilisation de watchOS App Groups dans Xamarin


Un App Group est un groupe qui permet à différentes applications (ou à une application et ses extensions) d’accéder à un emplacement partagé du stockage de fichiers. Vous pouvez utiliser des App Groups pour les données suivantes, par exemple :

- Apple Watch [paramètres](~/ios/watchos/app-fundamentals/settings.md).
- Partagé [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Partagé [fichiers](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Configurer un groupe de l’application

L’emplacement partagé est configuré à l’aide un [App Group](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), qui est configuré dans le **certificats, identificateurs et profils** section [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Cette valeur doit également être référencée dans de chaque projet **Entitlements.plist**.

### <a name="provisioning"></a>Provisionnement

Le groupe de l’application a un identificateur, qui est généralement votre ID d’offre groupée avec un `group.` préfixe. Par exemple, nous pourrions utiliser l’ID de Bundle `com.xamarin.WatchSettings` et le groupe de l’application `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Utiliser l’ID d’offre groupée com.xamarin.WatchSettings et le group.com.xamarin.WatchSettings de groupe d’application")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

Ainsi que la configuration du profil de provisionnement, **activer App Groups** dans le **Entitlements.plist** et entrez l’ID que vous avez choisie :

[![](app-groups-images/entitlements-sml.png "Configurer le fichier plist et entrez l’ID")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Déploiement

Vérifiez la configuration de l’application du groupe correctement dans votre [déploiement](~/ios/watchos/deploy-test/index.md#App_Groups) d’approvisionnement.


Pour plus d’informations, consultez le [application groupe fonctionnalités](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.


## <a name="related-links"></a>Liens associés

- [Apple partage de données avec votre application de conteneur](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Document de groupe de l’application d’Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
