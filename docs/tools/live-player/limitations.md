---
title: Limitations de Xamarin Live Player
description: Ce document décrit les limitations de Xamarin Live Player. Il traite des exigences de l’appareil, les fonctionnalités fonctionne avec les types de projets et d’autres rubriques divers.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: aff6990df1b710190f11c2d7fa09c8399e94f8af
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "40251033"
---
# <a name="limitations-of-xamarin-live-player"></a>Limitations de Xamarin Live Player

![Fonctionnalité en version préliminaire](~/media/shared/preview.png)

## <a name="device-requirements"></a>Exigences de l’appareil

L’application Xamarin Live Player prend en charge les appareils Android suivants :

- Android 4.2 ou version ultérieure.
- ARM-v7a, v8a de ARM, ARM64-v8a, x 86 ou x86_64 processeur.

## <a name="limitations"></a>Limitations

Il existe certaines limitations sur les choses que xamarin Live Player peut exécuter, y compris les éléments ci-dessous :

### <a name="ios"></a>iOS

Live Player n’est pas disponible pour iOS.

### <a name="xamarinforms"></a>Xamarin.Forms

- Renderers personnalisés ne sont pas pris en charge.
- Effets ne sont pas pris en charge.
- Contrôles personnalisés avec des propriétés pouvant être liées personnalisés ne sont pas pris en charge.
- Ressources incorporées ne sont pas pris en charge (ie. l’incorporation d’images ou autres ressources dans une bibliothèque de classes portable).
- Infrastructures MVVM de tiers ne sont pas pris en charge (p. ex. Prism, Mvvm Cross, Mvvm Light, etc.).

### <a name="other-project-types"></a>Autres types de projets

- Live Player n’est pas destinée à être des projets Android natifs (qui utilisent Android XML pour l’interface utilisateur).

### <a name="misc"></a>Divers

- Prise en charge pour la réflexion limitée (actuellement affecte certains packages NuGet populaires, telles que SQLite et Json.NET). Autres packages NuGet peut toujours être pris en charge.
- Certaines classes système ne peut pas être remplacés (par exemple, vous ne peut pas implémenter une sous-classe).
- Certaines fonctionnalités de plateforme qui nécessitent un approvisionnement ne fonctionnent pas dans l’application Xamarin Live Player (Toutefois il a été configuré pour les opérations courantes telles que l’accès de galerie de photos).
- Cibles personnalisées et les étapes de génération sont ignorés. Par exemple, les outils tels que Fody, réajuster, AutoFac et AutoMapper ne peut pas être incorporées.
- Projets F # ne sont pas pris en charge.
- Scénarios avancés avec des classes génériques personnalisés et les interfaces ne peuvent pas pris en charge.

Utilisez **signaler un problème** dans [Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017) ou [Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem) pour signaler des problèmes avec Xamarin Live Player.

## <a name="related-links"></a>Liens associés

- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Installation](~/tools/live-player/install.md)
