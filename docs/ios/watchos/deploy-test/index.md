---
title: Déploiement et test des applications watchOS avec Xamarin
description: Ce document décrit comment déployer et tester des applications watchOS avec Xamarin. Il fournit une liste de vérification de déploiement, aborde explicite et ID, d’application générique et examine les groupes d’applications.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7d626b8a968835813d87c93e3cead57a00c14000
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293390"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Déploiement et test des applications watchOS avec Xamarin

## <a name="deployment-checklist"></a>Liste de vérification de déploiement

Si vous déployez à un espion de test, ou charger sur l’App Store, vous devez suivre les étapes de cette page :

- Dans le **iOS centre de développement**:
  - [ID d’application](#App_IDs) ont été créés.
  - [Groupes d’applications](#App_Groups) configuré (si nécessaire).
  - Profils de provisionnement de distribution créé

- Dans votre solution :

  - Vérifiez le [ID d’ensemble et références de projet](~/ios/watchos/get-started/installation.md) sont définies.
  - Vérifiez vos icônes sont [configuré correctement](~/ios/watchos/app-fundamentals/icons.md).
  - Vérifier la correspondance de numéros de version de bundle dans tous les projets.
  - Configurer le **Entitlements.plist** pour les groupes d’application (si nécessaire).

* Puis suivez les instructions pour :
  - [Déployer sur une Apple Watch pour le test](~/ios/watchos/deploy-test/device.md), ou
  - [Charger sur l’App Store](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"/>

## <a name="app-ids"></a>ID d’application

Comme indiqué dans le [instructions d’installation](~/ios/watchos/get-started/installation.md), tous les trois projets dans une application Watch associées ID d’ensemble, telles que :

- Projet de Unified Xamarin.iOS- `com.xamarin.WatchKitCatalog`
- Projet de WatchKit Extension : `com.xamarin.WatchKitCatalog.watchkitextension`
- Projet d’application Watch- `com.xamarin.WatchKitCatalog.watchkitapp`

Les trois projets nécessitent un correspondant Distribution profil de provisionnement, soit à l’aide explicitement pour chaque ID d’application ou un caractère générique ID d’application.

### <a name="explicit-app-ids"></a>ID d’application explicite

Créer un **ID d’application** ID d’offre groupée de chaque projet (ce qui se présente comme suit sur le centre de développement iOS) :

![Les ID d’offre groupée dans le centre de développement iOS](images/appids-specific-sml.png)

Lorsque vous créez ou configurez les ID d’application, n’oubliez pas d’activer les fonctionnalités spécifiques de que votre application a besoin. Cela peut inclure des notifications push et les groupes d’applications.

Vous devez créer un profil de provisionnement de Distribution pour chaque application de ID.

### <a name="wildcard-app-id"></a>ID d’application générique

Vous pouvez également créer un caractère générique **ID d’application** qui correspond à tous les trois projets, tels que `com.xamarin.*`.

Notez que certaines fonctionnalités ne peut pas être utilisées avec un ID d’application générique (par exemple, des notifications push). Si votre application nécessite ces fonctionnalités, vous devez créer des ID d’application explicites.

Pour la distribution, vous devez uniquement créer un profil de provisionnement Distribution pour le caractère générique ID d’application.

<a name="App_Groups" />

## <a name="app-groups"></a>App Groups

Vous pouvez utiliser un groupe de l’application de partager des données entre votre application iOS et l’Extension Watch. Vous devez vous assurer que votre solution a :

- Configuré le **App Group** dans le portail des développeurs Apple **certificats, identificateurs et profils** section.

- Activé **App Groups** (et fourni la **ID de groupe application**) dans *à la fois* l’application iOS et l’Extension Watch **ID d’application** et  **Entitlements.plist**.

### <a name="certificates-identifiers--profiles"></a>Certificats, identificateurs et profils

Pour utiliser un groupe de l’application, créez une entrée dans le **App Groups** écran. Dans l’exemple ci-dessous, le groupe est nommé avec le même style DNS inverse qui est couramment utilisé pour l’ID d’application, mais avec la `group.` préfixe (qui est requis) :

![L’identificateur](images/appgroups-new-sml.png)

Le groupe de l’application apparaît alors dans la liste :

![La liste d’identificateur](images/appgroups-setup-sml.png)

Une fois que le groupe est créé, il peut être référencé dans votre **ID d’application** configuration. N’oubliez pas d’inclure à la fois l’application iOS et Extension Watch **ID d’application**.

![Configurations disponibles](images/appgroups-sml.png)

Faire **pas** activer App Groups dans le code d’application d’Apple Watch. Il n’est pas requis pour être activé sur l’Apple watch elle-même.

### <a name="entitlementsplist"></a>Entitlements.plist

Certaines fonctionnalités d’application (par exemple). Groupes d’applications) vous obligent à définir vos droits.
Double-cliquez pour modifier la **Entitlements.plist** fichier dans ces projets :

- projet d’application iOS
- Projet d’Extension Watch

.![L’éditeur Entitlements.plist](images/entitlements-plist-sml.png)

Faire **pas** activez les droits dans le projet d’application Watch. Il n’est pas requis pour être activé sur l’Apple watch elle-même.

## <a name="related-links"></a>Liens associés

- [Guide de présentation WatchKit Apple](https://developer.apple.com/app-store/watch/)
