---
title: Services de données et cloud
description: Les applications Xamarin.Forms peuvent consommer des services web implémentés à l’aide d’un large éventail de technologies, et ce guide examinera comment effectuer cette opération.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327504"
---
# <a name="data--cloud-services"></a>Services de données et cloud

_Les applications Xamarin.Forms peuvent consommer des services web implémentés à l’aide d’un large éventail de technologies, et ce guide examinera comment effectuer cette opération._

Pour une introduction à la consommation de services web multiplateforme sur la plateforme Xamarin, consultez [Introduction aux Services Web](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Comprendre l’exemple](~/xamarin-forms/data-cloud/walkthrough.md)

Cet article fournit une description de l’exemple d’application Xamarin.Forms qui montre comment communiquer avec les services web différents. Les sujets abordés incluent l’anatomie de l’application, les pages, le modèle de données et l’appel des opérations de service web.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Consommation de services web](~/xamarin-forms/data-cloud/consuming/index.md)

Ce guide montre comment communiquer avec les services web différents pour fournir créer, lire, mettre à jour, fonctionnalités et de suppression (CRUD) pour une application Xamarin.Forms. Les sujets abordés incluent la communication avec [services ASMX](consuming/asmx.md), [services WCF](consuming/wcf.md), [services REST](consuming/rest.md), et [Azure Mobile Apps](consuming/azure.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Authentification de l’accès aux services web](~/xamarin-forms/data-cloud/authentication/index.md)

Ce guide explique comment intégrer des services d’authentification dans une application Xamarin.Forms pour permettre aux utilisateurs de partager un serveur principal tout en ayant uniquement accès à leurs propres données. Les sujets abordés incluent [à l’aide de l’authentification de base avec un service REST](authentication/rest.md), [à l’aide du composant Xamarin.Auth pour s’authentifier auprès des fournisseurs d’identité OAuth](authentication/oauth.md)et à l’aide de l’authentification intégrée mécanismes proposés par [Azure Mobile Apps](authentication/azure.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Synchronisation des données avec les services web](sync/index.md)

Cet article explique comment ajouter la fonctionnalité de synchronisation hors connexion à une application Xamarin.Forms. Synchronisation hors connexion permet aux utilisateurs d’interagir avec une application mobile, affichage, ajout ou modification des données, même quand il n’est pas une connexion réseau. Les modifications sont stockées dans une base de données locale et une fois l’appareil est en ligne, les modifications peuvent être synchronisées avec le service web.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Envoi de notifications Push](push-notifications/index.md)

Cet article montre comment ajouter des notifications push à une application Xamarin.Forms. Azure Notification Hubs fournit une infrastructure push évolutif pour l’envoi de notifications push mobiles à partir de n’importe quel backend à n’importe quelle plateforme mobile, tout en éliminant la complexité d’un serveur principal d’avoir à communiquer avec les systèmes de notification de plateforme différente.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Stockage de fichiers dans le cloud](storage/index.md)

Cet article montre comment utiliser Xamarin.Forms pour stocker des données texte et binaires dans le stockage Azure et comment accéder aux données. Stockage Azure est une solution de stockage cloud évolutif qui peut être utilisée pour stocker des données non structurées et structurées.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Recherche de données dans le cloud](search/index.md)

Cet article montre comment utiliser la bibliothèque de recherche Microsoft Azure pour intégrer Azure Search dans une application Xamarin.Forms. Recherche Azure est un service cloud qui fournit l’indexation et l’interrogation des fonctionnalités pour les données téléchargées. Cette opération supprime les exigences de l’infrastructure et la recherche algorithme complexité traditionnellement avec implémentation de la fonctionnalité de recherche dans une application.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Stockage de données dans une base de données de documents](cosmosdb/index.md)

Ce guide explique comment utiliser la bibliothèque cliente Azure Cosmos DB .NET Standard pour intégrer une base de données de document Azure Cosmos DB dans une application Xamarin.Forms. Une base de données de document Azure Cosmos DB est une base de données NoSQL qui fournit l’accès à faible latence pour les documents JSON, offre un service de base de données rapide, hautement disponible et évolutif pour les applications qui nécessitent une mise à l’échelle transparente et une réplication globale.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Ajout d’intelligence avec Cognitive Services](cognitive-services/index.md)

Ce guide explique comment utiliser certaines des API Microsoft Cognitive Services dans une application Xamarin.Forms. COGNITIVE Services sont un ensemble d’API, les kits de développement logiciel et les services qui permettent aux développeurs de créer des applications plus intelligentes en ajoutant des fonctionnalités telles que la reconnaissance faciale, reconnaissance vocale et compréhension du langage.
