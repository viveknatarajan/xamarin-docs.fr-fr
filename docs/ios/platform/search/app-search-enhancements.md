---
title: Améliorations de la recherche de l’application dans Xamarin.iOS
description: Cet article aborde les améliorations Apple a apportées à la recherche de l’application dans iOS 10 et comment les implémenter dans Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: e61cb20f12f6373ef57beb759b933d3dd9b6e76e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110581"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Améliorations de la recherche de l’application dans Xamarin.iOS

_Cet article aborde les améliorations Apple a apportées à la recherche de l’application dans iOS 10 et comment les implémenter dans Xamarin.iOS._

Dans iOS 10, Apple a apporté plusieurs améliorations à la recherche d’application telles que le lien profond participatives, dans l’application recherche, Continuation de la recherche et de visualisation des résultats de la Validation. Cet article décrit l’implémentation de ces fonctionnalités dans une application Xamarin.iOS.

## <a name="about-app-search-enhancements"></a>Sur les améliorations de la recherche de l’application

Spotlight de base dans iOS 10 apporte plusieurs améliorations à la recherche d’application telles que :

- **Popularité de lien ciblé participatives (avec privacy différentielle)** -offre un moyen pour promouvoir le contenu de l’application lié à la profondeur dans les résultats de la recherche.
- **Dans l’application recherche** -utiliser le nouveau `CSSearchQuery` classe pour fournir la possibilité de recherche Spotlight dans l’application similaire au fonctionnement des applications de messagerie, les Messages et les Notes.
- **Rechercher la Continuation** - permet à un utilisateur de lancer une recherche dans Spotlight ou Safari, puis ouvrez une application et continuer cette recherche.
- **Visualisation des résultats de la Validation** -Apple [outil de Validation API application recherche](https://search.developer.apple.com/appsearch-validation-tool) affiche maintenant une représentation visuelle du balisage d’un site Web et lien profond lors de la réalisation de tests.
- **Message d’application partage d’images** -permet des images populaires dans l’application fournies pour le partage dans les Messages (via une Extension d’application Message) doit s’afficher dans les recherches d’actualités.

Les sections suivantes couvre ces rubriques plus en détail.

## <a name="crowdsourced-deep-link-popularity"></a>Lien ciblé participatives popularité

iOS 10 fournit un mécanisme pour compter la fréquence que liens profond populaires dans une application sont suivies par l’utilisateur et utilise ces informations pour améliorer le classement d’une application de contenu dans les résultats de recherche, tout en protégeant l’identité l’utilisateur à l’aide de  *Privacy différentielle*.

Pour de l’application qui utilisent `NSUserActivity` objets à fournir des URL de lien ciblé et la `EligibleForPublicIndexing` propriété définie sur `true`, iOS 10 soumet un sous-ensemble de *des hachages Privacy différentielle* pour les serveurs d’Apple. Ces informations sont ensuite utilisées pour promouvoir les plus courants dans l’application contenu dans les résultats de la recherche.

Pour plus d’informations sur l’implémentation de lien profond dans une application Xamarin.iOS, consultez notre [recherche avec NSUserActivity](~/ios/platform/search/nsuseractivity.md) documentation.

## <a name="in-app-searching"></a>Recherche dans l’application

En implémentant la nouvelle [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) classe, une application peut fournir la recherche et la technologie des règles correspondantes pour rechercher du contenu à l’intérieur de lui-même, sans que l’utilisateur d’avoir à quitter l’application (semblable à la messagerie, les Messages et les Notes application projecteur travail).

En règle générale, les applications qui prennent en charge `CSSearchQuery` ne devrez pas mettre à jour leur propres, index de recherche distinctes. 

## <a name="search-continuation"></a>Continuation de la recherche

Dans iOS 9, Apple a introduit les API de recherche (par exemple Spotlight de base, `NSUserActivity` et balisage web) pour fournir les souhaits d’approfondie du contenu au sein d’une application pour permettre aux utilisateurs de rechercher ce contenu à l’aide des interfaces de recherche Spotlight et de Safari. Consultez notre [nouvelles API de recherche](~/ios/platform/search/index.md) documentation pour plus d’informations.

Dans iOS 10 Apple s’appuie sur cette fonctionnalité en autorisant l’utilisateur lancer une recherche dans Spotlight ou Safari, puis continuer la recherche quand il ouvre une application. 

Pour implémenter cette fonctionnalité, modifiez l’application `Info.plist` , ajoutez le `CoreSpotlightContinuation` clé de type **booléenne** et définissez sa valeur sur `YES`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "Modification CoreSpotlightContinuation dans le fichier Info.plist")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "Modification CoreSpotlightContinuation dans le fichier Info.plist")](app-search-enhancements-images/search01.png#lightbox)

-----

Pour répondre à l’utilisateur poursuivre un résultat de recherche (`NSUserActivity`), modifier le `AppDelegate.cs` de fichiers et de remplacer le `ContinueUserActivity` (méthode). Exemple :

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

Ce code recherche le type d’action de continuation requête (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), puis lit la requête l’utilisateur actuel à partir de la `NSUserActivity` dictionnaire d’informations de la classe utilisateur (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). À ce stade, l’application doit prendre des mesures pour continuer la recherche de l’utilisateur.

Pour plus d’informations sur l’utilisation des recherches dans une application Xamarin.iOS, consultez notre [recherche avec Spotlight de base](~/ios/platform/search/corespotlight.md) documentation.

## <a name="visualization-of-validation-results"></a>Visualisation des résultats de la Validation

Apple [outil de Validation API application recherche](https://search.developer.apple.com/appsearch-validation-tool) affiche maintenant une représentation visuelle du balisage d’un site Web et lien profond (y compris le balisage, tel que défini à [Schema.org](http://schema.org/)) lors de la réalisation de tests.

À l’aide de l’outil de Validation, un développeur puisse voir les informations que le robot d’indexation Applebot a indexé pour le site telles que le titre, description, URL et tout autre pris en charge les éléments.

Pour plus d’informations sur l’utilisation de balisage Web, consultez notre [rechercher avec balisage Web](~/ios/platform/search/web-markup.md) documentation.

## <a name="message-app-image-sharing"></a>Image de l’application de partage du message

Si une Extension d’application Message fournit des images pour le partage dans les Messages, l’extension peut être configurée pour autoriser l’utilisateur à effectuer des recherches de Spotlight pour des images populaires à partir dans les Messages, sans avoir à quitter l’application.

Pour activer cette fonctionnalité, procédez comme suit :

1. Créer une Extension d’application de Message.
2. Ajouter le `com.apple.developer.associated-domains` aux droits de l’application et incluent une liste de domaines web qui hébergent les images du partage de l’Extension d’application de Message. Pour chaque domaine, spécifiez la `spotlight-image-search` service.
3. Ajouter un `apple-app-site-association` fichier vers le site Web qui héberge les images. Ce fichier contient un dictionnaire pour la `spotlight-image-search` de service et inclut l’ID de l’application, ce qui est le préfixe d’ID d’application ou ID de l’équipe suivi de l’ID d’offre groupée. Le fichier peut contenir jusqu'à 500 des chemins d’accès et des modèles qui seront indexées par Spotlight et inclus dans les recherches d’images populaires. Pour plus d’informations, consultez le site d’Apple [création et téléchargement du fichier Association](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) documentation.
4. Autoriser la Applebot analyser les sites Web. Consultez d’Apple [sur Applebot](https://support.apple.com/HT204683) documentation.

Consultez notre [intégration des applications Message](~/ios/platform/message-app-integration/index.md) documentation pour plus d’informations.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les améliorations Apple a apportées à la recherche de l’application dans iOS 10 et comment les implémenter dans Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
