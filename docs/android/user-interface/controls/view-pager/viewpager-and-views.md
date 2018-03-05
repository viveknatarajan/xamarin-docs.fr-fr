---
title: ViewPager avec des vues
description: "ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation geste. Navigation geste permet à l’utilisateur pour le passage de gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter une interface utilisateur de swipeable avec ViewPager et PagerTabStrip, comme les pages de données à l’aide de vues (ultérieur guide couvre l’utilisation de Fragments pour les pages)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d81f897fb7af39334cec4ea9f806533f09754079
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="viewpager-with-views"></a>ViewPager avec des vues

_ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation geste. Navigation geste permet à l’utilisateur pour le passage de gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter une interface utilisateur de swipeable avec ViewPager et PagerTabStrip, comme les pages de données à l’aide de vues (ultérieur guide couvre l’utilisation de Fragments pour les pages)._

<a name="overview" />
 
## <a name="overview"></a>Vue d'ensemble

Ce guide est une procédure pas à pas qui présente une démonstration détaillée de l’utilisation de `ViewPager` pour implémenter une galerie d’images d’arbres à feuilles caduques et persistants. Dans cette application, l’utilisateur fait glisser sa gauche et droite via un catalogue « arborescence » pour afficher les images de l’arborescence. En haut de chaque page du catalogue, le nom de l’arborescence est répertorié dans un`PagerTabStrip`, et une image de l’arborescence est affichée dans un `ImageView`. Un adaptateur est utilisé à l’interface du `ViewPager` au modèle de données sous-jacent. Cette application implémente un adaptateur dérivé `PagerAdapter`. 

Bien que `ViewPager`-en fonction des applications sont souvent implémentées avec `Fragment`s, il existe certains scénarios d’utilisation relativement simple où la complexité supplémentaire de `Fragment`s n’est pas nécessaire. Par exemple, l’application de la galerie de base des images illustrée dans cette procédure pas à pas ne requiert pas l’utilisation de `Fragment`s. Étant donné que le contenu est statique et l’utilisateur uniquement Assistant numérique avec leur dans les deux sens entre les différentes images, l’implémentation peut être conservée plus simple à l’aide de la disposition et les vues Android standards. 


<a name="start" />

## <a name="start-an-app-project"></a>Démarrer un projet d’application

Créez un projet Android appelé **TreePager** (consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) pour plus d’informations sur la création de nouveaux projets Android). Ensuite, lancez le Gestionnaire de Package NuGet. (Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Rechercher et installer **bibliothèque de prise en charge Android v4**: 

[![Capture d’écran de prise en charge v4 Nuget est sélectionné dans le Gestionnaire de Package NuGet.](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png)

Il installe également tout reaquired de packages supplémentaires par **bibliothèque de prise en charge Android v4**.


<a name="datasource" />

## <a name="add-an-example-data-source"></a>Ajouter une Source de données exemple

Dans cet exemple, la source de données de catalogue arborescence (représenté par la `TreeCatalog` classe) fournit la `ViewPager` avec le contenu de l’élément. 
`TreeCatalog` contient une collection prêts à l’emploi des images de l’arborescence et des titres d’arborescence que l’adaptateur utilise pour la création de `View`s. Le `TreeCatalog` constructeur ne requiert aucun argument :

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

La collection d’images dans `TreeCatalog` est organisé de telle sorte que chaque image est accessible par un indexeur. Par exemple, la ligne de code suivante récupère l’ID de ressource d’image pour la troisième image dans la collection : 

```csharp
int imageId = treeCatalog[2].imageId;
```

Étant donné que les détails d’implémentation de `TreeCatalog` ne sont pas pertinentes pour comprendre `ViewPager`, le `TreeCatalog` code n’est pas répertorié ici. Le code source `TreeCatalog` est disponible à l’adresse [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Télécharger ce fichier source (ou copiez et collez le code dans un nouveau **TreeCatalog.cs** fichier) et l’ajouter à votre projet. En outre, téléchargez et décompressez le [fichiers image](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) dans votre **drawable/ressources** dossier et les inclure dans le projet. 


<a name="layout" />

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

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 


<a name="setup" />

## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

Remplacez la méthode `OnCreate` par le code suivant :

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Ce code effectue les opérations suivantes :

1.  Définit la vue à partir de la **Main.axml** des ressources de mise en page.

2.  Récupère une référence à la `ViewPager` à partir de la mise en page.

3.  Instancie une nouvelle `TreeCatalog` comme source de données.

Lorsque vous générez et exécutez ce code, vous devez voir un affichage semblable à la capture d’écran suivante : 

[![Capture d’écran de l’application affichant une ViewPager vide](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png)

À ce stade, le `ViewPager` est vide, car il lui manque un adaptateur pour l’accès au contenu dans **TreeCatalog**. Dans la section suivante, un **PagerAdapter** est créé pour connecter le `ViewPager` à la **TreeCatalog**. 


<a name="adapter" />

## <a name="create-the-adapter"></a>Créer l’adaptateur

`ViewPager` utilise un objet de contrôleur de l’adaptateur se situe entre le `ViewPager` et la source de données (voir l’illustration de [adaptateur](~/android/user-interface/controls/view-pager/index.md#adapter)). Pour accéder à ces données, `ViewPager` nécessite que vous fournissiez un adaptateur personnalisé dérivé de `PagerAdapter`. Cet adaptateur remplit chaque `ViewPager` page avec le contenu de la source de données. Étant donné que cette source de données est spécifique à l’application, l’adaptateur personnalisé est le code qui comprend comment accéder aux données. En tant que l’Assistant numérique avec leur utilisateur via les pages de la `ViewPager`, l’adaptateur extrait les informations de la source de données et les charge dans les pages de le `ViewPager` à afficher. 

Lorsque vous implémentez un `PagerAdapter`, vous devez substituer les éléments suivants :

-   **InstantiateItem** &ndash; crée la page (`View`) pour une position donnée et l’ajoute à la `ViewPager`de collection de vues. 

-   **DestroyItem** &ndash; supprime une page à partir d’une position donnée.

-   **Nombre de** &ndash; propriété en lecture seule qui renvoie le nombre de vues (pages) disponibles. 

-   **IsViewFromObject** &ndash; détermine si une page est associée à un objet de clé spécifique. (Cet objet est créé par le `InstantiateItem` méthode.) Dans cet exemple, l’objet clé est le `TreeCatalog` objet de données.

Ajouter un nouveau fichier appelé **TreePagerAdapter.cs** et remplacez son contenu par le code suivant : 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Ce code choisit essentielles `PagerAdapter` implémentation. Dans les sections suivantes, chacune de ces méthodes est remplacé par code de travail. 


<a name="ctor" />

### <a name="implement-the-constructor"></a>Implémentez le constructeur

Lorsque l’application instancie le `TreePagerAdapter`, il fournit un contexte (le `MainActivity`) et une instanciation `TreeCatalog`. Ajoutez les variables de membre suivantes et le constructeur en haut de la `TreePagerAdapter` classe dans **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

L’objectif de ce constructeur est de stocker le contexte et `TreeCatalog` de l’instance qui le `TreePagerAdapter` utilisera. 


<a name="count" />

### <a name="implement-count"></a>Nombre de mettre en œuvre

Le `Count` implémentation est relativement simple : il retourne le nombre d’arbres dans le catalogue de l’arborescence. Remplacez `Count` par le code suivant :

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

Le `NumTrees` propriété du `TreeCatalog` retourne le nombre d’arbres (nombre de pages) dans le jeu de données.


<a name="instantiateitem" />

### <a name="implement-instantiateitem"></a>Implémenter InstantiateItem

Le `InstantiateItem` méthode crée la page pour une position donnée. Il doit également ajouter la vue qui vient d’être créé pour le `ViewPager`d’afficher la collection. Pour ce faire, le `ViewPager` se transmet en tant que le paramètre de conteneur. 

Remplacez la méthode `InstantiateItem` par le code suivant :

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Ce code effectue les opérations suivantes :

1.  Instancie une nouvelle `ImageView` pour afficher l’image de l’arborescence à la position spécifiée. L’application `MainActivity` est le contexte qui sera passé à la `ImageView` constructeur.

2.  Définit le `ImageView` ressource à la `TreeCatalog` ID de ressource à la position spécifiée de l’image.

3.  Convertit le conteneur passé `View` à un `ViewPager` référence.
    Notez que vous devez utiliser `JavaCast<ViewPager>()` effectuer correctement ce cast (cela est nécessaire afin que Android effectue une conversion de type d’exécution d’une vérification).

4.  Ajoute l’instancié `ImageView` à la `ViewPager` et retourne le `ImageView` à l’appelant.

Lorsque le `ViewPager` affiche l’image à `position`, il affiche cette `ImageView`. Au départ, `InstantiateItem` est appelée deux fois pour remplir les deux premières pages avec des vues. Comme l’utilisateur fait défiler, elle est appelée à nouveau pour mettre à jour les vues juste derrière et avant l’élément actuellement affiché. 


<a name="destroyitem" />

### <a name="implement-destroyitem"></a>Implémenter DestroyItem

Le `DestroyItem` méthode supprime une page à partir de la position donnée. Dans les applications où la vue à n’importe quelle position donnée peut changer, `ViewPager` doit disposer d’un moyen de la suppression d’une vue obsolète à cette position avant de la remplacer par une nouvelle vue. Dans le `TreeCatalog` exemple, la vue à chaque position ne change pas, par conséquent, une vue est supprimé par `DestroyItem` simplement seront ajoutés de nouveau lorsque `InstantiateItem` est appelée pour cette position. (Pour plus d’efficacité, d’implémenter un pool à recycler `View`qui sera affiché de nouveau à la même position.) 

Remplacez la méthode `DestroyItem` par le code suivant : 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Ce code effectue les opérations suivantes :

1.  Convertit le conteneur passé `View` dans un `ViewPager` référence.

2.  Effectue un cast de l’objet Java passé (`view`) en c# `View` (`view as View`) ;

3.  Supprime la vue de la `ViewPager`. 


<a name="isviewfromobject" />

### <a name="implement-isviewfromobject"></a>Implémenter IsViewFromObject

Comme les diapositives utilisateur gauche et droite via les pages de contenu, `ViewPager` appelle `IsViewFromObject` pour vérifier que l’enfant `View` à la position donnée est associé à l’objet l’adaptateur de position identique (par conséquent, l’objet de l’adaptateur est appelée un *clé d’objet*). Pour les applications relativement simples, l’association est une identité &ndash; clé de l’objet de la carte à cette instance est la vue qui a été retournée précédemment à la `ViewPager` via `InstantiateItem`. Toutefois pour les autres applications, la clé d’objet peut être une autre instance de classe spécifique à l’adaptateur est associé (mais pas identique) permet d’afficher l’enfant qui `ViewPager` affiche à cette position. Seul l’adaptateur connaît ou non de la vue passé et la clé d’objet sont associées. 

`IsViewFromObject` doit être implémentée pour `PagerAdapter` pour fonctionner correctement. Si `IsViewFromObject` retourne `false` pour une position donnée, `ViewPager` n’affichera pas la vue à cette position. Dans le `TreePager` application, l’objet de clé retournée par `InstantiateItem` *est* la page `View` d’une arborescence, de sorte que le code a uniquement vérifier l’identité (c'est-à-dire, la clé d’objet et la vue sont les mêmes). Remplacez `IsViewFromObject` par le code suivant : 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

<a name="addadapter" />

## <a name="add-the-adapter-to-the-viewpager"></a>Ajouter l’adaptateur à la ViewPager

Maintenant que le `TreePagerAdapter` est implémentée, il est temps pour l’ajouter à la `ViewPager`. Dans **MainActivity.cs**, ajoutez la ligne de code suivante à la fin de la `OnCreate` méthode :

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Ce code instancie le `TreePagerAdapter`, en passant le `MainActivity` comme contexte (`this`). L’instanciation `TreeCatalog` a été passée dans l’argument du constructeur deuxième. Le `ViewPager`de `Adapter` est définie à l’instanciation `TreePagerAdapter` objet ; ce prises le `TreePagerAdapter` dans le `ViewPager`. 

L’implémentation de base est maintenant terminée &ndash; générer et exécuter l’application. Vous devez voir la première image du catalogue d’arborescence apparaît à l’écran comme indiqué sur la gauche dans la capture d’écran suivante. Faites défiler pour voir plusieurs vues de l’arborescence, puis balayez vers la droite pour reculer dans le catalogue de l’arborescence : 

[![Application des captures d’écran de TreePager passant par les images de l’arborescence](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png)


<a name="pagetabstrip" />

## <a name="add-a-pager-indicator"></a>Ajouter un indicateur de récepteur de radiomessagerie

Ce minimum `ViewPager` implémentation affiche les images du catalogue d’arborescence, mais il ne fournit aucune indication d’où l’utilisateur est dans le catalogue. L’étape suivante consiste à ajouter un `PagerTabStrip`. Le `PagerTabStrip` informe l’utilisateur à la page s’affiche et fournit le contexte de navigation en affichant un indicateur des pages précédents et suivant. `PagerTabStrip` est destinée à être utilisée en tant qu’indicateur de la page actuelle d’un `ViewPager`; il fait défiler et met à jour en tant que l’Assistant numérique avec leur utilisateur sur chaque page. 

Ouvrez **Resources/layout/Main.axml** et ajoutez un `PagerTabStrip` à la disposition :

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
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

`ViewPager` et `PagerTabStrip` sont conçus pour fonctionner ensemble. Lorsque vous déclarez un `PagerTabStrip` à l’intérieur d’un `ViewPager` mise en page, le `ViewPager` recherche automatiquement le `PagerTabStrip` et le connecter à l’adaptateur. Lorsque vous générez et exécutez l’application, vous devez voir le vide `PagerTabStrip` affiché en haut de chaque écran : 

[![Capture d’écran de la vue rapprochée d’un PagerTabStrip vide](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png)


<a name="title" />

### <a name="display-a-title"></a>Afficher un titre

Pour ajouter un titre pour chaque onglet de page, implémentez le `GetPageTitleFormatted` méthode dans le `PagerAdapter`-classe dérivée. `ViewPager` appels `GetPageTitleFormatted` (si implémenté) pour obtenir la chaîne de titre qui décrit la page à la position spécifiée. Ajoutez la méthode suivante à la `TreePagerAdapter` classe dans **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Ce code extrait la chaîne de légende d’arborescence de la page spécifiée (position) dans le catalogue de l’arborescence, les convertit en Java `String`et le retourne à la `ViewPager`. Lorsque vous exécutez l’application avec cette nouvelle méthode, chaque page affiche la légende de l’arborescence de la `PagerTabStrip`. Vous devez voir le nom de l’arborescence en haut de l’écran sans soulignement : 

[![Captures d’écran de pages contenant des onglets de PagerTabStrip remplie de texte](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png)

Vous pouvez passez en arrière pour afficher chaque image de l’arborescence de la légende dans le catalogue. 


<a name="pagertitlestrip" />

### <a name="pagertitlestrip-variation"></a>Variante de PagerTitleStrip

`PagerTitleStrip` est très similaire à `PagerTabStrip` , sauf que `PagerTabStrip` ajoute un trait de soulignement de l’onglet actuellement sélectionné. Vous pouvez remplacer `PagerTabStrip` avec `PagerTitleStrip` dans la disposition et l’exécution de l’application pour vérifier son fonctionnement avec ci-dessus `PagerTitleStrip`: 

[![PagerTitleStrip par des soulignements supprimées du texte](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png)

Notez que le soulignement est supprimé lorsque vous convertissez `PagerTitleStrip`. 


<a name="summary" />
 
## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas fourni un exemple de la création d’un base `ViewPager`-application en sans utiliser de `Fragment`s. Elle présente une source de données d’exemple contenant des images et des chaînes de la légende, un `ViewPager` mise en page pour afficher les images et un `PagerAdapter` sous-classe qui se connecte la `ViewPager` à la source de données. Pour aider l’utilisateur de naviguer dans le jeu de données, les instructions ont été incluses qui expliquent comment ajouter un `PagerTabStrip` ou `PagerTitleStrip` pour afficher la légende de l’image en haut de chaque page. 


## <a name="related-links"></a>Liens associés

- [TreePager (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
