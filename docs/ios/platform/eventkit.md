---
title: EventKit dans Xamarin.iOS
description: Ce document décrit EventKit et comment l’utiliser dans Xamarin.iOS. Il aborde des calendriers, des événements de calendrier et des rappels, examine classes couramment utilisées lors de la programmation avec EventKit et bien plus encore.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: ea03c8b382e2de29bd20ab1d696d7abb7733e182
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61369256"
---
# <a name="eventkit-in-xamarinios"></a>EventKit dans Xamarin.iOS

iOS a intégré, deux applications liés au calendrier : l’Application de calendrier et l’Application des rappels. Il est assez simple de comprendre comment l’Application calendrier gère les données de calendrier, mais l’Application des rappels est moins évidente. Des rappels peuvent avoir réellement des dates qui s’y rapportent en termes de lorsqu’ils sont en raison, quand elles sont traitées, etc. Par conséquent, iOS stocke toutes les données de calendrier, qu’il s’agisse des événements de calendrier ou des rappels, dans un seul emplacement, appelé le *base de données de calendrier*.

Le framework EventKit fournit un moyen d’accéder à la *calendriers*, *événements de calendrier*, et *des rappels* données qui stocke de base de données de calendrier. Accès aux calendriers et calendrier événements est disponible depuis iOS 4, mais l’accès à des rappels est une nouveauté dans iOS 6.

Dans ce guide, nous allons couvrir :

-   **Notions de base EventKit** – il introduira les éléments fondamentaux de EventKit via les classes principales et permet de comprendre leur utilisation. Cette section se doivent de lire avant d’entreprendre la partie suivante du document. 
-   **Tâches courantes** – la section Tâches courantes est destinée à être une référence rapide sur la façon d’effectuer des opérations courantes telles que ; de l’énumération des calendriers, de création, de l’enregistrement et de récupération de calendrier des événements et rappels, mais aussi à l’aide de contrôleurs intégrés pour Création et modification des événements de calendrier. Cette section n’a pas besoin être lus avant vers l’arrière, comme il est destiné à être une référence pour effectuer une tâche particulière. 


Toutes les tâches dans ce guide sont disponibles dans l’exemple d’application compagnon :

 [![](eventkit-images/01.png "Les écrans d’application Compagnon exemple")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Configuration requise

EventKit a été introduit dans iOS 4.0, mais l’accès aux données de rappels a été introduit dans iOS 6.0. Par conséquent, pour faire du développement de EventKit général, vous devez cibler au moins les versions 4.0 et 6.0 pour les rappels.

En outre, l’application des rappels n’est pas disponible sur le simulateur, ce qui signifie que des rappels données également ne seront plus disponibles, sauf si vous les ajoutez tout d’abord. En outre, les demandes d’accès sont affichés uniquement à l’utilisateur sur le périphérique réel. Par conséquent, EventKit développement est mieux testé sur l’appareil.

## <a name="event-kit-basics"></a>Principes de base événement Kit

Lorsque vous travaillez avec EventKit, il est important d’avoir saisi les classes courantes et leur utilisation. Toutes ces classes figurent dans le `EventKit` et `EventKitUI` (pour le `EKEventEditController`).

### <a name="eventstore"></a>EventStore

Le *EventStore* classe est la classe la plus importante dans EventKit, car il est obligatoire pour effectuer des opérations dans EventKit. Il peut être considéré comme le stockage persistant ou le moteur de base de données, pour toutes les données EventKit. À partir de `EventStore` vous avez accès aux calendriers et événements de calendrier dans l’Application de calendrier, ainsi que des rappels dans l’Application des rappels.

Étant donné que `EventStore` est comme un moteur de base de données, il doit être à long terme, ce qui signifie qu’il doit être créé et détruit aussi peu que possible pendant la durée de vie d’une instance de l’application. En fait, il est recommandé lorsque vous créez une instance d’un `EventStore` dans une application, vous conservez cette référence autour pendant toute la durée de vie de l’application, sauf si vous ne savez vous n’en avez besoin à nouveau. en outre, tous les appels doivent accéder à un seul `EventStore` instance. Pour cette raison, le modèle Singleton est recommandé pour conserver une seule instance disponible.

#### <a name="creating-an-event-store"></a>Création d’un Store de l’événement

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

Le code ci-dessus utilise le modèle Singleton pour instancier une instance de la `EventStore` lorsque l’application chargée. Le `EventStore` est ensuite accessible dans le monde entier à partir de l’application comme suit :

```csharp
App.Current.EventStore;
```

Notez que tous les exemples d’ici utilisent ce modèle, afin qu’elles référencent le `EventStore` via `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Demander l’accès au calendrier et les données de rappel

Avant d’être autorisé à accéder aux données via le EventStore, une application doit tout d’abord demander l’accès à des données de rappels, en fonction de celles que vous devez ou données d’événements de calendrier. Pour faciliter cette tâche, le `EventStore` expose une méthode appelée `RequestAccess` qui, lorsqu’elle est appelée, affiche un affichage des alertes à l’utilisateur lui indiquant que l’application demande l’accès pour les données de calendrier, ou des données de rappel, selon les `EKEntityType`lui est passé. Car il déclenche un affichage des alertes, l’appel est asynchrone et appelle un gestionnaire d’achèvement passé comme un `NSAction` (ou Lambda) lui qui reçoit deux paramètres ; un booléen ou non l’accès a été accordé et un `NSError`, qui, si non-null sera contient les informations d’erreur dans la demande. Par exemple, ce qui suit codé de le demande accès aux données d’événement de calendrier et afficher une alerte afficher si la demande n’a pas été accordée.

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

Une fois que la demande a été accordée, elle sera mémorisé tant que l’application est installée sur l’appareil et ne s’affiche une alerte à l’utilisateur.
Toutefois, l’accès est donné uniquement le type de ressource, les événements de calendrier ou rappels accordés. Si une application a besoin d’accéder aux deux, il doit demander à la fois.

Étant donné que l’autorisation est mémorisée, il est relativement bon marché effectuer la demande à chaque fois, il est judicieux de toujours demander l’accès avant d’effectuer une opération.

En outre, étant donné que le Gestionnaire d’achèvement est appelé sur un thread distinct (non-IU), les mises à jour à l’interface utilisateur dans le Gestionnaire d’achèvement doivent être appelées par le biais de `InvokeOnMainThread`, sinon une exception est levée, et si ne pas interceptée, l’application se bloque.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` est une énumération qui décrit le type de `EventKit` élément ou données. Elle comporte deux valeurs : `Event` et un rappel. Il est utilisé dans un nombre de méthodes, y compris `EventStore.RequestAccess` pour indiquer à `EventKit` quel type de données pour accéder à ou à récupérer.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* représente un calendrier, qui contient un groupe d’événements de calendrier. Calendriers peuvent être stockées dans un grand nombre de différents emplacements, tels que localement, dans *iCloud*, dans un 3e partie emplacement du fournisseur comme un *Exchange Server* ou *Google*, etc. Nombre de fois où `EKCalendar` est utilisée pour indiquer `EventKit` où rechercher des événements ou des informations pour les enregistrer.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* se trouve dans le `EventKitUI` espace de noms et est un contrôleur intégré qui peut être utilisé pour modifier ou créer des événements de calendrier. Comme beaucoup intégrée dans les contrôleurs d’appareil photo, `EKEventEditController` fait l’essentiel pour vous dans l’affichage de l’interface utilisateur et de gestion de l’enregistrement.

### <a name="ekevent"></a>EKEvent

 *EKEvent* représente un événement de calendrier. Les deux `EKEvent` et `EKReminder` héritent `EKCalendarItem` et ont des champs tels que `Title`, `Notes`, et ainsi de suite.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* représente un élément de rappel.

### <a name="ekspan"></a>EKSpan

*EKSpan* est une énumération qui décrit l’étendue des événements lors de la modification des événements qui peuvent se répéter et a deux valeurs : *ThisEvent* et *FutureEvents*. `ThisEvent` signifie que toutes les modifications ne se produira à l’événement particulier de la série qui est référencée, tandis que `FutureEvents` affectera cet événement et tous les périodicités suivantes.

## <a name="tasks"></a>Tâches

Simplicité d’utilisation, l’utilisation de EventKit a été divisée en tâches courantes, décrits dans les sections suivantes.

### <a name="enumerate-calendars"></a>Énumérer des calendriers

Pour énumérer les calendriers de l’utilisateur a configuré sur l’appareil, appelez `GetCalendars` sur la `EventStore` et passez le type de calendriers (rappels ou événements) que vous souhaitez recevoir :

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Ajouter ou modifier un événement à l’aide du contrôleur intégré

Le *EKEventEditViewController* effectue beaucoup le gros du travail pour vous si vous souhaitez créer ou modifier un événement avec la même interface utilisateur qui est présentée à l’utilisateur lors de l’utilisation de l’Application calendrier :

 [![](eventkit-images/02.png "L’interface utilisateur qui est présentée à l’utilisateur lors de l’utilisation de l’Application calendrier")](eventkit-images/02.png#lightbox)

Pour l’utiliser, vous souhaitez déclarer en tant que variable de niveau classe, afin que vous ne vous retrouverez garbage collector si elle est déclarée au sein d’une méthode :

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Ensuite, pour le lancer : instancier, attribuez-lui une référence à la `EventStore`, associer un *EKEventEditViewDelegate* des délégations vers celui-ci et afficher à l’aide `PresentViewController`:

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

Pour utiliser un événement existant, consultez le *extraire un événement par ID* section par la suite.

Le délégué doit remplacer le `Completed` (méthode), qui est appelée par le contrôleur lorsque l’utilisateur a terminé avec la boîte de dialogue :

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

Si vous le souhaitez, dans le délégué, vous pouvez vérifier le *Action* dans le `Completed` méthode pour modifier l’événement, enregistrez de nouveau, ou effectuer d’autres opérations, s’il est annulé, etc. :

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

Pour créer un événement dans le code, utilisez le *FromStore* méthode de fabrique sur la `EKEvent` classe et définir des données dessus :

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

Vous devez définir le calendrier que vous souhaitez que l’événement enregistré dans, mais si vous n’avez aucune préférence, vous pouvez utiliser la valeur par défaut :

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Pour enregistrer l’événement, appelez le *SaveEvent* méthode sur le `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Une fois qu’elle est enregistrée, le *EventIdentifier* propriété sera actualisée avec un identificateur unique qui peut être utilisé ultérieurement pour récupérer l’événement :

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` est qu'une chaîne au format GUID.

### <a name="create-a-reminder-programmatically"></a>Créer un rappel par programmation

Création d’un rappel dans le code est semblable à celle de la création d’un événement de calendrier :

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

### <a name="retrieving-an-event-by-id"></a>Récupération d’un événement par ID

Pour récupérer un événement qu’il est ID, utilisez le *EventFromIdentifier* méthode sur le `EventStore` et passez-lui le `EventIdentifier` qui a été extraite de l’événement :

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Pour les événements, il est sont deux autres propriétés de l’identificateur, mais `EventIdentifier` est le seul qui fonctionne pour cela.

### <a name="retrieving-a-reminder-by-id"></a>Récupération d’un rappel par ID

Pour récupérer un rappel, utilisez le *GetCalendarItem* méthode sur le `EventStore` et passez-lui le *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Étant donné que `GetCalendarItem` retourne un `EKCalendarItem`, elle doit être convertie en `EKReminder` si vous avez besoin d’accéder aux données de rappel ou utiliser l’instance en tant qu’un `EKReminder` ultérieurement.

N’utilisez pas `GetCalendarItem` pour les événements de calendrier, au moment de l’écriture, il ne fonctionne pas.

### <a name="deleting-an-event"></a>Suppression d’un événement

Pour supprimer un événement de calendrier, appelez *RemoveEvent* sur votre `EventStore` et passer une référence à l’événement et le texte approprié `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Notez, toutefois, après la suppression d’un événement, la référence d’événement sera `null`.

### <a name="deleting-a-reminder"></a>Supprimer un rappel

Pour supprimer un rappel, appelez *RemoveReminder* sur la `EventStore` et passer une référence à la relance :

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Notez que dans le code ci-dessus est un cast en `EKReminder`, car `GetCalendarItem` a été utilisé pour la récupérer

### <a name="searching-for-events"></a>Recherche d’événements

Pour rechercher des événements de calendrier, vous devez créer un *NSPredicate* objet le *PredicateForEvents* méthode sur le `EventStore`. Un `NSPredicate` est une requête d’objet de données que l’option iOS utilise pour rechercher les correspondances :

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

Notez que les requêtes sont synchrones (blocage) et peuvent prendre beaucoup de temps, en fonction de la requête, vous pouvez donc faire tourner un nouveau thread ou d’une tâche pour ce faire.

### <a name="searching-for-reminders"></a>Recherchez des rappels

Recherchez des rappels est similaire aux événements ; elle nécessite un prédicat, mais l’appel est déjà asynchrone, afin de vous soucier n’a pas besoin de bloquer le thread :

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

Ce document a fourni une vue d’ensemble d’à la fois les éléments importants du framework EventKit et un nombre de tâches les plus courantes. Toutefois, le framework EventKit est très volumineux et plus puissant et inclut des fonctionnalités qui n’ont pas été introduites comme ici, : mises à jour, alarmes, configuration de périodicité sur les événements, l’inscription et l’écoute des modifications sur la base de données de calendrier, de configuration du lot définition de clôtures virtuelles et bien plus encore.  Pour plus d’informations, consultez d’Apple [calendrier et Guide de programmation des rappels](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Liens associés

- [Calendriers (exemple)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Introduction à iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Présentation des calendriers et des rappels](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
