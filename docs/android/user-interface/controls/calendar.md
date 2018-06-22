---
title: Calendrier
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d48af16f50c5a4482342d323b5c73b9c237cc83a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767974"
---
# <a name="calendar"></a>Calendrier


## <a name="calendar-api"></a>API de calendrier

Un nouvel ensemble de calendrier API introduites dans Android 4 prend en charge les applications qui sont conçues pour lire ou écrire des données sur le fournisseur de calendrier. Ces API prennent en charge un ensemble d’options d’interaction avec les données de calendrier, y compris la possibilité de lire et écrire des événements, les participants et les rappels. En utilisant le fournisseur de calendrier dans votre application, les données que vous ajoutez via l’API seront affiche dans l’application Calendrier intégré qui est fourni avec Android 4.


## <a name="adding-permissions"></a>Ajout d’autorisations

Lorsque vous travaillez avec la nouvelle API de calendrier dans votre application, la première chose à faire est ajouter les autorisations appropriées pour le manifeste Android. Les autorisations que vous devez ajouter sont `android.permisson.READ_CALENDAR` et `android.permission.WRITE_CALENDAR`, en fonction de la lecture et/ou l’écriture des données de calendrier.


## <a name="using-the-calendar-contract"></a>L’utilisation du contrat de calendrier

Une fois que vous définissez les autorisations, vous pouvez interagir avec les données de calendrier à l’aide de la `CalendarContract` classe. Cette classe fournit un modèle de données qui permet aux applications lorsqu’ils interagissent avec le fournisseur de calendrier. Le `CalendarContract` permet aux applications de résoudre l’URI pour les entités de calendrier, telles que les calendriers et événements. Il fournit également un moyen d’interagir avec les différents champs dans chaque entité, telles que nom et ID, ou les début d’un événement et date de fin d’un calendrier.

Examinons un exemple qui utilise l’API de calendrier. Dans cet exemple, nous allons examiner comment énumérer les calendriers et leurs événements, ainsi que l’ajout d’un nouvel événement à un calendrier.


## <a name="listing-calendars"></a>Liste des calendriers

Tout d’abord, nous allons examiner comment énumérer les calendriers qui ont été inscrits dans l’application de calendrier. Pour ce faire, nous pouvons instancier un `CursorLoader`. Introduit dans 3.0 Android (API 11), `CursorLoader` constitue le meilleur moyen de consommer un `ContentProvider`. Au minimum, nous devez spécifier l’Uri de contenu pour les calendriers et les colonnes que vous souhaitez retourner ; Cette spécification de colonne est appelée un _projection_.

Appel de la `CursorLoader.LoadInBackground` méthode nous permet d’interroger un fournisseur de contenu pour les données, tel que le fournisseur de calendrier.
`LoadInBackground` effectue l’opération de chargement réelle et retourne un `Cursor` avec les résultats de la requête.

Le `CalendarContract` nous permet de spécifier à la fois le contenu `Uri` et la projection. Pour obtenir le contenu `Uri` pour interroger les calendriers, nous pouvons simplement utiliser le `CalendarContract.Calendars.ContentUri` propriété comme suit :

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

À l’aide de la `CalendarContract` pour spécifier le calendrier des colonnes est tout aussi simple. Il suffit d’ajouter les champs dans la `CalendarContract.Calendars.InterfaceConsts` classe dans un tableau. Par exemple, le code suivant inclut les ID du calendrier, le nom complet et nom du compte :

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

Le `Id` est important d’inclure si vous utilisez un `SimpleCursorAdapter` pour lier les données à l’interface utilisateur, comme nous le verrons dans quelques instants. Avec l’Uri contenu et de projection en place, nous instancier le `CursorLoader` et appelez le `CursorLoader.LoadInBackground` méthode pour retourner un curseur avec les données de calendrier, comme indiqué ci-dessous :

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

L’interface utilisateur pour cet exemple contient un `ListView`, avec chaque élément dans la liste qui représente un calendrier simple. Le code XML suivant montre le balisage qui inclut la `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

En outre, nous devons spécifier l’interface utilisateur pour chaque élément de liste, qui sont placées dans un fichier XML comme suit :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

À ce stade, il est simplement normal code Android pour lier les données à partir du curseur à l’interface utilisateur. Nous allons utiliser un `SimpleCursorAdapter` comme suit :

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

Dans le code ci-dessus, l’adaptateur prend les colonnes spécifiées dans le `sourceColumns` de tableau et les écrit dans les éléments d’interface utilisateur dans le `targetResources` tableau pour chaque entrée de calendrier dans le curseur. L’activité utilisée ici est une sous-classe de `ListActivity`; il inclut le `ListAdapter` propriété à laquelle nous avons défini l’adaptateur.

Voici une capture d’écran montrant le résultat final, avec les informations de calendrier affichée dans le `ListView`:

[![CalendarDemo en cours d’exécution dans l’émulateur, deux entrées de calendrier](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>Liste les événements de calendrier

Suivant Examinons comment énumérer les événements d’un calendrier donné.
S’appuyant sur l’exemple ci-dessus, nous allons présenter une liste d’événements lorsque l’utilisateur sélectionne un des calendriers. Par conséquent, nous devez gérer la sélection d’éléments dans le code précédent :

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

Dans ce code, nous créons une intention d’ouvrir une activité de type `EventListActivity`, en passant l’intention ID du calendrier. Nous devons l’ID de connaître le calendrier pour rechercher des événements. Dans le `EventListActivity`de `OnCreate` (méthode), nous pouvons récupérer l’ID de la `Intent` comme indiqué ci-dessous :

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Maintenant nous allons les événements de requête pour ce calendrier ID. Le processus pour rechercher des événements est similaire à la façon dont nous avons interrogés pour obtenir la liste des calendriers précédemment, cette fois nous allons utiliser la `CalendarContract.Events` classe. Le code suivant crée une requête pour récupérer les événements :

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

Dans ce code, nous avons tout d’abord obtenir le contenu `Uri` pour les événements de la `CalendarContract.Events.ContentUri` propriété. Nous spécifions les colonnes d’événements que nous voulons récupérer dans le tableau eventsProjection.
Enfin, nous instancier un `CursorLoader` avec ces informations et l’appel du chargeur de `LoadInBackground` méthode pour retourner un `Cursor` avec les données d’événement.

Pour afficher les données d’événement dans l’interface utilisateur, nous pouvons utiliser le balisage et le code comme nous l’avons fait avant d’afficher la liste des calendriers. Là encore, nous utilisons `SimpleCursorAdapter` pour lier les données à un `ListView` comme indiqué dans le code suivant :

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

La principale différence entre ce code et le code que nous avons utilisé précédemment pour afficher la liste de calendrier est l’utilisation d’un `ViewBinder`, qui est défini sur la ligne :

```csharp
adapter.ViewBinder = new ViewBinder ();
```

La `ViewBinder` classe permet de contrôler davantage la façon dont nous lier des valeurs à des vues. Dans ce cas, nous utiliser pour convertir l’heure de début d’événement à partir de millisecondes à une chaîne de date, comme indiqué dans l’implémentation suivante :

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Cela affiche une liste des événements comme indiqué ci-dessous :

[![Capture d’écran de l’exemple d’application affichant les trois événements de calendrier](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>Ajout d’un événement de calendrier

Nous avons vu comment lire les données de calendrier. Maintenant nous allons voir comment ajouter un événement à un calendrier. Pour ce faire, veillez à inclure le `android.permission.WRITE_CALENDAR` autorisation nous l’avons mentionné précédemment. Pour ajouter un événement à un calendrier, nous allons :

1.  Créer un `ContentValues` instance.
1.  Utiliser des clés à partir de la `CalendarContract.Events.InterfaceConsts` classe pour remplir la `ContentValues` instance.
1.  Définir les fuseaux horaires pour le début de l’événement et l’heure de fin.
1.  Utilisez un `ContentResolver` pour insérer les données d’événement dans le calendrier.


Le code ci-dessous illustre ces étapes :

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Notez que si nous ne définissez pas le fuseau horaire, une exception de type `Java.Lang.IllegalArgumentException` sera levée. Étant donné que les valeurs de durée d’événement doivent être exprimées en millisecondes depuis l’époque, nous créons un `GetDateTimeMS` (méthode) (dans `EventListActivity`) pour convertir des spécifications de notre date dans un format de milliseconde :

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Si nous ajouter un bouton à la liste des événements UI et exécutez le code ci-dessus dans du bouton Gestionnaire d’événements click, l’événement est ajouté au calendrier et mis à jour dans la liste, comme indiqué ci-dessous :

[![Capture d’écran de l’exemple d’application avec le suivi d’un bouton de l’exemple d’événement ajouter des événements de calendrier](calendar-images/13.png)](calendar-images/13.png#lightbox)

Si nous ouvrir l’application Calendrier, nous allons voir que l’événement est écrit il ainsi :

[![Capture d’écran de l’application Calendrier affichant l’événement de calendrier sélectionné](calendar-images/14.png)](calendar-images/14.png#lightbox)

Comme vous pouvez le voir, Android autorise l’accès puissant et facile récupérer et conserver les données de calendrier, permettant aux applications d’intégrer les fonctionnalités de calendrier.


## <a name="related-links"></a>Liens associés

- [Démonstration de calendrier (exemple)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
