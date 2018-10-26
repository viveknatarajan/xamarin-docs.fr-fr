---
title: Un exemple de RecyclerView base
description: Un exemple d’application qui illustre l’utilisation de RecyclerView.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/30/2018
ms.openlocfilehash: 67bf38c11fd6e82e05b2a10704973ca10de03083
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108650"
---
# <a name="a-basic-recyclerview-example"></a>Un exemple de RecyclerView base

Pour comprendre comment `RecyclerView` fonctionne dans une application classique, cette rubrique explore la [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) exemple d’application, un exemple de code simple qui utilise `RecyclerView` pour afficher une grande collection de photos : 

[![Deux captures d’écran d’une application de RecyclerView qui utilise CardViews pour afficher les photos](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** utilise [CardView](~/android/user-interface/controls/card-view.md) pour implémenter chaque élément de la photographie dans le `RecyclerView` mise en page. Raison de `RecyclerView`d’avantages de performances, cet exemple d’application est en mesure de faire défiler rapidement une grande collection de photos sans heurts et sans délai perceptible.


### <a name="an-example-data-source"></a>Une Source de données exemple

Dans cet exemple d’application, une source de données « album photo » (représenté par le `PhotoAlbum` classe) fournit `RecyclerView` avec le contenu de l’élément.
`PhotoAlbum` est une collection de photos avec des sous-titres ; Lorsque vous instanciez, vous obtenez une collection prêtes à l’emploi de 32 photos :

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Chaque instance de la photo dans `PhotoAlbum` expose les propriétés qui vous permettent de lire l’ID de ressource image, `PhotoID`et sa chaîne de légende, `Caption`. La collection de photos est organisée de telle sorte que chaque photo est accessible par un indexeur. Par exemple, les lignes de code suivantes accéder à l’ID de ressource d’image et la légende pour la dixième photo dans la collection :

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` fournit également un `RandomSwap` méthode que vous pouvez appeler pour le remplacement de la première photo dans la collection avec une photo choisis au hasard ailleurs dans la collection :

```csharp
mPhotoAlbum.RandomSwap ();
```

Étant donné que les détails d’implémentation de `PhotoAlbum` ne sont pas pertinentes pour comprendre `RecyclerView`, le `PhotoAlbum` code source n’est pas présenté ici. Le code source à `PhotoAlbum` est disponible à l’adresse [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) dans le [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) exemple d’application.


### <a name="layout-and-initialization"></a>Mise en page et l’initialisation

Le fichier de disposition, **Main.axml**, se compose d’un seul `RecyclerView` au sein d’un `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Notez que vous devez utiliser le nom qualifié complet **android.support.v7.widget.RecyclerView** car `RecyclerView` est empaqueté dans une bibliothèque de prise en charge. Le `OnCreate` méthode de `MainActivity` initialise cette disposition, instancie l’adaptateur et prépare la source de données sous-jacente :

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Ce code effectue les opérations suivantes :

1. Instancie le `PhotoAlbum` source de données.

2. Passe au constructeur de la carte, la source de données d’album photo `PhotoAlbumAdapter` (qui est défini plus loin dans ce guide). 
   Notez qu’il est considéré comme une bonne pratique de passer de la source de données en tant que paramètre au constructeur de l’adaptateur. 

3. Obtient le `RecyclerView` à partir de la mise en page.

4. Se connecte à l’adaptateur dans le `RecyclerView` instance en appelant le `RecyclerView` `SetAdapter` méthode comme indiqué ci-dessus.

### <a name="layout-manager"></a>Gestionnaire de disposition

Chaque élément dans le `RecyclerView` se compose d’un `CardView` qui contient une image de photo et de la légende de la photo (détails sont traités dans le [détenteur de la vue](#view-holder) section ci-dessous). Prédéfinis `LinearLayoutManager` est utilisé pour présenter chacun `CardView` dans une disposition de défilement verticale :

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Ce code réside dans l’activité principale `OnCreate` (méthode). Le constructeur pour le Gestionnaire de disposition requiert un *contexte*, la `MainActivity` est passé à l’aide de `this` comme indiqué ci-dessus.

Au lieu d’utiliser le predefind `LinearLayoutManager`, vous pouvez incorporer dans un gestionnaire de disposition personnalisée qui affiche deux `CardView` éléments côte-à-côte, implémentation d’un effet d’animation déforment parcourir la collection de photos. Plus loin dans ce guide, vous verrez un exemple montrant comment modifier la disposition en échangeant dans un gestionnaire de mise en page différente.

<a name="view-holder" />

### <a name="view-holder"></a>Détenteur de la vue

La classe de détenteur de vue est appelée `PhotoViewHolder`. Chaque `PhotoViewHolder` instance conserve des références à la `ImageView` et `TextView` d’un élément de ligne, qui est disposé selon une `CardView` comme représentée ici :

[![Diagramme de CardView contenant un ImageView et le TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` dérive de `RecyclerView.ViewHolder` et contient des propriétés pour stocker les références à la `ImageView` et `TextView` illustrée à la disposition ci-dessus.
`PhotoViewHolder` se compose de deux propriétés et un constructeur :

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```
Dans cet exemple de code, le `PhotoViewHolder` une référence à la vue d’élément parent est passé au constructeur (le `CardView`) qui `PhotoViewHolder` encapsule. Notez que vous transférez toujours le parent affichage des éléments au constructeur de base. Le `PhotoViewHolder` constructeur appelle `FindViewById` sur la vue d’élément parent pour chacun de ses références de la vue enfant, localiser `ImageView` et `TextView`, stockant les résultats dans le `Image` et `Caption` propriétés, respectivement. L’adaptateur récupère ultérieurement d’afficher les références à partir de ces propriétés lorsqu’il met à jour cet `CardView`de vues enfants avec de nouvelles données.

Pour plus d’informations sur `RecyclerView.ViewHolder`, consultez le [référence de classe RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adaptateur

L’adaptateur charge chaque `RecyclerView` ligne contenant des données pour une photographie particulier. Pour une photo donnée à la position de ligne *P*, par exemple, l’adaptateur localise les données associées à la position *P* au sein de la source de données et copie ces données à la ligne d’élément à la position *P* dans le `RecyclerView` collection. L’adaptateur utilise le détenteur de la vue pour rechercher les références pour le `ImageView` et `TextView` à cette position afin qu’elle ait à appeler à plusieurs reprises `FindViewById` pour les vues que l’utilisateur parcourt la collection de photographie et réutilise les vues.

Dans **RecyclerViewer**, classe d’adaptateur est dérivée de `RecyclerView.Adapter` créer `PhotoAlbumAdapter`:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

Le `mPhotoAlbum` membre contient la source de données (l’album photo) qui est passée au constructeur ; le constructeur de copie de l’album photo dans cette variable de membre. Les éléments suivants requis `RecyclerView.Adapter` méthodes sont implémentées :

-   **`OnCreateViewHolder`** &ndash; Instancie le détenteur de fichier et affichage de disposition élément.

-   **`OnBindViewHolder`** &ndash; Charge les données à la position spécifiée dans les vues dont les références sont stockés dans le détenteur de la vue donnée.

-   **`ItemCount`** &ndash; Retourne le nombre d’éléments dans la source de données.

Le Gestionnaire de disposition appelle ces méthodes lorsqu’il positionne les éléments dans le `RecyclerView`. L’implémentation de ces méthodes est examinée dans les sections suivantes.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Les appels de gestionnaire de disposition `OnCreateViewHolder` lorsque la `RecyclerView` a besoin d’un nouveau titulaire d’affichage pour représenter un élément. `OnCreateViewHolder` augmente la vue des éléments à partir du fichier de disposition de la vue et encapsule la vue dans une nouvelle `PhotoViewHolder` instance. Le `PhotoViewHolder` constructeur localise et stocke des références à des vues enfants dans la mise en page comme décrit précédemment dans [détenteur de la vue](#view-holder).

Chaque élément de ligne est représenté par un `CardView` qui contient un `ImageView` (pour la photo) et un `TextView` (pour la légende). Cette disposition réside dans le fichier **PhotoCardView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Cette disposition représente un élément de ligne unique dans le `RecyclerView`. Le `OnBindViewHolder` (méthode) (décrit ci-dessous) copie les données à partir de la source de données dans le `ImageView` et `TextView` de cette disposition.
`OnCreateViewHolder` augmente cette disposition pour un emplacement de photo donnée dans le `RecyclerView` et instancie un nouveau `PhotoViewHolder` instance (qui localise et met en cache des références à la `ImageView` et `TextView` vues enfants dans associé `CardView` disposition) :

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

L’instance résultante de détenteur de vue, `vh`, est retourné à l’appelant (le Gestionnaire de disposition).


#### <a name="onbindviewholder"></a>OnBindViewHolder

Lorsque le Gestionnaire de disposition est prêt à afficher une vue particulière dans le `RecyclerView`de zone visible de l’écran, il appelle l’adaptateur `OnBindViewHolder` méthode pour remplir l’élément à la position de ligne spécifiée avec le contenu à partir de la source de données. `OnBindViewHolder` Obtient les informations de la photo pour la position de ligne spécifié (ressource d’image de la photo et la chaîne de légende de la photo) et copie ces données dans les vues associées. Vues se trouvent via les références stockées dans l’objet de conteneur de vue (ce qui est passé par le `holder` paramètre) :

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

L’objet de conteneur transmise dans la vue doit tout d’abord être converti dans le type de support affichage dérivé (dans ce cas, `PhotoViewHolder`) avant de les utiliser.
L’adaptateur charge la ressource d’image dans la vue référencée par le détenteur de vue `Image` propriété et il copie le texte de légende dans la vue référencée par le détenteur de vue `Caption` propriété. Cela *lie* la vue associée avec ses données.

Notez que `OnBindViewHolder` est le code qui traite directement avec la structure des données. Dans ce cas, `OnBindViewHolder` comprend comment mapper le `RecyclerView` position à son élément de données associées dans la source de données d’élément. Le mappage est simple dans ce cas, car la position peut être utilisée comme un index de tableau dans l’album photo ; Toutefois, les sources de données plus complexes peuvent nécessiter de code supplémentaire pour établir un mappage de ce type.


#### <a name="itemcount"></a>ItemCount

Le `ItemCount` méthode retourne le nombre d’éléments dans la collection de données. Dans l’exemple d’application de visionneuse de photos, le nombre d’éléments est le nombre de photos de l’album photo :

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Pour plus d’informations sur `RecyclerView.Adapter`, consultez le [référence de classe RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Assemblage toutes les

Résultant `RecyclerView` consistant en implémentation pour l’exemple d’application photo `MainActivity` code qui crée la source de données, Gestionnaire de disposition et l’adaptateur. `MainActivity` crée le `mRecyclerView` instance, instancie la source de données et l’adaptateur et s’intègre dans le Gestionnaire de disposition et la carte :

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` localise et met en cache les références de vue :

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` implémente les substitutions de méthode requis trois :

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

Lorsque ce code est compilé et exécuté, il crée la base photo affichage application comme indiqué dans les captures d’écran suivante :

[![Deux captures d’écran de l’application avec les cartes photo à défilement vertical d’affichage de photos](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Si les ombres ne sont pas dessinés (comme indiqué dans la capture d’écran ci-dessus), modifiez **Properties/Androidmanifest.XML** et ajoutez le paramètre d’attribut suivant à la `<application>` élément :

```xml
android:hardwareAccelerated="true"
```

Cette application de base prend uniquement en charge la navigation de l’album photo. Il ne répond pas aux événements tactiles élément, ni gère-t-il les modifications dans les données sous-jacentes. Cette fonctionnalité est ajoutée dans [étend l’exemple de RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).




### <a name="changing-the-layoutmanager"></a>Modification de la LayoutManager

Raison de `RecyclerView`de flexibilité, il est facile de modifier l’application pour utiliser un gestionnaire de mise en page différente. Dans l’exemple suivant, il est modifié pour afficher l’album photo avec une disposition de grille qui fait défiler horizontalement, plutôt qu’avec une disposition linéaire verticale. Pour ce faire, l’instanciation de gestionnaire de disposition est modifiée pour utiliser le `GridLayoutManager` comme suit :

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Cette modification du code remplace vertical `LinearLayoutManager` avec un `GridLayoutManager` qui présente une grille composée de deux lignes faire défiler dans le sens horizontal. Lorsque vous compilez et exécutez à nouveau l’application, vous verrez que les photos sont affichés dans une grille et que le défilement est horizontale et non verticale :

[![Capture d’écran de l’exemple d’application avec un défilement horizontal des photos dans une grille](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

En modifiant une seule ligne de code, est est possible de modifier l’application d’affichage pour utiliser une autre disposition avec un comportement différent.
Notez que ni le code d’adaptateur, ni la mise en page XML a dû être modifiés pour changer le style de disposition. 

Dans la rubrique suivante, [étend l’exemple de RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), cet exemple de base d’application est étendue pour gérer les événements de clic de l’élément et mettre à jour `RecyclerView` lorsque les modifications de source de données sous-jacentes.



## <a name="related-links"></a>Liens associés

- [RecyclerViewer (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Fonctionnalités et les parties de RecyclerView](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Extension de l’exemple de RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
