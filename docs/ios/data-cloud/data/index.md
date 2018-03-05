---
title: "accès aux données d’iOS"
description: "La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données. iOS a le moteur de base de données SQLite « intégré » et accès pour stocker et récupérer des données est simplifié par la plateforme de Xamarin. Ce document montre comment accéder à une base de données SQLite."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 9ca929f584ec2b0d98300e7645707131df89969f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-data-access"></a>accès aux données d’iOS

_La plupart des applications ont une spécification pour enregistrer les données sur le périphérique local. Sauf si la quantité de données est petite plus simplement, ceci nécessite généralement une base de données et une couche de données dans l’application pour gérer l’accès à la base de données. iOS a le moteur de base de données SQLite « intégré » et accès pour stocker et récupérer des données est simplifié par la plateforme de Xamarin. Ce document montre comment accéder à une base de données SQLite._

Xamarin.iOS prend en charge les API d’accès de base de données tels que :

-  Framework d’ADO.NET.
-  SQLite-NET 3e partie bibliothèque.

Ce guide fournit une vue d’ensemble des bases de données en général, avant de décrire comment configurer SQLite.NET et ADO.NET pour accéder aux bases de données SQLite dans vos applications Xamarin.iOS. 

La plupart du code dans ce document est complètement inter-plateformes et s’exécutera sur iOS ou Android sans modification. Il existe deux exemples d’applications indiqués :

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – les opérations de données Simple écrit les résultats dans un fichier texte affichent le contrôle ;
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – intègre les opérations de données en une petite application qui répertorie et modifie une structure de données simple.

Les deux exemples de solutions contiennent iOS et Android application des exemples.

Pour les applications de Xamarin.Forms, vous devez lire [des bases de données](~/xamarin-forms/app-fundamentals/databases.md) qui explique comment travailler avec SQLite dans une bibliothèque PCL avec Xamarin.Forms.

## <a name="sections"></a>Sections

-  [Introduction](introduction.md)
-  [Configuration](configuration.md)
-  [Utilisation de SQLite.NET ORM](using-sqlite-orm.md)
-  [Utilisation d'ADO.NET](using-adonet.md)
-  [Utilisation de données dans une application](using-data-in-an-app.md)


## <a name="summary"></a>Récapitulatif

Ce chapitre décrit l’accès aux données dans Xamarin.iOS à l’aide de SQLite comme moteur de base de données. La base de données est accessible « directement » à l’aide de la syntaxe ADO.NET, ou vous pouvez inclure les ORM SQLite.NET et effectuer des opérations de données en c#.

Nous avons vu comment deux exemples : une qui contient le code d’accès aux données très simple qui sorties pour un champ de texte et une application simple qui inclut créer, lire, mettre à jour et supprimer des fonctionnalités. Nous avons abordé également des threads et comment votre application avec une base de données SQLite prédéfinie de valeur initiale.

Pour des exemples supplémentaires de l’accès aux données d’inter-plateformes, consultez notre [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) étude de cas.

## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recettes de données](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
