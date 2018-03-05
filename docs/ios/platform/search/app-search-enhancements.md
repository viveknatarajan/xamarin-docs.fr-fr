---
title: "Améliorations de la recherche de l’application"
description: "Cet article décrit les améliorations Apple a apportées à la recherche de l’application dans iOS 10 et comment les implémenter dans Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 95f7ad5069abfe4dff82659c0fbc79eef2125e15
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="app-search-enhancements"></a>Améliorations de la recherche de l’application

_Cet article décrit les améliorations Apple a apportées à la recherche de l’application dans iOS 10 et comment les implémenter dans Xamarin.iOS._

Dans iOS 10, Apple a apporté plusieurs améliorations à la recherche d’application telles que Crowdsourced lien profond, la recherche dans l’application, Continuation de la recherche et visualisation des résultats de la Validation. Cet article décrit l’implémentation de ces fonctionnalités dans une application Xamarin.iOS.

## <a name="about-app-search-enhancements"></a>Sur les améliorations de la recherche de l’application

Spotlight Core dans iOS 10 apporte plusieurs améliorations à la recherche de l’application tel que :

- **Popularité de lien ciblé Crowdsourced (avec confidentialité différentielle)** -permet de promouvoir le contenu lié en profondeur l’application dans les résultats de la recherche.
- **Dans l’application recherche** -utiliser la nouvelle `CSSearchQuery` classe afin de fournir la possibilité de recherche Spotlight dans l’application similaire au fonctionnement des applications de messagerie, les Messages et les Notes.
- **Rechercher la Continuation** : permet à un utilisateur de lancer une recherche dans la galerie ou Safari, puis ouvrir une application et continuer la recherche.
- **Visualisation des résultats de la Validation** -Apple [outil de Validation API application recherche](https://search.developer.apple.com/appsearch-validation-tool) affiche maintenant une représentation visuelle du balisage d’un site Web et lien profond lors de la réalisation des tests.
- **Message d’application de partage Image** -permet des images dans l’application populaires fournis pour le partage dans les Messages (via une Extension d’application Message) pour qu’il apparaisse dans les recherches de la galerie.

Les sections suivantes couvrent les ces rubriques plus en détail.

## <a name="crowdsourced-deep-link-popularity"></a>Lien ciblé Crowdsourced popularité

iOS 10 fournit un mécanisme pour compter la fréquence que populaires approfondie des liens dans une application sont suivies par l’utilisateur et utilise ces informations pour améliorer le classement d’une application de contenu dans les résultats de recherche, tout en protégeant les identités de l’utilisateur à l’aide de  *Confidentialité différentielle*.

Pour application qui utilisent `NSUserActivity` objets fournir l’URL du lien ciblé et avoir le `EligibleForPublicIndexing` propriété la valeur `true`, iOS 10 soumet un sous-ensemble de *différentielle confidentialité hachages* pour les serveurs Apple. Ces informations sont ensuite utilisées pour promouvoir populaires contenu dans l’application dans les résultats de la recherche.

Pour plus d’informations sur l’implémentation de lien profond dans une application Xamarin.iOS, veuillez consulter notre [recherche avec NSUserActivity](~/ios/platform/search/nsuseractivity.md) documentation.

## <a name="in-app-searching"></a>Dans l’application recherche

En implémentant la nouvelle [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) (classe), une application peut fournir la recherche et la technologie des règles correspondantes pour rechercher du contenu à l’intérieur de lui-même, sans que l’utilisateur de devoir quitter l’application (semblable à la messagerie, les Messages et les Notes application projecteur travail).

En règle générale, les applications qui prennent en charge `CSSearchQuery` aurez pas à gérer leur propres, index de recherche distincte. 

## <a name="search-continuation"></a>Continuation de la recherche

Dans iOS 9, Apple a introduit les API de recherche (telles que Core Spotlight, `NSUserActivity` et le balisage de web) pour fournir des besoins-approfondie du contenu au sein d’une application pour permettre aux utilisateurs de rechercher ce contenu à l’aide des interfaces de recherche de la galerie et Safari. Consultez notre [nouvelles API de recherche](~/ios/platform/search/index.md) documentation pour plus de détails.

Dans iOS les 10 Apple s’appuie sur cette fonctionnalité, en permettant à l’utilisateur de lancer une recherche dans la galerie ou Safari, puis continuer la recherche lorsqu’ils ouvrent une application. 

Pour implémenter cette fonctionnalité, modifiez l’application `Info.plist` , ajoutez le `CoreSpotlightContinuation` clé de type **booléenne** et définissez sa valeur sur `YES`:

[[ide name="xs]]

[ ![](app-search-enhancements-images/search01.png "Modification CoreSpotlightContinuation dans le fichier Info.plist")](app-search-enhancements-images/search01.png)

[[/ide]]

[[ide name="vs]]

[ ![](app-search-enhancements-images/searchw01.png "Modification CoreSpotlightContinuation dans le fichier Info.plist")](app-search-enhancements-images/search01.png)

[[/ide]]

Pour répondre à l’utilisateur poursuivre un résultat de recherche (`NSUserActivity`), modifier le `AppDelegate.cs` de fichier et remplacez le `ContinueUserActivity` (méthode). Exemple :

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

Ce code recherche le type d’action de continuation requête (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), puis lit la requête en cours de l’utilisateur à partir de la `NSUserActivity` dictionnaire de la classe utilisateur info (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). À ce stade, l’application doit prendre des mesures pour continuer la recherche de l’utilisateur.

Pour plus d’informations sur l’utilisation des recherches dans une application Xamarin.iOS, veuillez consulter notre [recherche avec Core Spotlight](~/ios/platform/search/corespotlight.md) documentation.

## <a name="visualization-of-validation-results"></a>Visualisation des résultats de la Validation

Apple [outil de Validation API application recherche](https://search.developer.apple.com/appsearch-validation-tool) affiche maintenant une représentation visuelle du balisage d’un site Web et lien profond (y compris le balisage, tel que défini à [Schema.org](http://schema.org/)) lors de la réalisation des tests.

À l’aide de l’outil de Validation, un développeur peut voir les informations que le robot d’indexation Applebot a indexé pour le site, telles que le titre, description, URL et d’autres éléments de pris en charge.

Pour plus d’informations sur l’utilisation par un balisage Web, consultez notre [recherche par un balisage Web](~/ios/platform/search/web-markup.md) documentation.

## <a name="message-app-image-sharing"></a>Image d’application de partage de message

Si une Extension d’application Message fournit des images pour le partage dans les Messages, l’extension peut être configurée pour autoriser l’utilisateur d’effectuer la recherche Spotlight images populaires depuis les Messages, sans quitter l’application.

Pour activer cette fonctionnalité, procédez comme suit :

1. Créer une Extension d’application de Message.
2. Ajouter le `com.apple.developer.associated-domains` aux droits de l’application et incluent une liste de domaines web qui hébergent les images du partage de l’Extension d’application de Message. Pour chaque domaine, spécifiez la `spotlight-image-search` service.
3. Ajouter un `apple-app-site-association` fichier vers le site Web qui héberge les images. Ce fichier contient un dictionnaire pour la `spotlight-image-search` de service et inclut l’ID de l’application, qui est le préfixe de l’ID d’équipe ou ID d’application suivi par l’ID d’offre groupée. Le fichier peut contenir jusqu'à 500 des chemins d’accès et des modèles qui seront indexés par des actualités et inclus dans les recherches d’image courantes. Pour plus d’informations, consultez le site d’Apple [création et téléchargement du fichier Association](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) documentation.
4. Autoriser la Applebot analyser les sites Web. Consultez d’Apple [Applebot sur](https://support.apple.com/en-us/HT204683) documentation.

Consultez notre [intégration de l’application Message](~/ios/platform/message-app-integration/index.md) documentation pour plus de détails.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les améliorations Apple a apportées à la recherche de l’application dans iOS 10 et comment les implémenter dans Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
