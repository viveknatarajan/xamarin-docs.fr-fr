---
title: Création d’un Fragment
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 339de4930242e35c40b034af2ce6ba47fe1543af
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023677"
---
# <a name="creating-a-fragment"></a>Création d’un Fragment

Pour créer un Fragment, une classe doit hériter de `Android.App.Fragment` , puis vous remplacez le `OnCreateView` (méthode). `OnCreateView` sera appelée par l’activité hébergement lorsqu’il est temps de mettre le Fragment sur l’écran et retournera un `View`. Classique `OnCreateView` crée ce `View` en gonfler un fichier de disposition, puis l’attacher à un conteneur parent. Les caractéristiques du conteneur sont importantes car Android appliquera les paramètres de disposition du parent à l’interface utilisateur du Fragment. L'exemple suivant illustre ce comportement :

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Le code ci-dessus augmenteront la vue `Resource.Layout.Example_Fragment`et l’ajouter comme une vue enfant à la `ViewGroup` conteneur.


> [!NOTE]
> Sous-classes de fragment doivent avoir une valeur par défaut public constructeur sans argument.

## <a name="adding-a-fragment-to-an-activity"></a>Ajout d’un Fragment à une activité

Il existe deux façons qu’un Fragment peut être hébergé à l’intérieur d’une activité :

-   **Manière déclarative** &ndash; Fragments peuvent être utilisées de manière déclarative dans `.axml` des fichiers de disposition à l’aide de la `<Fragment>` balise.

-   **Par programmation** &ndash; Fragments peuvent également être instanciés dynamiquement à l’aide de la `FragmentManager` les API de la classe.

Utilisation par programmation via la `FragmentManager` classe nous aborderons plus loin dans ce guide.

### <a name="using-a-fragment-declaratively"></a>À l’aide d’un Fragment de façon déclarative

Ajout d’un Fragment à l’intérieur de la disposition nécessite que vous utilisiez le `<fragment>` balise et identifier le Fragment en fournissant une la `class` attribut ou le `android:name` attribut. L’extrait de code suivant montre comment utiliser le `class` attribut pour déclarer un `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Cet extrait de code suivant montre comment déclarer un `fragment` à l’aide de la `android:name` attribut pour identifier la classe de Fragment :

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Lorsque l’activité est en cours de création, Android instancie chaque Fragment spécifié dans le fichier de disposition et insérer la vue est créée à partir de `OnCreateView` à la place de la `Fragment` élément.
Les fragments sont ajoutés de façon déclarative à une activité sont statiques et resteront sur l’activité jusqu'à ce qu’il est détruit ; Il n’est pas possible de remplacer ou supprimer ce Fragment pendant la durée de vie de l’activité à laquelle elle est associée de manière dynamique.

Chaque Fragment doit être assigné un identificateur unique :

-  **Android : id** &ndash; comme avec d’autres éléments d’interface utilisateur dans un fichier de disposition, il s’agit un ID unique.

-  **Android : balise** &ndash; cet attribut est une chaîne unique.

Si aucun des deux méthodes précédentes est utilisé, le Fragment suppose l’ID de la vue du conteneur. Dans l’exemple suivant où ni `android:id` ni `android:tag` est fourni, Android attribue l’ID `fragment_container` au Fragment :

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>Cas de nom de package

Android n’autorise pas les caractères majuscules dans les noms de package ; une exception sera levée lorsque vous tentez de la vue de la majoration si un nom de package contient un caractère majuscule. Toutefois, Xamarin.Android est généralement plus indulgent avec et tolérer les caractères majuscules dans l’espace de noms.

Par exemple, les deux extraits de code suivants fonctionnera avec Xamarin.Android. Toutefois, le second extrait de code entraîne un `android.view.InflateException` doit être levée par une application Android basée sur Java pure.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

OU

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>Cycle de vie de fragment

Fragments ont leur propre cycle de vie qui est quelque peu indépendant des, mais il est toujours affecté par, le [cycle de vie de l’activité hébergement](~/android/app-fundamentals/activity-lifecycle/index.md).
Par exemple, lorsqu’une activité est maintenu, toutes ses Fragments associés sont interrompues. Le diagramme suivant illustre le cycle de vie du Fragment.

[![Organigramme illustrant le cycle de vie du Fragment](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>Méthodes de cycle de vie de création de fragment

La liste ci-dessous illustre le flux des différents rappels dans le cycle de vie d’un Fragment comme il est en cours de création :

-   **`OnInflate()`** &ndash; Appelé lorsque le Fragment est créé dans le cadre d’une disposition de vue. Cela peut être appelée immédiatement après que le Fragment est créé de façon déclarative à partir d’un fichier XML de disposition. Le Fragment n’est pas associé à son activité encore, mais la **activité**, **Bundle**, et **AttributeSet** à partir de la vue hiérarchie sont passés comme paramètres. Cette méthode est utilisée pour l’analyse la **AttributeSet** et, pour enregistrer les attributs qui peut-être être utilisée ultérieurement par le Fragment.

-   **`OnAttach()`** &ndash; Appelée une fois que le Fragment est associé à l’activité. Il s’agit de la première méthode à exécuter lorsque le Fragment est prêt à être utilisé. En règle générale, les Fragments ne doivent pas implémenter un constructeur ou remplacer le constructeur par défaut. Tous les composants qui sont requis pour le Fragment doivent être initialisés dans cette méthode.

-   **`OnCreate()`** &ndash; Appelée par l’activité pour créer le Fragment. Lorsque cette méthode est appelée, la hiérarchie d’affichage de l’activité d’hébergement ne peut pas être complètement instanciée, donc le Fragment ne doit pas dépendre de tous les composants de la hiérarchie d’affichage de l’activité jusqu'à ce que plus tard au cours du cycle de vie du Fragment. Par exemple, n’utilisez pas cette méthode pour effectuer des ajustements ou des ajustements à l’interface utilisateur de l’application. Il s’agit de la première heure à laquelle le Fragment peut commencer à collecter des données dont il a besoin. Le Fragment est en cours d’exécution dans le thread d’interface utilisateur à ce stade, donc éviter tout traitement de longue durée ou effectuer ce traitement sur un thread d’arrière-plan. Cette méthode peut être ignorée si **SetRetainInstance(true)** est appelée.
    Cette solution est décrites plus en détail ci-dessous.

-   **`OnCreateView()`** &ndash; Crée la vue pour le Fragment.
    Cette méthode est appelée une fois l’activité **OnCreate()** méthode est terminée. À ce stade, il est déconseillé d’interagir avec la hiérarchie d’affichage de l’activité. Cette méthode doit retourner la vue qui sera utilisée par le Fragment.

-   **`OnActivityCreated()`** &ndash; Appelée après **Activity.OnCreate** a été effectuée par l’activité d’hébergement.
    Ultimes à l’interface utilisateur doivent être effectuées pour l’instant.

-   **`OnStart()`** &ndash; Appelé après que l’activité conteneur a été reprise. Cela rend le Fragment visibles par l’utilisateur. Dans de nombreux cas, le Fragment contiendra le code qui serait normalement dans les **OnStart()** méthode d’une activité.

-   **`OnResume()`** &ndash; Il s’agit de la dernière méthode appelée avant que l’utilisateur peut interagir avec le Fragment. Un exemple du type de code qui doit être effectuée dans cette méthode est d’activer les fonctionnalités d’un appareil de l’utilisateur peut interagir avec, telles que l’appareil photo qui les services d’emplacement. Services telles que celles-ci peuvent entraîner une décharge excessive de la batterie, cependant, et une application doit limiter leur utilisation pour préserver l’autonomie.


### <a name="fragment-destruction-lifecycle-methods"></a>Méthodes de cycle de vie de Destruction de fragment

La liste suivante décrit les méthodes de cycle de vie qui sont appelées comme un Fragment est en cours de destruction :

-   **`OnPause()`** &ndash; L’utilisateur n’est plus en mesure d’interagir avec le Fragment. Ce cas se présente, car une autre opération de Fragment modifie ce Fragment ou l’activité d’hébergement est suspendue. Il est possible que l’activité qui héberge ce Fragment peut toujours être visible, autrement dit, l’activité dans le focus est partiellement transparente ou n’occupe pas de mode plein écran. Lorsque cette méthode devient active, il est le premier signe que l’utilisateur quitte le Fragment. Le Fragment doit enregistrer toutes les modifications.

-   **`OnStop()`** &ndash; Le Fragment n’est plus visible. L’activité de l’hôte est peut-être arrêté, ou une opération de Fragment est modifiant dans l’activité. Ce rappel a le même objectif en tant que **Activity.OnStop**.

-   **`OnDestroyView()`** &ndash; Cette méthode est appelée pour nettoyer les ressources associées à la vue. Cela est appelé lorsque la vue associée au Fragment a été détruite.

-   **`OnDestroy()`** &ndash; Cette méthode est appelée lorsque le Fragment n’est plus en cours d’utilisation. Il est toujours associé à l’activité, mais le Fragment n’est plus fonctionnel. Cette méthode doit libérer toutes les ressources qui sont en cours d’utilisation par le Fragment, comme un [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) qui peut être utilisé pour un appareil photo. Cette méthode peut être ignorée si **SetRetainInstance(true)** est appelée. Cette solution est décrites plus en détail ci-dessous.

-   **`OnDetach()`** &ndash; Cette méthode est appelée juste avant que le Fragment n’est plus associé à l’activité. La hiérarchie d’affichage du Fragment n’existe plus, et toutes les ressources utilisées par le Fragment doivent être libérées à ce stade.


### <a name="using-setretaininstance"></a>À l’aide de SetRetainInstance

Il est possible pour un Fragment indiquer qu’il ne doit pas être complètement détruite si l’activité doit être recréée. Le `Fragment` classe fournit la méthode `SetRetainInstance` à cet effet. Si `true` est passé à cette méthode, lorsque l’activité est redémarrée, la même instance du Fragment sera utilisée. Si cela se produit, toutes les méthodes de rappel seront appelées à l’exception de la `OnCreate` et `OnDestroy` rappels de cycle de vie. Ce processus est illustré dans le diagramme de cycle de vie indiqué ci-dessus (par les lignes en pointillés vertes).


## <a name="fragment-state-management"></a>Gestion de l’état de fragment

Fragments de mai enregistrer et restaurer leur état pendant le cycle de vie du Fragment à l’aide d’une instance d’un `Bundle`. L’offre groupée permet un Fragment à enregistrer des données en tant que paires clé/valeur et est utile pour les données simples ne nécessitant pas de la quantité de mémoire. Un Fragment peut enregistrer son état avec un appel à `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Quand une nouvelle instance d’un Fragment est créé, l’état enregistré dans le `Bundle` devient disponible pour la nouvelle instance via le `OnCreate`, `OnCreateView`, et `OnActivityCreated` méthodes de la nouvelle instance.
L’exemple suivant montre comment récupérer la valeur `current_choice` à partir de la `Bundle`:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

Substitution de `OnSaveInstanceState` est un mécanisme approprié pour l’enregistrement des données temporaires dans un Fragment entre les changements d’orientation, telles que la `current_choice` valeur dans l’exemple ci-dessus. Toutefois, l’implémentation par défaut de `OnSaveInstanceState` s’occupe de l’enregistrement des données temporaires dans l’interface utilisateur pour chaque vue qui a un ID attribué. Par exemple, examinez une application qui a un `EditText` élément défini dans XML comme suit :

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Dans la mesure où le `EditText` contrôle a une `id` affecté, le Fragment enregistre automatiquement les données dans le widget lorsque `OnSaveInstanceState` est appelée.


### <a name="bundle-limitations"></a>Limitations de l’offre groupée

Bien que l’utilisation `OnSaveInstanceState` rend plus facile enregistrer les données temporaires, utilisation de cette méthode présente certaines limitations :

-  Si le Fragment n’est pas ajouté à la pile de retour, alors que son état n’est pas restauré lorsque l’utilisateur appuie sur le **retour** bouton.

-  Lorsque le groupe est utilisé pour enregistrer les données, ces données sont sérialisées. Cela peut entraîner des retards de traitement.


## <a name="contributing-to-the-menu"></a>Contribution au Menu

Fragments peuvent contribuer à des éléments au menu de leur activité d’hébergement.
Une activité gère tout d’abord les éléments de menu. Si l’activité n’a pas de gestionnaire, l’événement sera passé pour le Fragment, qui gérera puis.

Pour ajouter des éléments au menu de l’activité, un Fragment doit faire deux choses.
Tout d’abord, le Fragment doit implémenter la méthode `OnCreateOptionsMenu` et placer ses éléments dans le menu, comme indiqué dans le code suivant :

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

Le menu dans l’extrait de code précédent est agrandie dans le code XML suivant, situé dans le fichier `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

Ensuite, le Fragment doit appeler `SetHasOptionsMenu(true)`. L’appel à cette méthode avertit Android que le Fragment a des éléments de menu à contribuer au menu option. Sauf si l’appel à cette méthode est effectué, les éléments de menu pour le Fragment ne seront pas ajoutés au menu d’option de l’activité. En général, cela dans la méthode de cycle de vie `OnCreate()`, comme illustré dans l’extrait de code suivant :

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

L’écran suivant montre à quoi ressemblerait ce menu :

[![Capture d’écran de l’exemple d’application de courses mon affichage des éléments de menu](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
