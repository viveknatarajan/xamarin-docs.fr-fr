---
title: EventKit
description: "Ce guide fournit une vue d’ensemble sur la façon d’accéder et d’utiliser des calendriers, CalendarEvents et des rappels les données stockées dans la base de données de calendrier, tel qu’exposé via la EventKit. Elle couvre les principales classes et leur rôle dans la programmation de EventKit, ainsi que d’un nombre de tâches courantes associées à l’infrastructure EventKit."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a08bc67a9af653a9a646ad62071df0400ce58c12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="eventkit"></a>EventKit

_Ce guide fournit une vue d’ensemble sur la façon d’accéder et d’utiliser des calendriers, CalendarEvents et des rappels les données stockées dans la base de données de calendrier, tel qu’exposé via la EventKit. Elle couvre les principales classes et leur rôle dans la programmation de EventKit, ainsi que d’un nombre de tâches courantes associées à l’infrastructure EventKit._

iOS a deux applications dépendant de calendrier intégrées : l’Application de calendrier et l’Application des rappels. Il est assez simple à comprendre comment l’Application calendrier gère les données de calendrier, mais l’Application des rappels est moins évidente. Rappels peuvent avoir réellement les dates en termes d’associés lorsqu’ils sont à l’échéance, quand ils sont terminées, etc. Par conséquent, iOS stocke toutes les données de calendrier, qu’il s’agisse d’événements de calendrier ou de rappels, dans un seul emplacement, appelée la *base de données de calendrier*.

Le framework EventKit fournit un moyen d’accéder à la *calendriers*, *calendrier*, et *rappels* calendrier de base de données stocke les données. Accès aux calendriers et calendrier événements est disponible depuis iOS 4, mais les accès aux rappels est une nouveauté dans iOS 6.

Dans ce guide, nous allons couvrir :

-   **Notions de base EventKit** – cela oblige les éléments fondamentaux de EventKit via les classes principales et fournit une description de leur utilisation. Cette section est obligatoire lors de la lecture avant d’aborder la partie suivante du document. 
-   **Tâches courantes** – la section des tâches courantes est destinée à être un aide-mémoire sur la façon d’effectuer les opérations courantes telles que ; le calendrier de l’énumération des calendriers, la création, l’enregistrement et la récupération des événements et rappels, mais aussi à l’aide de contrôleurs intégrés pour Création et modification des événements de calendrier. Cette section n’a pas besoin être lus avant vers l’arrière, car il est destiné à être une référence pour effectuer des tâches particulières. 


Toutes les tâches dans ce guide sont disponibles dans l’exemple d’application compagnon :

 [![](eventkit-images/01.png "Les écrans d’application Compagnon exemple")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Configuration requise

EventKit a été introduit dans iOS 4.0, mais l’accès aux données de rappels a été introduit dans iOS 6.0. Par conséquent, pour faire un développement EventKit général, vous devez cibler au moins les versions 4.0 et 6.0 pour les rappels.

En outre, l’application des rappels n’est pas disponible sur le simulateur, ce qui signifie que les données des rappels également sera pas disponibles, sauf si vous les ajoutez tout d’abord. En outre, les demandes d’accès sont uniquement affichés à l’utilisateur sur le périphérique réel. Par conséquent, EventKit développement est mieux testé sur l’appareil.

## <a name="event-kit-basics"></a>Événement Kit notions de base

Lorsque vous utilisez EventKit, il est important d’avoir compris les classes courantes et leur utilisation saisi. Toutes ces classes se trouvent dans le `EventKit` et `EventKitUI` (pour le `EKEventEditController`).

### <a name="eventstore"></a>EventStore

Le *EventStore* classe est la classe la plus importante dans EventKit car il a requis pour effectuer des opérations dans EventKit. Il peut être considéré comme le stockage persistant, ou moteur de base de données, pour toutes les données EventKit. À partir de `EventStore` vous avez accès à la fois les calendriers et événements du calendrier dans l’Application de calendrier, ainsi que les rappels dans l’Application des rappels.

Étant donné que `EventStore` est comme un moteur de base de données, il convient de longue durée, ce qui signifie qu’il doit être créé et détruit aussi peu que possible au cours de la durée de vie d’une instance de l’application. En fait, il est recommandé lorsque vous créez une instance d’un `EventStore` dans une application, vous conservez cette référence pour toute la durée de vie de l’application, sauf si vous êtes certain que vous n’en avez besoin à nouveau. en outre, tous les appels doivent accéder à un seul `EventStore` instance. Pour cette raison, le modèle de Singleton est recommandé de conserver une seule instance disponible.

#### <a name="creating-an-event-store"></a>Création d’un récepteur d’événements

Le code suivant illustre un moyen efficace de créer une instance unique de la `EventStore` classe et le rendre disponible statiquement à partir d’une application :

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

Le code ci-dessus utilise le modèle de Singleton pour instancier une instance de la `EventStore` lorsque l’application charge. Le `EventStore` est ensuite accessible globalement à partir de l’application comme suit :

```csharp
App.Current.EventStore;
```

Notez que tous les exemples d’ici utilisant ce modèle, afin qu’ils référencent le `EventStore` via `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Demander l’accès au calendrier et les données de rappel

Avant de pouvoir accéder aux données via le EventStore, une application doit d’abord demander accès aux données des événements de calendrier ou des données de rappels, en fonction de celui que vous avez besoin. Pour faciliter cette tâche, le `EventStore` expose une méthode appelée `RequestAccess` qui, lorsqu’elle est appelée, affiche un affichage des alertes à l’utilisateur indiquant que l’application demande l’accès pour les données de calendrier, ou des données de rappel, selon les `EKEntityType`lui est transmise. Car elle déclenche un affichage des alertes, l’appel est asynchrone et appelle un gestionnaire d’achèvement passé en tant qu’un `NSAction` (ou Lambda) lui qui reçoit deux paramètres ; une valeur booléenne de l’accès accordé ou non et un `NSError`, qui, si non-null sera contient les informations d’erreur dans la demande. Par exemple, suit codé demande accès aux données d’événement de calendrier et afficher permet d’afficher une alerte si la demande n’a pas été accordée.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

Lorsque la demande a été accordée, il est mémorisée tant que l’application est installée sur l’appareil et ne s’affiche une alerte à l’utilisateur.
Toutefois, l’accès est donné uniquement le type de ressource, les événements de calendrier ou des rappels accordés. Si une application a besoin d’accéder à la fois, elle doit demander l’autorisation à la fois.

Étant donné que l’autorisation est mémorisée, il est relativement peu coûteux effectuer la demande à chaque fois, il est conseillé de toujours demander l’accès avant d’effectuer une opération.

En outre, étant donné que le Gestionnaire d’achèvement est appelé sur un thread séparé (sans interface utilisateur), les mises à jour l’interface utilisateur dans le Gestionnaire d’achèvement doivent être appelés `InvokeOnMainThread`, sinon une exception est levée et si ne pas interceptée, l’application se bloque.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` est une énumération qui décrit le type de `EventKit` élément ou données. Elle comporte deux valeurs : `Event` et de rappel. Il est utilisé dans un nombre de méthodes, y compris `EventStore.RequestAccess` pour indiquer à `EventKit` le type de données à obtenir l’accès à ou à récupérer.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* représente un calendrier, qui contient un groupe d’événements du calendrier. Calendriers peuvent être stockées dans un grand nombre d’emplacements différents, tels que localement, dans *iCloud*, dans un 3e partie emplacement du fournisseur comme un *Exchange Server* ou *Google*, etc. Nombre de fois où `EKCalendar` sert à indiquer `EventKit` où rechercher des événements, ou les enregistrer.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* se trouvent dans le `EventKitUI` espace de noms et est un contrôleur intégré qui peut être utilisé pour modifier ou créer des événements de calendrier. Très comparable intégrée dans les contrôleurs de l’appareil photo, `EKEventEditController` effectue l’essentiel pour vous dans l’affichage de l’interface utilisateur et de gestion de l’enregistrement.

### <a name="ekevent"></a>EKEvent

 *EKEvent* représente un événement de calendrier. Les deux `EKEvent` et `EKReminder` hériter `EKCalendarItem` et avoir des champs tels que `Title`, `Notes`, et ainsi de suite.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* représente un élément de rappel.

### <a name="ekspan"></a>EKSpan

*EKSpan* est une énumération qui décrit l’étendue des événements lors de la modification des événements qui peuvent se répéter et a deux valeurs : *ThisEvent* et *FutureEvents*. `ThisEvent` signifie que toutes les modifications ne se produira à un événement particulier de la série qui est référencée, alors que `FutureEvents` affectera cet événement et tous les récurrences futures.

## <a name="tasks"></a>Tâches

Pour faciliter l’utilisation, l’utilisation de EventKit a été divisée en tâches courantes, décrits dans les sections suivantes.

### <a name="enumerate-calendars"></a>Énumérer des calendriers

Pour énumérer les calendriers de l’utilisateur a configuré sur l’appareil, appelez `GetCalendars` sur la `EventStore` et passer le type de calendriers (rappels ou événements) que vous souhaitez recevoir :

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Ajouter ou modifier un événement à l’aide du contrôleur intégré

Le *EKEventEditViewController* effectue un grand nombre de tâches pour vous si vous souhaitez créer ou modifier un événement avec la même interface utilisateur qui est présentée à l’utilisateur lors de l’utilisation de l’Application calendrier :

 [![](eventkit-images/02.png "L’interface utilisateur qui est présentée à l’utilisateur lors de l’utilisation de l’Application calendrier")](eventkit-images/02.png#lightbox)

Pour l’utiliser, que vous souhaitez déclarer en tant que variable de niveau classe, afin que vous ne trouverez pas par le garbage collector s’il est déclaré dans une méthode :

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Ensuite, pour le lancer : instancier, attribuez-lui une référence à la `EventStore`, rattachez une *EKEventEditViewDelegate* déléguer à elle, alors l’afficher à l’aide de `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

Si vous le souhaitez, si vous voulez préremplir l’événement, vous pouvez créer un tout nouvel événement (comme indiqué ci-dessous), ou vous pouvez récupérer un événement enregistré :

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Si vous ne souhaitez pas préremplir l’interface utilisateur, veillez à définir la propriété d’événement sur le contrôleur :

```csharp
eventController.Event = newEvent;
```

Pour utiliser un événement existant, consultez le *extraire un événement par ID* section ultérieurement.

Le délégué doit remplacer le `Completed` (méthode), qui est appelé par le contrôleur, quand l’utilisateur a terminé avec la boîte de dialogue :

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

Si vous le souhaitez, dans le délégué, vous pouvez vérifier le *Action* dans les `Completed` d’une méthode pour modifier l’événement, enregistrez de nouveau, ou effectuer d’autres opérations, s’il est annulé, etc. :

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Création d’un événement par programmation

Pour créer un événement dans le code, utilisez la *FromStore* méthode de fabrique sur le `EKEvent` , puis définissez toutes les données sur ce dernier :

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Vous devez définir le calendrier que vous souhaitez que l’événement enregistré dans, mais si vous ne disposez d’aucune préférence, vous pouvez utiliser la valeur par défaut :

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Pour enregistrer l’événement, appelez le *SaveEvent* méthode sur le `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Une fois qu’elle est enregistrée, le *EventIdentifier* propriété sera mise à jour avec un identificateur unique qui peut être utilisé ultérieurement pour extraire l’événement :

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` est qu'une chaîne au format GUID.

### <a name="create-a-reminder-programmatically"></a>Créer un rappel par programmation

Création d’un rappel dans le code est similaire à la création d’un événement de calendrier :

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Pour enregistrer, appelez le *SaveReminder* méthode sur le `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>La récupération d’un événement par ID

Pour extraire un événement qu’elle est ID, utilisez la *EventFromIdentifier* méthode sur le `EventStore` et lui passer le `EventIdentifier` qui a été extraite de l’événement :

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Pour les événements, il est sont deux autres propriétés de l’identificateur, mais `EventIdentifier` est le seul qui fonctionne pour cela.

### <a name="retrieving-a-reminder-by-id"></a>La récupération d’un rappel par ID

Pour récupérer un rappel, utilisez le *GetCalendarItem* méthode sur le `EventStore` et lui passer le *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Étant donné que `GetCalendarItem` retourne un `EKCalendarItem`, il doit être converti en `EKReminder` si vous devez accéder aux données de rappel ou utiliser l’instance comme un `EKReminder` ultérieurement.

N’utilisez pas `GetCalendarItem` pour les événements de calendrier, au moment de la rédaction, il ne fonctionne pas.

### <a name="deleting-an-event"></a>Suppression d’un événement

Pour supprimer un événement de calendrier, appelez *RemoveEvent* sur votre `EventStore` et passer une référence à l’événement et les `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Notez toutefois, après un événement a été supprimé, la référence d’événement sera `null`.

### <a name="deleting-a-reminder"></a>Supprimer un rappel

Pour supprimer un rappel, appelez *RemoveReminder* sur la `EventStore` et passer une référence pour le rappel :

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Notez que dans le code ci-dessus est un cast vers `EKReminder`, car `GetCalendarItem` a été utilisé pour la récupérer

### <a name="searching-for-events"></a>Recherchez des événements

Pour rechercher des événements de calendrier, vous devez créer un *NSPredicate* de l’objet le *PredicateForEvents* méthode sur le `EventStore`. Un `NSPredicate` est une requête qui iOS utilise pour rechercher les correspondances de l’objet de données :

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

Une fois que vous avez créé le `NSPredicate`, utilisez le *EventsMatching* méthode sur le `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Notez que les requêtes sont synchrone (bloquante) et peuvent prendre beaucoup de temps, en fonction de la requête, vous pouvez donc lancer un nouveau thread ou d’une tâche pour ce faire.

### <a name="searching-for-reminders"></a>Recherchez des rappels

Recherche des rappels est semblable aux événements ; Il requiert un prédicat, mais l’appel est déjà asynchrone, afin de vous soucier pas besoin de bloquer le thread :

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Récapitulatif

Ce document a fourni une vue d’ensemble d’à la fois les éléments importants du framework EventKit et un nombre de tâches les plus courantes. Toutefois, le framework EventKit est très volumineux et plus puissant et comprend des fonctionnalités qui n’ont pas été introduites comme ici, : mises à jour, configuration des alertes, la configuration de périodicité sur les événements, l’inscription et l’écoute des modifications sur la base de données de calendrier, du lot paramètre GeoFences et bien plus encore.  Pour plus d’informations, consultez Apple [calendrier et le Guide de programmation des rappels](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Liens associés

- [Calendriers (exemple)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Introduction à iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Introduction aux calendriers et des rappels](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
