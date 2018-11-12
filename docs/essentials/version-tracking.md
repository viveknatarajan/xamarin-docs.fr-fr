---
title: 'Xamarin.Essentials : Suivi de Version'
description: La classe VersionTracking vous permet de vérifier la version de votre application et son numéro de build. Cette classe vous permet également d'obtenir des informations supplémentaires telles que si c’est la première fois l’application lancée.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1eae0bf7c21dd7efa7655633896bdb2897f9d782
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674858"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials : Suivi de Version

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **VersionTracking** vous permet de connaître les numéros de version et de build des applications. Elle vous permet également de voir des informations supplémentaires, par exemple le premier lancement de l’application ou, pour la version actuelle, d’obtenir les informations de build précédentes, etc.

## <a name="using-version-tracking"></a>Utilisation de **VersionTracking**

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-version-tracking"></a>Utilisation du suivi des versions

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La première fois que vous utilisez la classe **VersionTracking** elle démarrera le suivi de la version actuelle. Vous devez appeler la fonction `Track` au plus tôt dans votre application, à chaque fois que celle-ci est lancée afin de s'assurer que les informations sur la version actuelle soient suivies :

```csharp
VersionTracking.Track();
```

Après le premier appel de `Track` vous pouvez lire les informations de version :

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre de la plateforme

Toutes les informations de version sont stockées à l’aide des[préférences](preferences.md) de Xamarin.Essentials, sont stockées avec comme nom de fichier **[votre-application-PACKAGE-ID].xamarinessentials.versiontracking** et suivent la même persistance des données que décrite dans la documentation des [préférences](preferences.md#persistence).

## <a name="api"></a>API

- [Code source de la fonctionnalité de suivi des versions](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Documentation sur l’API de suivi des versions](xref:Xamarin.Essentials.VersionTracking)
