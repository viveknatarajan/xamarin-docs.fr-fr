---
title: Classes de fragment spécialisé
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 75d95d630415cdaa4c0c1ed3b8ddebb32b8e3c4d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60948008"
---
# <a name="specialized-fragment-classes"></a>Classes de fragment spécialisé

L’API de Fragments fournit autres sous-classes qui encapsulent certaines des fonctionnalités plus courantes utilisées dans les applications. Ces sous-classes sont :

-   **ListFragment** &ndash; ce Fragment est utilisé pour afficher une liste d’éléments liés à une source de données tel qu’un tableau ou un curseur.

-   **DialogFragment** &ndash; ce Fragment est utilisé comme un wrapper autour d’une boîte de dialogue. Le Fragment affichera la boîte de dialogue sur son activité.

-   **PreferenceFragment** &ndash; ce Fragment est utilisé pour afficher les objets de préférence sous forme de listes.



## <a name="the-listfragment"></a>Le ListFragment

Le `ListFragment` est très similaire dans les concepts et des fonctionnalités à la `ListActivity`; il est un wrapper qui héberge un `ListView` dans un Fragment. L’image ci-dessous montre un `ListFragment` s’exécutant sur une tablette et un téléphone :

[![Captures d’écran de ListFragment sur une tablette et sur un téléphone](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>Liaison de données avec le ListAdapter

Le `ListFragment` classe fournit déjà une disposition par défaut, il est donc pas nécessaire de remplacer `OnCreateView` pour afficher le contenu de la `ListFragment`. Le `ListView` est lié aux données en utilisant un `ListAdapter` implémentation. L’exemple suivant montre comment cela peut être fait à l’aide d’un simple tableau de chaînes :

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

Lors de la définition du `ListAdapter`, il est important d’utiliser le `ListFragment.ListAdapter` propriété et non le `ListView.ListAdapter` propriété. À l’aide de `ListView.ListAdapter` entraîne le code d’initialisation important doit être ignorée.



### <a name="responding-to-user-selection"></a>Réponse à la sélection de l’utilisateur

Pour répondre aux sélections de l’utilisateur, une application doit remplacer le `OnListItemClick` (méthode). L’exemple suivant montre une telle possibilité :

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

Dans le code ci-dessus, lorsque l’utilisateur sélectionne un élément dans le `ListFragment`, un nouveau Fragment est affiché dans l’activité hébergement, en affichant plus de détails sur l’élément qui a été sélectionné.



## <a name="dialogfragment"></a>DialogFragment

Le *DialogFragment* est un Fragment qui est utilisé pour afficher un objet de la boîte de dialogue à l’intérieur d’un Fragment qui flotte au-dessus de fenêtre de l’activité. Il est destiné à remplacer la boîte de dialogue géré API (à partir de dans Android 3.0). La capture d’écran suivante montre un exemple d’un `DialogFragment`:

[![Capture d’écran de DialogFragment affichage Ajouter nouveau EditBox de véhicule](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

Un `DialogFragment` garantit que l’état entre le Fragment et de la boîte de dialogue reste cohérente. Toutes les interactions et le contrôle de l’objet de la boîte de dialogue doivent se produire par le biais du `DialogFragment` API et ne pas être effectuée avec des appels directs sur l’objet de la boîte de dialogue. Le `DialogFragment` API fournit à chaque instance avec un `Show()` méthode qui est utilisé pour afficher un Fragment. Il existe deux façons de vous débarrasser d’un Fragment :

-  Appelez `DialogFragment.Dismiss()` sur la `DialogFragment` instance. 

-  Afficher un autre `DialogFragment`.

Pour créer un `DialogFragment`, une classe hérite de `Android.App.DialogFragment,` et remplace ensuite une des deux méthodes suivantes :

- **OnCreateView** &ndash; cela crée et retourne une vue.

- **OnCreateDialog** &ndash; cette opération crée une boîte de dialogue personnalisée. Il est généralement utilisé pour afficher un *AlertDialog*. Lorsque vous substituez cette méthode, il n’est pas nécessaire de remplacer `OnCreateView` .



### <a name="a-simple-dialogfragment"></a>Un Simple DialogFragment

La capture d’écran suivante montre un simple `DialogFragment` qui a un `TextView` et deux `Button`%s :

[![DialogFragment exemple avec un TextView et deux boutons](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

Le `TextView` affiche le nombre de fois que l’utilisateur a cliqué sur un bouton dans le `DialogFragment`, tandis que le bouton autres fermera le Fragment. Le code de `DialogFragment` est :

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```


### <a name="displaying-a-fragment"></a>Affichage d’un Fragment

Comme tous les Fragments, un `DialogFragment` s’affiche dans le contexte d’un `FragmentTransaction`.

Le `Show()` méthode sur un `DialogFragment` prend un `FragmentTransaction` et un `string` en tant qu’entrée. La boîte de dialogue sera ajouté à l’activité et le `FragmentTransaction` validée.

Le code suivant illustre une façon possible une activité peut utiliser le `Show()` méthode pour afficher un `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>Faire disparaître un Fragment

Appel `Dismiss()` sur une instance d’un `DialogFragment` provoque un Fragment à supprimer de l’activité et cette transaction est validée.
Les méthodes de cycle de vie de Fragment standards qui sont concernés par la destruction d’un Fragment seront être appelées.


### <a name="alert-dialog"></a>Boîte de dialogue alerte

Au lieu de remplacer `OnCreateView`, un `DialogFragment` peut se substituer à la place `OnCreateDialog`. Cela permet à une application créer un [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) qui est géré par un Fragment. Le code suivant est un exemple qui utilise le `AlertDialog.Builder` pour créer un `Dialog`:

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```



## <a name="preferencefragment"></a>PreferenceFragment

Pour aider à gérer les préférences, l’API de Fragments fournit le `PreferenceFragment` sous-classe. Le `PreferenceFragment` est similaire à la [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash; affiche une hiérarchie de préférences de l’utilisateur dans un Fragment. Lorsque l’utilisateur interagit avec les préférences, elles seront automatiquement enregistrées à [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html).
Dans Android 3.0 ou des applications plus élevées, utilisez le `PreferenceFragment` pour gérer les préférences dans les applications. L’illustration suivante montre un exemple d’un `PreferenceFragment`:

[![PreferencesFragment exemple avec inline, boîte de dialogue et les préférences de lancement](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>Créer un Fragment de préférence à partir d’une ressource

La préférence de Fragment peut être agrandie à partir d’un fichier de ressources XML à l’aide de la [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) (méthode). Un emplacement logique pour appeler cette méthode dans le cycle de vie du Fragment serait dans le `OnCreate` (méthode).

Le `PreferenceFragment` indiqué dans l’illustration ci-dessus a été créé en chargeant une ressource à partir de XML. Le fichier de ressources est :

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

Le code de la préférence de Fragment est comme suit :

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```



### <a name="querying-activities-to-create-a-preference-fragment"></a>Interrogation des activités pour créer un Fragment de préférence

Une autre technique pour la création d’un `PreferenceFragment` implique l’interrogation des activités. Chaque activité peut utiliser le [métadonnées\_clé\_préférence](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) attribut qui pointe vers un fichier de ressources XML. Dans Xamarin.Android, pour cela, orner une activité avec la `MetaDataAttribute`, puis en spécifiant le fichier de ressources à utiliser. Le `PreferenceFragment` classe fournit la méthode [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)) qui peut être utilisé pour interroger une activité pour trouver cette ressource XML et une hiérarchie de préférence pour qu’il la majoration.

Un exemple de ce processus est fourni dans l’extrait de code suivant, qui utilise `AddPreferencesFromIntent` pour créer un `PreferenceFragment`:

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android ressemblera à la classe `MyActivityWithPreference`. La classe doit être ornée de le `MetaDataAttribute,` comme indiqué dans l’extrait de code suivant :

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

Le `MetaDataAttribute` déclare un fichier de ressources XML que le `PreferenceFragment` utilisera l’augmentation de la hiérarchie de préférence. Si le `MetatDataAttribute` n’est pas fourni, puis une exception est levée au moment de l’exécution. Lorsque ce code s’exécute, le `PreferenceFragment` s’affiche comme dans la capture d’écran suivante :

[![Capture d’écran de l’exemple d’application avec PreferenceFragment affiché](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
