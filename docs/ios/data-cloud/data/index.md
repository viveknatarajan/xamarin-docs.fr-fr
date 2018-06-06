---
title: Accès aux données de Xamarin.iOS
description: Ce document liens vers des guides qui décrivent comment travailler avec des bases de données locales dans une application Xamarin.iOS. Contenu lié traite SQLite.NET, ADO.NET et bien plus encore.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: a986ea9931f62497e5a6863c84bd4041983d66d9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784574"
---
# <a name="xamarinios-data-access"></a>Accès aux données de Xamarin.iOS

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
