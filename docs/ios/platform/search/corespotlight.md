---
title: Recherche avec Core Spotlight
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 0074cfcdc4a80f66f64d12f8e5c0a5e81ab2b8a1
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="search-with-core-spotlight"></a>Recherche avec Core Spotlight

La galerie de Core est une nouvelle infrastructure pour iOS 9 qui présente une API de base de données de type pour ajouter, modifier ou supprimer des liens vers du contenu dans votre application. Les éléments qui ont été ajoutés à l’aide de la galerie de base sera disponibles dans la recherche Spotlight sur l’appareil iOS.

Pour obtenir un exemple des types de contenu qui peuvent être indexées à l’aide de la galerie de noyaux, examinez les Messages d’Apple, courrier, calendrier et Notes applications. Tous les utilisent actuellement Core Spotlight pour fournir des résultats de la recherche.

## <a name="creating-an-item"></a>Création d’un élément

Voici un exemple de création d’un élément et l’indexation à l’aide de la galerie de base :

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

Lorsque l’utilisateur appuie sur un élément ajouté au résultat de recherche via Core Spotlight pour votre application, le `AppDelegate` méthode `ContinueUserActivity` est appelé (cette méthode est également utilisée pour `NSUserActivity`). Par exemple :

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

Notez que cette fois nous sont pour l’activité ayant une `ActivityType` de `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Mise à jour d’un élément

Il peut arriver lorsqu’un élément de Index que nous avons créé avec Core Spotlight doivent être modifiés, telle qu’une modification dans le titre ou une image miniature est requise. Pour ce faire, nous utilisons la même méthode que celle utilisée pour créer initialement l’index.
Nous créons un nouveau `CSSearchableItem` utilisant le même ID que celui utilisé pour créer l’élément et d’attacher un nouveau `CSSearchableItemAttributeSet` contenant des attributs modifiés :

[![](corespotlight-images/corespotlight02.png "Mise à jour d’une vue d’ensemble de l’élément")](corespotlight-images/corespotlight02.png#lightbox)

Lorsque cet élément est écrit dans l’index de recherche, l’élément existant est mis à jour avec les nouvelles informations.

## <a name="deleting-an-item"></a>Suppression d’un élément

Core Spotlight fournit plusieurs façons de supprimer un élément de l’index lorsqu’il n’est plus nécessaire.

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

Ensuite, vous pouvez supprimer un groupe d’éléments de l’index par leur nom de domaine. Par exemple :

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
## <a name="additional-core-spotlight-features"></a>Galerie de fonctionnalités supplémentaires

Galerie de base comporte les fonctionnalités suivantes permettant de conserver l’index précises et récentes :

- **Prise en charge de la mise à jour du lot** : Si votre application a besoin pour créer ou modifier un grand groupe d’index en même temps, l’intégralité du lot peut être envoyé à la `Index` méthode de la `CSSearchableIndex` classe dans un seul appel.
- **Répondre aux modifications d’Index** : à l’aide du `CSSearchableIndexDelegate` votre application peut répondre à des modifications et des notifications à partir de l’index de recherche.
- **Appliquer la Protection des données** – utilisant les classes de protection des données, vous pouvez implémenter la sécurité sur les éléments que vous ajoutez à l’index de recherche à l’aide de la galerie de base.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de programmation de recherche de l’application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
