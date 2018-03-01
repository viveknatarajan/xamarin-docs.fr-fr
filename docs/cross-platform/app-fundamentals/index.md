---
title: Notions de base des applications
description: "Principaux Concepts d’applications"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 9e4e7705e1ca29b6abf716a48ae3fa0e7c1a19ec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>Notions de base des applications

Cette section fournit un guide sur certains des plus courants des tâches de choses ou concepts que les développeurs doivent connaître lors du développement d’applications mobiles.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

En choisissant de Xamarin et en conservant les quelques éléments à l’esprit lorsque vous concevez et développez vos applications mobiles, vous pouvez réaliser code considérable partage entre les plateformes mobiles, réduire le temps de commercialisation, tirer parti des compétences existantes, répondre à la demande du client pour l’accès mobile, et réduire la complexité d’inter-plateformes. &nbsp;Ce document présente des recommandations principales pour tirer parti de ces avantages pour les applications d’utilitaire et de productivité.

## <a name="code-sharing-optionscode-sharingmd"></a>[Options de partage de code](code-sharing.md)

En savoir plus sur le code de différentes options disponibles pour les projets de Xamarin, y compris les bibliothèques de classes portables (PCLs), les projets partagés et des bibliothèques Standard de .NET de partage.


## <a name="accessibilityaccessibilitymd"></a>[Accessibilité](accessibility.md)

Conseils pour la création d’applications accessibles.


## <a name="localizationlocalizationmd"></a>[Localisation](localization.md)

Instructions pour créer des applications prenant en charge les paramètres régionaux qui peuvent être traduites dans plusieurs langues.


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

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[Cross-Platform l’accès aux données](~/xamarin-forms/data-cloud/index.md)

La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données. iOS et Android ont le moteur de base de données SQLite « intégré » et accès pour stocker et récupérer des données est simplifié par la plateforme de Xamarin. Le [accès aux données Android](~/android/data-cloud/data-access/index.md), [iOS l’accès aux données](~/ios/data-cloud/data/index.md), et [Xamarin.Forms l’accès aux données](~/xamarin-forms/data-cloud/index.md) guides fournissent des exemples d’accès aux SQLite sur chaque plateforme.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[Sécurité de la couche transport](transport-layer-security.md)

Informations sur l’implémentation de SSL/TLS correcte selectingthe pour sécuriser la connectivité de réseau de votre application.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[Notifications](~/xamarin-forms/data-cloud/push-notifications/index.md)

Applications mobiles utilisent des notifications discrètement d’informer l’utilisateur des événements d’application spécifique s’est produit. Les notifications sont généralement utilisées pour informer l’utilisateur de l’état d’un processus d’application qui s’exécute en arrière-plan. Un exemple peut télécharger un fichier volumineux. Ce fichier peut prendre beaucoup de temps de téléchargement, pour cette activité doit avoir lieu en arrière-plan. Lorsque le téléchargement est terminé, l’utilisateur est informé du fait par une notification.
En outre, notification ares pas uniquement limité à des applications locales. Il est également possible pour les applications serveur publier des notifications à des applications mobiles. Cet article explique comment utiliser des notifications sur Android et iOS.
