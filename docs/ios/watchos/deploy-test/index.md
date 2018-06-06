---
title: Déployer et tester des applications watchOS avec Xamarin
description: Ce document décrit comment déployer et tester des applications watchOS développées avec Xamarin. Il fournit une liste de vérification de déploiement, explique explicite et l’application générique ID et examine les groupes d’applications.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 778583456e74bb7ed3a85dce96bcdbc487aef57a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790940"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Déployer et tester des applications watchOS avec Xamarin

## <a name="deployment-checklist"></a>Liste de vérification de déploiement

Si vous déployez à un test de surveillance, ou télécharger vers l’App Store, vous devez effectuer les étapes de cette page :

- Dans le **iOS Dev Center**:
  - [ID d’application](#App_IDs) ont été créés.
  - [Groupes d’applications](#App_Groups) configuré (si nécessaire).
  - Distribution de configuration ou les profils créés

- Dans votre solution :

  - Vérifiez le [ID d’ensemble et références de projet](~/ios/watchos/get-started/installation.md) sont définies.
  - Vérifiez vos icônes sont [configuré correctement](~/ios/watchos/app-fundamentals/icons.md).
  - Vérifiez la correspondance de numéros de version offre groupée dans tous les projets.
  - Configurer le **Entitlements.plist** pour les groupes d’application (si nécessaire).

* Puis suivez les instructions pour :
  - [Déployer sur un Apple Watch pour le test](~/ios/watchos/deploy-test/device.md), ou
  - [Télécharger vers le magasin d’applications](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"/>

## <a name="app-ids"></a>ID d’application

Comme indiqué dans le [instructions d’installation](~/ios/watchos/get-started/installation.md), tous les trois projets dans une application de surveillance ont liées ID d’ensemble, telles que :

- Projet d’unifié Xamarin.iOS- `com.xamarin.WatchKitCatalog`
- Projet d’Extension de WatchKit- `com.xamarin.WatchKitCatalog.watchkitextension`
- Projet d’application espion- `com.xamarin.WatchKitCatalog.watchkitapp`

Les trois projets nécessitent une Distribution de configuration profil correspondant, à l’aide explicitement pour chaque ID d’application ou un caractère générique identifiant d’application.

### <a name="explicit-app-ids"></a>ID d’application explicite

Créer un **ID d’application** pour l’ID d’offre groupée de chaque chaque projet (qui doit ressembler à ceci sur le centre de développement iOS) :

![Les ID d’ensemble dans le centre de développement iOS](images/appids-specific-sml.png)

Lors de la création ou la configuration des ID de l’application, pensez à activer les fonctionnalités spécifiques requises par votre application. Cela peut inclure des notifications push et groupes d’applications.

Vous devez créer un profil de configuration de Distribution pour chaque application de ID.

### <a name="wildcard-app-id"></a>ID d’application générique

Vous pouvez également créer un caractère générique **ID d’application** qui représente tous les trois projets, tel que `com.xamarin.*`.

Notez que certaines fonctionnalités ne peut pas être utilisées avec un ID d’application générique (par exemple, des notifications push). Si votre application requiert ces fonctionnalités, vous devez créer les ID d’application explicite.

Pour la distribution, vous devez uniquement créer un profil de configuration Distribution pour le caractère générique identifiant d’application.

<a name="App_Groups" />

## <a name="app-groups"></a>App Groups

Vous pouvez utiliser un groupe d’application de partager des données entre votre application iOS et l’Extension de surveillance. Vous devez vous assurer que votre solution :

- Configuré le **application groupe** dans le portail des développeurs Apple **certificats, les identificateurs et les profils** section.

- Activé **App Groups** (et fourni la **ID de groupe d’application**) dans *les deux* l’application iOS et l’Extension de surveillance **ID d’application** et  **Entitlements.plist**.

### <a name="certificates-identifiers--profiles"></a>Les certificats, les identificateurs et les profils

Pour utiliser un groupe d’application, créez une entrée dans le **App Groups** écran. Dans l’exemple ci-dessous, le groupe est nommé avec le même style DNS inverse qui est couramment utilisé pour l’ID de l’application, mais avec la `group.` préfixe (qui est requis) :

![L’identificateur](images/appgroups-new-sml.png)

Le groupe de l’application apparaît dans la liste :

![La liste d’identificateur](images/appgroups-setup-sml.png)

Une fois que le groupe est créé, il peut être référencée dans votre **ID d’application** configuration. N’oubliez pas d’inclure l’iOS application et l’Extension de surveillance **ID de l’application**.

![Consifurations disponibles](images/appgroups-sml.png)

Faire **pas** activer App Groups dans le code d’application d’Apple Watch. Il n’est pas nécessaire d’être activé sur la montre.

### <a name="entitlementsplist"></a>Entitlements.plist

Certaines fonctionnalités d’application (par exemple). Groupes d’applications) nécessitent que vous permet de définir vos droits.
Double-cliquez pour modifier la **Entitlements.plist** fichier dans ces projets :

- projet d’application iOS
- Projet d’Extension espion

.![L’éditeur Entitlements.plist](images/entitlements-plist-sml.png)

Faire **pas** activez les droits dans le projet d’application de surveillance. Il n’est pas nécessaire d’être activé sur la montre.

## <a name="related-links"></a>Liens associés

- [Guide de présentation WatchKit Apple](https://developer.apple.com/app-store/watch/)
