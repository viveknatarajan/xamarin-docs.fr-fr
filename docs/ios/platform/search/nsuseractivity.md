---
title: Recherche avec NSUserActivity
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 8376ce2ccff6732fa0c89d6030b9af36d29c5085
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="search-with-nsuseractivity"></a>Recherche avec NSUserActivity

`NSUserActivity` a été introduit dans iOS 8 et est utilisé pour fournir les données pour la transférer.
Permet de créer des activités dans des parties spécifiques de votre application peut ensuite être passée hors tension à une autre instance de votre application en cours d’exécution sur un appareil iOS différents. Le périphérique de réception peut ensuite continuer l’activité a démarré sur l’appareil précédente, en haut à droite de prélèvement où l’utilisateur s’est arrêté. Pour plus d’informations sur l’utilisation de remise, consultez notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) documentation.

Vous débutez avec iOS 9, `NSUserActivity` peut être indexé (publiques et privées) et recherche à partir de la recherche Spotlight et Safari. En marquant un `NSUserActivity` en tant que métadonnées indexables consultable et l’ajout, l’activité peut être répertoriée dans les résultats de recherche sur l’appareil iOS.

[![](nsuseractivity-images/apphistory01.png "La vue d’ensemble de l’historique de l’application")](nsuseractivity-images/apphistory01.png#lightbox)

Si l’utilisateur sélectionne un résultat de recherche qui appartient à une activité à partir de votre application, l’application est lancée et l’activité décrite par la `NSUserActivity` sera redémarré et présentées à l’utilisateur.

Les propriétés suivantes de `NSUserActivity` sont utilisés pour prendre en charge la recherche de l’application :

 - `EligibleForHandoff` – If `true`, cette activité peut être utilisée dans une opération de transfert.
 - `EligibleForSearch` – If `true`, cette activité sera ajoutée à l’index sur le périphérique et présentée dans les résultats de la recherche.
 - `EligibleForPublicIndexing` – If `true`, cette activité sera ajoutée à l’index de cloud en fonction d’Apple et présentée aux utilisateurs (via la recherche) qui n’ont pas déjà installé votre application sur son appareil iOS. Consultez le [Public l’indexation de recherche](#Public-Search-Indexing) section ci-dessous pour plus de détails.
 - `Title` : Fournit un titre pour votre activité et est affiché dans les résultats de recherche. Les utilisateurs peuvent également rechercher le texte du titre lui-même.
 - `Keywords` – Est un tableau de chaînes utilisées pour décrire votre activité qui sera indexée et consultable par l’utilisateur final.
 - `ContentAttributeSet` – Est un `CSSearchableItemAttributeSet` utilisée pour mieux décrire votre activité en détail et fournissent du contenu rich dans les résultats de recherche.
 - `ExpirationDate` – Si vous souhaitez que d’une activité uniquement être affichée jusqu'à une date donnée, vous pouvez fournir cette date ici.
 - `WebpageURL` – Si l’activité peut être affichée sur le web, ou si votre application prend en charge des liens profonds de Safari, vous pouvez définir le lien pour visiter ici.

## <a name="nsuseractivity-quickstart"></a>Démarrage rapide de NSUserActivity

Suivez ces instructions pour implémenter une recherche `NSUserActivity` dans votre application :

- [Création d’identificateurs de Type d’activité](#creatingtypeid)
- [Création d’une activité](#createactivity)
- [Répondre à une activité](#respondactivity)
- [L’indexation de recherche publics](#indexing)

Il existe quelques [des avantages supplémentaires](#benefits) à l’utilisation de `NSUserActivity` pour effectuer des recherches votre contenu.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Création d’identificateurs de Type d’activité

Avant de pouvoir créer une activité de recherche, vous devez créer un _identificateur de Type d’activité_ pour l’identifier. L’identificateur de Type d’activité est une chaîne courte est ajoutée à la `NSUserActivityTypes` tableau de l’application **Info.plist** fichier utilisé pour identifier un Type d’activité utilisateur donné. Il y aura une entrée dans le tableau pour chaque activité qui prend en charge de l’application et expose à la recherche de l’application. 

Apple suggère à l’aide d’une notation DNS inverse-style pour l’identificateur de Type d’activité pour éviter les collisions. Par exemple : `com.company-name.appname.activity` activités basées sur pour une application spécifique ou `com.company-name.activity` pour les activités qui peuvent s’exécuter sur plusieurs applications.

L’identificateur de Type d’activité est utilisée lorsque vous créez un `NSUserActivity` instance pour identifier le type d’activité. Lors de la poursuite d’une activité en tant que le résultat de l’utilisateur en appuyant sur un résultat de recherche, le Type d’activité (avec l’ID d’équipe de l’application) détermine quelle application pour lancer pour continuer l’activité.

Pour créer les identificateurs de Type d’activité requis pour prendre en charge ce comportement, modifiez le **Info.plist** de fichiers et de basculer vers le **Source** vue. Ajouter un `NSUserActivityTypes` de clé et de créer des identificateurs dans le format suivant :

[![](nsuseractivity-images/type01.png "Les identificateurs requis dans l’éditeur plist NSUserActivityTypes clé")](nsuseractivity-images/type01.png#lightbox)

Dans l’exemple ci-dessus, nous avons créé un nouvel identificateur de Type d’activité pour l’activité de recherche (`com.xamarin.platform`). Lorsque vous créez vos propres applications, remplacez le contenu de la `NSUserActivityTypes` de tableau avec les identificateurs de Type d’activité spécifiques aux activités de votre application prend en charge.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Création d’une activité

Voici un exemple de création d’une activité pour une recherche d’index sur le périphérique hébergé :

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Nous pouvons ajouter d’autres détails en définissant le `ContentAttributeSet` propriété de notre `NSUserActivity` comme suit :

[![](nsuseractivity-images/apphistory02.png "Vue d’ensemble des détails sur la recherche Ajout")](nsuseractivity-images/apphistory02.png#lightbox)

En utilisant un `ContentAttributeSet` vous pouvez créer des résultats de recherche riches inciter les utilisateurs finaux d’interagir avec eux.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Répondre à une activité

Pour répondre à l’utilisateur en appuyant sur un résultat de recherche (`NSUserActivity`) pour notre application, vous devez modifier le **AppDelegate.cs** de fichier et remplacez le `ContinueUserActivity` (méthode). Exemple :

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Notez qu’il s’agit de la substitution de la même méthode utilisée pour répondre aux demandes de transfert. Maintenant si l’utilisateur clique sur un lien à partir de notre application dans les résultats de la recherche Spotlight, notre application sera mises au premier plan (ou démarrée se n'exécute pas déjà) et le contenu, la navigation ou la fonctionnalité représentée par ce lien s’affiche :

[![](nsuseractivity-images/apphistory03.png "Restaurez l’état précédent de recherche")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>L’indexation de recherche publics

Comme nous l’avons vu, iOS 9 facilite fournir l’accès de recherche de contenu de l’application et les fonctionnalités que l’utilisateur a déjà découverts et utilisée sur un appareil iOS donné. L’indexation publique iOS 9 offre un moyen pour les utilisateurs qui n’ont pas découvert contenu ou des fonctionnalités encore (ou qui n’ont pas installé même de l’application) pour obtenir ces résultats dans leurs recherches trop.

L’indexation publique fonctionne de la façon suivante :

1. Lorsque vous créez une activité pour votre application, vous pouvez le marquer comme étant public.
2. Activités publiques sont envoyées à Apple et indexées dans le cloud.
3. Comme des utilisateurs interagissent avec votre application sur un appareil et utilisent la fonctionnalité spécifique ou un contenu représenté par l’activité, il se situe dans le classement.
4. Les résultats de publics populaires seront disponibles à d’autres utilisateurs, même s’ils n’ont pas l’application est installée.
5. Ces résultats publics seront afficheront dans la recherche Spotlight et Safari (si l’activité inclut une URL).

Nous pouvons effectuer l’activité de recherche privé que nous avons créés ci-dessus et le développer pour être public :

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Fait une activité a été défini pour l’indexation de public en définissant `EligibleForPublicIndexing = true`, cela ne signifie pas qu’il sera être ajouté automatiquement à index de cloud public d’Apple. Les conditions suivantes doivent être remplies tout d’abord :

1. Il doit apparaître dans les résultats de la recherche et être sélectionnée par de nombreux utilisateurs. Les résultats restent privées jusqu'à ce qu’un seuil d’engagement activité a été remplie.
2. Approvisionnement d’application empêche toutes les données spécifiques à l’utilisateur d’être indexés et rendue publique.

<a name="benefits" />

## <a name="additional-benefits"></a>Autres avantages

En adoptant recherche application via `NSUserActivity` dans votre application, vous obtenez également les fonctionnalités suivantes :

- **Procédure de transfert** - étant donné que l’application recherche exposé par le contenu, navigation et/ou les fonctionnalités à l’aide du même mécanisme comme procédure de transfert (`NSUserActivity`), vous pouvez facilement autoriser les utilisateurs de votre application à démarrer une activité sur un périphérique et de le poursuivre sur un autre.
- **Suggestions de Siri** -actives à partir de votre application, ainsi que les suggestions standards Siri Suggestions fait normalement, peuvent être automatiquement suggérés.
- **Les rappels Smart Siri** -les utilisateurs pourront demander Siri leur rappeler les activités à partir de votre application.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de programmation de recherche de l’application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Exemple de & billet de Blog](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
