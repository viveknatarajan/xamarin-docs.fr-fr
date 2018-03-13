---
title: "Procédure pas à pas : création d’une interface utilisateur à onglets avec TabHost"
description: "Cet article vous guide dans la création d’une interface utilisateur à onglets dans Xamarin.Android à l’aide de l’API TabHost."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2dd397e824ce7735be4421c3f258852de3f77ecb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>Procédure pas à pas : création d’une interface utilisateur à onglets avec TabHost

_Cet article vous guide dans la création d’une interface utilisateur à onglets dans Xamarin.Android à l’aide de l’API TabHost._

> [!NOTE]
> `TabHost` est une ancienne API a été déconseillée par Google. Les développeurs sont encouragés à créer des applications à onglets à l’aide de la [ActionBar](~/android/user-interface/controls/action-bar.md). Le `ActionBar` est disponible dans toutes les version d’Android. Il a été introduite dans 3.0 Android (API niveau 11) et a été déplacée dans Android 2.2 (API niveau 8) et Android 2.3 (API de niveau 10) dans le [V7 AppCompat bibliothèque](http://developer.android.com/tools/support-library/features.html#v7-appcompat), qui est disponible pour Xamarin.Android via la [Xamarin Bibliothèque de prise en charge Android - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package.

Cet article vous guide dans la création d’une interface utilisateur à onglets dans Xamarin.Android à l’aide de la `TabHost` API. Il s’agit d’une ancienne API est disponible dans toutes les versions d’Android. Cet exemple crée une application avec trois onglets, avec la logique de chaque onglet encapsulée dans une activité.
La capture d’écran suivante est un exemple de l’application que nous allons créer :

![Capture d’écran de l’application avec plusieurs onglets](creating-a-tabbed-ui-images/image02.png)


## <a name="creating-the-application"></a>Création de l’application

Téléchargez et décompressez [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/).
Ce projet sert de point de départ pour notre application et contient des images. Si vous examinez ce projet, vous verrez que nous avons déjà créé les ressources drawable pour les icônes de l’onglet.

Première nous allons mettre à jour le fichier de disposition **Resources/Layout/Main.axml** qui hébergera les onglets. Modifier **Resources/Layout/Main.axml** et insérez le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

La capture d’écran suivante montre la mise en page dans le Concepteur de Xamarin :

[![Capture d’écran de la mise en page TabHost dans le Concepteur de Xamarin](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png#lightbox)

Le TabHost doit avoir deux vues enfants qu’il contient : un `TabWidget` et un `FrameLayout`. À la position la `TabWidget` et `FrameLayout` verticalement à l’intérieur du `TabHost`, un `LinearLayout` est utilisé. Le FrameLayout est l’emplacement du contenu pour chaque onglet, qui est vide, car le `TabHost` incorporeront automatiquement chaque activité lors de l’exécution. Il existe plusieurs règles qui doivent être observées en matière de création de la disposition pour les interfaces utilisateur à onglets :

-   Le `TabHost` doit avoir l’id `@android:id/tabhost`.

-   Le `TabWidget` doit avoir l’id `@android:id/tabs`.

-   Le `FrameLayout` doit avoir l’id `@android:id/tabcontent`.

-   `TabHost` nécessite une activité qu’il gère pour hériter de `TabActivity`. Par conséquent, il est important de sous-classe `TabActivity` ici &ndash; une activité régulière ne fonctionnera pas.

Modifiez le fichier **MainActivity.cs** afin que la classe `MainActivity` sous-classes `TabActivity` comme indiqué dans l’extrait de code suivant :

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

Créez quatre classes activité distinctes dans votre projet : `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, et `WhatsOnActivity`. Chaque activité composent l’interface utilisateur d’un onglet. Pour ces activités sont désormais un stub qui affiche un `TextView` avec un message simple. Modifier le code dans chaque activité pour contenir les éléments suivants `OnCreate` implémentation :

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

Notez que le code ci-dessus n’utilise pas un fichier de disposition. Elle crée simplement un `TextView` avec du texte et qui définit `TextView` comme l’affichage du contenu. Dupliquer ce pour chacun des trois activités restantes.

Ensuite, nous assignons les icônes pour chaque onglet. Chaque onglet requiert deux icônes &ndash; un pour l’état sélectionné et un pour l’état non sélectionné. Un exemple de ces deux icônes différentes peut être consulté dans les deux images suivantes (les icônes nécessaires pour cette application ont déjà été ajoutés à l’exemple de projet) :

![Capture d’écran de l’état sélectionné et les icônes d’état non sélectionné](creating-a-tabbed-ui-images/tab-icons.png)


Nous affectera les ressources drawable vers les onglets de l’icône en définissant un *de la liste État Drawable*. Liste d’état drawables sont une ressources drawable spéciales qui sont définis en XML et vous permettent de spécifier différentes images qui sont spécifiques à l’état de cet élément. Dans cet exemple, il existe une image qui est utilisée lorsqu’un onglet est sélectionné et l’autre qui est utilisé lors de l’onglet n’est pas sélectionnée. Pour gagner du temps, les drawables liste d’état nécessaires ont été ajoutées au projet. La liste suivante répertorie les fichiers et le XML contient :

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

Onglets sont ajoutés à la `TabHost` par programme, qui est une tâche très répétitive. Pour résoudre cela, ajoutez la méthode suivante à la classe `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

Chaque onglet dans le `TabHost` est représenté par une instance de la de la `TabHost.TabSpec` classe. Cette instance contient les métadonnées nécessaires pour restituer l’onglet, en particulier :

-   **Icône et le texte** &ndash; à afficher dans le `TabWidget`.

-   **Onglet contenu** &ndash; cela peut être soit un `Activity` ou un `View` et s’affiche lorsque l’onglet est sélectionné.

-   **Balise unique** &ndash; chaque onglet doit avoir une balise unique qui lui est affectée.

Nous devons ajouter une `TabHost.TabSpec` instance pour chaque onglet dans notre application. Vous permet de le faire à l’étape suivante.

Mettre à jour de la méthode `OnCreate` dans `MainActivity` afin qu’il ressemble au code suivant :

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

Exécutez l'application. Votre application doit ressembler à la capture d’écran présenté au début de cette procédure pas à pas.

C’est tout ! Nous avons créé une application à onglets qui donne à l’utilisateur de facilement accéder aux différentes parties d’une application.



## <a name="summary"></a>Récapitulatif

Ce chapitre décrit des mises en page à onglets et vous guider à travers le processus de création d’une application à onglets. La procédure pas à pas vous a montré comment utiliser un `TabActivity` augmentation une disposition de fichiers qui héberge un `TabHost` et un `TabWidget`. Le `TabHost` a été rempli avec une collection de `TabHost.TabSpec` objets qui sont utilisées par le `TabHost` lors de l’exécution pour instancier les activités qui seraient utilisées dans chaque onglet.



## <a name="related-links"></a>Liens associés

- [TabHostWalkthrough (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Package NuGet de prise en charge Android bibliothèque v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [bibliothèque d’appcompat V7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
