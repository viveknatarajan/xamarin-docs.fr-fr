---
title: "Introduction à ContentProviders"
description: "Le système d’exploitation Android utilise des fournisseurs de contenu pour faciliter l’accès aux données partagées, telles que des fichiers multimédias, les contacts et les informations relatives au calendrier. Cet article présente la classe ContentProvider et fournit deux exemples de son utilisation."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 42a20f4f0bc16cf42aa54fd0758db8a5db7c9048
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="intro-to-contentproviders"></a>Introduction à ContentProviders

_Le système d’exploitation Android utilise des fournisseurs de contenu pour faciliter l’accès aux données partagées, telles que des fichiers multimédias, les contacts et les informations relatives au calendrier. Cet article présente la classe ContentProvider et fournit deux exemples de son utilisation._


## <a name="content-providers-overview"></a>Vue d’ensemble des fournisseurs de contenu

A *ContentProvider* encapsule un référentiel de données et fournit une API pour y accéder. Le fournisseur existe dans le cadre d’une application Android qui généralement fournit également une interface utilisateur pour l’affichage et la gestion des données. Le principal avantage de l’utilisation d’un fournisseur de contenu est l’activation d’autres applications d’accéder facilement aux données encapsulées à l’aide d’un objet de fournisseur de client (appelé un *ContentResolver*). Ensemble, un fournisseur de contenu et le programme de résolution de contenu offrent une API cohérente entre les applications d’accès aux données est simple pour créer et consommer. Toute application peut choisir d’utiliser `ContentProviders` pour gérer les données en interne et l’exposer à d’autres applications.

A `ContentProvider` est également requis pour votre application fournir des suggestions de recherche personnalisée, ou si vous souhaitez fournir la possibilité de copier des données complexes à partir de votre application et le coller dans d’autres applications. Ce document montre comment accéder à et générer `ContentProviders` avec Xamarin.Android.

La structure de cette section est la suivante :

- **Son fonctionnement** &ndash; une vue d’ensemble de ce que le `ContentProvider` est conçu pour et son fonctionnement.

- **Utilisation d’un fournisseur de contenu** &ndash; un exemple d’accès de la liste de Contacts.

- **Pour partager des données à l’aide de ContentProvider** &ndash; l’écriture et de consommation un `ContentProvider` dans la même application.

`ContentProviders` et les curseurs qui fonctionnent sur leurs données sont souvent utilisées pour remplir les ListViews. Reportez-vous à la [guide ListViews et adaptateurs](~/android/user-interface/layouts/list-view/index.md) pour plus d’informations sur l’utilisation de ces classes.

`ContentProviders` exposées par Android (ou autres applications) sont un moyen simple pour inclure des données provenant d’autres sources dans votre application. Ils vous permettent d’accéder et de présenter des données telles que la liste des Contacts, photos ou des événements de calendrier à partir de votre application et permettent à l’utilisateur d’interagir avec ces données.

Personnalisé `ContentProviders` sont un moyen pratique pour empaqueter vos données pour une utilisation dans votre propre application, ou pour une utilisation par d’autres applications (y compris des utilisations spéciales telles que copier/coller et de recherche personnalisée).

Les rubriques de cette section fournissent des exemples simples de consommation et d’écriture `ContentProvider` code.



## <a name="related-links"></a>Liens associés

- [](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [Guide du développeur de fournisseurs de contenu](http://developer.android.com/guide/topics/providers/content-providers.html)
- [Référence de classe de ContentProvider](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [Référence de classe de ContentResolver](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [Référence de classe de ListView](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [Référence de classe CursorAdapter](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [Référence de classe de UriMatcher](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [Référence de classe de ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
