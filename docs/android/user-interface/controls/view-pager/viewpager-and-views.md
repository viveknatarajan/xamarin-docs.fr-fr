---
title: ViewPager avec des vues
description: ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation de geste. Navigation geste permet à l’utilisateur balayez gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter une interface utilisateur swipeable avec ViewPager et PagerTabStrip, à l’aide de vues en tant que les pages de données (un guide suivant explique comment utiliser des Fragments pour les pages).
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a8b7fa53d3384821d028e4a88ba22071a17e5bd9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61311567"
---
# <a name="viewpager-with-views"></a>ViewPager avec des vues

_ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation de geste. Navigation geste permet à l’utilisateur balayez gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter une interface utilisateur swipeable avec ViewPager et PagerTabStrip, à l’aide de vues en tant que les pages de données (un guide suivant explique comment utiliser des Fragments pour les pages)._

 
## <a name="overview"></a>Vue d'ensemble

Ce guide est une procédure pas à pas qui fournit une démonstration pas à pas comment utiliser `ViewPager` pour implémenter une galerie d’images d’arbres à feuilles caduques et persistants. Dans cette application, l’utilisateur fait glisser sa gauche et droite au moyen d’un catalogue « arborescence » pour afficher des images de l’arborescence. En haut de chaque page du catalogue, le nom de l’arborescence est répertorié dans un`PagerTabStrip`, et une image de l’arborescence est affichée dans un `ImageView`. Un adaptateur est utilisé pour l’interface du `ViewPager` au modèle de données sous-jacent. Cette application implémente un adaptateur dérivé `PagerAdapter`. 

Bien que `ViewPager`-applications sont souvent implémentées avec `Fragment`s, il existe certains scénarios d’utilisation relativement simple où la complexité supplémentaire de `Fragment`s n’est pas nécessaire. Par exemple, l’application de galerie d’image de base illustrée dans cette procédure pas à pas ne nécessite pas l’utilisation de `Fragment`s. Étant donné que le contenu est statique et l’utilisateur uniquement où les balayages dans les deux sens entre les différentes images, l’implémentation peut être conservée plus simple à l’aide de dispositions et vues Android standards. 



## <a name="start-an-app-project"></a>Démarrer un projet d’application

Créer un nouveau projet Android appelé **TreePager** (consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) pour plus d’informations sur la création de nouveaux projets Android). Ensuite, lancez le Gestionnaire de Package NuGet. (Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas : Inclusion d’un package NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Rechercher et installer **v4 de la bibliothèque de prise en charge Android**: 

[![Capture d’écran de prise en charge v4 Nuget sélectionné dans le Gestionnaire de Package NuGet.](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

Cette opération installera également n’importe quel reaquired des packages supplémentaires par **v4 de la bibliothèque de prise en charge Android**.



## <a name="add-an-example-data-source"></a>Ajouter une Source de données exemple

Dans cet exemple, la source de données de catalogue arborescence (représenté par le `TreeCatalog` classe) fournit la `ViewPager` avec le contenu de l’élément. 
`TreeCatalog` contient une collection prêtes à l’emploi d’images de l’arborescence et les titres d’arborescence que l’adaptateur utilise pour la création de `View`s. Le `TreeCatalog` constructeur ne requiert aucun argument :

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

La collection d’images dans `TreeCatalog` est organisé de telle sorte que chaque image est accessible par un indexeur. Par exemple, la ligne de code suivante récupère l’ID de ressource d’image pour la troisième image dans la collection : 

```csharp
int imageId = treeCatalog[2].imageId;
```

Étant donné que les détails d’implémentation de `TreeCatalog` ne sont pas pertinentes pour comprendre `ViewPager`, le `TreeCatalog` code n’est pas répertorié ici. Le code source à `TreeCatalog` est disponible à l’adresse [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Téléchargez ce fichier source (ou copiez et collez le code dans un nouveau **TreeCatalog.cs** fichier) et ajoutez-le à votre projet. En outre, téléchargez et décompressez le [fichiers image](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) dans votre **ressources/drawable** dossier et les inclure dans le projet. 



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

3.  Instancie un nouveau `TreeCatalog` comme source de données.

Lorsque vous générez et exécutez ce code, vous devez obtenir un affichage semblable à la capture d’écran suivante : 

[![Capture d’écran de l’application affichant un ViewPager vide](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

À ce stade, le `ViewPager` est vide, car il lui manque un adaptateur pour accéder au contenu dans **TreeCatalog**. Dans la section suivante, un **PagerAdapter** est créé pour connecter le `ViewPager` à la **TreeCatalog**. 


## <a name="create-the-adapter"></a>Créer l’adaptateur

`ViewPager` utilise un objet de contrôleur de l’adaptateur se situe entre la `ViewPager` et la source de données (voir l’illustration dans [adaptateur](~/android/user-interface/controls/view-pager/index.md#adapter)). Pour accéder à ces données, `ViewPager` nécessite que vous fournissiez un adaptateur personnalisé dérivé `PagerAdapter`. Cet adaptateur remplit chaque `ViewPager` page avec du contenu à partir de la source de données. Étant donné que cette source de données est spécifique à l’application, l’adaptateur personnalisé est le code qui comprend comment accéder aux données. Comme les où les balayages utilisateur via les pages de la `ViewPager`, l’adaptateur extrait les informations de la source de données et les charge dans les pages pour le `ViewPager` à afficher. 

Lorsque vous implémentez un `PagerAdapter`, vous devez substituer les éléments suivants :

-   **InstantiateItem** &ndash; crée la page (`View`) pour une position donnée et l’ajoute à la `ViewPager`de collection de vues. 

-   **DestroyItem** &ndash; supprime une page à partir d’une position donnée.

-   **Nombre** &ndash; propriété en lecture seule qui retourne le nombre de vues (pages) disponibles. 

-   **IsViewFromObject** &ndash; détermine si une page est associée à un objet de clé spécifique. (Cet objet est créé par le `InstantiateItem` (méthode).) Dans cet exemple, l’objet de clé est le `TreeCatalog` objet de données.

Ajoutez un nouveau fichier appelé **TreePagerAdapter.cs** et remplacez son contenu par le code suivant : 

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

Ce code écrase essentielles `PagerAdapter` implémentation. Dans les sections suivantes, chacune de ces méthodes est remplacé par code de travail. 



### <a name="implement-the-constructor"></a>Implémentez le constructeur

Lorsque l’application instancie le `TreePagerAdapter`, il fournit un contexte (le `MainActivity`) et une instanciation `TreeCatalog`. Ajoutez les variables de membre suivantes et le constructeur en haut de la `TreePagerAdapter` classe **TreePagerAdapter.cs**: 

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



### <a name="implement-count"></a>Nombre de mettre en œuvre

Le `Count` implémentation est relativement simple : il retourne le nombre d’arbres dans le catalogue de l’arborescence. Remplacez `Count` par le code suivant :

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

Le `NumTrees` propriété du `TreeCatalog` retourne le nombre d’arbres (nombre de pages) dans le jeu de données.



### <a name="implement-instantiateitem"></a>Implémenter InstantiateItem

Le `InstantiateItem` méthode crée la page pour une position donnée. Il doit également ajouter la vue qui vient d’être créé à le `ViewPager`d’afficher la collection. Pour permettre cela, le `ViewPager` se transmet en tant que le paramètre de conteneur. 

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

1.  Instancie un nouveau `ImageView` pour afficher l’image de l’arborescence à la position spécifiée. L’application `MainActivity` est le contexte qui sera passé à la `ImageView` constructeur.

2.  Définit le `ImageView` ressource à la `TreeCatalog` ID de ressource à la position spécifiée de l’image.

3.  Convertit le conteneur passé `View` à un `ViewPager` référence.
    Notez que vous devez utiliser `JavaCast<ViewPager>()` pour effectuer correctement ce cast (cela est nécessaire afin que Android effectue une conversion de type runtime-checked).

4.  Ajoute l’instancié `ImageView` à la `ViewPager` et retourne le `ImageView` à l’appelant.

Lorsque le `ViewPager` affiche l’image à `position`, il affiche cette `ImageView`. Initialement, `InstantiateItem` est appelée deux fois pour remplir les deux premières pages avec des vues. Comme l’utilisateur fait défiler, elle est appelée à nouveau pour mettre à jour les vues juste derrière et avant l’élément actuellement affiché. 



### <a name="implement-destroyitem"></a>Implémenter DestroyItem

Le `DestroyItem` méthode supprime une page à partir de la position donnée. Dans les applications où la vue à n’importe quelle position donnée peut changer, `ViewPager` doit avoir un moyen de la suppression d’une vue obsolète à cette position avant en la remplaçant par une nouvelle vue. Dans le `TreeCatalog` exemple, la vue à chaque position ne change pas, donc une vue supprimée par `DestroyItem` simplement seront ajoutés de nouveau lorsque `InstantiateItem` est appelée pour cette position. (Pour une meilleure efficacité, permettant d’implémenter un pool à recycler `View`s sera affiche à nouveau à la même position.) 

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

2.  Convertit l’objet Java passé (`view`) dans un C# `View` (`view as View`) ;

3.  Supprime la vue de la `ViewPager`. 



### <a name="implement-isviewfromobject"></a>Implémenter IsViewFromObject

Comme les diapositives utilisateur gauche et droite au moyen de pages de contenu, `ViewPager` appels `IsViewFromObject` pour vérifier que l’enfant `View` est à la position donnée est associé à l’objet l’adaptateur de position identique (par conséquent, un objet d’adaptateur est appelé un *clé d’objet*). Pour les applications relativement simples, l’association est une des identités &ndash; clé d’objet de l’adaptateur à cette instance est la vue qui a été retournée précédemment à la `ViewPager` via `InstantiateItem`. Toutefois pour les autres applications, la clé de l’objet peut être une autre instance de classe spécifique à l’adaptateur est associé (mais pas identique) permet d’afficher l’enfant qui `ViewPager` affiche à cette position. Seul l’adaptateur sait ou non la vue passé et la clé d’objet sont associés. 

`IsViewFromObject` doit être implémentée pour `PagerAdapter` pour fonctionner correctement. Si `IsViewFromObject` retourne `false` pour une position donnée, `ViewPager` n’affichera pas la vue à cette position. Dans le `TreePager` application, l’objet de clé retournée par `InstantiateItem` *est* la page `View` d’une arborescence de sorte que le code a uniquement vérifier les identités (autrement dit, la clé d’objet et la vue sont les mêmes). Remplacez `IsViewFromObject` par le code suivant : 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>Ajouter l’adaptateur à la ViewPager

Maintenant que le `TreePagerAdapter` est implémenté, il est temps pour l’ajouter à la `ViewPager`. Dans **MainActivity.cs**, ajoutez la ligne de code suivante à la fin de la `OnCreate` méthode :

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Ce code instancie le `TreePagerAdapter`, en passant le `MainActivity` comme contexte (`this`). L’instancié `TreeCatalog` est passé dans le deuxième argument du constructeur. Le `ViewPager`de `Adapter` propriété est définie à l’instanciation `TreePagerAdapter` objet ; ce s’intègre le `TreePagerAdapter` dans le `ViewPager`. 

L’implémentation de base est maintenant terminée &ndash; générer et exécuter l’application. Vous devez voir la première image du catalogue d’arborescence s’affichent sur l’écran, comme indiqué sur la gauche dans la capture d’écran suivante. Effectuez un balayage gauche pour afficher plusieurs vues de l’arborescence, puis balayez vers la droite pour vous déplacer dans via le catalogue de l’arborescence : 

[![Application des captures d’écran de TreePager passant au moyen d’images d’arborescence](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Ajouter un indicateur de radiomessagerie

Ce minimum `ViewPager` implémentation affiche les images du catalogue d’arborescence, mais il ne fournit aucune indication sur où l’utilisateur se trouve dans le catalogue. L’étape suivante consiste à ajouter un `PagerTabStrip`. Le `PagerTabStrip` informe l’utilisateur en tant que de page qui s’affiche et fournit le contexte de navigation en affichant un indicateur de pages précédent et suivant. `PagerTabStrip` est destiné à être utilisé comme indicateur pour la page active d’un `ViewPager`; il fait défiler et met à jour en tant que les où les balayages utilisateur sur chaque page. 

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

`ViewPager` et `PagerTabStrip` sont conçus pour fonctionner ensemble. Lorsque vous déclarez un `PagerTabStrip` à l’intérieur d’un `ViewPager` mise en page, le `ViewPager` recherche automatiquement le `PagerTabStrip` et connectez-le à la carte. Lorsque vous générez et exécutez l’application, vous devez voir le vide `PagerTabStrip` affiché en haut de chaque écran : 

[![Capture d’écran de la vue rapprochée d’un PagerTabStrip vide](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>Afficher un titre

Pour ajouter un titre à chaque onglet de page, implémentez le `GetPageTitleFormatted` méthode dans le `PagerAdapter`-classe dérivée. `ViewPager` appels `GetPageTitleFormatted` (si implémenté) pour obtenir la chaîne de titre qui décrit la page à la position spécifiée. Ajoutez la méthode suivante à la `TreePagerAdapter` classe **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Ce code récupère la chaîne de légende d’arborescence de la page spécifiée (position) dans le catalogue de l’arborescence, il convertit en un Java `String`et le retourne à la `ViewPager`. Lorsque vous exécutez l’application avec cette nouvelle méthode, chaque page affiche la légende de l’arborescence dans le `PagerTabStrip`. Vous devez voir le nom de l’arborescence en haut de l’écran sans soulignement : 

[![Captures d’écran de pages avec des onglets de PagerTabStrip remplie de texte](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Vous pouvez balayez en arrière pour afficher chaque image de l’arborescence de la légende dans le catalogue. 



### <a name="pagertitlestrip-variation"></a>Variante de PagerTitleStrip

`PagerTitleStrip` est très similaire à `PagerTabStrip` , à ceci près que `PagerTabStrip` ajoute un trait de soulignement pour l’onglet actuellement sélectionné. Vous pouvez remplacer `PagerTabStrip` avec `PagerTitleStrip` dans la disposition et l’exécution de l’application pour vérifier son fonctionnement avec ci-dessus `PagerTitleStrip`: 

[![PagerTitleStrip soulignements supprimé à partir du texte](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Notez que le trait de soulignement est supprimé lorsque vous convertissez en `PagerTitleStrip`. 


 
## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas fourni un exemple pas à pas montrant comment créer un base `ViewPager`-en fonction d’application sans utiliser `Fragment`s. Il affiche une source de données d’exemple contenant des images et des chaînes de légende, un `ViewPager` mise en page pour afficher les images et un `PagerAdapter` sous-classe qui se connecte le `ViewPager` à la source de données. Obtenir des instructions ont été incluses pour aider l’utilisateur à naviguer dans le jeu de données, qui expliquent comment ajouter un `PagerTabStrip` ou `PagerTitleStrip` pour afficher la légende de l’image en haut de chaque page. 


## <a name="related-links"></a>Liens associés

- [TreePager (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
