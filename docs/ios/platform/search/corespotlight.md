---
title: Recherche avec Spotlight de base dans Xamarin.iOS
description: Ce document décrit comment utiliser Spotlight de base dans une application Xamarin.iOS pour fournir des liens vers du contenu de dans l’application. Il explique comment créer, restaurer, mettre à jour et supprimer des éléments de recherche.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: fb9ddcc39bd33199dc370897250cd0d74597612f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61248471"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Recherche avec Spotlight de base dans Xamarin.iOS

Spotlight de base est une nouvelle infrastructure pour iOS 9 qui présente une API de type de base de données pour ajouter, modifier ou supprimer des liens vers du contenu dans votre application. Les éléments qui ont été ajoutés à l’aide de Spotlight de base sera disponibles dans la recherche Spotlight sur l’appareil iOS.

Pour obtenir un exemple des types de contenu qui peuvent être indexées à l’aide de Spotlight de base, examinez d’Apple Messages, courrier, calendrier et les Notes des applications. Elles actuellement utilisent toutes Spotlight de base pour fournir des résultats de la recherche.

## <a name="creating-an-item"></a>Création d’un élément

Voici un exemple de création d’un élément et l’indexation à l’aide de Spotlight de base :

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Ces informations ressemble à ce qui suit dans un résultat de recherche :

[![](corespotlight-images/corespotlight01.png "Vue d’ensemble de résultats de recherche Spotlight de base")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Restauration d’un élément

Lorsque l’utilisateur appuie sur un élément ajouté au résultat de recherche via Spotlight de base pour votre application, le `AppDelegate` méthode `ContinueUserActivity` est appelée (cette méthode est également utilisée pour `NSUserActivity`). Exemple :

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Notez que cette fois nous sommes à cocher pour l’activité ayant une `ActivityType` de `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>La mise à jour d’un élément

Il peut arriver lorsqu’un élément d’Index que nous avons créé avec Spotlight de base doivent être modifiées, comme une modification dans le titre ou l’image miniature est nécessaire. Pour ce faire, nous utilisons la même méthode que celle utilisée pour créer initialement l’index.
Nous créons un nouvel `CSSearchableItem` utilisant le même ID que celui utilisé pour créer l’élément et attacher un nouveau `CSSearchableItemAttributeSet` contenant des attributs modifiés :

[![](corespotlight-images/corespotlight02.png "La mise à jour une vue d’ensemble de l’élément")](corespotlight-images/corespotlight02.png#lightbox)

Lorsque cet élément est écrit dans l’index de recherche, l’élément existant est mis à jour avec les nouvelles informations.

## <a name="deleting-an-item"></a>Suppression d’un élément

Spotlight de base fournit plusieurs façons de supprimer un élément d’index lorsqu’il n’est plus nécessaire.

Tout d’abord, vous pouvez supprimer un élément par son identificateur, par exemple :

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Ensuite, vous pouvez supprimer un groupe d’éléments de l’index par leur nom de domaine. Exemple :

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Enfin, vous pouvez supprimer tous les éléments d’Index par le code suivant :

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Fonctionnalités supplémentaires de Spotlight

Spotlight de base comprend les fonctionnalités suivantes qui vous aider à conserver l’index précises et à jour :

- **Prise en charge de la mise à jour du lot** – si votre application a besoin créer ou modifier un grand nombre d’index en même temps, l’ensemble du lot peut être envoyé à la `Index` méthode de la `CSSearchableIndex` classe dans un seul appel.
- **Répondre aux modifications d’Index** : à l’aide du `CSSearchableIndexDelegate` votre application peut répondre aux modifications et des notifications à partir de l’index de recherche.
- **Appliquer la Protection des données** – en utilisant les classes de protection de données, vous pouvez implémenter la sécurité sur les éléments que vous ajoutez à l’index de recherche à l’aide de Spotlight de base.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de programmation de recherche de l’application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
