---
title: Projets de bibliothèque NuGet multiplateforme (également appelé Nugetizer 3000)
description: Ce document décrit comment utiliser l’outil de Nugetizer 3000 pour créer automatiquement des packages NuGet pour partager du code entre plateformes.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1d48bc28aa4477361ca8057fda91ee3258f36a73
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270426"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>Projets de bibliothèque multiplateforme NuGet (Nugetizer 3000)

_Créer automatiquement des packages NuGet pour partager du code entre plateformes à l’aide de la Nugetizer 3000 !_

Il est possible de créer automatiquement des packages NuGet pour partager du code entre plateformes via les _Nugetizer 3000_. Il est possible de créer des packages NuGet à partir de projets de bibliothèque existants ou en créant un nouveau **projet de bibliothèque multiplateforme**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Le Nugetizer 3000 est inclus avec Visual Studio pour Mac &ndash; recherchez le **bibliothèque > Mulitplatform bibliothèque** projet type dans le **fichier > nouveau** fenêtre :

[![](images/mulitplatform-library-sml.png "Créer la nouvelle fenêtre de la bibliothèque de plateformes multiples")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour utiliser le Nugetizer 3000 dans Visual Studio, veuillez [Téléchargez et exécutez le programme d’installation VSIX](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Génération des packages NuGet

Une fois configuré, chaque build du projet génère un package NuGet terminé, ce qui peut être utilisé pour partager du code avec d’autres applications en interne ou téléchargé vers [NuGet.org](https://www.nuget.org).

Il existe trois scénarios d’utilisation de cette fonctionnalité :

- [Projets de bibliothèque existants](existing-library.md)

  Créer un package NuGet à partir de projets de bibliothèque de classes portable (ou .NET Standard) existants.

- [Création d’un nouveau projet de bibliothèque multiplateforme](single-codebase.md)

  Créer une nouvelle bibliothèque pour partager du code courants via NuGet, à l’aide d’une bibliothèque de classes portable ou .NET Standard.

- [Création de nouveaux projets de bibliothèque spécifique à la plateforme](platform-specific.md)

  Créer une nouvelle bibliothèque et NuGet qui inclut le code spécifique à la plateforme pour iOS et Android et utilise un projet partagé pour contenir le code commun et les projets spécifiques à la plateforme pour prendre en charge les fonctionnalités spécifiques à iOS ou Android.

Reportez-vous à la [guide sur les métadonnées](metadata.md) pour plus d’informations sur les métadonnées obligatoires et facultatifs qui doivent être ajoutées à n’importe quel package NuGet.

## <a name="further-nuget-information"></a>Plus d’informations de NuGet

En savoir plus sur [création manuelle des packages NuGet pour Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) et comment [inclure un package NuGet dans une application](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Microsoft [Documentation de NuGet](https://docs.microsoft.com/nuget/) contient des informations plus détaillées sur la **.nupkg** format et à l’aide de packages NuGet dans Visual Studio.

La discussion de conception pour les projets de Package NuGet (également appelé) NuGetizer 3000) est disponible sur le [dépôt NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

## <a name="related-links"></a>Liens associés

- [Cas d’utilisation de NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Créer manuellement des Packages NuGet pour Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Documentation de NuGet](https://docs.microsoft.com/nuget/)
- [Notes de publication](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
