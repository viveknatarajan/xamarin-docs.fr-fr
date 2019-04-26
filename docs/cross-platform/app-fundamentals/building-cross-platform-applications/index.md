---
title: Création d’applications multiplateformes
description: Cette section présente un résumé, plus six parties, comment créer des applications à l’aide de la plateforme de développement Xamarin – à partir de comprendre le fonctionne de Xamarin à la conception d’applications mobiles et de test et de déploiement sur les différents magasins d’applications.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 683400e24844308769f0562552641216d45e7d11
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61276373"
---
# <a name="building-cross-platform-applications"></a>Création d’applications multiplateformes

Il existe deux options pour partager du code entre les applications mobiles multiplateforme : Partagé Asset projets et des bibliothèques de classes portables. Ces options sont [abordées ici](~/cross-platform/app-fundamentals/code-sharing.md); plus d’informations sur [bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md) et [les projets partagés](~/cross-platform/app-fundamentals/shared-projects.md) est également disponible.

<a name="Sections" />

 [Vue d’ensemble](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Partie 1 – Présentation de la plateforme Mobile Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Partie 2 : Architecture](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Partie 3 : configuration d’une Solution multiplateforme Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Partie 4 – affaire à plusieurs plateformes](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Partie 5 – stratégies de partage de Code pratiques](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Partie 6 : Test et approbations de l’App Store](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>Études de cas

Les principes présentés dans ce document sont placés dans les exercices pratiques dans l’exemple d’application *Tasky*, ainsi que [prédéfinis applications](https://xamarin.com/prebuilt) comme [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky est une application de liste de tâches simples pour iOS, Android et Windows Phone.
Il illustre les principes fondamentaux de la création d’une application multiplateforme avec Xamarin et utilise une base de données SQLite locale.

 [![tasky list](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [![tasky list](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Lire le [étude de cas Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).

## <a name="summary"></a>Récapitulatif

Cette section présente les outils de développement de Xamarin et explique comment créer des applications qui ciblent plusieurs plateformes mobiles.

Il couvre une architecture en couches ce code de structures pour une réutilisation sur plusieurs plateformes et décrit les modèles de logiciels différents qui peuvent être utilisées dans cette architecture.

Des exemples sont donnés de fonctions d’application courantes (par exemple, les opérations de fichier et de réseau) et comment elles peuvent reposer de façon inter-plateformes.

Enfin, il brièvement présente le test et fournit des références à une étude de cas qui place ces principes en action.

## <a name="related-links"></a>Liens associés

- [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md)
- [Étude de cas : Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky exemple d’application (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Développement d’applications Mobile Xamarin : Cross-Platform C# et Xamarin.Forms, principes de base (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Développement mobile avec C# par Greg manilles (o ' Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Professionnel Cross-Platform Mobile Development dans C# par Scott Olson, John Hunter, Ben Horgen, Kenny présente (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
