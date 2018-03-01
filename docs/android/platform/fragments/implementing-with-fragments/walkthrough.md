---
title: "Procédure pas à pas"
ms.topic: article
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 7441fbe1fc686dc4fa5cb67cbfc5ae6353f32c93
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough"></a>Procédure pas à pas

Dans les étapes suivantes, une application de base est créée avec des fragments. La première étape consiste à créer un nouveau Xamarin.Android pour le projet Android.

## <a name="1-create-a-project"></a>1. Créer un projet

Créez un nouveau projet Xamarin.Android appelé **FragmentSample**. Le **Minimum Android** version doit être définie pour Android 3.1 ou version ultérieure, comme indiqué dans l’image ci-dessous :

[![Définition de la version minimale Android](walkthrough-images/00.png)](walkthrough-images/00.png)


## <a name="2-create-the-mainactivity"></a>2. Créer le MainActivity

Ensuite, nous avons besoin créer la `MainActivity` classe. Il s’agit de l’activité de démarrage de l’application. Cette activité va héberger un ou deux fragments, en fonction de la taille de l’écran. `MainActivity` Cette opération en chargeant le fichier de disposition qui convient à l’appareil.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` Sous-classes fragment doivent avoir une valeur par défaut public constructeur sans argument.

## <a name="3-create-the-layout-files"></a>3. Créer les fichiers de mise en page

Les deux différentes tailles d’écran requièrent deux fichiers de mise en page différente. Nous allons créer un nouveau dossier, **ressources/disposition-Large**et créer une nouvelle disposition appelée **activity_main.axml**. Nous allons également renommer le fichier de disposition par défaut en tant que **Resources/Layout/activity_main.axml**. Après ces modifications, les dossiers de mise en page doivent ressembler à la capture d’écran suivante :

[![Capture d’écran de dossiers de disposition dans l’IDE](walkthrough-images/01.png)](walkthrough-images/01.png)


Charge et utiliser le fichier de disposition dans tous les appareils **disposition/ressources**.
Il s’agit d’une mise en page très simple qui affiche simplement un `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Pour les appareils qui ont un grand écran, Android chargera le fichier de disposition dans **ressources/disposition-Large**. Le contenu de la disposition pour les tablettes est comme suit :

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

Le fichier de disposition pour les écrans plus grands est légèrement différent. Non seulement le `TitlesFragment` affiché dans ce fichier de mise en page, mais un `FrameLayout` est ajouté à droite en regard du fragment. Sur les écrans plus grands, la `DetailsFragment` est ajouté par programme à `MainActivity` lorsque l’utilisateur sélectionne un jeu. Nous expliquons plus en détail comment procéder.

3.2 Android a introduit une nouvelle façon de spécifier des dispositions d’écran. Ces qualificateurs nouvelle spécifient la quantité d’espace à votre disposition doit, au lieu de la taille de l’écran. Si cette application a été destinée à s’exécuter uniquement sur Android 3.2 ou version ultérieure, il faudrait créer un **disposition/ressources-sw600dp** dossier (au lieu du dossier **ressource/disposition-Large**) pour le fichier de disposition  **activity_main.axml**. Ce fichier de ressources est chargé par tous les appareils qui ont une largeur d’écran minimale de 600 indépendant de la densité de pixels. Toutefois, en tant que cette application est définie sur la cible Android 3.1 ou une version ultérieure, il utilise le qualificateur de ressource plus anciens.

## <a name="4-create-the-titlesfragment"></a>4. Créer le TitlesFragment

`TitlesFragment` s’afficher les titres de la lecture de différentes, par conséquent, nous allons ajouter un nouveau fragment au projet appelé `TitlesFragment`:

[![Ajout d’un nouveau fragment au projet TitlesFragment](walkthrough-images/02.png)](walkthrough-images/02.png)

Après avoir `TitlesFragment` a été ajouté, nous devons modifier la classe afin qu’elle hérite de `Android.App.ListFragment`. `ListFragment` est un type de fragment spécialisées qui inclut les fonctionnalités de la liste.
`TitlesFragment` remplace également `OnActivityCreated` (une autre méthode de cycle de vie de fragment) et fournir une `Adapter` qui `ListFragment` utilisera pour remplir la liste :

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

Comme mentionné précédemment, notre application possède deux dispositions pour `MainActivity`. Le code dans `OnActivityCreated` détecte la présence de la `FrameLayout` et détermine quel fichier de disposition a été chargé. Si le `FrameLayout` existe dans la mise en page, puis le `_isDualPane` est défini à `true`. Le `_isDualPane` indicateur est utilisé ailleurs dans l’activité, notamment par la `ShowDetails` (méthode). Le `ShowDetails` méthode est abordée plus en détail ci-dessous.

`TitlesFragment` est une liste et doit répondre aux sélections de l’utilisateur dans la liste. Pour ce faire, `TitlesFragment` remplace la méthode `OnListItemClick`. À l’intérieur de `OnListItemClick`, un nouveau `DetailsFragment` est créé et affiché dans le `FrameLayout`, par programme. Le code à l’intérieur de `TitlesFragment` est :

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

Le code détermine à partir de l’appareil comment mettre en forme et afficher les guillemets de la lecture sélectionnée. Dans le cas des tablettes, le `_isDualPane` indicateur a la valeur `true`, et par conséquent, la demande s’affiche à côté du `TitlesFragment`. Si la lecture sélectionnée `id` n’est pas déjà affiché, puis une nouvelle `DetailsFragment` est créé et ensuite chargées dans le `FrameLayout` sur l’activité. Pour d’autres périphériques qui n’ont pas d’un affichage de grande taille &ndash; les téléphones, par exemple &ndash; `isDualPane` a la valeur `false` donc un nouveau `DetailsActivity` sera démarré.

<a name="5. Create_the_DetailsActivity" />

## <a name="5-create-the-detailsactivity"></a>5. Créer le DetailsActivity

Le `DetailsActivity` affiche le `DetailsFragment` pour les périphériques de petite taille. Pour le voir, tout d’abord nous ajouterons une nouvelle activité pour le projet nommé `DetailsActivity`. `DetailsActivity` est une activité très simple. Elle crée et puis y héberger un nouveau `DetailsFragment` pour la lecture `id` qui a été envoyé :

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Notez qu’aucun fichier de disposition est chargé pour `DetailsActivity`. Au lieu de cela, `DetailsFragment` est chargé dans l’affichage de l’activité racine. Cette vue racine a l’ID spécial `Android.Resource.Id.Content`. Un nouveau `DetailFragment` est créé, puis à cette vue racine à l’intérieur d’un `FragmentTransaction` qui est créé par l’activité `FragmentManager`.

<a name="6. Create_the_DetailsFragment" />

## <a name="6-create-the-detailsfragment"></a>6. Créer le DetailsFragment

Maintenant, nous allons ajouter un autre fragment à l’application nommée `DetailsFragment`. Ce fragment affichera un guillemet à partir de la lecture sélectionné. Le code suivant montre l’intégralité `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

Dans l’ordre pour `DetailsFragment` pour fonctionner correctement, il doit avoir l’index de la lecture est sélectionnée dans le `TitlesFragment`. Il existe plusieurs façons pour fournir cette valeur à `DetailsFragment`; dans cet exemple, la lecture `Id` est placé dans un groupe et qui offre groupée est stockée dans la propriété Arguments d’une instance de la `DetailsFragment`. La propriété `ShownPlayId` est fournie pour des raisons pratiques &ndash; il sera utilisé par les instances de `DetailsFragment` pour récupérer cette valeur à partir de l’application.

`OnCreateView` est appelée lorsque le fragment doit dessiner son interface utilisateur et doit retourner un `Android.Views.View` objet. Dans la plupart des cas, il s’agit d’un `View` agrandie à partir d’un fichier existant de la mise en page. Dans le cas de l’exemple ci-dessus, le fragment allez générer par programmation de la vue qui sera utilisée pour l’affichage.

Félicitations ! Vous venez de créer une application qui utilise des fragments de simplifier le développement sur des facteurs de forme.

Dans le [section suivante](supporting-pre-honeycomb.md), nous allons étendre cette application afin qu’il fonctionne sur les appareils Android avant 3.0.

