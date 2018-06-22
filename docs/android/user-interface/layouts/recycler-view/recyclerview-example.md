---
title: Un exemple de base RecyclerView
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d5be838dcb5530ece76c3701d8fce10403622e8d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770610"
---
# <a name="a-basic-recyclerview-example"></a>Un exemple de base RecyclerView


Pour comprendre comment `RecyclerView` fonctionne dans une application classique, cette rubrique présente la [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) exemple d’application, un exemple de code simple qui utilise `RecyclerView` pour afficher une grande collection de photos : 

[![Deux captures d’écran d’une application RecyclerView qui utilise CardViews pour afficher les photos](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** utilise [CardView](~/android/user-interface/controls/card-view.md) pour implémenter chaque élément de la photographie dans le `RecyclerView` mise en page. Raison de `RecyclerView`d’avantages de performances, cet exemple d’application est en mesure de faire défiler rapidement une grande collection de photos sans heurts et sans délai détectable.


### <a name="an-example-data-source"></a>Une Source de données exemple

Dans cet exemple d’application, une source de données « album photo » (représenté par la `PhotoAlbum` classe) fournit `RecyclerView` avec le contenu de l’élément.
`PhotoAlbum` est une collection de photos avec des légendes ; Lorsque vous instanciez, vous obtenez une collection prêts à l’emploi de 32 photos :

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Chaque instance de la photo dans `PhotoAlbum` expose les propriétés qui vous permettent de lire l’ID de ressource d’image, `PhotoID`et sa chaîne de légende, `Caption`. La collection de photos est organisée de telle sorte que chaque photo est accessible par un indexeur. Par exemple, les lignes de code suivantes accéder à l’ID de ressource d’image et la légende pour la dixième photo dans la collection :

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` fournit également un `RandomSwap` méthode que vous pouvez appeler pour le remplacement de la première photo dans la collection avec une photo choisis au hasard ailleurs dans la collection :

```csharp
mPhotoAlbum.RandomSwap ();
```

Étant donné que les détails d’implémentation de `PhotoAlbum` ne sont pas pertinentes pour comprendre `RecyclerView`, le `PhotoAlbum` code source n’est pas présenté ici. Le code source `PhotoAlbum` est disponible à l’adresse [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) dans les [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) exemple d’application.


### <a name="layout-and-initialization"></a>Mise en page et l’initialisation

Le fichier de disposition, **Main.axml**, se compose d’un seul `RecyclerView` dans un `LinearLayout`:

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

Notez que vous devez utiliser le nom qualifié complet **android.support.v7.widget.RecyclerView** car `RecyclerView` est empaqueté dans une bibliothèque de prise en charge. Le `OnCreate` méthode `MainActivity` initialise cette disposition, instancie l’adaptateur et prépare la source de données sous-jacente :

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

2. Passe de la source de données d’albums photo au constructeur de la carte, `PhotoAlbumAdapter` (qui est défini plus loin dans ce guide). 
   Notez qu’il est considéré comme une meilleure pratique de passer de la source de données en tant que paramètre au constructeur de la carte. 

3. Obtient le `RecyclerView` à partir de la mise en page.

4. Se connecte à l’adaptateur dans le `RecyclerView` instance en appelant le `RecyclerView` `SetAdapter` méthode comme indiqué ci-dessus.

### <a name="layout-manager"></a>Gestionnaire de disposition

Chaque élément dans le `RecyclerView` est composée d’un `CardView` qui contient une image photo et une légende de la photo (détails sont traités dans le [détenteur de vue](#view-holder) section ci-dessous). Prédéfinis `LinearLayoutManager` est utilisé pour disposer chacune `CardView` dans une disposition de défilement verticale :

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Ce code se trouve dans l’activité principale `OnCreate` (méthode). Le constructeur pour le Gestionnaire de disposition requiert un *contexte*, donc le `MainActivity` est passé à l’aide de `this` comme indiqué ci-dessus.

Au lieu d’utiliser le le predefind `LinearLayoutManager`, vous pouvez incorporer dans un gestionnaire de disposition personnalisée qui affiche deux `CardView` éléments côte à côte, mise en œuvre d’un effet d’animation de procédures pour parcourir la collection de photos. Plus loin dans ce guide, vous verrez un exemple de modification de la disposition en échangeant dans un gestionnaire de mise en page différente.

<a name="view-holder" />

### <a name="view-holder"></a>Détenteur d’affichage

La classe d’affichage détenteur est appelée `PhotoViewHolder`. Chaque `PhotoViewHolder` instance conserve des références à la `ImageView` et `TextView` d’un élément de ligne, qui est disposé selon une `CardView` comme représentée ici :

[![Diagramme de CardView contenant un ImageView et le TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` dérive de `RecyclerView.ViewHolder` et contient des propriétés pour stocker les références à la `ImageView` et `TextView` indiqué dans le schéma ci-dessus.
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
Dans cet exemple de code, la `PhotoViewHolder` une référence à la vue des éléments parent est passé au constructeur (le `CardView`) qui `PhotoViewHolder` encapsule. Notez que vous toujours transférez le parent affichage d’un élément au constructeur de base. Le `PhotoViewHolder` appels de constructeurs `FindViewById` sur la vue des éléments parents pour chacun de ses références de la vue enfant, recherchez `ImageView` et `TextView`, stockant les résultats dans le `Image` et `Caption` propriétés, respectivement. L’adaptateur récupère ultérieurement afficher les références de ces propriétés lorsqu’il met à jour ce `CardView`de vues enfants avec de nouvelles données.

Pour plus d’informations sur `RecyclerView.ViewHolder`, consultez la [référence de classe RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adaptateur

L’adaptateur charge chacun `RecyclerView` ligne contenant des données pour une photo particulière. Pour une photo donnée à la position de ligne *P*, par exemple, l’adaptateur recherche les données associées à la position *P* au sein de la source de données et les copies de ces données à la ligne d’élément à la position *P* dans le `RecyclerView` collection. L’adaptateur utilise le détenteur de la vue pour rechercher les références pour le `ImageView` et `TextView` à cet emplacement afin qu’il ne doit pas nécessairement appeler `FindViewById` pour les vues que l’utilisateur fait défiler la collection de la photographie et réutilise les vues.

Dans **RecyclerViewer**, une classe d’adaptateur est dérivée de `RecyclerView.Adapter` créer `PhotoAlbumAdapter`:

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

Le `mPhotoAlbum` membre contient la source de données (l’album) qui est passée au constructeur ; le constructeur de copie de l’album dans cette variable de membre. Les éléments suivants requis `RecyclerView.Adapter` méthodes sont implémentées :

-   **`OnCreateViewHolder`** &ndash; Instancie le détenteur de fichier et affichage de mise en page élément.

-   **`OnBindViewHolder`** &ndash; Charge les données à la position spécifiée dans les vues dont les références sont stockés dans le détenteur de la vue donnée.

-   **`ItemCount`** &ndash; Retourne le nombre d’éléments dans la source de données.

Le Gestionnaire de disposition appelle ces méthodes pendant qu’il est positionner des éléments dans le `RecyclerView`. L’implémentation de ces méthodes est examinée dans les sections suivantes.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Les appels de gestionnaire de disposition `OnCreateViewHolder` lorsque la `RecyclerView` a besoin d’un nouveau titulaire d’affichage pour représenter un élément. `OnCreateViewHolder` augmente la vue des éléments à partir du fichier de disposition de l’affichage et encapsule la vue dans une nouvelle `PhotoViewHolder` instance. Le `PhotoViewHolder` constructeur localise et stocke les références aux vues enfants dans la disposition comme décrit précédemment dans [détenteur de la vue](#view-holder).

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

Cette disposition représente un élément de ligne unique dans le `RecyclerView`. Le `OnBindViewHolder` (décrites ci-dessous) copie des données à partir de la source de données dans le `ImageView` et `TextView` de cette mise en page.
`OnCreateViewHolder` augmente cette disposition pour un emplacement donné photo dans la `RecyclerView` et instancie une nouvelle `PhotoViewHolder` instance (qui recherche et met en cache des références à la `ImageView` et `TextView` vues enfants dans le type `CardView` mise en page) :

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

L’instance résultante détenteur de vue, `vh`, est retourné à l’appelant (le Gestionnaire de disposition).


#### <a name="onbindviewholder"></a>OnBindViewHolder

Lorsque le Gestionnaire de disposition est prêt à afficher une vue particulière dans le `RecyclerView`de zone de visible de l’écran, elle appelle l’adaptateur `OnBindViewHolder` méthode pour remplir l’élément à la position de la ligne spécifiée avec le contenu de la source de données. `OnBindViewHolder` Obtient les informations de la photo pour la position de la ligne spécifiée (ressource d’image de la photo et la chaîne de légende de la photo) et la copie de ces données dans les vues associées. Vues se trouvent via des références stockées dans l’objet de conteneur d’affichage (qui est passé par le `holder` paramètre) :

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

L’objet de conteneur passé dans la vue doit tout d’abord être converti dans le type de support affichage dérivé (dans ce cas, `PhotoViewHolder`) avant de les utiliser.
L’adaptateur charge la ressource image dans la vue référencée par le détenteur de vue `Image` propriété, qui copie le texte de légende dans la vue référencée par le détenteur de vue `Caption` propriété. Cela *lie* la vue associée avec ses données.

Notez que `OnBindViewHolder` est le code qui traite directement avec la structure des données. Dans ce cas, `OnBindViewHolder` comprend comment mapper le `RecyclerView` position à son élément de données associées dans la source de données d’élément. Le mappage est simple dans ce cas, car la position peut être utilisée comme index de tableau dans l’album ; Toutefois, les sources de données plus complexes peuvent nécessiter du code supplémentaire pour établir un mappage de ce type.


#### <a name="itemcount"></a>ItemCount

Le `ItemCount` méthode retourne le nombre d’éléments dans la collection de données. Dans l’exemple d’application de visionneuse de photos, le nombre d’éléments est le nombre de photos dans l’album :

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Pour plus d’informations sur `RecyclerView.Adapter`, consultez la [référence de classe RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Montage

Résultant `RecyclerView` implémentation pour l’exemple d’application photo se compose de `MainActivity` le code qui crée la source de données, Gestionnaire de disposition et l’adaptateur. `MainActivity` crée le `mRecyclerView` instance, instancie la source de données et l’adaptateur et s’intègre dans le Gestionnaire de disposition et la carte :

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

`PhotoViewHolder` recherche et met en cache les références de vue :

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

Lorsque ce code est compilé et exécuté, il crée la photo base affichage application, comme indiqué dans les captures d’écran suivants :

[![Deux captures d’écran de l’application avec défilement cartes photo d’affichage de photos](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Cette application de base prend uniquement en charge la navigation de l’album. Il ne répond pas aux événements élément tactile, ni il gère les modifications dans les données sous-jacentes. Cette fonctionnalité est ajoutée dans [pour étendre l’exemple RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).


### <a name="changing-the-layoutmanager"></a>Modification de la LayoutManager

Raison de `RecyclerView`de flexibilité, il est facile de modifier l’application pour utiliser un gestionnaire de mise en page différente. Dans l’exemple suivant, il est modifié pour afficher l’album avec une disposition de grille défile horizontalement, plutôt qu’avec une disposition linéaire verticale. Pour ce faire, l’instanciation de gestionnaire de disposition est modifiée pour utiliser le `GridLayoutManager` comme suit :

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Cette modification du code remplace vertical `LinearLayoutManager` avec un `GridLayoutManager` qui présente une grille composée de deux lignes de défilement dans le sens horizontal. Lorsque vous compilez et exécutez de nouveau l’application, vous verrez que les photos sont affichés dans une grille et que le défilement est horizontale et non verticale :

[![Capture d’écran de l’exemple d’application avec un défilement horizontal de photos dans une grille](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

En modifiant une seule ligne de code, est possible de modifier l’application d’affichage pour utiliser une autre disposition avec un comportement différent.
Notez que le code d’adaptateur, ni la structure XML a dû être modifiés pour changer le style de disposition. 

Dans la rubrique suivante, [pour étendre l’exemple RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), cet exemple de base d’application est étendue pour gérer les événements de clic de l’élément et de mettre à jour `RecyclerView` lorsque les modifications de source de données sous-jacentes.



## <a name="related-links"></a>Liens associés

- [RecyclerViewer (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Fonctionnalités et les parties RecyclerView](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Pour étendre l’exemple RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
