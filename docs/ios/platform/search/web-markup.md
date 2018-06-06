---
title: Recherche avec des balises Web Xamarin.iOS
description: Ce document décrit comment créer des résultats de la recherche basée sur le web, un lien vers une application Xamarin.iOS. Elle explique comment activer le contenu web indexation, de fabrication du site Web de votre application détectable, à l’aide de bannières de l’application active, les liens universels et bien plus encore.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 438a65de3eb78f849493e3478bce5522a325d0cd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787992"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Recherche avec des balises Web Xamarin.iOS

Pour les applications qui fournissent l’accès à leur contenu via un site web (non seulement à partir de l’application), contenu web peut être marqué avec liens spéciaux qui seront analysés par Apple et fournissent un lien profond à votre application sur son appareil iOS 9.

Si votre application iOS prend déjà en charge la liaison complète mobile et votre site Web affiche des liens ciblés vers le contenu au sein de votre application, Apple _Applebot_ robot d’indexation de ce contenu et les ajouter automatiquement à leur Index Cloud :

[![](web-markup-images/webmarkup01.png "Vue d’ensemble des Index de cloud")](web-markup-images/webmarkup01.png#lightbox)

Apple affichera ces résultats dans les résultats de recherche de la galerie et Safari.
Si l’utilisateur appuie sur un de ces résultats (et qu’ils ont votre application est installée), puis à entreprendre pour le contenu de votre application :

[![](web-markup-images/webmarkup02.png "Profondeur de la liaison à partir d’un site Web dans les résultats de la recherche")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>L’activation de l’indexation de contenu Web

Quatre étapes sont requises pour que vous contenu l’application consultable à l’aide de Web balisage :

1. Assurez-vous qu’Apple permettre découvrir et d’index du site Web de votre application en le définissant comme le **prise en charge** ou **Marketing** site Web dans iTunes Connect.
2. Assurez-vous que le site Web de votre application contient le balisage requis pour implémenter le lien profond mobile. Consultez les sections ci-dessous pour plus de détails.
3. Activer le lien ciblé de gérer dans votre application iOS.
4. Ajouter le balisage pour les données structurées exposés par le site Web de votre application pour fournir un résultat riche et attrayant à l’utilisateur final. Cette étape n’est pas strictement obligatoire, il est vivement recommandé par Apple.

Les sections suivantes passe en revue ces étapes en détail.

## <a name="make-your-apps-website-discoverable"></a>Rendre du site Web de votre application détectable

Le plus simple d’avoir Apple à trouver le site Web de votre application consiste à utiliser en tant que le **prise en charge** ou **Marketing** site Web lorsque vous envoyez votre application à Apple via iTunes Connect.

## <a name="using-smart-app-banners"></a>À l’aide d’une application intelligente bannières

Fournir une bannière d’application actives sur votre site Web pour présenter un lien clair dans votre application. Si l’application n’est pas déjà installée, Safari vous invite automatiquement l’utilisateur à installer votre application. Sinon, l’utilisation peut appuyer sur le **vue** lien pour lancer votre application à partir du site Web. Par exemple, pour créer une bannière d’application active, vous pouvez utiliser le code suivant :

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Pour plus d’informations, consultez le site d’Apple [promouvoir des applications avec les bannières application Smart](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) documentation.

## <a name="using-universal-links"></a>À l’aide de liens universels

Nouveau sur iOS 9, universel fournissent une meilleure alternative à intelligent application bannières ou schémas d’URL personnalisées existantes en fournissant les éléments suivants :

- **Unique** – la même URL ne peut pas être demandée par plusieurs sites Web.
- **Sécuriser** – un certificat signé est requis pour le site Web qui garantit que le site Web est détenu par vous et correctement liée à la votre application.
- **Flexible** – l’utilisateur final peut contrôler si l’URL ouvre le site Web ou l’application.
- **Universel** – la même URL peut être utilisée pour définir le contenu de votre site Web et de votre application.

## <a name="using-twitter-cards"></a>L’utilisation des cartes de Twitter

Vous pouvez fournir des liens ciblés vers le contenu de votre application à l’aide d’une carte Twitter. Exemple :

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Pour plus d’informations, consultez le site de Twitter [protocole de carte Twitter](http://dev.twitter.com/cards/mobile) documentation.

## <a name="using-facebook-app-links"></a>À l’aide de liens d’application Facebook

Vous pouvez fournir des liens ciblés vers le contenu de votre application à l’aide d’un Facebook application Link. Exemple :

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Pour plus d’informations, consultez le site de Facebook [application liens](http://applinks.org) documentation.

## <a name="opening-deep-links"></a>Lors de l’ouverture des liens profonds

Vous devez ajouter la prise en charge de l’ouverture et l’affichage des liens profonds dans votre application Xamarin.iOS. Modifier la **AppDelegate.cs** de fichier et remplacez le `OpenURL` méthode pour gérer le format d’URL personnalisé. Exemple :

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

Dans le code ci-dessus, nous recherchons une URL contenant `/appname` et en passant la valeur de `query` (`123` dans cet exemple) à un contrôleur de la vue personnalisée dans notre application pour afficher le contenu demandé à l’utilisateur.

## <a name="providing-rich-results-with-structured-data"></a>Fournit les résultats de riches avec des données structurées

En incluant le balisage de données structurées, vous pouvez fournir des résultats de recherche riches à l’utilisateur final qui vont au-delà de simplement un titre et une description. Inclure des images, les données d’application spécifiques (tels que des évaluations) et les actions à résultats à l’aide de balisage de données structurées.

Riches résultats sont plus intéressantes et peuvent aider à améliorer votre classement dans le Cloud en fonction des Index de recherche par offerte davantage d’utilisateurs d’interagir avec eux.

Une option permettant de balisage de données structurées consiste à l’aide de Open Graph. Exemple :

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Pour plus d’informations, consultez la [Open Graph](http://ogp.me) site Web.

Un autre format commun pour le balisage de données structurées est format de microdonnées de Schema.org. Exemple :

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Les mêmes informations peuvent être représentées au format de JSON-LD de Schema.org :

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

Voici un exemple de métadonnées à partir de votre site Web fournit les résultats de recherche riches à l’utilisateur final :

[![](web-markup-images/deeplink01.png "Résultats via le balisage de données structurées de recherche avancée")](web-markup-images/deeplink01.png#lightbox)

Apple prend actuellement en charge les types suivants de schéma à partir de Schema.org :

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Offres
 - Organisation
 - PriceRange
 - Recette
 - SearchAction

Pour plus d’informations sur ces types de schéma, consultez [Schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>En fournissant des Actions avec des données structurées

Types de données structurées spécifiques permettra à un résultat de recherche être utilisables par l’utilisateur final. Les actions suivantes sont actuellement prises en charge :

 - Composer un numéro de téléphone.
 - Mise en route de la direction de mappage à une adresse donnée.
 - Lecture d’un fichier audio ou vidéo.

Par exemple, la définition d’une Action pour composer un numéro de téléphone peut se présenter comme suit :

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Lorsque ce résultat de recherche est présenté à l’utilisateur final, une icône de téléphone petit s’affichera dans le résultat. Si l’utilisateur clique sur l’icône, le numéro spécifié sera appelé.

Ajoutez une action pour lire un fichier audio à partir du résultat de recherche le code HTML suivant :

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Enfin, le code HTML suivant ajoutez une action pour obtenir des instructions à partir des résultats de la recherche :

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Pour plus d’informations, consultez le site d’Apple [Site de développement recherche application](http://developer.apple.com/ios/search/).



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de programmation de recherche de l’application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
