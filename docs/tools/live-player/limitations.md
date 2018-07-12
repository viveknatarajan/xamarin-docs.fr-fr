---
title: Limitations de Xamarin Live Player
description: Ce document décrit les limitations de Xamarin Live Player. Il traite des exigences de l’appareil, les fonctionnalités fonctionne avec les types de projets et d’autres rubriques divers.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815280"
---
# <a name="limitations-of-xamarin-live-player"></a>Limitations de Xamarin Live Player

![Fonctionnalité en version préliminaire](~/media/shared/preview.png)

## <a name="device-requirements"></a>Exigences de l’appareil
L’application Xamarin Live Player prend en charge les appareils suivants :

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 ou version ultérieure.
- ARM-v7a, v8a de ARM, ARM64-v8a, x 86 ou x86_64 processeur.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 ou version ultérieure.
- Processeur de ARM64.

-----

## <a name="limitations"></a>Limitations

Il existe certaines limitations sur les choses que xamarin Live Player peut exécuter, y compris les éléments ci-dessous :

### <a name="xamarinforms"></a>Xamarin.Forms

- Renderers personnalisés ne sont pas pris en charge.
- Effets ne sont pas pris en charge.
- Contrôles personnalisés avec des propriétés pouvant être liées personnalisés ne sont pas pris en charge.
- Ressources incorporées ne sont pas pris en charge (ie. l’incorporation d’images ou autres ressources dans une bibliothèque de classes portable).
- Infrastructures MVVM de tiers ne sont pas pris en charge (p. ex. Prism, Mvvm Cross, Mvvm Light, etc.).
- Catalogues de composants sur iOS ne sont pas pris en charge.

### <a name="other-project-types"></a>Autres types de projets

- Ne vise pas Live Player pour Android natif ou les projets iOS (qui utilisent Android XML ou des tables de montage séquentiel pour l’interface utilisateur).

### <a name="misc"></a>Divers

- Prise en charge pour la réflexion limitée (actuellement affecte certains packages NuGet populaires, telles que SQLite et Json.NET). Autres packages NuGet peut toujours être pris en charge.
- Certaines classes système ne peut pas être remplacés (par exemple, vous ne peut pas implémenter une sous-classe).
- Certaines fonctionnalités de plateforme qui nécessitent un approvisionnement ne fonctionnent pas dans l’application Xamarin Live Player (Toutefois il a été configuré pour les opérations courantes telles que l’accès de galerie de photos).
- Cibles personnalisées et les étapes de génération sont ignorés. Par exemple, les outils tels que Fody, réajuster, AutoFac et AutoMapper ne peut pas être incorporées.
- Projets F # ne sont pas pris en charge sur Android et prise en charge sur iOS limitée
- Scénarios avancés avec des classes génériques personnalisés et les interfaces ne peuvent pas pris en charge.

Signalez les problèmes supplémentaires sur [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Liens associés

- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Installation](~/tools/live-player/install.md)
