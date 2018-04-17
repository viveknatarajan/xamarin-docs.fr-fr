---
title: Services de données et cloud
description: Xamarin.Forms applications peuvent consommer des services web implémentés à l’aide d’un large éventail de technologies, et ce guide examine comment effectuer cette opération.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="data--cloud-services"></a>Services de données et cloud

_Xamarin.Forms applications peuvent consommer des services web implémentés à l’aide d’un large éventail de technologies, et ce guide examine comment effectuer cette opération._

Pour une présentation de la consommation de services web d’inter-plateformes sur la plateforme de Xamarin, consultez [Introduction aux Services Web](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Comprendre l’exemple](~/xamarin-forms/data-cloud/walkthrough.md)

Cet article fournit une procédure pas à pas de l’exemple d’application Xamarin.Forms qui montre comment communiquer avec les services web différents. Rubriques couvertes incluent la composition de l’application, les pages, le modèle de données et l’appel d’opérations de service web.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Consommation de services web](~/xamarin-forms/data-cloud/consuming/index.md)

Ce guide montre comment communiquer avec les services web différents pour fournir créer, lire, mettre à jour et supprimer des fonctionnalités à une application de Xamarin.Forms (CRUD). Rubriques couvertes incluent la communication avec [services ASMX](consuming/asmx.md), [services WCF](consuming/wcf.md), [services REST](consuming/rest.md), et [Azure Mobile Apps](consuming/azure.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Authentification de l’accès aux services web](~/xamarin-forms/data-cloud/authentication/index.md)

Ce guide explique comment intégrer des services d’authentification à une application de Xamarin.Forms pour permettre aux utilisateurs de partager un serveur principal lors de l’accès uniquement à leurs propres données. Les rubriques couvertes incluent [à l’aide de l’authentification de base avec un service REST](authentication/rest.md), [à l’aide du composant Xamarin.Auth pour s’authentifier auprès de fournisseurs d’identité OAuth](authentication/oauth.md)et à l’aide de l’authentification intégrée mécanismes de prestation [Azure Mobile Apps](authentication/azure.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Synchronisation des données avec les services web](sync/index.md)

Cet article explique comment ajouter des fonctionnalités de synchronisation hors connexion à une application de Xamarin.Forms. Synchronisation hors connexion permet aux utilisateurs d’interagir avec une application mobile, affichage, ajout ou modification des données, même lorsqu’il n’est pas une connexion réseau. Modifications sont stockées dans une base de données locale, et une fois que l’appareil est en ligne, les modifications peuvent être synchronisées avec le service web.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Envoi de notifications Push](push-notifications/index.md)

Cet article explique comment ajouter des notifications push à une application de Xamarin.Forms. Azure Notification Hubs fournit une infrastructure évolutive push pour l’envoi de notifications push mobiles à partir de n’importe quel serveur principal pour toutes les plateformes mobiles, tout en éliminant la complexité d’un service principal ayant communiquer avec les systèmes de notification de plateforme différente.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Stockage de fichiers dans le cloud](storage/index.md)

Cet article explique comment utiliser Xamarin.Forms pour stocker les données texte et binaires dans le stockage Azure et comment accéder aux données. Stockage Azure est une solution de stockage cloud évolutives qui peut être utilisée pour le stockage des données structurées et non structurées.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Recherche de données dans le cloud](search/index.md)

Cet article explique comment utiliser la bibliothèque de recherche de Microsoft Azure d’intégration d’Azure Search à une application de Xamarin.Forms. Azure Search est un service cloud qui fournit l’indexation et l’interrogation des fonctionnalités pour les données téléchargées. Cette opération supprime les exigences de l’infrastructure et la recherche algorithme complexité traditionnellement dans l’implémentation de la fonctionnalité de recherche dans une application.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Stockage de données dans une base de données de documents](cosmosdb/index.md)

Ce guide montre comment utiliser la bibliothèque cliente Azure Cosmos DB .NET Standard pour intégrer une base de données du document de base de données Azure Cosmos dans une application de Xamarin.Forms. Une base de données du document de base de données Azure Cosmos est une base de données NoSQL qui fournit l’accès à faible latence aux documents JSON, offrant un service de base de données rapide, hautement disponible et évolutive pour les applications qui nécessitent une réplication globale et une échelle transparente.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Ajout d’intelligence avec Cognitive Services](cognitive-services/index.md)

Ce guide explique comment utiliser une partie de l’API des Services Microsoft cognitifs dans une application de Xamarin.Forms. Services cognitifs sont un ensemble d’API, les kits de développement logiciel et les services disponibles aux développeurs pour rendre leurs applications plus intelligente en ajoutant des fonctionnalités telles que la reconnaissance des visages, la reconnaissance vocale et compréhension de la langue.
