---
title: Comment utiliser Google Maps et un emplacement avec Xamarin.Android
description: Cet article explique comment utiliser des cartes et emplacement avec Xamarin.Android. Il couvre tout depuis en tirant parti de l’application des cartes intégrées à l’utilisation de la V2 mappe des API Android de Google directement.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: 78908e1b1ad6d3f572ce45c8189d8f4ee48ad6ee
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61176565"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Comment utiliser Google Maps et un emplacement avec Xamarin.Android

_Cet article explique comment utiliser des cartes et emplacement avec Xamarin.Android. Il couvre tout depuis en tirant parti de l’application des cartes intégrées à l’utilisation de la V2 mappe des API Android de Google directement._

## <a name="maps-overview"></a>Vue d’ensemble de mappages

Technologies de mappage sont un complément omniprésent sur des appareils mobiles. Les ordinateurs portables et ordinateurs de bureau n’ont tendance à avoir connaissance des emplacements intégrée. Quant à eux, les périphériques mobiles utilisent ces applications pour localiser les appareils et pour afficher des informations d’emplacement variation. Android a technologie puissante et intégrée qui affiche les données d’emplacement sur les cartes à l’aide de matériel d’emplacement qui peut-être être disponible sur l’appareil. Cet article aborde un spectre de ce que les applications de cartes sous Xamarin.Android ont à offrir, notamment : 

-  À l’aide intégrée mappe l’application pour ajouter rapidement des fonctionnalités de mappage.
-  Utilisation de l’API de cartes pour contrôler l’affichage d’une carte.
-  À l’aide de diverses techniques pour ajouter des superpositions de graphiques.

Les rubriques de cette section couvrent une large gamme de fonctionnalités de mappage.
Tout d’abord, elles expliquent comment tirer parti des applications de cartes intégrées d’Android et comment afficher une vue panoramique rue d’un emplacement. Puis, ils abordent l’utilisation de l’API de cartes pour incorporer des fonctions de mappage directement dans une application, qui décrivent les deux le contrôle la position et l’affichage d’une carte, ainsi que l’ajout de superpositions graphiques.


## <a name="related-links"></a>Liens associés

- [MapsAndLocationDemo_v3 (sample)](https://developer.xamarin.com/samples/monodroid/MapsAndLocationDemo_v3/)
- [Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Obtention d’une clé API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Liste des intentions : Ouverture des Applications de Google sur les appareils Android](https://developer.android.com/guide/appendix/g-app-intents.html)
- [Emplacement et mappages](https://developer.android.com/guide/topics/location/index.html)
