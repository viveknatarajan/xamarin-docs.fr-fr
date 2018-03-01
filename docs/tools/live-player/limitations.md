---
title: Limitations
description: Certaines limitations de Live Xamarin Player
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Limitations

![Fonctionnalité d’aperçu](~/media/shared/preview.png)

## <a name="device-requirements"></a>Configuration requise du périphérique
L’application Xamarin Live Player prend en charge les périphériques suivants :

### <a name="ios"></a>iOS
- iOS 9.0 ou version ultérieure.
- Processeur ARM64.
- Vérifiez le [App Store](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) pour une liste des périphériques pris en charge.

### <a name="android"></a>Android
- Android 4.2 ou version ultérieure.
- ARM-v7a, ARM-v8a, ARM64-v8a, x 86 ou x86_64 processeur.


## <a name="limitations"></a>Limitations

Il existe certaines restrictions sur les éléments que xamarin Live Player peuvent s’exécuter, y compris les éléments ci-dessous :

### <a name="android"></a>Android
- Les interfaces utilisateur Android conçues avec les fichiers AXML ne sont pas pris en charge actuellement.

### <a name="ios"></a>iOS
- Certaines fonctionnalités d’animation iOS ne sont pas pris en charge.
- les fichiers XIB iOS ne sont pas pris en charge.

### <a name="xamarinforms"></a>Xamarin.Forms
- Les convertisseurs personnalisés ne sont pas pris en charge.
- Effets ne sont pas pris en charge.
- Contrôles personnalisés avec des propriétés personnalisées pouvant être liés ne sont pas pris en charge.
- Les ressources incorporées ne sont pas pris en charge (ie. l’incorporation d’images ou autres ressources dans une bibliothèque PCL).

### <a name="misc"></a>Divers
- Prise en charge pour la réflexion limitée (actuellement affecte certains NuGets populaires, tels que SQLite et Json.NET). Autres NuGets sont toujours prises en charge.
- Certaines classes système ne peut pas être remplacées (par exemple, vous ne peut pas implémenter une sous-classe).
- Certaines fonctionnalités de plateforme qui nécessitent la mise en service ne peut pas fonctionner dans l’application Xamarin Live Player (toutefois qu’il a été configuré pour les opérations courantes telles que l’accès de la galerie photo).
- Cibles personnalisées et des étapes de génération sont ignorés. Par exemple, des outils tels que Fody ne peut pas être incorporés.

> [!WARNING]
> **Remarque :** Xamarin Live Player ne fonctionne pas avec Xamarin Studio

Veuillez signaler d’autres problèmes sur [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Liens associés

- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Installation](~/tools/live-player/install.md)
