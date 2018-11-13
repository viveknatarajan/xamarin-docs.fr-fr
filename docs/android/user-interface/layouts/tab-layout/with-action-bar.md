---
title: Dispositions à onglets avec la barre d’action
description: Ce guide présente et explique comment utiliser les APIs ActionBar pour créer une interface utilisateur à onglets dans une application Xamarin.Android.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 01ff4426dfb4764b7d9ca22703aa2b1667a90454
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528544"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Dispositions à onglets avec la barre d’action

_Ce guide présente et explique comment utiliser les APIs ActionBar pour créer une interface utilisateur à onglets dans une application Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

La barre d’action est un modèle d’interface utilisateur Android qui est utilisé pour fournir une interface utilisateur cohérente pour les fonctionnalités clés telles que les onglets, d’identité de l’application, de menus et de recherche. Dans Android 3.0 (niveau d’API 11), Google a présenté les APIs ActionBar la plateforme Android. Les APIs ActionBar introduire des thèmes d’interface utilisateur pour fournir une apparence cohérente et des classes qui permettent d’interfaces utilisateur à onglets. Ce guide explique comment ajouter des onglets de la barre d’Action à une application Xamarin.Android. Il explique également comment utiliser la bibliothèque de prise en charge Android version 7 rétroporter ActionBar onglets pour les applications Xamarin.Android ciblant Android 2.1 pour Android 2.3. 

Notez que `Toolbar` est un composant de barre d’action plus récente et plus généralisé que vous devez utiliser à la place de `ActionBar` (`Toolbar` a été conçu pour remplacer `ActionBar`). Pour plus d’informations, consultez [barre d’outils](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="requirements"></a>Configuration requise

N’importe quelle application Xamarin.Android qui cible le niveau d’API 11 (Android 3.0) ou une version ultérieure a accès aux APIs ActionBar dans le cadre de l’API Android natif. 

Certaines APIs ActionBar ont été portées de revenir au niveau de l’API 7 (Android 2.1) et sont disponibles via le [V7 AppCompat bibliothèque](http://developer.android.com/tools/support-library/features.html#v7-appcompat), qui est rendue disponible pour les applications Xamarin.Android via la [Xamarin de bibliothèque de prise en charge Android - V7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package.



## <a name="introducing-tabs-in-the-actionbar"></a>Présentation des onglets dans la barre d’action

La barre d’action tente d’afficher toutes ses onglets simultanément et égaliser tous les onglets en fonction de la largeur de l’étiquette d’onglet plus large. Ceci est illustré par la capture d’écran suivante : 

![Capture d’écran de la barre d’action avec tous les onglets de taille égale indiqué](with-action-bar-images/image1.png)

Lors de la barre d’action ne peut pas afficher tous les onglets, il configurera les onglets dans une vue déroulante horizontalement. L’utilisateur peut balayer gauche ou droite pour voir les autres onglets. Cette capture d’écran à partir de Google Play montre un exemple de ceci : 

![Capture d’écran des onglets dans une vue déroulante horizontalement](with-action-bar-images/image2.png)

Chaque onglet dans la barre d’action doit être associé à un [ *fragment*](~/android/platform/fragments/index.md). Lorsque l’utilisateur sélectionne un onglet, l’application affiche le fragment associé à l’onglet. La barre d’action n’est pas chargée d’afficher le fragment approprié à l’utilisateur. Au lieu de cela, la barre d’action sera avertir une application des changements d’état dans un onglet via une classe qui implémente l’interface ActionBar.ITabListener. Cette interface fournit trois méthodes de rappel qui appelle Android lorsque l’état de l’onglet change : 

-  **OnTabSelected** -cette méthode est appelée lorsque l’utilisateur sélectionne l’onglet. Il doit afficher le fragment.

-  **OnTabReselected** -cette méthode est appelée lorsque l’onglet est déjà activée mais est à nouveau par l’utilisateur. Ce rappel sert généralement à actualisation/mettre à jour le fragment affiché.

-  **OnTabUnselected** -cette méthode est appelée lorsque l’utilisateur sélectionne un autre onglet. Ce rappel est utilisé pour enregistrer l’état dans le fragment affiché avant qu’il disparaisse.

Xamarin.Android encapsule le `ActionBar.ITabListener` avec des événements sur le `ActionBar.Tab` classe. Les applications peuvent assigner des gestionnaires d’événements à un ou plusieurs de ces événements. Il existe trois événements (une pour chaque méthode dans `ActionBar.ITabListener`) qui déclenche un onglet de barre d’action : 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>Ajout d’onglets à la barre d’action

La barre d’action est native pour Android 3.0 (niveau d’API 11) et les versions ultérieures et est disponible pour n’importe quelle application Xamarin.Android qui cible cette API au minimum. 

Les étapes suivantes expliquent comment ajouter des onglets de la barre d’action à une activité Android : 

1. Dans le `OnCreate` méthode d’une activité &ndash; *avant d’initialiser les widgets d’interface utilisateur* &ndash; une application doit définir le `NavigationMode` sur le `ActionBar` à `ActionBar.NavigationModeTabs` comme indiqué dans ce code extrait de code :

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Créer un nouvel onglet à l’aide `ActionBar.NewTab()`.

3. Affecter des gestionnaires d’événements ou fournir une personnalisée `ActionBar.ITabListener` implémentation qui répond aux événements qui sont déclenchés lorsque l’utilisateur interagit avec les onglets de la barre d’action.

4. L’onglet qui a été créé à l’étape précédente pour ajouter le `ActionBar`.


Le code suivant est un exemple d’utilisation de ces étapes pour ajouter des onglets à une application qui utilise des gestionnaires d’événements pour répondre aux modifications d’état : 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>Vs de gestionnaires d’événements ActionBar.ITabListener

Les applications doivent utiliser des gestionnaires d’événements et `ActionBar.ITabListener` pour différents scénarios. Gestionnaires d’événements offrent une certaine quantité de commodité syntaxique ; ils vous évitent d’avoir à créer une classe et implémenter `ActionBar.ITabListener`. Cet pratique est fourni à un coût &ndash; Xamarin.Android effectue cette transformation pour vous, une classe de création et implémentation `ActionBar.ITabListener` pour vous. Cela convient lorsqu’une application a un nombre limité d’onglets. 

Lorsque vous traitez avec plusieurs onglets, ou le partage des fonctionnalités communes entre les onglets de la barre d’action, il peut être plus efficace en termes de mémoire et de performances pour créer une classe personnalisée qui implémente `ActionBar.ITabListener`et le partage d’une seule instance de la classe. Cela réduira le nombre de GREF à l’aide d’une application Xamarin.Android. 



### <a name="backwards-compatibility-for-older-devices"></a>Vers l’arrière compatibilité pour les appareils plus anciens

Le [bibliothèque de prise en charge Android v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) back ports onglets ActionBar à Android 2.1 (API niveau 7). Onglets sont accessibles dans une application Xamarin.Android une fois que ce composant a été ajouté au projet.

Pour utiliser la barre d’action, une activité doit sous-classe `ActionBarActivity` et utiliser le thème AppCompat comme illustré dans l’extrait de code suivant :

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Une activité peut obtenir une référence à sa barre d’action à partir de la `ActionBarActivity.SupportingActionBar` propriété. L’extrait de code suivant illustre un exemple de configuration de la barre d’action dans une activité :

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```


## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons abordé la création d’une interface utilisateur à onglets dans une application Xamarin.Android à l’aide de la barre d’action. Nous avons abordé comment ajouter des onglets à la barre d’action et comment une activité peut interagir avec les événements onglet via le `ActionBar.ITabListener` interface. Nous avons également vu comment backports de package de bibliothèque de prise en charge Android v7 AppCompat ActionBar onglets aux anciennes versions d’Android. 


## <a name="related-links"></a>Liens associés

- [ActionBarTabs (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [Barre d’outils](~/android/user-interface/controls/tool-bar/index.md)
- [Fragments](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Modèle de la barre d’action](http://developer.android.com/design/patterns/actionbar.html)
- [V7 Android AppCompat](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Package NuGet AppCompat prise en charge de Xamarin.Android bibliothèque v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
