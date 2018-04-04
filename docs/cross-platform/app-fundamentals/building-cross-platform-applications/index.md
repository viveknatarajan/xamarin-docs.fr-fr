---
title: Building Cross-Platform Applications
description: Dans un résumé plus de six parties, cette section décrit comment créer des applications à l’aide de la plateforme de développement Xamarin – permet de comprendre comment Xamarin fonctionne à la conception d’applications mobiles et de test et de déploiement pour les magasins d’applications différentes.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: b9d167d8874ecfde9094bacc26cff3e7d1b10bab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="sharing-code-options"></a>Options de partage de Code

Il existe deux options pour le partage de code entre les applications mobiles multiplateforme : projets d’actifs partagés et des bibliothèques de classes portables. Ces options sont [présentées ici](~/cross-platform/app-fundamentals/code-sharing.md); plus d’informations sur [bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md) et [les projets partagés](~/cross-platform/app-fundamentals/shared-projects.md) est également disponible.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>Building Cross-Platform Mobile Apps

 [Vue d’ensemble](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Partie 1 : présentation de la plateforme Mobile de Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Partie 2 : Architecture](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Partie 3 : configuration d’une Solution de plateforme Xamarin croisée](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Partie 4 – vous traitez des plateformes multiples](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Partie 5 – Code pratique des stratégies de partage](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Partie 6 : Test et approbations de l’App Store](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>Études de cas

Les principes présentés dans ce document sont placées dans la pratique dans l’exemple d’application *Tasky*, ainsi que [prégénérées applications](https://xamarin.com/prebuilt) comme [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky est une application de liste de tâches simple pour iOS, Android et Windows Phone.
Il illustre les principes fondamentaux de la création d’une application multiplateforme avec Xamarin et utilise une base de données local SQLite.

 [![liste tasky](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky liste](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Lecture la [étude de cas Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).


## <a name="summary"></a>Récapitulatif

Cette section présente les outils de développement de Xamarin et explique comment créer des applications qui visent plusieurs plateformes mobiles.

Il couvre une architecture multiniveau que le code pour une réutilisation des structures sur plusieurs plateformes et décrit les modèles de logiciels différents qui peuvent être utilisées dans cette architecture.

Des exemples sont donnés de fonctions d’application courants (tels que les opérations de fichier et de réseau) et comment ils peuvent être assemblés de manière inter-plateformes.

Pour finir, il brièvement présente le test et fournit des références à une étude de cas qui place ces principes en action.



## <a name="related-links"></a>Liens associés

- [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md)
- [Étude de cas : Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky exemple d’application (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Développement d’applications mobiles de Xamarin : Inter-plateformes c# et notions de base Xamarin.Forms](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [Développement pour appareils mobiles avec c#, par Greg manilles (o ' Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Professionnel développement Mobile multiplateforme en c# par Scott Olson, John analyseur, Ben Horgen, Kenny présente (Wrox)](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
