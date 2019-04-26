---
title: Extension de l’exemple de RecyclerView
description: Ajout de gestionnaires d’événements click de l’élément à l’application d’exemple de RecyclerView.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: eca0f58a470228ce8e6331defe88c1ef727cef57
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036099"
---
# <a name="extending-the-recyclerview-example"></a>Extension de l’exemple de RecyclerView


L’application de base décrit dans [un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) réellement ne fait pas grand-chose &ndash; simplement fait défiler elle affiche une liste fixe d’éléments de photographie pour faciliter la navigation. Dans les applications réelles, les utilisateurs souhaitent être en mesure d’interagir avec l’application en appuyant sur les éléments dans l’affichage. En outre, la source de données sous-jacente peut changer (ou être modifiée par l’application), et le contenu de l’affichage doit rester cohérent avec ces modifications. Dans les sections suivantes, vous allez apprendre à gérer les événements de clic de l’élément et mettre à jour `RecyclerView` lorsque les modifications de source de données sous-jacentes.


### <a name="handling-item-click-events"></a>Gestion des événements de clic de l’élément

Quand un utilisateur touche un élément dans le `RecyclerView`, un événement de clic de l’élément est généré pour notifier l’application quel élément a été touché. Cet événement n’est pas généré par `RecyclerView` &ndash; au lieu de cela, la vue item (lequel est encapsulée dans le détenteur de la vue) détecte les contacts tactiles et signale ces touches comme événements de clic.

Pour illustrer comment gérer les événements de clic de l’élément, les étapes suivantes expliquent comment l’application photo-affichage de base est remplacée par le photographie avait été couvertes par l’utilisateur de rapport. Lorsqu’un événement de clic de l’élément se produit dans l’exemple d’application, la séquence suivante se produit :

1.  La photographie `CardView` détecte l’événement click de l’élément et indique à l’adaptateur.

2.  L’adaptateur transmet l’événement (avec les informations de position d’élément) au Gestionnaire de clic de l’élément de l’activité.

3.  Gestionnaire de clic de l’élément de l’activité répond à l’événement click de l’élément.

Tout d’abord, un membre de gestionnaire d’événement appelé `ItemClick` est ajouté à la `PhotoAlbumAdapter` définition de classe :

```csharp
public event EventHandler<int> ItemClick;
```

Ensuite, une méthode de gestionnaire d’événements clic de l’élément est ajoutée à `MainActivity`.
Ce gestionnaire affiche brièvement un toast qui indique quel élément de la photographie a été touchée :

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Ensuite, une ligne de code est nécessaire pour inscrire le `OnItemClick` gestionnaire avec `PhotoAlbumAdapter`. Pour ce faire, vous est immédiatement après `PhotoAlbumAdapter` est créé : 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

Dans cet exemple de base, l’inscription du gestionnaire a lieu dans l’activité principale `OnCreate` (méthode), mais une application de production peut inscrire le gestionnaire dans `OnResume` et annuler son inscription dans `OnPause` &ndash; consultez [cycle de vie des activités ](~/android/app-fundamentals/activity-lifecycle/index.md) pour plus d’informations.

`PhotoAlbumAdapter` appelle maintenant `OnItemClick` lorsqu’elle reçoit un événement de clic de l’élément. L’étape suivante consiste à créer un gestionnaire dans l’adaptateur qui déclenche ce `ItemClick` événement. La méthode suivante, `OnClick`, est ajouté directement après l’adaptateur `ItemCount` méthode :

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Cela `OnClick` méthode est l’adaptateur *écouteur* pour les événements de clic de l’élément à partir des vues de l’élément. Avant cet écouteur peut être inscrit avec une vue de l’élément (via le détenteur de vue de la vue de l’élément), le `PhotoViewHolder` constructeur doit être modifié pour accepter cette méthode comme un argument supplémentaire, puis inscrire `OnClick` avec la vue item `Click` événement.
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

Le `itemView` paramètre contient une référence à la `CardView` qui a été touchée par l’utilisateur. Notez que la classe de base de détenteur de vue sait la position de la disposition de l’élément (`CardView`) qu’elle représente (via le `LayoutPosition` propriété), et cette position est transmise à l’adaptateur `OnClick` méthode lorsqu’un événement de clic de l’élément a lieu. L’adaptateur `OnCreateViewHolder` méthode est modifiée pour passer de la carte `OnClick` méthode au constructeur de la vue du titulaire du :

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Maintenant lorsque vous générez et exécutez l’exemple d’application photo-affichage, en appuyant sur une photo dans l’affichage entraîne un toast apparaisse qui signale le photographie a été touchée :

[![Exemple Toast qui apparaît lorsqu’une carte de la photo est activé par un clic](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Cet exemple montre qu’une seule approche pour l’implémentation des gestionnaires d’événements avec `RecyclerView`. Une autre approche qui peut être utilisée ici consiste à placer des événements sur le détenteur de la vue et d’avoir à l’adaptateur de s’abonner à ces événements. Si l’exemple d’application photo fourni une fonctionnalité d’édition de photos, des événements distincts seraient nécessaires pour le `ImageView` et `TextView` au sein de chaque `CardView`: aborde le `TextView` lancerait une `EditView` boîte de dialogue qui permet à l’utilisateur de modifier la légende et a un impact sur le `ImageView` lancerait un outil de retouche photo qui permet à l’utilisateur rogner ou faire pivoter la photo. Selon les besoins de votre application, vous devez concevoir la meilleure approche pour gérer et répondre aux événements tactiles.

Pour illustrer comment `RecyclerView` peuvent être mis à jour lorsque les modifications de jeu de données, l’exemple d’application photo-affichage peuvent être modifiées pour choisir une photo dans la source de données et d’échanger avec la première photo de manière aléatoire. Tout d’abord, un **choisir aléatoire** bouton est ajouté à l’application d’exemple photo **Main.axml** disposition :

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

Ensuite, le code est ajouté à la fin de l’activité principale `OnCreate` méthode à rechercher le `Random Pick` bouton de la disposition et attacher un gestionnaire à celui-ci :

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

Ce gestionnaire appelle l’album photo `RandomSwap` méthode lorsque le **choisir aléatoire** l’appui sur bouton. Le `RandomSwap` méthode aléatoirement permute une photo avec la première photo dans la source de données, puis retourne l’index de la photo échanger de manière aléatoire. Lorsque vous compilez et exécutez l’exemple d’application avec ce code, en appuyant sur la **choisir aléatoire** bouton n’entraîne pas une modification de l’affichage, car le `RecyclerView` n’a pas connaissance de la modification de la source de données.

Pour conserver `RecyclerView` mis à jour une fois que la source de données change, la **choisir aléatoire** cliquez sur Gestionnaire doit être modifié pour appeler l’adaptateur `NotifyItemChanged` méthode pour chaque élément dans la collection a changé (dans ce cas, deux articles ont modifié : la photo de première et la photo échangée). Cela entraîne `RecyclerView` pour mettre à jour son affichage afin qu’il soit cohérent avec le nouvel état de la source de données :

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

Désormais, lorsque le **choisir aléatoire** l’appui sur bouton, `RecyclerView` met à jour l’affichage pour montrer qu’une photo plus bas dans la collection a été permutée avec la première photo dans la collection :

[![Première capture d’écran avant la permutation, la deuxième capture d’écran après la permutation](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Bien sûr, `NotifyDataSetChanged` aurait pu être appelé au lieu de faire les deux appels à `NotifyItemChanged`, mais cela forcera `RecyclerView` pour actualiser la collection entière, même si seulement deux éléments de la collection avaient changé. Appel `NotifyItemChanged` est beaucoup plus efficace que l’appel `NotifyDataSetChanged`.


## <a name="related-links"></a>Liens associés

- [RecyclerViewer (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Fonctionnalités et les parties de RecyclerView](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Un exemple de RecyclerView base](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
