---
title: ActionBar
ms.topic: article
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 64a5ac7e0c448205da66f9790a506ca34a944140
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="actionbar"></a>ActionBar


## <a name="overview"></a>Vue d'ensemble

Lorsque vous utilisez `TabActivity`, le code pour créer les icônes de l’onglet n’a aucun effet quand exécuter par rapport à l’infrastructure Android 4.0. Bien que le point de vue fonctionnel qu’il fonctionne comme il le faisait dans les versions d’Android 2.3, avant du `TabActivity` classe proprement dite a été déconseillée dans 4.0. Une nouvelle façon de créer une interface avec onglets qui utilise la barre d’Action, nous aborderons ensuite a été introduite.


## <a name="action-bar-tabs"></a>Onglets de la barre d’action

La barre d’Action inclut la prise en charge pour l’ajout d’interfaces à onglets dans Android 4.0.
La capture d’écran suivante montre un exemple d’une telle interface.

[![Capture d’écran de l’application en cours d’exécution dans un émulateur ; deux onglets sont affichés.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Pour créer des onglets dans la barre d’Action, nous devons tout d’abord définir son `NavigationMode` pour prendre en charge les onglets de propriété. Android 4, un `ActionBar` propriété n’est disponible sur la classe d’activité, nous pouvons utiliser pour définir le `NavigationMode` comme suit :

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Après cela, nous pouvons créer un onglet en appelant le `NewTab` méthode sur la barre d’Action. Avec cette instance de l’onglet, nous pouvons appeler le `SetText` et `SetIcon` méthodes pour définir le texte de l’étiquette de l’onglet et l’icône ; ces appels sont effectués dans l’ordre dans le code ci-dessous :

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Nous pouvons ajouter l’onglet Toutefois, nous devons traiter la `TabSelected` événement. Dans ce gestionnaire, nous pouvons créer le contenu de l’onglet. Onglets de la barre d’action sont conçus pour fonctionner avec *Fragments*, qui sont des classes qui représentent une partie de l’interface utilisateur dans une activité. Pour cet exemple, vue de son contienne un seul `TextView`, ce qui nous augmentation dans notre `Fragment` sous-classe comme suit :

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);
       
        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

L’argument d’événement passé dans le `TabSelected` événement est de type `TabEventArgs`, qui inclut un `FragmentTransaction` propriété que nous pouvons utiliser pour ajouter le fragment, comme indiqué ci-dessous :

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Enfin, nous pouvons ajouter l’onglet à la barre d’Action en appelant le `AddTab` méthode comme indiqué dans ce code :

```csharp
this.ActionBar.AddTab (tab);
```

Pour obtenir un exemple complet, consultez la *HelloTabsICS* projet dans l’exemple de code pour ce document.


## <a name="shareactionprovider"></a>ShareActionProvider

La `ShareActionProvider` classe permet à une action de partage puisse avoir lieu à partir de la barre d’Action. Il prend en charge de la création d’une vue de l’action avec une liste d’applications qui peut gérer un mode de partage et conserve un historique des applications précédemment utilisés pour accéder facilement aux leur d’ultérieurement à partir de la barre d’Action. Cela permet aux applications de partager des données via une expérience utilisateur est cohérente dans Android.


### <a name="image-sharing-example"></a>Exemple de partage de l’image

Par exemple, voici une capture d’écran de la barre d’Action avec un élément de menu pour partager une image (effectuée à partir de la [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) exemple). Quand l’utilisateur appuie sur l’élément de menu dans la barre d’Action, le ShareActionProvider charge de l’application pour gérer une intention est associée le `ShareActionProvider`. Dans cet exemple, l’application de messagerie a été précédemment utilisée, et il est présenté dans la barre d’Action.

[![Capture d’écran de l’icône dans la barre d’Action de l’application de messagerie](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Lorsque l’utilisateur clique sur l’élément dans la barre d’Action, l’application de messagerie qui contient l’image partagée est lancée, comme indiqué ci-dessous :

[![Capture d’écran de l’application de messagerie afficher singe image](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Spécification de l’action de classe de fournisseur

Pour utiliser le `ShareActionProvider`, définissez le `android:actionProviderClass` attribut sur un élément de menu dans le fichier XML pour le menu de la barre d’Action comme suit :

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>GONFLAGE du Menu

Pour augmenter le menu, nous remplaçons `OnCreateOptionsMenu` dans la sous-classe d’activité. Une fois que nous avons une référence au menu, nous pouvons obtenir le `ShareActionProvider` à partir de la `ActionProvider` propriété de l’élément de menu et puis utiliser la méthode SetShareIntent pour définir le `ShareActionProvider`d’intention, comme indiqué ci-dessous :

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       
           
    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>Création de l’objectif

Le `ShareActionProvider` utilisera l’intention, passée à la `SetShareIntent` méthode dans le code ci-dessus, pour lancer l’activité appropriée. Dans ce cas, nous créons une intention d’envoyer une image en utilisant le code suivant :

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

L’image dans l’exemple de code ci-dessus est inclus en tant qu’un élément multimédia avec l’application et copié vers un emplacement accessible publiquement lors de la création de l’activité, il est donc accessible à d’autres applications, telles que l’application de messagerie. L’exemple de code qui accompagne cet article contient la source complète de cet exemple, qui illustre son utilisation.



## <a name="related-links"></a>Liens associés

- [Hello onglets ICS (exemple)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [Démonstration de ShareActionProvider (exemple)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
