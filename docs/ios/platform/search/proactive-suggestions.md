---
title: Introduction aux Suggestions proactives dans Xamarin.iOS
description: Cet article montre comment utiliser des Suggestions proactives dans l’application Xamarin.iOS pour favoriser l’engagement en autorisant le système de façon proactive présenter automatiquement des informations utiles à l’utilisateur.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2ab0147f918b36dc47ef6eed7d9bf1b6295d9733
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870402"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Introduction aux Suggestions proactives dans Xamarin.iOS

_Cet article montre comment utiliser des Suggestions proactives dans l’application Xamarin.iOS pour favoriser l’engagement en autorisant le système de façon proactive présenter automatiquement des informations utiles à l’utilisateur._

Nouveau à iOS 10, Suggestions proactives présent news façons pour les utilisateurs à s’engager avec une application Xamarin.iOS en informations utiles présentes de manière proactive automatiquement à l’utilisateur aux moments opportuns.

iOS 10 présente les nouvelles façons d’engagement de conduite à l’application en autorisant le système de manière proactive présenter des informations utiles automatiquement à l’utilisateur aux moments opportuns. Tout comme iOS 9 fourni la possibilité d’ajouter une recherche approfondie à l’application à l’aide de Spotlight, remise et les Suggestions de Siri (consultez [nouvelles API de recherche](~/ios/platform/search/index.md)), avec iOS 10 une application peut exposer des fonctionnalités qui peuvent être présentées à l’utilisateur par le système depuis le emplacements suivants :

- Le sélecteur d’application
- L’écran de verrouillage
- CarPlay
- Maps
- Interactions de Siri
- Suggestions de QuickType

L’application expose cette fonctionnalité dans le système à l’aide d’une collection de technologies telles que `NSUserActivity`, balisage web, Spotlight de base, MapKit, Media Player et UIKit. En outre, en fournissant la prise en charge de Suggestion Proactive pour l’application, il obtient une intégration plus profonde Siri gratuitement.

## <a name="location-based-suggestions"></a>Emplacement en fonction des Suggestions

Nouveau à iOS 10, le `NSUserActivity` classe inclut un `MapItem` propriété qui permet au développeur de fournir des informations d’emplacement qui peuvent être utilisées dans d’autres contextes. Par exemple, si l’application affiche des commentaires sur des restaurants, le développeur peut définir le `MapItem` propriété à l’emplacement du restaurant l’utilisateur est affiché dans l’application. Si l’utilisateur passe à l’application de cartes, les emplacement du restaurant sont automatiquement disponible.

Si l’application prend en charge la recherche de l’application, il peut utiliser les nouveaux composants de l’adresse de la `CSSearchableItemAttributesSet` classe pour spécifier des emplacements de l’utilisateur doit pouvoir à visiter. En définissant le `MapItem` propriété, les autres propriétés sont renseignés automatiquement.

En plus du paramètre le `Latitude` et `Longitude` les composant Propriétés d’adresse, il est recommandé que l’application fournisse la `NamedLocation` et `PhoneNumbers` propriétés trop, par conséquent, Siri peut initier un appel à l’emplacement.

## <a name="web-markup-based-suggestions"></a>Balisage Web en fonction des Suggestions

iOS 9 ajouté à la possibilité d’inclure la balise de données structurées dans le site Web qui enrichit le contenu que les utilisateurs voient dans les résultats de la recherche Spotlight et Safari (consultez [recherche avec le balisage Web](~/ios/platform/search/web-markup.md)). iOS 10 ajoute la possibilité d’inclure la balise en fonction du lieu (telles que [omettre](http://schema.org/PostalAddress) tel que défini par [Schema.org](http://schema.org/)) pour améliorer l’expérience utilisateur. Par exemple, si un vues aux utilisateurs un emplacement marqué sur le site Web, le système peut suggérer au même emplacement lorsqu’ils ouvrent des mappages.

## <a name="text-based-suggestions"></a>Texte en fonction des Suggestions

UIKit a été développée dans iOS 10 à inclure le [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) propriété de la [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) classe pour spécifier la signification sémantique du contenu dans une zone de texte. Avec ces informations en place, le système peut sélectionner généralement automatiquement le type de clavier appropriés, améliorer les suggestions de correction automatique et intégrer proactivement des informations à partir d’autres applications et les sites Web.

Par exemple, si l’utilisateur entre du texte dans un champ de texte marqué `UITextContentType.FullStreetAddress`, le système peut suggérer le remplissage automatique du champ avec l’emplacement de l’utilisateur a été récemment affichage.

## <a name="media-based-suggestions"></a>Support en fonction des Suggestions

Si l’application lit le média à l’aide du [MPPlayableContentManager](xref:MediaPlayer.MPPlayableContentManager) API, iOS 10 permet aux utilisateurs de visualiser la pochette d’album et lire des éléments multimédias via l’application sur l’écran de verrouillage.

## <a name="contextual-siri-reminders"></a>Rappels de Siri contextuelles

Permet à l’utilisateur à utiliser Siri pour effectuer rapidement un rappel pour afficher le contenu qu’ils sont actuellement affiché dans l’application à une date ultérieure. Par exemple, si elles ont été afficher une révision de restaurant dans l’application, ils pourraient appeler Siri et dire *« Me le rappeler à ce sujet lorsque j’obtiens accueil. »* Siri génèrerait le rappel avec un lien vers la révision dans l’application.

## <a name="contact-based-suggestions"></a>Contact en fonction des Suggestions

Autorise l’application contacts (et les coordonnées associées) apparaissent dans le **contacter** application sur l’appareil iOS, ainsi que tous les contacts des utilisateurs stockés dans le système.

## <a name="ride-sharing-based-suggestions"></a>Tour de partage en fonction des Suggestions

Si une application de partage tour utilise le [MKDirectionsRequest](xref:MapKit.MKDirectionsRequest) API, iOS 10 présente en tant qu’option dans le sélecteur d’application lorsque l’utilisateur est susceptible de vouloir un tour. L’application doit également être enregistrée comme une application de partage de course, en spécifiant le `MKDirectionsModeRideShare` pour le [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) clés dans son `Info.plist` fichier.

Si l’application prend uniquement en charge le partage de tour, la suggestion système débuteriez avec *« Obtenir un tour à... »*, si d’autres types de routage direction (par exemple, Walking ou vélo) sont pris en charge, le système utilisera *« Get directions vers... »*

> [!IMPORTANT]
> Le [MKMapItem](xref:MapKit.MKMapItem) objet qui reçoit l’application peut ne pas inclure les informations de longitude et latitude et nécessitera géocodage.

## <a name="implementing-proactive-suggestions"></a>Implémentation de Suggestions proactives

Ajout de Suggestion Proactive prise en charge pour une application Xamarin.iOS est généralement aussi simple que l’implémentation de certaines API ou le développement sur quelques API de l’application peut-être déjà être implémentés par.

Suggestions proactives fonctionnent avec les applications de trois façons :

- **`NSUserActivity` et Schema.org**  -  `NSUserActivity` aide le système qui décrit les informations que l’utilisateur travaille actuellement avec sur l’écran. Schema.org ajoute des capacités similaires aux pages web.
- **Suggestions d’emplacement** : si l’application offre ou consomme des informations basées sur l’emplacement, ces API extension offre de nouvelles façons de partager ces informations entre applications.
- **Suggestions d’application Media** : le système peut promouvoir l’application et son contenu multimédia en fonction du contexte de l’interaction utilisateur avec l’appareil iOS.

Est pris en charge dans l’application en implémentant les éléments suivants :

- **Procédure de transfert**  -  `NSUserActivity` a été ajoutée dans iOS 8 pour prendre en charge de la remise, ce qui permet au développeur de lancer une activité sur un périphérique, puis continuez sur un autre (voir [Introduction à la procédure de transfert](~/ios/platform/handoff.md)).
- **Spotlight recherche** -iOS 9 ajouté la possibilité de promouvoir le contenu de l’application depuis les résultats de la recherche Spotlight à l’aide de `NSUserActivity` (consultez [recherche avec Spotlight de base](~/ios/platform/search/corespotlight.md)).
- **Les rappels de Siri contextuelles** - dans iOS 10, `NSUserActivity` a été étendu pour autoriser Siri à effectuer rapidement un rappel pour afficher le contenu de l’utilisateur est actuellement affiché dans l’application à une date ultérieure.
- **Suggestions d’emplacement** -iOS 10 améliore `NSUserActivity` pour capturer les emplacements affichés à l’intérieur de l’application et de les promouvoir à différents endroits du système.
- **Les requêtes Siri contextuelles**  -  `NSUserActivity` fournit le contexte pour les informations présentées à l’intérieur de l’application à Siri afin que l’utilisateur puisse directions ou lieux un appel appel Siri à partir de l’application.
- **Contactez les Interactions** - nouvelle dans iOS 10, `NSUserActivity` permet aux applications de communications d’être promus à partir d’une carte de contact (dans l’application Contacts) comme une méthode de communication de remplacement.

Toutes ces fonctionnalités ont une chose en commun, ils utilisent tous `NSUserActivity` dans un formulaire ou autre pour fournir leur fonctionnalité. 

## <a name="nsuseractivity"></a>NSUserActivity

Comme indiqué ci-dessus, `NSUserActivity` aide le système qui décrit les informations que l’utilisateur travaille actuellement avec sur l’écran. `NSUserActivity` un état non activable est mise en cache mécanisme permettant de capturer l’activité de l’utilisateur quand ils naviguent via l’application. Par exemple, examinons une application de restaurant :

[![](proactive-suggestions-images/activity02.png "L’état de non activable NSUserActivity mécanisme de mise en cache")](proactive-suggestions-images/activity02.png#lightbox)

Avec les interactions suivantes :

1. Comme l’utilisateur travaille avec l’application, un `NSUserActivity` est créé pour recréer l’état de l’application plus tard.
2. Si l’utilisateur recherche un restaurant, le même modèle de création d’activités est suivi.
3. Et là encore, lorsque l’utilisateur affiche un résultat. Dans ce dernier cas, l’utilisateur consulte un emplacement et dans iOS 10, le système est plus conscient de certains concepts (tels que les interactions de communication ou d’emplacement).

Examinons plus en détail le dernier écran :

[![](proactive-suggestions-images/activity03.png "Les détails de NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

Ici l’application consiste à créer un `NSUserActivity` et il a été rempli avec les informations pour recréer l’état ultérieurement. L’application a également inclus des métadonnées telles que le nom et l’adresse de l’emplacement. Avec cette activité est créée, l’application informe iOS qu’il représente l’état actuel de l’utilisateur.

L’application décide alors si l’activité est publiée par voie hertzienne pour la remise, enregistrée sous la forme d’une valeur temporaire pour obtenir des suggestions d’emplacement ou ajoutée à l’index de Spotlight sur l’appareil pour l’affichage des résultats de recherche.

Pour plus d’informations sur la recherche des transitions graduelles et Spotlight, veuillez consulter notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) et [iOS 9 nouvelles API de recherche](~/ios/platform/search/index.md) guides.

### <a name="creating-an-activity"></a>Création d’une activité

Avant de créer une activité, un identificateur de Type d’activité est nécessaire pour être créé pour être identifié. L’identificateur de Type d’activité est une chaîne courte est ajoutée à la `NSUserActivityTypes` tableau de l’application `Info.plist` fichier utilisé pour identifier un Type d’activité utilisateur donné. Il y aura une entrée dans le tableau pour chaque activité qui prend en charge de l’application et expose à la recherche de l’application. Consultez notre [création Type identificateurs de référence sur l’activité](~/ios/platform/search/nsuseractivity.md) pour plus d’informations.

Étudier un exemple d’une activité :

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Une activité est créée à l’aide d’un identificateur de Type d’activité. Ensuite, certaines métadonnées qui définissent l’activité sont créée pour cet état peut être restauré à une date ultérieure. Ensuite, l’activité est un titre significatif et attachée aux informations utilisateur. Enfin, certaines fonctionnalités sont activées et que l’activité est envoyée au système.

Le code ci-dessus peut encore être amélioré pour inclure les métadonnées qui fournit le contexte à l’activité en apportant les modifications suivantes :

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Si le développeur a un site Web qui est capable d’afficher les mêmes informations que l’application, l’application peut inclure l’URL et le contenu peut être affiché sur d’autres appareils qui n’ont pas l’application est installée (via la procédure de transfert) :

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Restauration d’une activité

Pour répondre à l’utilisateur en appuyant sur un résultat de recherche (`NSUserActivity`) pour l’application, vous devez modifier le **AppDelegate.cs** de fichiers et de remplacer le `ContinueUserActivity` (méthode). Exemple :

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

Le développeur devra s’assurer c’est le même identificateur de Type d’activité (`com.xamarin.platform`) en tant que l’activité créée ci-dessus. L’application utilise les informations stockées dans le `NSUserActivity` pour restaurer l’état dans lequel l’utilisateur s’est arrêté.

### <a name="benefits-of-creating-an-activity"></a>Avantages de la création d’une activité

Avec la quantité minimale de code présenté ci-dessus, l’application est maintenant en mesure de tirer parti des trois nouvelles fonctionnalités d’iOS 10 :

- **Handoff**
- **Recherche Spotlight**
- **Rappels de Siri contextuelles**

La section suivante sera un œil à activer les deux autres nouvelles fonctionnalités iOS 10 :

- **Suggestions d’emplacement**
- **Requêtes Siri contextuelles**

### <a name="location-based-suggestions"></a>Emplacement en fonction des Suggestions 

Prenons l’exemple de l’application de recherche de restaurant ci-dessus. Si elle a implémenté `NSUserActivity` et correctement rempli toutes les métadonnées et les attributs, l’utilisateur serait en mesure d’effectuer les opérations suivantes :

1. Trouver un restaurant dans l’application qu’ils souhaiteraient pour répondre à un ami chez.
2. Comme l’utilisateur s’éloigne de l’application à l’aide du sélecteur d’application multitâche, le système affiche automatiquement une suggestion (en bas de l’écran) pour obtenir des instructions au restaurant à l’aide de leur application de navigation favori.
3. Si l’utilisateur bascule vers l’application Messages et commence à taper *« Rendez-vous à »*, le clavier QuickType suggère automatiquement de coller l’adresse du restaurant.
4. Si l’utilisateur passe à l’application de cartes, les adresses du restaurant sont suggérés automatiquement en tant que destination.
5. Cela fonctionne même pour les applications tierces 3e (qui prennent en charge `NSUserActivity`), de sorte que l’utilisateur peut basculer vers une application de partage de course et les adresses du restaurant sont automatiquement suggéré en tant que destination il également.
6. Elle également fournit un contexte à Siri, l’utilisateur peut appeler Siri au sein de l’application de restaurant et demandez *« Get directions... »* et Siri fournira des instructions au restaurant l’affichage de l’utilisateur.

Toutes les fonctionnalités ci-dessus a une chose en commun, tous les indiquent où la suggestion à l’origine provient. Dans le cas de l’exemple ci-dessus, il est l’application de révision de restaurant fictive.

iOS 10 a été amélioré pour activer cette fonctionnalité pour une application via plusieurs petites modifications et ajouts aux infrastructures existantes :

- `NSUserActivity` comporte des champs supplémentaires pour capturer les informations d’emplacement qui sont affichées à l’intérieur de l’application.
- Plusieurs ajouts ont été apportées à MapKit et CoreSpotlight pour capturer l’emplacement.
- Prenant en charge les fonctionnalités d’emplacement a été ajoutée à Siri, cartes, claviers, multitâche et autres applications au sein du système.

Pour implémenter des suggestions basées sur l’emplacement, démarrer avec le même code d’activité présenté ci-dessus :

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Si l’application utilise MapKit, c’est aussi simple que l’ajout de l’objet map actuel `MKMapItem` à l’activité :

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Si l’application n’utilise pas MapKit, il peut adopter la recherche de l’application et spécifier les nouveaux attributs pour l’emplacement suivants :

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Examinons le code ci-dessus en détail. Tout d’abord, le nom de l’emplacement est requis dans chaque instance :

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Ensuite, la description de texte en fonction de l’emplacement est requise pour les instances textuel (par exemple, le clavier QuickType) :

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

La latitude et la longitude sont facultatifs, mais assurez-vous que l’utilisateur est acheminé vers l’emplacement exact de que l’application, si vous voulez les envoyer, elle devrait donc être inclus :

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

En définissant les numéros de téléphone, l’application peut accéder à Siri afin de l’utilisateur peut appeler Siri à partir de l’application en disant quelque chose comme : *« Appeler cet emplacement »*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Enfin, l’application peut indiquer si l’instance est approprié pour la navigation et les appels téléphoniques :

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Implémentation des Interactions de Contact

Nouveau dans iOS 10, applications de communication sont profondément intégrées à l’application Contacts à partir de la carte de visite. Pour les applications qui ont implémenté des Interactions de Contact, l’utilisateur peut ajouter des informations de l’application donnée à des personnes spécifiques dans leurs contacts. Et si, par exemple, ils appuyez sur le bouton d’action rapide en haut d’une carte pour envoyer un message, l’application attachée est répertoriée en tant qu’option pour envoyer le message à partir de.

Si une application tierce 3e est sélectionnée, il peut mémorisé et présenté sous la forme la façon par défaut pour la prochaine fois que l’utilisateur souhaite les contacter la personne donnée du message.

Interactions de contact sont implémentées dans l’application à l’aide `NSUserActivity` et la nouvelle infrastructure d’intentions introduit dans iOS 10. Pour plus d’informations sur l’utilisation des intentions, veuillez consulter notre [présentation des Concepts de SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) et [implémentation de SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) guides.

#### <a name="donating-interactions"></a>Interactions de dons

Examinons comment l’application peut faire un don interactions :

[![](proactive-suggestions-images/activity04.png "Vue d’ensemble des Interactions de dons")](proactive-suggestions-images/activity04.png#lightbox)

L’application crée un `INInteraction` objet qui contient un **intention** (`INIntent`), **Participants** et **métadonnées**. Le **intention** représente une action de l’utilisateur comme un appel vidéo ou l’envoi d’un message texte. Le **Participants** incluent les destinataires de la communication. Le **métadonnées** définit des informations supplémentaires telles que l’envoi correctement le message, etc.

Le développeur jamais directement crée une instance de `INIntent` ou `INIntentResponse`, ils utiliseront une des classes enfant spécifique (selon la tâche à l’accomplissement de l’application pour le compte de l’utilisateur) qui héritent de ces classes parentes. Par exemple, `INSendMessageIntent` et `INSendMessageIntentResponse` pour envoyer un message texte. 

Une fois que l’interaction est entièrement remplie, appelez le `DonateInteraction` méthode pour informer le système que l’interaction peut être utilisé.

Quand l’utilisateur interagit avec l’application à partir de la carte de visite, l’interaction obtient fournie avec un `NSUserActivity`, qui est ensuite utilisé pour lancer l’application :

[![](proactive-suggestions-images/activity05.png "L’interaction obtient fournie avec un NSUserActivity qui est utilisé pour lancer l’application")](proactive-suggestions-images/activity05.png#lightbox)

Examinons l’exemple suivant d’une intention d’envoi de Message :

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

En examinant ce code en détail, il crée et remplit une instance de `NSUserActivity` (comme indiqué dans le [création d’une activité](#creating-an-activity) section ci-dessus). Ensuite, il crée une instance de `INSendMessageIntent` (qui hérite `INIntent`) et la remplit avec les détails du message envoyé :

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

Un `INSendMessageIntentResponse` est créée et passe le `NSUserActivity` créé ci-dessus :

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

Un `INInteraction` est construit à partir de l’intention d’envoi de Message (`INSendMessageIntent`) et de réponse (`INSendMessageIntentResponse`) venez de créer :

```csharp
var interaction = new INInteraction (intent, response);
```

Enfin, le système est la notification de l’Interaction :

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Un gestionnaire d’achèvement est passé dans où l’application peut répondre à la donation réussi ou échoué.

### <a name="activities-best-practices"></a>Activités de meilleures pratiques

Apple suggère les meilleures pratiques suivantes lorsque vous travaillez avec des activités :

- Utilisez `NeedsSave` des mises à jour de la charge utile différé.
- Veillez à conserver une référence forte à l’activité en cours.
- Transférer uniquement les petites charges utiles qui incluent des informations de base pour restaurer l’état.
- Assurez-vous que les identificateurs de Type d’activité sont unique et descriptif de les spécifier à l’aide de la notation DNS inverse. 

## <a name="schemaorg"></a>Schema.org

Comme indiqué ci-dessus, `NSUserActivity` aide le système qui décrit les informations que l’utilisateur travaille actuellement avec sur l’écran. Schema.org ajoute des capacités similaires aux pages web.

Schema.org peut fournir les mêmes types d’interactions basées sur l’emplacement sur le site Web. Apple conçu les suggestions d’emplacement nouveau fonctionne aussi bien lorsqu’ils sont affichés dans Safari, comme ils le font dans une application native.

Certaines notions Schema.org :

- Il fournit une norme de vocabulaire de balisage open web.
- Il fonctionne en incluant des métadonnées structurées sur les pages web.
- Il existe plus de 500 schémas représentant les différents concepts disponibles.
- En l’implémentant sur le site Web, le développeur peut acquérir quelques-uns des avantages de l’utilisation de `NSUserActivity` dans une application native.

Les schémas sont organisées dans une arborescence comme structure, où les types spécifiques tels que *Restaurant*, hériter des types plus génériques tel que *entreprise Local*. Pour plus d’informations, consultez [Schema.org](http://schema.org).

Par exemple, si la page web contient les données suivantes :

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

Si l’utilisateur visite cette page dans Safari et ensuite basculées vers une autre application, les informations d’emplacement à partir de la page seraient être capturées et proposées en tant qu’une Suggestion d’emplacement dans d’autres parties du système (comme indiqué dans les activités ci-dessus).

Safari extraira quoi que ce soit sur une page web qui adhère à une des propriétés de schéma suivantes :

- **PostalAddress**
- **GeoCoordinates**
- Une propriété de téléphone.

Pour plus d’informations, consultez notre [recherche avec le balisage Web](~/ios/platform/search/web-markup.md) guide.

## <a name="consuming-location-suggestions"></a>Consommation des Suggestions d’emplacement

La section suivante couvre la consommation Suggestion d’emplacement issues d’autres parties du système (par exemple, l’application Maps) ou d’autres applications tierces 3e.

Il existe deux façons que l’application peut utiliser des Suggestions d’emplacement :

- Via le clavier QuickType.
- Directement par consomme les informations d’emplacement dans les applications de routage.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Suggestions d’emplacement et le clavier QuickType

Si l’application utilise des adresses dans les formats de texte en fonction, l’application peut tirer parti de vos Suggestions d’emplacement par l’UI QuickType. iOS 10 développe l’UI QuickType avec les fonctionnalités suivantes :

- L’application peut ajouter des conseils sur l’intention de sémantique pour les champs de texte dans l’interface utilisateur.
- L’application peut obtenir des suggestions proactives dans l’application.
- L’application peut bénéficier de correction automatique améliorée.

La nouvelle `TextContentType` propriété des contrôles de champ de texte dans iOS 10 permet au développeur de définir l’intention de sémantique pour la valeur que l’utilisateur va entrer dans un champ donné. Exemple :

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Voulez-vous indiquer au système de que l’application attend que l’utilisateur à entrer une adresse postale complète dans le champ donné. Ainsi, le clavier QuickType fournir automatiquement des Suggestions d’emplacement sur le clavier lorsque l’utilisateur entre une valeur dans ce champ.

Voici quelques-uns des types plus courants disponibles pour les développeurs dans le `UITextContentType` classe statique :

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Applications de routage et les Suggestions d’emplacements

Cette section sera un œil à la consommation des Suggestions d’emplacement directement à partir d’une application de routage. Pour l’application de routage ajouter cette fonctionnalité, le développeur s’appuiera existant `MKDirectionsRequest` framework comme suit :

- Pour promouvoir l’application dans le multitâche.
- Pour inscrire l’application comme une application de routage.
- Pour gérer le lancement de l’application avec un MapKit `MKDirectionsRequest` objet.
- Afin de permettre à iOS pour en savoir plus suggérer l’application à l’utilisateur aux moments opportuns, selon l’intérêt des utilisateurs.

Lorsque l’application est démarrée avec un MapKit `MKDirectionsRequest` de l’objet, il doit automatiquement démarrer en donnant les instructions de l’utilisateur à l’emplacement demandé, ou présenter une interface utilisateur qui facilite l’utilisateur commencer à obtenir des instructions. Exemple :


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

Examinons ce code en détail. Il vérifie s’il s’agit d’une demande de destination valide :

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Si elle est, il crée un `MKDirectionsRequest` à partir de l’URL :

```csharp
var request = new MKDirectionsRequest(url);
```

Nouveau dans iOS 10, l’application peut être envoyée une adresse qui n’a pas de coordonnées géographiques, dans qui entraînent que le développeur a besoin d’encoder l’adresse :

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Suggestions d’application multimédia

Si l’application gère n’importe quel type de média comme une application de podcast ou un contenu multimédia de diffusion en continu audio ou vidéo, avec iOS 10, il peut tirer parti des Suggestions de supports dans le système.

Pour les applications qui gèrent des médias, iOS prend en charge les comportements suivants :

- iOS promeut les applications dont l’utilisateur est susceptible d’utiliser en fonction de leur comportement précédent.
- Suggestions relatives à l’application seront affiche dans Spotlight et la vue aujourd'hui.
- Si l’application répond à un des déclencheurs suivants, il peut être élevé pour une suggestion d’écran de verrouillage :
    - Après avoir connecté un casque ou d’un appareil Bluetooth établit une connexion.
    - Après l’obtention d’une voiture.
    - Après avoir arrivant à domicile ou travail. 

En incluant un simple appel d’API dans iOS 10, le développeur peut créer une expérience d’écran de verrou plus attrayante pour les utilisateurs de l’application multimédia. À l’aide de la `MPPlayableContentManager` classe pour gérer la lecture du média, les contrôles de média complet (tels que ceux présentés par l’application de musique) s’affiche sur l’écran de verrouillage de l’application.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>Récapitulatif

Cet article a couvert les Suggestions proactives et vous a montré comment le développeur peut les utiliser pour le trafic de lecteur vers l’application Xamarin.iOS. Il couvert l’étape pour implémenter des Suggestions proactives et affiche des instructions d’utilisation.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Guide de programmation de SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
