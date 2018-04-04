---
title: Pour étendre l’exemple RecyclerView
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 83147261a2d5458272f7e2bc105154da4308f4b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="extending-the-recyclerview-example"></a>Pour étendre l’exemple RecyclerView


L’application de base décrits dans [exemple RecyclerView A](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) réellement ne fait pas grand-chose &ndash; simplement défile elle affiche une liste fixe d’éléments de photographie pour faciliter la navigation. Dans les applications réelles, les utilisateurs souhaitent pouvoir interagir avec l’application en appuyant sur les éléments dans l’affichage. En outre, la source de données sous-jacente peut modifier (ou être modifiée par l’application), et le contenu de l’affichage doit rester cohérent avec ces modifications. Dans les sections suivantes, vous allez apprendre à gérer les événements de clic de l’élément et de mettre à jour `RecyclerView` lorsque les modifications de source de données sous-jacentes.


### <a name="handling-item-click-events"></a>La gestion des événements de clic de l’élément

Lorsqu’un utilisateur touche un élément dans le `RecyclerView`, un événement de clic de l’élément est généré pour avertir l’application, ce qui concerne l’élément a été couvertes. Cet événement n’est pas généré par `RecyclerView` &ndash; au lieu de cela, la vue de l’élément (qui est inclue dans le détenteur de vue) détecte les fonctions tactiles et signale ces fonctions tactiles comme événements de clic.

Pour illustrer comment gérer les événements de clic de l’élément, les étapes suivantes expliquent comment l’application de photos-affichage de base est modifiée au rapport qui photographie avait été couvertes par l’utilisateur. Lorsqu’un événement de clic de l’élément se produit dans l’exemple d’application, la séquence suivante se produit :

1.  De la photographie `CardView` détecte l’événement click de l’élément et indique à l’adaptateur.

2.  L’adaptateur transmet l’événement (avec les informations de position d’élément) au Gestionnaire de clic de l’élément de l’activité.

3.  Gestionnaire de clic de l’élément de l’activité répond à l’événement click de l’élément.

Tout d’abord, un membre de gestionnaire d’événements appelé `ItemClick` est ajouté à la `PhotoAlbumAdapter` définition de classe :

```csharp
public event EventHandler<int> ItemClick;
```

Ensuite, une méthode de gestionnaire d’événements click de l’élément est ajoutée à `MainActivity`.
Ce gestionnaire affiche brièvement un toast qui indique quel élément de la photographie a été couvertes :

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Ensuite, une ligne de code est nécessaire pour inscrire le `OnItemClick` gestionnaire avec `PhotoAlbumAdapter`. Pour cela, vous pouvez immédiatement après `PhotoAlbumAdapter` est créé (dans l’activité principale `OnCreate` (méthode)) :

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` allez maintenant appeler `OnItemClick` lorsqu’elle reçoit un événement de clic de l’élément. L’étape suivante consiste à créer un gestionnaire dans l’adaptateur qui déclenche cette `ItemClick` événement. La méthode suivante, `OnClick`, est ajouté immédiatement après l’adaptateur `ItemCount` méthode :

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Cela `OnClick` méthode est l’adaptateur *écouteur* pour les événements de clic de l’élément à partir des vues de l’élément. Avant cet écouteur peut être inscrit avec une vue d’article (via le détenteur de vue de la vue item), le `PhotoViewHolder` constructeur doit être modifié pour accepter cette méthode comme un argument supplémentaire et inscrire `OnClick` avec la vue de l’élément `Click` événement.
Voici le texte modifié `PhotoViewHolder` constructeur :

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Le `itemView` paramètre contient une référence à la `CardView` qui a été couvertes par l’utilisateur. Notez que la classe de base titulaire vue connaît la position de l’élément (`CardView`) qu’elle représente (via la `LayoutPosition` propriété), et cette position est transmise à l’adaptateur `OnClick` méthode lorsqu’un événement de l’élément par clic se produit. L’adaptateur `OnCreateViewHolder` méthode est modifiée pour passer de l’adaptateur `OnClick` méthode au constructeur de l’exploitant vue :

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Maintenant lorsque vous générez et exécutez l’exemple d’application Affichage, en appuyant sur une photo dans l’affichage entraîne un toast à afficher que les rapports qui photographie a été couvertes :

[![Exemple Toast qui apparaît lorsqu’une carte de la photo est cliqué.](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Cet exemple montre qu’une seule approche pour implémenter des gestionnaires d’événements avec `RecyclerView`. Une autre approche qui peut être utilisée ici consiste à placer des événements sur le détenteur de l’affichage et de disposer de l’adaptateur s’abonner à ces événements. Si l’exemple d’application photo fourni une capacité d’édition de photos, des événements distincts seraient nécessaires pour le `ImageView` et `TextView` dans chaque `CardView`: aborde la `TextView` lance une `EditView` boîte de dialogue qui permet à l’utilisateur de modifier la légende et des touches finales sur le `ImageView` lance un outil retouches photo qui permet à l’utilisateur rogner ou faire pivoter la photo. Selon les besoins de votre application, vous devez concevoir la meilleure approche pour la gestion et de réponse aux événements tactiles.

Pour illustrer comment `RecyclerView` peuvent être mis à jour lorsque les modifications du jeu de données, l’exemple d’application affichage peuvent être modifiées de façon aléatoire, choisissez une photo dans la source de données et remplacez la première photo. Tout d’abord, un **Pick aléatoire** bouton est ajouté à l’application exemple photo **Main.axml** disposition :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Ensuite, le code est ajouté à la fin de l’activité principale `OnCreate` méthode pour localiser le `Random Pick` bouton de la mise en page et attacher un gestionnaire :

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Ce gestionnaire appelle l’album `RandomSwap` méthode lors de la **Pick aléatoire** bouton. Le `RandomSwap` méthode aléatoirement permute une photo avec la première photo dans la source de données, puis retourne l’index de la photo échangés de façon aléatoire. Lorsque vous compilez et exécutez l’exemple d’application avec ce code, en touchant le **Pick aléatoire** bouton n’entraîne pas une modification de l’affichage, car le `RecyclerView` n’a pas connaissance de la modification de la source de données.

Pour conserver `RecyclerView` mis à jour après la source de données change, la **Pick aléatoire** cliquez sur Gestionnaire doit être modifiée pour appeler l’adaptateur `NotifyItemChanged` méthode pour chaque élément dans la collection a changé (dans ce cas, deux articles ont modifié : la première photo et la photo permutée). Cela entraîne `RecyclerView` pour mettre à jour son affichage afin qu’il soit cohérent avec le nouvel état de la source de données :

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Désormais, lorsque le **Pick aléatoire** bouton drainé, `RecyclerView` met à jour l’affichage pour afficher qu’une photo plus bas dans la collection ont été permutée avec la première photo dans la collection :

[![Capture d’écran de première avant la permutation, la capture d’écran seconde après l’échange](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Bien entendu, `NotifyDataSetChanged` aurait pu être appelé au lieu d’effectuer les deux appels à `NotifyItemChanged`, mais cela forcera `RecyclerView` pour actualiser la collection entière, même si seulement deux éléments de la collection avaient changé. Appel de `NotifyItemChanged` est beaucoup plus efficace que l’appel `NotifyDataSetChanged`.


## <a name="related-links"></a>Liens associés

- [RecyclerViewer (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Fonctionnalités et les parties RecyclerView](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Un exemple de base RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
