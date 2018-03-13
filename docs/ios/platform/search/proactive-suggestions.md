---
title: "Présentation des Suggestions Proactive"
description: "Cet article explique comment utiliser des Suggestions Proactive dans l’application Xamarin.iOS à l’engagement de lecteur en permettant au système de façon proactive présenter automatiquement des informations utiles à l’utilisateur."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 15f649440b2a855189acff33afcef5e8272a0769
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-proactive-suggestions"></a>Présentation des Suggestions Proactive

_Cet article explique comment utiliser des Suggestions Proactive dans l’application Xamarin.iOS à l’engagement de lecteur en permettant au système de façon proactive présenter automatiquement des informations utiles à l’utilisateur._

Nouveau pour iOS 10, Suggestions Proactive présentes news aux utilisateurs des moyens de s’engager avec une application Xamarin.iOS par des informations utiles présentes de manière proactive automatiquement à l’utilisateur au moment opportun.

iOS 10 présente les nouvelles façons d’implication conduite à l’application en autorisant le système de façon proactive présente des informations utiles automatiquement à l’utilisateur au moment opportun. Comme d’iOS 9 fourni la possibilité d’ajouter une recherche approfondie à l’application à l’aide de la galerie, remise et les Suggestions de Siri (consultez [nouvelles API de recherche](~/ios/platform/search/index.md)), avec iOS 10 une application peut exposer des fonctionnalités qui peuvent être présentées à l’utilisateur par le système à partir du emplacements suivants :

- Le sélecteur d’application
- L’écran de verrouillage
- CarPlay
- Maps
- Interactions de Siri
- Suggestions de QuickType

L’application expose cette fonctionnalité dans le système à l’aide d’une collection de technologies telles que `NSUserActivity`, balisage web, Core Spotlight, MapKit, lecteur multimédia et UIKit. En outre, en fournissant la prise en charge de Suggestion Proactive pour l’application, il obtient une intégration plus étroite Siri gratuitement.

## <a name="location-based-suggestions"></a>Selon l’emplacement des Suggestions

Vous débutez avec iOS 10, le `NSUserActivity` classe inclut un `MapItem` propriété qui permet au développeur de fournir des informations sur l’emplacement qui peuvent être utilisées dans d’autres contextes. Par exemple, si l’application affiche les revues sur des restaurants, le développeur peut définir le `MapItem` propriété à l’emplacement du restaurant l’utilisateur est affiché dans l’application. Si l’utilisateur passe à l’application de cartes, les emplacement du restaurant sont automatiquement disponible.

Si l’application prend en charge la recherche de l’application, il peut utiliser les nouveaux composants de l’adresse de la `CSSearchableItemAttributesSet` classe pour spécifier les emplacements que l’utilisateur peut visiter. En définissant le `MapItem` propriété, les autres propriétés sont automatiquement renseignés.

En plus du paramètre de la `Latitude` et `Longitude` des propriétés de composant d’adresse, il est recommandé que l’application fournisse la `NamedLocation` et `PhoneNumbers` propriétés, par conséquent, Siri peut initier un appel à l’emplacement.

## <a name="web-markup-based-suggestions"></a>Balisage Web en fonction des Suggestions

iOS 9 ajouté à la possibilité d’inclure le balisage des données structurées dans le site Web qui enrichit le contenu que les utilisateurs voient dans les résultats de la recherche Spotlight et Safari (consultez [recherche par un balisage Web](~/ios/platform/search/web-markup.md)). iOS 10 ajoute la possibilité d’inclure la balise de basée sur l’emplacement (tel que [omettre](http://schema.org/PostalAddress) comme défini par [Schema.org](http://schema.org/)) pour améliorer l’expérience utilisateur. Par exemple, si une vue des utilisateurs un emplacement marqué sur le site Web, le système peut suggérer au même emplacement lors de l’ouverture de mappages.

## <a name="text-based-suggestions"></a>Des Suggestions basées sur du texte

UIKit a été développée dans iOS 10 à inclure le [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) propriété de la [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) classe pour spécifier la sémantique du contenu dans une zone de texte. Avec ces informations en place, le système peut sélectionner généralement automatiquement le type de clavier appropriés, améliorer les suggestions de correction automatique et proactive intégrer des informations provenant d’autres applications et les sites Web.

Par exemple, si l’utilisateur entre du texte dans un champ de texte marqué `UITextContentType.FullStreetAddress`, le système peut suggérer le remplissage automatique du champ avec l’emplacement de l’utilisateur a été récemment affichage.

## <a name="media-based-suggestions"></a>Support en fonction des Suggestions

Si l’application est lue à l’aide du support le [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) API iOS 10 permet aux utilisateurs de consulter la pochette et lire des médias dans l’application sur l’écran de verrouillage.

## <a name="contextual-siri-reminders"></a>Rappels Siri contextuelles

Permet à l’utilisateur Siri permet d’effectuer rapidement un rappel pour afficher le contenu, qu'ils affichée dans l’application à une date ultérieure. Par exemple, si elles ont été affichage revue sur un restaurant dans l’application, ils pourraient appeler Siri et dire *« Me le rappeler à ce sujet s’accueil. »* Siri génère le rappel avec un lien vers la révision dans l’application.

## <a name="contact-based-suggestions"></a>Contact en fonction des Suggestions

Permet à l’application contacts (et des informations connexes contacts) s’affichent dans le **contactez** application sur l’appareil iOS, ainsi que tous les contacts de l’utilisateur est stocké dans le système.

## <a name="ride-sharing-based-suggestions"></a>Gérer des Suggestions en fonction de partage

Si une application de partage de gestion utilise le [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) API iOS 10 présente en tant qu’option dans le sélecteur de l’application lorsque l’utilisateur est susceptible de vouloir une commutation. L’application doit également être enregistrée en tant qu’une application de partage de compétences en spécifiant le `MKDirectionsModeRideShare` pour le [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) clé dans son `Info.plist` fichier.

Si l’application prend uniquement en charge le partage des cas, la suggestion système débuteriez avec *« D’obtenir une porté à... »*, si d’autres types de direction de routage (par exemple, cannes ou vélo) sont pris en charge, le système utilisera *« Obtenir des instructions pour... »*

> [!IMPORTANT]
> **Remarque :** le [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) objet qui reçoit de l’application ne peut pas inclure les informations de longitude et de latitude et nécessitera le géocodage.

## <a name="implementing-proactive-suggestions"></a>Implémentation de Suggestions Proactive

Prise en charge pour une application Xamarin.iOS ajoutant Proactive Suggestion est généralement aussi simple que quelques API de mise en œuvre ou le développement sur quelques API l’application peut-être déjà être mise en œuvre.

Suggestions proactive fonctionnent avec les applications de trois manières principales :

- **`NSUserActivity` et Schema.org**  -  `NSUserActivity` permet le système qui décrit les informations que l’utilisateur travaille actuellement avec sur l’écran. Schema.org ajoute des fonctionnalités similaires à des pages web.
- **Suggestions d’emplacement** - si l’application offre ou consomme des informations sur l’emplacement de base, ces API extension offre de nouvelles façons de partager ces informations entre les applications.
- **Des Suggestions de média application** : le système peut promouvoir l’application et son contenu média basé sur le contexte de l’interaction utilisateur avec l’appareil iOS.

Est pris en charge dans l’application en implémentant les éléments suivants :

- **Procédure de transfert**  -  `NSUserActivity` a été ajoutée dans iOS 8 pour prendre en charge la remise, ce qui permet au développeur de lancer une activité sur un périphérique, puis continuez sur un autre (voir [Introduction à la procédure de transfert](~/ios/platform/handoff.md)).
- **Mettre en évidence recherche** -iOS 9 a ajouté la possibilité de promouvoir contenu de l’application depuis les résultats de la recherche Spotlight à l’aide de `NSUserActivity` (consultez [recherche avec Core Spotlight](~/ios/platform/search/corespotlight.md)).
- **Les rappels Siri contextuelles** - dans iOS 10, `NSUserActivity` a été développée pour autoriser Siri à effectuer rapidement un rappel pour afficher le contenu de l’utilisateur est affichée dans l’application à une date ultérieure.
- **Suggestions d’emplacement** -iOS 10 améliore `NSUserActivity` pour capturer les emplacements affichés à l’intérieur de l’application et de les promouvoir à différents endroits du système.
- **Les demandes Siri contextuelles**  -  `NSUserActivity` fournit le contexte pour les informations présentées à l’intérieur de l’application à Siri afin que l’utilisateur puisse directions ou place un appel appel Siri à partir de l’application.
- **Contactez les Interactions** - nouvelle dans iOS 10, `NSUserActivity` permet aux applications de communication à promouvoir à partir d’une carte de contact (dans l’application Contacts) en tant qu’une méthode de communication de remplacement.

Toutes ces fonctionnalités ont un point commun, ils utilisent tous `NSUserActivity` dans un formulaire ou autre pour fournir leur fonctionnalité. 

## <a name="nsuseractivity"></a>NSUserActivity

Comme indiqué précédemment, `NSUserActivity` permet le système qui décrit les informations que l’utilisateur travaille actuellement avec sur l’écran. `NSUserActivity` un état non activable met en cache le mécanisme de capture de l’activité de l’utilisateur lorsqu’ils naviguent dans l’application. Examinons, par exemple, une application de restaurant :

[![](proactive-suggestions-images/activity02.png "L’état de non activable NSUserActivity mécanisme de mise en cache")](proactive-suggestions-images/activity02.png#lightbox)

Avec les interactions suivantes :

1. Lorsque l’utilisateur travaille avec l’application, un `NSUserActivity` est créé pour recréer l’état de l’application plus tard.
2. Si l’utilisateur recherche un restaurant, le même modèle de création d’activités est suivi.
3. Et, lorsque l’utilisateur affiche un résultat. Dans ce dernier cas, l’utilisateur consulte un emplacement et dans iOS 10, le système est plus prenant en charge de certains concepts (tels que les interactions de communication ou d’emplacement).

Regardez plus près le dernier écran :

[![](proactive-suggestions-images/activity03.png "Les détails de NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

L’application consiste à créer ici un `NSUserActivity` et il a été remplie avec les informations pour recréer l’état ultérieurement. L’application a également inclus des métadonnées telles que le nom et l’adresse de l’emplacement. Avec cette activité est créée, l’application informe iOS qu’il représente l’état actuel de l’utilisateur.

L’application décide alors que si l’activité vont être publiée par liaison radio pour la transférer, enregistrée sous la forme d’une valeur temporaire pour obtenir des suggestions d’emplacement ou ajoutée à l’index de la galerie sur l’appareil pour l’affichage dans les résultats de la recherche.

Pour plus d’informations sur la recherche Spotlight et de remise, consultez notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) et [iOS 9 nouvelles API de recherche](~/ios/platform/search/index.md) guides.

### <a name="creating-an-activity"></a>Création d’une activité

Avant de créer une activité, un identificateur de Type d’activité devront-elles être créée pour l’identifier. L’identificateur de Type d’activité est une chaîne courte est ajoutée à la `NSUserActivityTypes` tableau de l’application `Info.plist` fichier utilisé pour identifier un Type d’activité utilisateur donné. Il y aura une entrée dans le tableau pour chaque activité qui prend en charge de l’application et expose à la recherche de l’application. Consultez notre [référence identificateurs de Type d’activité de création](~/ios/platform/search/nsuseractivity.md) pour plus d’informations.

Examiner un exemple d’une activité :

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

Une nouvelle activité est créée à l’aide d’un identificateur de Type d’activité. Ensuite, des métadonnées de définition de l’activité sont créée pour cet état peut être restauré à une date ultérieure. Puis, l’activité est de titre explicite et attachée à l’utilisateur. Enfin, certaines fonctionnalités sont activées et que l’activité est envoyée au système.

Le code ci-dessus peut être amélioré davantage pour inclure des métadonnées qui fournit le contexte à l’activité en apportant les modifications suivantes :

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

Si le développeur a un site Web qui est capable d’afficher les mêmes informations que l’application, l’application peut inclure l’URL et le contenu puisse être affiché sur d’autres appareils qui n’ont pas l’application est installée (via la procédure de transfert) :

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Restauration d’une activité

Pour répondre à l’utilisateur en appuyant sur un résultat de recherche (`NSUserActivity`) pour l’application, vous devez modifier le **AppDelegate.cs** de fichier et remplacez le `ContinueUserActivity` (méthode). Exemple :

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

Le développeur doit Assurez-vous qu’est le même identificateur de Type d’activité (`com.xamarin.platform`) en tant que l’activité créée ci-dessus. L’application utilise les informations stockées dans le `NSUserActivity` pour restaurer l’état dans lequel l’utilisateur s’est arrêté.

### <a name="benefits-of-creating-an-activity"></a>Avantages de la création d’une activité

Avec la quantité minimale de code présenté plus haut, l’application est maintenant en mesure de tirer parti des trois nouvelles fonctionnalités iOS 10 :

- **Handoff**
- **Recherche Spotlight**
- **Rappels Siri contextuelles**

La section suivante s’intéresser à l’activation de deux autres nouvelles fonctionnalités iOS 10 :

- **Suggestions d’emplacement**
- **Demandes Siri contextuelles**

### <a name="location-based-suggestions"></a>Selon l’emplacement des Suggestions 

Prenons l’exemple de l’application de recherche de restaurant ci-dessus. Si elle a implémenté `NSUserActivity` et renseigné correctement toutes les métadonnées et les attributs, l’utilisateur pourront effectuer les opérations suivantes :

1. Dans l’application à répondre à une fonction friend à de trouver un restaurant.
2. Lorsque l’utilisateur se trouve en dehors de l’application à l’aide du sélecteur d’application multitâche, le système affiche automatiquement une suggestion (en bas de l’écran) pour obtenir des instructions au restaurant à l’aide de leur application de navigation Favoris.
3. Si l’utilisateur bascule vers l’application Messages et commence à taper *« Rendez-vous à »*, le clavier QuickType suggère automatiquement les coller dans l’adresse du restaurant.
4. Si l’utilisateur passe à l’application de cartes, les adresses du restaurant sont suggérés automatiquement en tant que destination.
5. Cela fonctionne même pour les applications tierces 3e (qui prennent en charge `NSUserActivity`), de sorte que l’utilisateur peut basculer vers une application de partage de gestion et les adresses du restaurant sont automatiquement suggéré en tant que destination il également.
6. Il fournit également le contexte à Siri, afin que l’utilisateur peut appeler Siri au sein de l’application de restaurant et demandez à *« Get directions... »* et Siri fournissent des instructions au restaurant l’affichage de l’utilisateur.

Toutes les fonctionnalités ci-dessus a une chose en commun, tous les indiquent où la suggestion initialement provient. Dans le cas de l’exemple ci-dessus, il est l’application de révision du restaurant fictive.

iOS 10 a été améliorée pour activer cette fonctionnalité pour une application via plusieurs petites modifications et ajouts aux infrastructures existantes :

- `NSUserActivity` a des champs supplémentaires pour la capture d’informations sur l’emplacement qui sont affichées à l’intérieur de l’application.
- Plusieurs ajouts ont été apportées à MapKit et CoreSpotlight pour capturer l’emplacement.
- Fonctionnalités prenant en charge d’emplacement a été ajoutée à Siri, cartes, claviers, travaux multitâches et autres applications au sein du système.

Pour implémenter des suggestions d’emplacement, démarrer avec le même code d’activité présenté ci-dessus :

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

Si l’application est à l’aide de MapKit, il est aussi simple que l’ajout de la carte active `MKMapItem` à l’activité :

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Si l’application n’est pas à l’aide de MapKit, elle peut adopter la recherche de l’application et spécifier les nouveaux attributs pour l’emplacement suivants :

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

Examinons le code ci-dessus en détail. Tout d’abord, le nom de l’emplacement est requis dans chaque instance de :

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Ensuite, la description de texte en fonction de l’emplacement est nécessaire pour les instances en fonction de texte (par exemple, le clavier QuickType) :

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

La latitude et la longitude sont facultatifs, mais assurez-vous que l’utilisateur est redirigé vers l’emplacement exact de que l’application souhaitant de les envoyer, il doit être inclus :

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

En définissant les numéros de téléphone, l’application peut accéder à Siri afin de l’utilisateur peut appeler Siri à partir de l’application en disant quelque chose comme, *« Appeler cet emplacement »*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Enfin, l’application peut indiquer si l’instance est appropriée pour la navigation et les appels téléphoniques :

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Mise en œuvre des Interactions du Contact

Nouveau dans iOS 10, les applications de communication sont profondément intégrées à l’application Contacts à partir de la carte de visite. Pour les applications qui ont implémenté les Interactions du Contact, l’utilisateur peut ajouter des informations de l’application donnée à des personnes spécifiques dans leurs contacts. Et si, par exemple, ils atteint le bouton d’action rapide en haut d’une carte pour envoyer un message, l’application attachée est répertoriée comme une option pour envoyer le message à partir de.

Si une application de partie 3 rd est sélectionnée, il peut mémorisé et présenté en tant que la méthode par défaut pour la prochaine fois que l’utilisateur souhaite les contacter la personne donnée du message.

Interactions de contact sont implémentées dans l’application à l’aide de `NSUserActivity` et la nouvelle infrastructure intentions introduite dans iOS 10. Pour plus d’informations sur l’utilisation des modes, consultez notre [présentation des Concepts de SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) et [SiriKit d’implémentation](~/ios/platform/sirikit/implementing-sirikit.md) guides.

#### <a name="donating-interactions"></a>Donnez des Interactions

Découvrez comment l’application peut faire un don interactions :

[![](proactive-suggestions-images/activity04.png "Vue d’ensemble des Interactions de don")](proactive-suggestions-images/activity04.png#lightbox)

L’application crée un `INInteraction` objet qui contient un **intention** (`INIntent`), **Participants** et **métadonnées**. Le **intention** représente une action de l’utilisateur comme un appel vidéo ou envoyer un message texte. Le **Participants** incluent les destinataires de la communication. Le **métadonnées** définit par exemple correctement envoyer le message, etc. plus d’informations.

Le développeur jamais directement crée une instance de `INIntent` ou `INIntentResponse`, ils utilisent une des classes enfants spécifiques (basés sur la tâche de la réalisation de l’application pour le compte de l’utilisateur) qui héritent de ces classes parent. Par exemple, `INSendMessageIntent` et `INSendMessageIntentResponse` pour envoyer un message texte. 

Une fois que l’interaction est entièrement remplie, appelez le `DonateInteraction` pour informer le système de l’interaction est disponible à utiliser.

Quand l’utilisateur interagit avec l’application à partir de la carte de visite, l’interaction obtient fournie avec un `NSUserActivity`, qui est ensuite utilisé pour lancer l’application :

[![](proactive-suggestions-images/activity05.png "L’interaction obtient fournie avec un NSUserActivity qui est utilisé pour lancer l’application")](proactive-suggestions-images/activity05.png#lightbox)

Examinons l’exemple suivant d’un mode de Message d’envoi :

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

Examinez ce code en détail, il crée et remplit une instance de `NSUserActivity` (comme indiqué dans le [création d’une activité](#Creating-an-Activity) section ci-dessus). Ensuite, il crée une instance de `INSendMessageIntent` (qui hérite de `INIntent`) et le remplit avec les détails de l’envoi du message :

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

Un gestionnaire d’achèvement est passé dans où l’application peut répondre au don réussi ou échoué.

### <a name="activities-best-practices"></a>Activités de meilleures pratiques

Apple suggère les meilleures pratiques suivantes lorsque vous travaillez avec des activités :

- Utilisez `NeedsSave` pour les mises à jour de la charge utile lazy.
- Veillez à conserver une référence forte à l’activité actuelle.
- Transférer des petites charges utiles qui incluent des informations de base pour restaurer l’état.
- Assurez-vous que les identificateurs de Type d’activité sont unique et descriptif à l’aide de la notation DNS inverse afin de les spécifier. 

## <a name="schemaorg"></a>Schema.org

Comme indiqué ci-dessus, `NSUserActivity` permet le système qui décrit les informations que l’utilisateur travaille actuellement avec sur l’écran. Schema.org ajoute des fonctionnalités similaires à des pages web.

Schema.org peut fournir les mêmes types d’interactions selon l’emplacement sur le site Web. Apple conçu les suggestions d’emplacement nouveau fonctionne aussi bien lorsqu’ils sont affichés dans Safari, comme dans une application native.

Certains arrière-plan Schema.org :

- Il fournit une norme de vocabulaire de balisage ouvrir un site web.
- Il fonctionne en incluant des métadonnées structurées sur les pages web.
- Il existe plus de 500 schémas représentant les différents concepts disponibles.
- En implémentant la sur le site Web, le développeur peut acquérir quelques-uns des avantages de l’utilisation de `NSUserActivity` dans une application native.

Les schémas sont organisées dans une arborescence de la structure, où spécifique types tels que *Restaurant*, hériter des types plus génériques tel que *entreprise Local*. Pour plus d’informations, consultez [Schema.org](http://schema.org).

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

Si l’utilisateur a visité cette page dans Safari et ensuite basculer vers une autre application, les informations d’emplacement à partir de la page seraient être capturées et proposées comme emplacement Suggestion dans d’autres parties du système (comme indiqué dans les activités ci-dessus).

Safari extraira quoi que ce soit sur une page web qui est conforme à une des propriétés de schéma suivantes :

- **PostalAddress**
- **GeoCoordinates**
- Une propriété de téléphone.

Pour plus d’informations, consultez notre [recherche par un balisage Web](~/ios/platform/search/web-markup.md) guide.

## <a name="consuming-location-suggestions"></a>Consommation des Suggestions d’emplacement

La section suivante couvre la consommation de Suggestion d’emplacement issues d’autres parties du système (par exemple, l’application Maps) ou d’autres applications tierces 3e.

Il existe deux méthodes principales pour que l’application peut consommer des Suggestions d’emplacement :

- Via le clavier QuickType.
- Directement par consomme les informations d’emplacement dans les applications de routage.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Suggestions d’emplacement et le clavier QuickType

Si l’application gère les adresses figurant dans les formats de texte en fonction, l’application peut tirer parti des Suggestions d’emplacement via la QuickType UI. iOS 10 développe l’UI QuickType avec les fonctionnalités suivantes :

- L’application peut ajouter des indications sur l’intention de sémantique pour les champs de texte dans l’interface utilisateur.
- L’application peut obtenir des suggestions proactive dans l’application.
- L’application peut bénéficier de la correction automatique améliorée.

La nouvelle `TextContentType` propriété des contrôles de champ de texte dans iOS 10 permet aux développeurs de définir l’intention de la sémantique pour la valeur que l’utilisateur va entrer dans un champ donné. Exemple :

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Serait le système que l’application attend l’utilisateur à entrer une adresse postale complète dans le champ donné. Cela permettra à l’aide du clavier QuickType fournir automatiquement des Suggestions d’emplacement sur le clavier lorsque l’utilisateur entre une valeur dans ce champ.

Voici quelques exemples de types les plus courants disponibles pour les développeurs dans le `UITextContentType` classe statique :

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Applications de routage et les Suggestions d’emplacements

Cette section s’intéresser à consommer des Suggestions d’emplacement directement à partir d’au sein d’une application de routage. Pour l’application de routage ajouter cette fonctionnalité, le développeur reposera existants `MKDirectionsRequest` framework comme suit :

- Pour promouvoir l’application dans les travaux multitâches.
- Pour inscrire l’application comme une application de routage.
- Pour gérer le lancement de l’application avec un MapKit `MKDirectionsRequest` objet.
- Afin de permettre iOS pour apprendre à proposer de l’application à l’utilisateur à des moments appropriés, selon l’intérêt des utilisateurs.

Lorsque l’application est lancée avec un MapKit `MKDirectionsRequest` de l’objet, il doit automatiquement démarrer en donnant les instructions de l’utilisateur à l’emplacement demandé, ou présenter une interface utilisateur qui facilite la tâche à commencer à obtenir des instructions pour l’utilisateur. Exemple :


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

Si elle est, elle crée un `MKDirectionsRequest` à partir de l’URL :

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

## <a name="media-app-suggestions"></a>Suggestions d’application de support

Si l’application gère n’importe quel type de média, comme une application podcast ou un contenu multimédia en continu audio ou vidéo, avec iOS 10, il peut tirer parti des Suggestions de supports dans le système.

Pour les applications qui gèrent des médias, iOS prend en charge les comportements suivants :

- iOS promeut les applications dont l’utilisateur est susceptible d’utiliser en fonction de leur comportement précédent.
- Des suggestions relatives à l’application seront affiche dans la galerie et la vue aujourd'hui.
- Si l’application répond à un des déclencheurs suivants, il peut être élevé pour une suggestion d’écran de verrouillage :
    - Après avoir connecté des écouteurs ou un appareil Bluetooth établit une connexion.
    - Après l’obtention d’une voiture.
    - Après avoir arrivant à votre domicile ou travail. 

En incluant un simple appel d’API dans iOS 10, le développeur peut créer une expérience d’écran verrou plus conviviale pour les utilisateurs de l’application de support. À l’aide de la `MPPlayableContentManager` classe pour gérer la lecture du média, les contrôles de média complet (telles que celles présentées par l’application de la musique) s’affiche sur l’écran de verrouillage pour l’application.


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

Cet article a couvert les Suggestions Proactive et a montré comment le développeur peut les utiliser pour le trafic de lecteur vers l’application Xamarin.iOS. Il couvert l’étape pour implémenter des Suggestions Proactive et affiche les instructions d’utilisation.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Guide de programmation SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
