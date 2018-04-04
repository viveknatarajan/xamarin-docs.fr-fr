---
title: Limitations
description: Certaines limitations de Live Xamarin Player
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: 0068540ec385ab3be56865c7728eb3128154a2ea
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="limitations"></a>Limitations

![Fonctionnalité d’aperçu](~/media/shared/preview.png)

## <a name="device-requirements"></a>Configuration requise du périphérique
L’application Xamarin Live Player prend en charge les périphériques suivants :

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 ou version ultérieure.
- ARM-v7a, ARM-v8a, ARM64-v8a, x 86 ou x86_64 processeur.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 ou version ultérieure.
- Processeur ARM64.

-----

## <a name="limitations"></a>Limitations

Il existe certaines restrictions sur les éléments que xamarin Live Player peuvent s’exécuter, y compris les éléments ci-dessous :

### <a name="xamarinforms"></a>Xamarin.Forms
- Les convertisseurs personnalisés ne sont pas pris en charge.
- Effets ne sont pas pris en charge.
- Contrôles personnalisés avec des propriétés personnalisées pouvant être liés ne sont pas pris en charge.
- Les ressources incorporées ne sont pas pris en charge (ie. l’incorporation d’images ou autres ressources dans une bibliothèque PCL).
- Infrastructures MVVM tiers ne sont pas pris en charge (c'est-à-dire,). Prisme, Mvvm Cross, Mvvm Light, etc.).
- Catalogues Asset sur iOS ne sont pas pris en charge.

### <a name="other-project-types"></a>Autres types de projet
- Le lecteur en direct n’est pas destiné pour Android natif ou les projets iOS (qui utilisent XML Android ou Storyboards pour l’interface utilisateur).

### <a name="misc"></a>Divers
- Prise en charge pour la réflexion limitée (actuellement affecte certains NuGets populaires, tels que SQLite et Json.NET). Autres NuGets peut toujours être pris en charge.
- Certaines classes système ne peut pas être remplacées (par exemple, vous ne peut pas implémenter une sous-classe).
- Certaines fonctionnalités de plateforme qui nécessitent la mise en service ne peut pas fonctionner dans l’application Xamarin Live Player (toutefois qu’il a été configuré pour les opérations courantes telles que l’accès de la galerie photo).
- Cibles personnalisées et des étapes de génération sont ignorés. Par exemple, les outils tels que Fody, Retit, AutoFac, et AutoMapper ne peut pas être incorporé.
- Projets F # ne sont pas pris en charge sur Android et la prise en charge sur iOS
- Scénarios avancés avec des classes génériques personnalisés et les interfaces ne peuvent pas pris en charge.

Veuillez signaler d’autres problèmes sur [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Liens associés

- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Installation](~/tools/live-player/install.md)
