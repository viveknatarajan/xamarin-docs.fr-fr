---
title: Partage de code
description: Principaux Concepts d’applications
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 4cca202627d1b901e00532c92598ffddd48b4853
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code"></a>Partage de code

Cette section fournit un guide sur certains des plus courants des tâches de choses ou concepts que les développeurs doivent connaître lors du développement d’applications mobiles.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Vue d’ensemble du partage de code](code-sharing.md)

En savoir plus sur le code de différentes options disponibles pour les projets de Xamarin, y compris les bibliothèques de classes portables (PCLs), les projets partagés et des bibliothèques Standard de .NET de partage.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md)

Les projets de bibliothèque de classes portables vous permettent de créer et distribuer des assemblys qui contiennent du code partagé pour s’exécuter sur plusieurs plateformes. Pour créer une bibliothèque de classes portables (ou « PCL »), vous sélectionnez tout d’abord les plateformes à cibler, puis écrire du code par rapport à un jeu secondaire du .NET Framework qui est disponible dans le profil défini pour ces plateformes. Ce document décrit comment créer et utiliser PCLs avec Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)

Projets partagés vous permettent d’écrire du code commun qui est référencé par un nombre de projets d’application différent. Le code est compilé dans le cadre de chaque projet de référence et peut inclure des directives de compilateur pour aider à intégrer des fonctionnalités spécifiques à la plateforme dans la base de code partagé. Cet article explique comment les projets partagés fonctionnent et comment les créer et les utiliser avec les projets de Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard est une nouvelle option de partage de code entre plateformes. Il fonctionne de façon similaire aux bibliothèques de classes Portable ; code repose sur une version spécifique (actuellement 1.0 via 1.6) et peut ensuite être consommé par d’autres projets qui prennent en charge ce niveau ou une version ultérieure. Projets .NET standard sont pris en charge dans Visual Studio, Visual Studio pour Windows et Xamarin Studio 6.2 pour Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projets : Les bibliothèques multiplateformes pour le partage de Code](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Les packages NuGet peuvent être générés automatiquement à partir de projets standards PCL ou .NET ; et les projets partagés peuvent être insérés dans les packages NuGet « commutateur et de leurre » à l’aide du type de projet NuGet distinct. Cette section explique comment créer des packages NuGet pour chaque scénario de partage de code.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Créer manuellement les Packages NuGet pour Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Conseils de création de packages NuGet qui fonctionnent avec la plateforme de Xamarin.
