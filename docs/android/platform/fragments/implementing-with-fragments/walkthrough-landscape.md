---
title: Procédure pas à pas sur les fragments - Partie 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 7ec8ad6ce428107d2255dd07c7e69c9e77780c09
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61026275"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Procédure pas à pas des fragments &ndash; paysage

Le [Fragments Walkthrough &ndash; partie 1](./walkthrough.md) vous a montré comment créer et utiliser des fragments dans une application Android qui cible des écrans plus petits sur un téléphone. L’étape suivante de cette procédure pas à pas consiste à modifier l’application pour tirer parti de l’espace supplémentaire horizontal sur tablette &ndash; il y aura une seule activité qui sera toujours la liste de lecture (le `TitlesFragment`) et `PlayQuoteFragment` sera ajouté dynamiquement à l’activité en réponse à une sélection effectuée par l’utilisateur :

[![Application en cours d’exécution sur tablette](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Téléphones qui sont exécutent en mode paysage bénéficient également de cette amélioration :

[![Application en cours d’exécution sur un téléphone Android en mode paysage](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>La mise à jour de l’application pour gérer l’orientation paysage

Les modifications suivantes seront appuient sur le travail qui a été effectué dans le [Fragments Walkthrough - téléphone](./walkthrough.md)

1. Créer une disposition différente pour afficher à la fois le `TitlesFragment` et `PlayQuoteFragment`.
1. Mise à jour `TitlesFragment` pour détecter si l’appareil affiche les deux fragments simultanément et de modifier le comportement en conséquence.
1. Mise à jour `PlayQuoteActivity` à fermer quand l’appareil est en mode paysage.

## <a name="1-create-an-alternate-layout"></a>1. Créer une disposition différente

Lors de l’activité de la principale est créée sur un appareil Android, Android décidera de disposition à charger selon l’orientation de l’appareil. Par défaut, Android fournit le **Resources/layout/activity_main.axml** fichier de disposition. Pour les appareils qui se chargent en mode paysage Android fournira la **Resources/layout-land/activity_main.axml** fichier de disposition. Le guide sur [ressources Android](/xamarin/android/app-fundamentals/resources-in-android) contient plus de détails sur la façon dont Android décide quelle ressource fichiers à charger pour une application.

Créer une disposition différente qui cible **paysage** l’orientation en suivant les étapes décrites dans le [autres dispositions](/xamarin/android/user-interface/android-designer/alternative-layout-views) guide. Cela doit ajouter un nouveau fichier de ressources de mise en page au projet, **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Autre disposition dans l’Explorateur de solutions](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Autre disposition dans le panneau Solution](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Après avoir créé la mise en page à l’autre, modifiez la source du fichier **Resources/layout-land/activity_main.axml** afin qu’elle corresponde à ce document XML :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

La vue de la racine de l’activité de porte l’ID de ressource `two_fragments_layout` et dispose de deux vues de sous-élément, un `fragment` et un `FrameLayout`. Alors que le `fragment` est statiquement chargé, le `FrameLayout` agit comme un « espace réservé » qui sera remplacé au moment de l’exécution par le `PlayQuoteFragment`. Chaque fois qu’une nouvelle lecture est sélectionné dans le `TitlesFragment`, le `playquote_container` sera mis à jour avec une nouvelle instance de la `PlayQuoteFragment`.

Chaque vue secondaire occupe toute la hauteur de leur parent. La largeur de chaque sous-affichage est contrôlée par le `android:layout_weight` et `android:layout_width` attributs. Dans cet exemple, chaque sous-affichage occupera de 50 % de largeur fournissent par le parent. Consultez [document de Google sur le LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) pour plus d’informations sur _disposition poids_.

## <a name="2-changes-to-titlesfragment"></a>2. Modifications apportées aux TitlesFragment

Une fois que la mise en page à l’autre a été créé, il est nécessaire de mettre à jour `TitlesFragment`. Lorsque l’application affiche les deux fragments sur une seule activité, puis `TitlesFragment` doit charger le `PlayQuoteFragment` dans l’activité parente. Sinon, `TitlesFragment` doit lancer le `PlayQuoteActivity` hôte auquel le `PlayQuoteFragment`. Un indicateur booléen aidera `TitlesFragment` déterminer laquelle il doit utiliser le comportement. Cet indicateur sera initialisé dans le `OnActivityCreated` (méthode).

Tout d’abord, ajoutez une variable d’instance en haut de la `TitlesFragment` classe :

```csharp
bool showingTwoFragments;
```

Ensuite, ajoutez l’extrait de code suivant à `OnActivityCreated` pour initialiser la variable : 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Si l’appareil est en cours d’exécution en mode paysage, puis le `FrameLayout` avec l’ID de ressource `playquote_container` seront visibles sur l’écran, donc `showingTwoFragments` seront initialisés à `true`. Si l’appareil est en cours d’exécution en mode portrait, puis `playquote_container` ne sera pas sur l’écran, donc `showingTwoFragments` sera `false`.

Le `ShowPlayQuote` méthode devez modifier la façon dont il affiche un guillemet &ndash; dans un fragment ou le lancement d’une nouvelle activité.  Mise à jour le `ShowPlayQuote` méthode pour charger un fragment lors de l’affichage de deux fragments, sinon il doit lancer une activité :

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Si l’utilisateur a sélectionné un play est différent de celui qui est actuellement affiché dans `PlayQuoteFragment`, puis un nouveau `PlayQuoteFragment` est créé et remplace le contenu de la `playquote_container` dans le contexte d’un `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Code complet pour TitlesFragment

Après avoir effectué toutes les modifications antérieures à `TitlesFragment`, la classe complète doit correspondre à ce code :

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }


        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. Modifications apportées aux PlayQuoteActivity

Il existe un détail final pour prendre en charge : `PlayQuoteActivity` n’est pas nécessaire quand l’appareil est en mode paysage. Si l’appareil est en mode paysage le `PlayQuoteActivity` ne doit pas être visible. Mise à jour le `OnCreate` méthode de `PlayQuoteActivity` afin qu’elle se ferme. Ce code est la version finale de `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Cette modification ajoute une vérification de l’orientation du périphérique. S’il est en mode paysage, puis `PlayQuoteActivity` se fermer.

## <a name="4-run-the-application"></a>4. Exécuter l'application

Une fois ces modifications effectuées, exécutez l’application, faites pivoter le périphérique pour paysage (si nécessaire), puis sélectionnez un jeu. La demande doit être affichée sur le même écran que la liste de lecture :

[![Application en cours d’exécution sur un téléphone Android en mode paysage](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Application en cours d’exécution sur une tablette Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
