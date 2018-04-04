---
title: Fournisseurs de contenu comment fonctionnent
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b4c674176be5af09d6b780d79923737a364d1591
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="how-content-providers-work"></a>Fournisseurs de contenu comment fonctionnent

Il existe deux classes impliquées dans un `ContentProvider` interaction :

- **ContentProvider** &ndash; implémente une API qui expose un jeu de données d’une manière standard. Les principales méthodes sont des requêtes, Insert, Update et Delete.

- **ContentResolver** &ndash; un proxy statique qui communique avec un `ContentProvider` pour accéder à ses données, soit à partir de la même application ou d’une autre application.

Un fournisseur de contenu est normalement soutenu par une base de données SQLite, mais l’API signifie que la consommation du code n’a pas besoin connaître le SQL sous-jacent. Les requêtes sont effectuées via un Uri à l’aide de constantes pour référencer les noms de colonne (afin de réduire les dépendances sur la structure de données sous-jacente) et un `ICursor` est retournée pour le code de consommation d’itérer sur.


## <a name="consuming-a-contentprovider"></a>Utilisation d’un ContentProvider

`ContentProviders` exposer leurs fonctionnalités via un Uri qui est enregistré dans le **AndroidManifest.xml** de l’application qui publie les données. Il est une convention où l’Uri et les colonnes de données qui sont exposées doivent être disponibles en tant que constantes pour faciliter la lier aux données. Android intégré de le `ContentProviders` tous fournissent des classes de commodité avec des constantes qui font référence à la structure de données dans le [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) espace de noms.



### <a name="built-in-providers"></a>Fournisseurs intégrés

Android offre un accès à un large éventail de systèmes et l’utilisation de données utilisateur `ContentProviders`:

- *Navigateur* &ndash; signets et l’historique du navigateur (nécessite l’autorisation `READ_HISTORY_BOOKMARKS` et/ou `WRITE_HISTORY_BOOKMARKS`).

- *Journal d’appels* &ndash; appels récents apportées ou reçu avec l’appareil.

- *Contacts* &ndash; détaillée des informations à partir de la liste de contacts de l’utilisateur, y compris les personnes, des téléphones, des photos et des groupes.

- *MediaStore* &ndash; contenu de l’appareil de l’utilisateur : données vidéo, images (y compris les miniatures) et audio (albums, artistes, genres, sélections).

- *Paramètres* &ndash; préférences et les paramètres de l’appareil de l’échelle du système.

- *UserDictionary* &ndash; contenu du dictionnaire défini par l’utilisateur utilisé pour l’entrée de texte prédictive.

- *Messagerie vocale* &ndash; l’historique des messages vocaux.



## <a name="classes-overview"></a>Vue d’ensemble des classes

Classes principales utilisées lorsque vous travaillez avec un `ContentProvider` sont indiquées ici :

[![Diagramme de classes de l’application de fournisseur de contenu et les interactions d’application de consommation](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

Dans ce diagramme, le `ContentProvider` implémente des requêtes et enregistre les URI qui utilisent d’autres applications pour localiser les données. Le `ContentResolver` agit comme un « proxy » pour le `ContentProvider` (interroger, insérer, mettre à jour et supprimer les méthodes). Le `SQLiteOpenHelper` contient les données utilisées par le `ContentProvider`, mais il n’est pas directement exposée à consommant des applications.
Le `CursorAdapter` passe du curseur renvoyé par le `ContentResolver` à afficher dans un `ListView`. Le `UriMatcher` est une classe d’assistance qui analyse URI lors du traitement des requêtes.

L’objectif de chaque classe est décrite ci-dessous :

- **ContentProvider** &ndash; implémenter les méthodes de cette classe abstraite pour exposer des données. L’API est accessible à d’autres classes et les applications via l’attribut Uri qui est ajouté à la définition de classe.

- **SQLiteOpenHelper** &ndash; vous aide à mettre en œuvre de la banque de données SQLite est exposé par le `ContentProvider`.

- **UriMatcher** &ndash; utilisez `UriMatcher` dans votre `ContentProvider` mise en œuvre pour aider à gérer les URI sont utilisés pour interroger le contenu.

- **ContentResolver** &ndash; consommation du code utilise un `ContentResolver` pour accéder à un `ContentProvider` instance. Les deux classes, ensemble, prenez soin des problèmes de communication entre processus, les données d’être facilement partagées entre les applications. Consommation du code ne crée jamais un `ContentProvider` classe explicitement ; au lieu de cela, les données sont accessibles par la création d’un curseur basé sur un Uri exposé par le `ContentProvider` application.

- **CursorAdapter** &ndash; utilisez `CursorAdapter` ou `SimpleCursorAdapter` pour afficher les données accessibilitées via un `ContentProvider`.

Le `ContentProvider` API permet aux consommateurs d’effectuer diverses opérations sur les données, telles que :

-  Interroger les données pour retourner des listes ou des enregistrements individuels.
-  Modifier des enregistrements individuels.
-  Ajout de nouveaux enregistrements.
-  Supprimer des enregistrements.

Ce document contient un exemple qui utilise fournie par le système `ContentProvider`, ainsi que d’un exemple simple en lecture seule qui implémente un personnalisé `ContentProvider`.

