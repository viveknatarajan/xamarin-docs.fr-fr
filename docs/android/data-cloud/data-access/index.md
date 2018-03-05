---
title: "Accès aux données de Xamarin.Android"
description: "La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données.  Android est le moteur de base de données SQLite 'intégré' et accès pour stocker et récupérer des données est simplifié par la plateforme de Xamarin. Ce document montre comment accéder à une base de données SQLite d’une manière inter-plateformes."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 301c4330afe6ff7808ca7b09f6cc5260517aae43
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-data-access"></a>Accès aux données de Xamarin.Android

_La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données.  Android est le moteur de base de données SQLite 'intégré' et accès pour stocker et récupérer des données est simplifié par la plateforme de Xamarin. Ce document montre comment accéder à une base de données SQLite d’une manière inter-plateformes._

## <a name="data-access-overview"></a>Vue d’ensemble de données Access

La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données. Android à la fois possède le moteur de base de données Sqlite « intégré » et l’accès aux données est simplifié par la plateforme de Xamarin qui est livré avec le fournisseur de données SQLite.

Xamarin.Android prend en charge les API d’accès de base de données tels que :

-  Framework d’ADO.NET.
-  SQLite-NET 3e partie bibliothèque.

La plupart du code dans cette section est complètement inter-plateformes et s’exécutera sur iOS ou Android sans modification. Il existe deux exemples d’applications indiqués :

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; des opérations de données Simple écrit les résultats dans un fichier texte affichent le contrôle ;

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; intègre les opérations de données en une petite application qui répertorie et modifie une structure de données simple.

Les deux exemples de solutions contiennent iOS et Android application des exemples.

Pour les applications de Xamarin.Forms, vous devez lire [des bases de données](~/xamarin-forms/app-fundamentals/databases.md) qui explique comment travailler avec SQLite dans une bibliothèque PCL avec Xamarin.Forms.

Les rubriques de cette section traitent des accès aux données dans Xamarin.Android à l’aide de SQLite comme moteur de base de données. La base de données est accessible « directement » à l’aide de syntaxe ADO.NET ou vous pouvez inclure les ORM SQLite.NET et effectuer des opérations de données en c#.

Deux échantillons sont examinés : une qui contient le code d’accès aux données très simple qui sorties pour un champ de texte et une application simple qui inclut créer, lire, mettre à jour et supprimer des fonctionnalités. Thread et comment votre application avec une base de données SQLite prédéfinie de valeur initiale est également présenté.

Pour des exemples supplémentaires de l’accès aux données d’inter-plateformes, consultez notre [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) étude de cas.


## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recettes de données Android](https://developer.xamarin.com/recipes/android/data/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
