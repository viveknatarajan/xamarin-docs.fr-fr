---
title: Données et des Services de cloud computing dans les applications Xamarin.iOS
description: Ce document liens vers des guides qui décrivent comment travailler avec des données locales, iCloud et CloudKit dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: a733c1af34b577786a7e18eeafa13da4327dddc6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784258"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Données et des Services de cloud computing dans les applications Xamarin.iOS

##  <a name="data-accessiosdata-clouddataindexmd"></a>[Accès aux données](~/ios/data-cloud/data/index.md)

La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données. iOS a le moteur de base de données Sqlite « intégré » et l’accès aux données est simplifié par la plateforme de Xamarin qui est livré avec le fournisseur de données SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

L’API de stockage iCloud dans iOS 5 permet aux applications enregistrer des documents de l’utilisateur et les données spécifiques à l’application dans un emplacement central et accéder à ces éléments à partir de tous les périphériques l’utilisateur.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Le framework CloudKit simplifie le développement d’applications qui iCloud de l’accès. Cela inclut la récupération des données d’application et droits de l’élément multimédia ainsi que la possibilité de stocker en toute sécurité les informations de l’application. Ce kit fournit aux utilisateurs une couche d’anonymat en autorisant l’accès aux applications avec leur ID iCloud sans partager des informations personnelles.