---
title: ViewPager avec des Fragments
description: "ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation geste. Navigation geste permet à l’utilisateur pour le passage de gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter une interface utilisateur de swipeable avec ViewPager, comme les pages de données à l’aide de Fragments."
ms.topic: article
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: cd71617cce209ef0127023f69c2b503fee031e43
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-fragments"></a>ViewPager avec des Fragments

_ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation geste. Navigation geste permet à l’utilisateur pour le passage de gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter une interface utilisateur de swipeable avec ViewPager, comme les pages de données à l’aide de Fragments._

 
## <a name="overview"></a>Vue d'ensemble

`ViewPager` est souvent utilisé conjointement avec des fragments afin qu’il soit plus facile à gérer le cycle de vie de chaque page dans le `ViewPager`. Dans cette procédure pas à pas, `ViewPager` est utilisé pour créer un une application appelée **FlashCardPager** qui présente une série de problèmes mathématiques sur les cartes de flash. Chaque carte flash est implémenté comme un fragment. L’utilisateur fait glisser sa gauche et droite via les cartes flash et clique sur un problème mathématique pour afficher sa réponse. Cette application crée un `Fragment` instance pour chaque carte flash et le met en œuvre un adaptateur dérivé `FragmentPagerAdapter`. Dans [Viewpager et les vues](~/android/user-interface/controls/view-pager/viewpager-and-views.md), la plupart du travail a été effectuée `MainActivity` des méthodes de cycle de vie. Dans **FlashCardPager**, la plupart du travail est effectuée par un `Fragment` dans un de ses méthodes de cycle de vie. 

Ce guide ne couvre pas les principes fondamentaux de fragments &ndash; si vous n’êtes pas encore familiarisé avec des fragments de Xamarin.Android, consultez [Fragments](~/android/platform/fragments/index.md) pour vous aider à démarrer avec des fragments. 



## <a name="start-an-app-project"></a>Démarrer un projet d’application

Créez un projet Android appelé **FlashCardPager**. Ensuite, lancez le Gestionnaire de Package NuGet (pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Rechercher et installer le **Xamarin.Android.Support.v4** package comme expliqué dans [Viewpager et les vues](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 



## <a name="add-an-example-data-source"></a>Ajouter une Source de données exemple

Dans **FlashCardPager**, la source de données est un jeu de cartes flash représenté par le `FlashCardDeck` classe ; ces fournit la source de données la `ViewPager` avec le contenu de l’élément. `FlashCardDeck` contient une collection prêts à l’emploi des problèmes mathématiques et des réponses. Le `FlashCardDeck` constructeur ne requiert aucun argument : 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

La collection de cartes flash dans `FlashCardDeck` est organisé de telle sorte que chaque carte flash sont accessibles par un indexeur. Par exemple, la ligne de code suivante récupère le problème de carte flash quatrième jeu : 

```csharp
string problem = flashCardDeck[3].Problem;
```

Cette ligne de code récupère la réponse correspondante à la précédente d’un problème :

```csharp
string answer = flashCardDeck[3].Answer;
```

Étant donné que les détails d’implémentation de `FlashCardDeck` ne sont pas pertinentes pour comprendre `ViewPager`, le `FlashCardDeck` code n’est pas répertorié ici.
Le code source `FlashCardDeck` est disponible à l’adresse [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Télécharger ce fichier source (ou copiez et collez le code dans un nouveau **FlashCardDeck.cs** fichier) et l’ajouter à votre projet.



## <a name="create-a-viewpager-layout"></a>Créer une disposition de ViewPager

Ouvrez **Resources/layout/Main.axml** et remplacez son contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

Ce code XML définit un `ViewPager` qui occupe la totalité de l’écran. Notez que vous devez utiliser le nom qualifié complet **android.support.v4.view.ViewPager** car `ViewPager` est empaqueté dans une bibliothèque de prise en charge. `ViewPager` est disponible uniquement à partir de la [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); il n’est pas disponible dans le Kit de développement logiciel Android.


## <a name="set-up-viewpager"></a>Configurer ViewPager

Modifier **MainActivity.cs** et ajoutez le code suivant `using` instructions :

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Modifier la `MainActivity` déclaration de classe afin qu’elle est dérivée de `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` est dérivé de`FragmentActivity` (au lieu de `Activity`), car `FragmentActivity` sait comment gérer la prise en charge des fragments. Remplacez la méthode `OnCreate` par le code suivant : 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Ce code effectue les opérations suivantes :

1.  Définit la vue à partir de la **Main.axml** des ressources de mise en page.

2.  Récupère une référence à la `ViewPager` à partir de la mise en page.

3.  Instancie une nouvelle `FlashCardDeck` comme source de données.

Lorsque vous générez et exécutez ce code, vous devez voir un affichage semblable à la capture d’écran suivante : 

[![Application de capture d’écran de FlashCardPager avec ViewPager vide](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

À ce stade, le `ViewPager` est vide, car il lui manque les fragments qui servent remplir le `ViewPager`, et il lui manque un adaptateur pour la création de ces fragments à partir des données **FlashCardDeck**. 

Dans les sections suivantes, un `FlashCardFragment` est de créer et d’implémenter les fonctionnalités de chaque carte flash et un `FragmentPagerAdapter` est créé pour connecter le `ViewPager` pour les fragments créés à partir des données dans le `FlashCardDeck`. 



## <a name="create-the-fragment"></a>Créer le Fragment

Chaque carte flash seront géré par un fragment de l’interface utilisateur appelé `FlashCardFragment`. `FlashCardFragment`d’affichage affiche les informations de contenu avec une seule carte flash. Chaque instance de `FlashCardFragment` sera hébergée par le `ViewPager`. 
`FlashCardFragment`d’affichage se compose d’un `TextView` qui affiche le texte de problème de carte flash. Cette vue sera implémenter un gestionnaire d’événements qui utilise un `Toast` pour afficher la réponse quand l’utilisateur appuie sur la question de la carte flash. 



### <a name="create-the-flashcardfragment-layout"></a>Créer la disposition de FlashCardFragment

Avant de `FlashCardFragment` peut être implémentée, sa disposition doit être définie. Cette disposition est une mise en page du conteneur de fragment d’un fragment unique. Ajouter une nouvelle disposition Android à **ressources/disposition** appelé **flashcard_layout.axml**. Ouvrez **Resources/layout/flashcard_layout.axml** et remplacez son contenu par le code suivant : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

Cette disposition définit un fragment unique carte flash ; chaque fragment est composé d’un `TextView` qui affiche un problème mathématique à l’aide d’une police de grande taille (100sp). Ce texte est centré verticalement et horizontalement sur la carte flash. 



### <a name="create-the-initial-flashcardfragment-class"></a>Créer la classe FlashCardFragment initiale

Ajouter un nouveau fichier appelé **FlashCardFragment.cs** et remplacez son contenu par le code suivant :

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

Ce code choisit essentielles `Fragment` définition qui servira à afficher une carte flash. Notez que `FlashCardFragment` est dérivée de la version de bibliothèque de prise en charge des `Fragment` défini dans `Android.Support.V4.App.Fragment`. Le constructeur est vide pour que le `newInstance` méthode de fabrique est utilisé pour créer un nouveau `FlashCardFragment` au lieu d’un constructeur. 

Le `OnCreateView` du cycle de vie méthode crée et configure le `TextView`. Elle augmente la mise en page pour du fragment `TextView` et retourne l’augmentée `TextView` à l’appelant. `LayoutInflater` et `ViewGroup` sont passés à `OnCreateView` afin qu’il peut augmenter la mise en page. Le `savedInstanceState` groupe contient des données qui `OnCreateView` permet de recréer le `TextView` à partir d’un état enregistré. 

Vue de son est agrandie explicitement par l’appel à `inflater.Inflate`. Le `container` argument est le parent de la vue et le `false` indicateur ordonne à l’inflater à ne pas l’ajout de la vue augmentée au parent de la vue (, il est ajouté quand `ViewPager` l’adaptateur de l’appel `GetItem` méthode plus loin dans cette procédure pas à pas). 



### <a name="add-state-code-to-flashcardfragment"></a>Ajoutez le Code d’état à FlashCardFragment

Comme une activité, un fragment possède un `Bundle` qu’il utilise pour enregistrer et récupérer son état. Dans **FlashCardPager**, cette `Bundle` est utilisé pour enregistrer la question et la réponse de texte de la carte flash. Dans **FlashCardFragment.cs**, ajoutez le code suivant `Bundle` clés vers le haut de la `FlashCardFragment` définition de classe : 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Modifier la `newInstance` méthode de fabrique afin qu’elle crée un `Bundle` de l’objet et les clés ci-dessus pour stocker la question passée et répondre de texte dans le fragment après qu’il est instancié : 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

Modifier la méthode de cycle de vie du fragment `OnCreateView` pour récupérer ces informations à partir de l’offre groupée dans le passé et de charger le texte de la question dans la `TextBox`: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

Le `answer` variable n’est pas utilisée ici, mais elle sera utilisée ultérieurement lorsque le code du Gestionnaire d’événements est ajouté à ce fichier. 


## <a name="create-the-adapter"></a>Créer l’adaptateur

`ViewPager` utilise un objet de contrôleur de l’adaptateur se situe entre le `ViewPager` et la source de données (voir l’illustration de la ViewPager [adaptateur](~/android/user-interface/controls/view-pager/index.md#adapter) article). Pour accéder à ces données, `ViewPager` nécessite que vous fournissiez un adaptateur personnalisé dérivé de `PagerAdapter`. Étant donné que cet exemple utilise des fragments, il utilise un `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` est dérivée de `PagerAdapter`. 
`FragmentPagerAdapter` représente chaque page comme un `Fragment` qui est conservée persistante dans le Gestionnaire de fragment pour tant que l’utilisateur peut revenir à la page. En tant que l’Assistant numérique avec leur utilisateur via les pages de la `ViewPager`, le `FragmentPagerAdapter` extrait les informations de la source de données et l’utilise pour créer `Fragment`s pour le `ViewPager` à afficher. 

Lorsque vous implémentez un `FragmentPagerAdapter`, vous devez substituer les éléments suivants :

-   **Nombre de** &ndash; propriété en lecture seule qui renvoie le nombre de vues (pages) disponibles.

-   **GetItem** &ndash; retourne le fragment à afficher pour la page spécifiée.

Ajouter un nouveau fichier appelé **FlashCardDeckAdapter.cs** et remplacez son contenu par le code suivant :

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

Ce code choisit essentielles `FragmentPagerAdapter` implémentation. Dans les sections suivantes, chacune de ces méthodes est remplacé par code de travail. L’objectif du constructeur est de passer le Gestionnaire de fragment pour le `FlashCardDeckAdapter`du constructeur de classe de base. 



### <a name="implement-the-adapter-constructor"></a>Implémentez le constructeur d’adaptateur

Lorsque l’application instancie le `FlashCardDeckAdapter`, il fournit une référence au Gestionnaire de fragment et une instanciation `FlashCardDeck`. Ajoutez la variable de membre suivante en haut de la `FlashCardDeckAdapter` classe dans **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Ajoutez la ligne suivante de code pour le `FlashCardDeckAdapter` constructeur : 

```csharp
this.flashCardDeck = flashCards;
```

Cette ligne de code stocke le `FlashCardDeck` de l’instance qui le `FlashCardDeckAdapter` utilisera. 



### <a name="implement-count"></a>Nombre de mettre en œuvre

Le `Count` implémentation est relativement simple : il retourne le nombre de cartes flash dans le jeu de cartes flash. Remplacez `Count` par le code suivant : 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


Le `NumCards` propriété du `FlashCardDeck` retourne le nombre de cartes de flash (nombre de fragments) dans le jeu de données. 



### <a name="implement-getitem"></a>Implémenter GetItem

Le `GetItem` méthode retourne le fragment associé à la position donnée. Lorsque `GetItem` est appelée pour une position dans le jeu de cartes flash, elle retourne un `FlashCardFragment` configuré pour afficher le problème de carte flash à cette position. Remplacez la méthode `GetItem` par le code suivant : 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Ce code effectue les opérations suivantes :

1.  Recherche la chaîne de problème mathématiques dans la `FlashCardDeck` pont pour la position spécifiée. 

2.  Recherche la chaîne de réponse dans le `FlashCardDeck` pont pour la position spécifiée. 

3.  Appelle le `FlashCardFragment` méthode de fabrique `newInstance`, en passant les chaînes de problème et de réponse de carte flash. 

4.  Crée et retourne une nouvelle carte flash `Fragment` qui contient le texte de question et une réponse de cette position. 

Lorsque le `ViewPager` restitue le `Fragment` à `position`, il affiche la `TextBox` qui contient la chaîne de problème mathématique résidant à `position` dans le jeu de cartes flash. 



## <a name="add-the-adapter-to-the-viewpager"></a>Ajouter l’adaptateur à la ViewPager

Maintenant que le `FlashCardDeckAdapter` est implémentée, il est temps pour l’ajouter à la `ViewPager`. Dans **MainActivity.cs**, ajoutez la ligne de code suivante à la fin de la `OnCreate` méthode :

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Ce code instancie le `FlashCardDeckAdapter`, en passant le `SupportFragmentManager` dans le premier argument. (Le `SupportFragmentManager` propriété du FragmentActivity est utilisée pour obtenir une référence à la `FragmentManager` &ndash; pour plus d’informations sur la `FragmentManager`, consultez [Fragments de la gestion des](~/android/platform/fragments/managing-fragments.md).) 

L’implémentation de base est maintenant terminée &ndash; générer et exécuter l’application.
Vous devez voir la première image du pont carte flash apparaissent sur l’écran comme indiqué sur la gauche dans la capture d’écran suivante. Faites défiler pour voir les autres cartes flash, balayez vers la droite puis remettre en place via les cartes flash :

[![Captures d’écran de l’exemple d’application FlashCardPager sans indicateur de récepteur de radiomessagerie](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Ajouter un indicateur de récepteur de radiomessagerie

Ce minimum `ViewPager` implémentation affiche chaque carte flash dans le pont, mais il ne fournit aucune indication d’où l’utilisateur est dans le pont. L’étape suivante consiste à ajouter un `PagerTabStrip`. Le `PagerTabStrip` informe l’utilisateur à quel problème numéro s’affiche et fournit le contexte de navigation en affichant un indicateur des cartes flash précédents et suivant. 

Ouvrez **Resources/layout/Main.axml** et ajoutez un `PagerTabStrip` à la disposition :

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

  <android.support.v4.view.PagerTabStrip
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_gravity="top"
      android:paddingBottom="10dp"
      android:paddingTop="10dp"
      android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

Lorsque vous générez et exécutez l’application, vous devez voir le vide `PagerTabStrip` affiché en haut de chaque carte flash : 

[![Gros plan de PagerTabStrip sans texte](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Afficher un titre

Pour ajouter un titre pour chaque onglet de page, vous devez implémenter la `GetPageTitleFormatted` méthode dans l’adaptateur. `ViewPager` appels `GetPageTitleFormatted` (si implémenté) pour obtenir la chaîne de titre qui décrit la page à la position spécifiée. Ajoutez la méthode suivante à la `FlashCardDeckAdapter` classe dans **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Ce code convertit la position dans le jeu de cartes flash en un nombre de problème. La chaîne obtenue est convertie en Java `String` qui est retourné à le `ViewPager`. Lorsque vous exécutez l’application avec cette nouvelle méthode, chaque page affiche le nombre de problème dans le `PagerTabStrip`: 

[![Captures d’écran de FlashCardPager avec le numéro de problème affiché au-dessus de chaque page](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Vous pouvez faites dans les deux sens pour voir le numéro de problème dans le pont de carte flash qui s’affiche en haut de chaque carte flash. 



## <a name="handle-user-input"></a>Gérer l’entrée d’utilisateur

**FlashCardPager** présente une série de cartes flash fragment dans un `ViewPager`, mais il ne dispose pas encore d’un moyen pour afficher la réponse pour chaque problème. Dans cette section, un gestionnaire d’événements est ajouté à la `FlashCardFragment` pour afficher la réponse lorsque l’utilisateur appuie sur le texte de problème de carte flash. 

Ouvrez **FlashCardFragment.cs** et ajoutez le code suivant à la fin de la `OnCreateView` méthode juste avant que la vue est retournée à l’appelant : 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Cela `Click` Gestionnaire d’événements affiche la réponse dans un Toast s’affiche lorsque l’utilisateur appuie sur le `TextBox`. Le `answer` variable initialisée précédemment lorsque les informations d’état a été lu à partir de l’application qui a été passée à `OnCreateView`. Générer et exécuter l’application, puis cliquez sur le texte de problème sur chaque carte flash pour afficher la réponse : 

[![Application des captures d’écran de FlashCardPager Toasts lorsque drainé problème mathématique](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

Le **FlashCardPager** présentées dans cette procédure pas à pas utilise un `MainActivity` dérivé `FragmentActivity`, mais vous pouvez également dériver `MainActivity` de `AppCompatActivity` (qui prend également en charge la gestion des fragments). Pour afficher un `AppCompatActivity` exemple, consultez [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) dans la galerie d’exemples. 



## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas fourni un exemple de la création d’un base `ViewPager`-en fonction à l’aide de l’application `Fragment`s. Elle présente une source de données d’exemple contenant la carte flash questions et réponses, une `ViewPager` mise en page pour afficher les cartes flash et un `FragmentPagerAdapter` sous-classe qui se connecte la `ViewPager` à la source de données. Pour aider l’utilisateur de naviguer dans les cartes flash, les instructions ont été incluses qui expliquent comment ajouter un `PagerTabStrip` pour afficher le numéro du problème en haut de chaque page. Enfin, le code de gestion des événements a été ajouté pour afficher la réponse lorsque l’utilisateur appuie sur un problème de carte flash. 



## <a name="related-links"></a>Liens associés

- [FlashCardPager (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
