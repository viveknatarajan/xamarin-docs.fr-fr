---
title: "Sélecteur d’heure"
description: "Sélection d’une heure en utilisant TimePickerDialog et DialogFragment"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 4b3299ad138b5cd74ce77cac1da49d21a833fe1a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="time-picker"></a>Sélecteur d’heure

Pour permettre à l’utilisateur de sélectionner une heure, vous pouvez utiliser [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Les applications Android utilisent généralement `TimePicker` avec [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) pour la sélection d’une valeur d’heure &ndash; Cela permet de garantir une interface cohérente entre les périphériques et les applications. `TimePicker` permet aux utilisateurs de sélectionner l’heure du jour en mode de 24 heures ou de 12 heures AM/PM.
`TimePickerDialog` est une classe d’assistance qui encapsule le `TimePicker` dans une boîte de dialogue.

[![Capture d’écran de la boîte de dialogue du sélecteur de temps en action](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png)

## <a name="overview"></a>Vue d'ensemble

Affichent les applications Android modernes le `TimePickerDialog` dans un [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Cela permet à une application pour afficher le `TimePicker` comme une boîte de dialogue contextuelle ou l’incorporer dans une activité. En outre, le `DialogFragment` gère le cycle de vie et l’affichage de la boîte de dialogue, ce qui réduit la quantité de code qui doit être implémentée.

Ce guide montre comment utiliser le `TimePickerDialog`, elle est encapsulée dans un `DialogFragment`. L’application d’exemple affiche la `TimePickerDialog` comme une boîte de dialogue modale lorsque l’utilisateur clique sur un bouton sur une activité. Lorsque l’heure est définie par l’utilisateur, la boîte de dialogue se ferme et un gestionnaire de mises à jour une `TextView` sur l’écran de l’activité avec l’heure à laquelle a été sélectionné.

## <a name="requirements"></a>Configuration requise

L’exemple d’application pour ce guide cible Android 4.1 (API de niveau
16) ou version ultérieure, mais peut être utilisé avec Android 3.0 (API niveau 11 ou version ultérieure). Il est possible de prendre en charge les versions antérieures d’Android avec l’ajout de la bibliothèque de prise en charge Android v4 au projet et certaines modifications du code.

## <a name="using-the-timepicker"></a>À l’aide de la TimePicker

Cet exemple étend `DialogFragment`; l’implémentation de la sous-classe de `DialogFragment` (appelé `TimePickerFragment` ci-dessous) héberge et affiche un `TimePickerDialog`. Lorsque l’exemple d’application est lancé, il affiche un **choix temps** bouton ci-dessus un `TextView` qui sera utilisé pour afficher l’heure sélectionnée :

[![Écran de l’application exemple initiale](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png)

Lorsque vous cliquez sur le **choix temps** bouton, le lancement d’application exemple le `TimePickerDialog` comme indiqué dans cette capture d’écran :

[![Capture d’écran de la boîte de dialogue Sélecteur d’heure par défaut affiché par l’application](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png)

Dans le `TimePickerDialog`, en sélectionnant une heure, puis en cliquant sur le **OK** bouton provoque le `TimePickerDialog` pour appeler la méthode [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Cette interface est implémentée par l’hébergement `DialogFragment` (`TimePickerFragment`, comme décrit ci-dessous). En cliquant sur le **Annuler** bouton provoque le fragment et la boîte de dialogue pour être rejetées.

`DialogFragment` Retourne l’heure sélectionnée pour l’hébergement Actvity de trois manières :

1. **Appel d’une méthode ou de la définition d’une propriété** &ndash; l’activité peuvent fournir une propriété ou méthode spécifiquement pour la définition de cette valeur.

2. **Le déclenchement d’un événement** &ndash; le `DialogFragment` peut définir un événement sera déclenché lorsque `OnTimeSet` est appelé.

3. **À l’aide un `Action`**  &ndash; le `DialogFragment` peuvent appeler une `Action<DateTime>` pour afficher l’heure de l’activité. L’activité fournira le `Action<DateTime` lors de l’instanciation du `DialogFragment`. 

Cet exemple utilise la technique de tiers, ce qui nécessite que l’approvisionnement de l’activité une `Action<DateTime>` gestionnaire à la `DialogFragment`.


<a name="start" />

## <a name="start-an-app-project"></a>Démarrer un projet d’application

Démarrer un nouveau projet Android appelé **TimePickerDemo** (si vous n’êtes pas familiarisé avec la création de projets de Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) pour apprendre à créer un nouveau projet).

Modifier **Resources/layout/Main.axml** et remplacez son contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

Il s’agit d’un base [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) avec un [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) qui affiche l’heure et un [bouton](https://developer.xamarin.com/api/type/Android.Widget.Button/) qui ouvre le `TimePickerDialog`. Notez que cette mise en page utilise des chaînes codées en dur et les dimensions pour rendre l’application plus simple et plus facile à comprendre &ndash; une application de production utilise généralement des ressources pour ces valeurs (comme indiqué dans le [DatePicker](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) exemple de code).

Modifier **MainActivity.cs** et remplacez son contenu par le code suivant :

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

Lorsque vous générez et exécutez cet exemple, vous devez voir un écran de démarrage semblable à la capture d’écran suivante :

[![Écran initial de l’application](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png)

En cliquant sur le **choix temps** bouton n’a aucun effet car la `DialogFragment` n’a pas encore été implémentée pour afficher le `TimePicker`.
L’étape suivante consiste à créer cette `DialogFragment`.


<a name="extend_dialogfragment" />

## <a name="extending-dialogfragment"></a>Extension DialogFragment

Pour étendre `DialogFragment` pour une utilisation avec `TimePicker`, il est nécessaire de créer une sous-classe qui est dérivée de `DialogFragment` et implémente `TimePickerDialog.IOnTimeSetListener`. Ajoutez la classe suivante au **MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

Cela `TimePickerFragment` classe est divisé en éléments plus petits et expliquée dans la section suivante.

<a name="details" />

### <a name="dialogfragment-implementation"></a>Implémentation de DialogFragment

`TimePickerFragment` implémente plusieurs méthodes : une méthode de fabrique, une méthode lors de l’instanciation de la boîte de dialogue et le `OnTimeSet` méthode de gestionnaire requis par `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` est une sous-classe de `DialogFragment`. Il implémente également la `TimePickerDialog.IOnTimeSetListener` interface (autrement dit, il fournit requis `OnTimeSet` (méthode)) :

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` est initialisé à des fins de journalisation (*MyTimePickerFragment* peut être modifié sur la chaîne que vous souhaitez utiliser). Le `timeSelectedHandler` Action est initialisée à un délégué vide afin d’éviter les exceptions de référence null :

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   Le `NewInstance` méthode de fabrique est appelée pour instancier un nouvel `TimePickerFragment`. Cette méthode prend un `Action<DateTime>` gestionnaire est appelé lorsque l’utilisateur clique sur le **OK** situé dans le `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Lorsque le fragment doit être affichée, Android appelle la `DialogFragment` méthode [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Cette méthode crée un nouveau `TimePickerDialog` de l’objet et l’initialise avec l’activité, l’objet de rappel (qui est l’instance actuelle de la `TimePickerFragment`) et l’heure actuelle :

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   Lorsque l’utilisateur modifie le paramètre de temps dans le `TimePicker` boîte de dialogue, la `OnTimeSet` méthode est appelée. `OnTimeSet` Crée un `DateTime` à l’aide de la date actuelle et les fusions dans le temps (en heures et minutes) sélectionnées par l’utilisateur de l’objet :

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   Cela `DateTime` objet est passé à la `timeSelectedHandler` qui est enregistré avec le `TimePickerFragment` objet au moment de la création. `OnTimeSet` appelle ce gestionnaire pour mettre à jour d’affichage de l’heure de l’activité et l’heure sélectionnée (ce gestionnaire est implémenté dans la section suivante) :

    ```csharp
    timeSelectedHandler (selectedTime);
    ```


<a name="time_picker_fragment" />

## <a name="displaying-the-timepickerfragment"></a>Afficher le TimePickerFragment

Maintenant que le `DialogFragment` a été implémentée, il est temps d’instancier le `DialogFragment` à l’aide de la `NewInstance` méthode de fabrique et l’afficher en appelant [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

Ajoutez la méthode suivante à `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

Après avoir `TimeSelectOnClick` instancie un `TimePickerFragment`, il crée et passe un délégué d’une méthode anonyme qui met à jour la valeur de temps passé dans Affichage de l’heure de l’activité. Enfin, il lance le `TimePicker` fragment de la boîte de dialogue (via `DialogFragment.Show`) pour afficher les `TimePicker` à l’utilisateur.

À la fin de la `OnCreate` (méthode), ajoutez la ligne suivante pour attacher le Gestionnaire d’événements pour le **choix temps** bouton qui lance la boîte de dialogue :

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Lorsque le **choix temps** bouton est activé, `TimeSelectOnClick` sera appelé pour afficher le `TimePicker` fragment de la boîte de dialogue à l’utilisateur.


<a name="try-it" />

## <a name="try-it"></a>Essayez !

Générez et exécutez l’application. Lorsque vous cliquez sur le **choix temps** bouton, le `TimePickerDialog` s’affiche dans le format d’heure par défaut pour l’activité (dans ce cas, 12 heures AM/PM mode) :

[![Boîte de dialogue s’affiche en mode de AM/PM](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png)
   
Lorsque vous cliquez sur **OK** dans les `TimePicker` boîte de dialogue, le Gestionnaire des mises à jour de l’activité `TextView` avec l’heure sélectionnée, puis se ferme :

[![Heure de A/M s’affichent dans le contrôle TextView d’activité](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png)

Ensuite, ajoutez la ligne suivante de code pour `OnCreateDialog` immédiatement après `is24HourFormat` est déclarée et initialisée :

```csharp
is24HourFormat = true;
```

Cette modification force l’indicateur passé à la `TimePickerDialog` constructeur soit `true` afin que le mode de 24 heures est utilisé au lieu du format d’heure de l’activité d’hébergement. Lorsque vous générez et exécutez de nouveau l’application, cliquez sur le **choix temps** bouton, le `TimePicker` boîte de dialogue s’affiche désormais au format 24 heures :

[![Boîte de dialogue TimePicker au format 24 heures](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png)

Étant donné que le gestionnaire appelle [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) pour imprimer la durée de l’activité `TextView`, l’heure est toujours imprimé dans le format AM/PM de 12 heures par défaut.


<a name="summary" />

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment afficher un `TimePicker` widget comme une boîte de dialogue modale de menu contextuel à partir d’une activité Android. Il fourni un échantillon `DialogFragment` mise en œuvre et la `IOnTimeSetListener` interface. Cet exemple a également montré comment les `DialogFragment` peut interagir avec l’hôte de l’activité pour afficher l’heure sélectionnée.


## <a name="related-links"></a>Liens associés

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
