---
title: Introduction à Contentprovider
description: Le système d’exploitation Android utilise des fournisseurs de contenu pour faciliter l’accès aux données partagées telles que des fichiers multimédias, les contacts et les informations de calendrier. Cet article présente la classe ContentProvider et fournit deux exemples illustrant comment l’utiliser.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 60f9420fc630bcfc44a6b19356b2315ac8bcd523
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670309"
---
# <a name="intro-to-contentproviders"></a>Introduction à Contentprovider

_Le système d’exploitation Android utilise des fournisseurs de contenu pour faciliter l’accès aux données partagées telles que des fichiers multimédias, les contacts et les informations de calendrier. Cet article présente la classe ContentProvider et fournit deux exemples illustrant comment l’utiliser._


## <a name="content-providers-overview"></a>Vue d’ensemble des fournisseurs de contenu

Un *ContentProvider* encapsule un référentiel de données et fournit une API pour y accéder. Le fournisseur existe dans le cadre d’une application Android qui généralement fournit également une interface utilisateur pour l’affichage et la gestion des données. Le principal avantage de l’utilisation d’un fournisseur de contenu est permettant aux autres applications facilement accéder aux données encapsulées à l’aide d’un objet de fournisseur de client (appelé un *ContentResolver*). Ensemble, un fournisseur de contenu et le programme de résolution de contenu offrent une API cohérente entre les applications pour accéder aux données qui est simple à créer et consommer. N’importe quelle application peut choisir d’utiliser `ContentProviders` pour gérer les données en interne et également pour l’exposer à d’autres applications.

Un `ContentProvider` est également requise pour votre application fournir des suggestions de recherche personnalisée, ou si vous souhaitez fournir la possibilité de copier des données complexes à partir de votre application à coller dans d’autres applications. Ce document montre comment accéder à et générer `ContentProviders` avec Xamarin.Android.

La structure de cette section est la suivante :

- **Son fonctionnement** &ndash; une vue d’ensemble de ce à quoi le `ContentProvider` est conçu pour et comment il fonctionne.

- **Utilisation d’un fournisseur de contenu** &ndash; un exemple d’accès de la liste de Contacts.

- **Pour partager des données à l’aide de ContentProvider** &ndash; écriture et de consommation une `ContentProvider` dans la même application.

`ContentProviders` et les curseurs qui opèrent sur leurs données sont souvent utilisés pour remplir les ListViews. Reportez-vous à la [guide ListViews et Adapters](~/android/user-interface/layouts/list-view/index.md) pour plus d’informations sur l’utilisation de ces classes.

`ContentProviders` exposées par Android (ou autres applications) sont un moyen simple pour inclure des données provenant d’autres sources dans votre application. Ils vous permettent d’accéder et de présenter des données telles que la liste Contacts, les photos ou les événements de calendrier à partir de votre application et permettent à l’utilisateur d’interagir avec ces données.

Personnalisé `ContentProviders` sont un moyen pratique pour empaqueter vos données pour une utilisation au sein de votre propre application, ou pour une utilisation par d’autres applications (y compris des utilisations spéciales telles que recherche personnalisée et copier/coller).

Les rubriques de cette section fournissent des exemples simples de consommation et d’écriture `ContentProvider` code.



## <a name="related-links"></a>Liens associés

- [Démonstration de ContactsAdapter (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [Guide du développeur de fournisseurs de contenu](https://developer.android.com/guide/topics/providers/content-providers.html)
- [Référence de classe de ContentProvider](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [Référence de classe de ContentResolver](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [Référence de classe de ListView](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [Référence de classe CursorAdapter](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [Référence de classe de UriMatcher](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [Référence de classe de ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
