---
title: Galerie
ms.topic: article
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: da6815a073d93379c8564f3ff91023deb20b0d55
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="gallery"></a>Galerie

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) est un widget de disposition permet d’afficher les éléments dans une liste de défilement horizontal et positionne la sélection actuelle dans le centre de la vue.

> [!IMPORTANT]
> Ce widget a été déconseillé dans Android 4.1 (API niveau 16). 

Dans ce didacticiel, vous allez créer une galerie de photos, puis affichez un message toast à chaque fois qu’un élément de la galerie est sélectionné.

Après le `Main.axml` mise en page est défini pour l’affichage du contenu, le `Gallery` est capturée à partir de la mise en page avec [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
Le [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) propriété est ensuite utilisée pour définir un adaptateur personnalisé ( `ImageAdapter`) comme source pour tous les éléments à afficher dans le dallery. Le `ImageAdapter` est créé à l’étape suivante.

Pour faire en cas de clic sur un élément dans la galerie, un délégué anonyme est abonné à la [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) événement. Il montre un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) qui affiche la position d’index (base zéro) de l’élément de sélectionnés (dans un scénario réel, la position pourrait servir à obtenir l’image de la taille complète pour une autre tâche).

Tout d’abord, il existe des quelques variables membres, y compris un tableau d’ID qui font référence aux images enregistrées dans le répertoire resources drawable (**drawable/ressources**).

Suivant est le constructeur de classe, où le [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) pour un `ImageAdapter` instance est définie et enregistrée dans un champ local.
Ensuite, il implémente certaines méthodes requises héritées [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).
Le constructeur et le [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) propriété sont explicites. Normalement, [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/) doit retourner l’objet réel, à la position spécifiée dans l’adaptateur, mais elle est ignorée pour cet exemple. De même, [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/) doit retourner l’id de ligne de l’élément, mais il n’est pas nécessaire ici.

La méthode effectue le travail pour appliquer une image à un [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) qui seront incorporés dans les [ `Gallery` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) dans cette méthode, le membre [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) est permet de créer un nouveau [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).
Le [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) est préparée en appliquant une image à partir du tableau local de ressources drawable, en définissant le [ `Gallery.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/) hauteur et la largeur de l’image, la définition de l’échelle en fonction de la [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) dimensions, puis enfin en définissant l’arrière-plan d’utiliser l’attribut styleable acquis dans le constructeur.

Consultez [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) pour une autre image, options de mise à l’échelle.

## <a name="walkthrough"></a>Procédure pas à pas

Démarrer un nouveau projet nommé *HelloGallery*.

![Capture d’écran de projet Android dans la boîte de dialogue Nouvelle Solution](gallery-images/hellogallery1.png)

Rechercher des photos que vous souhaitez utiliser, ou [télécharger ces exemples d’images](http://developer.android.com/shareables/sample_images.zip).
Ajoutez les fichiers image dans le fichier **ressources/Drawable** active. Dans le **propriétés** fenêtre, définir l’Action de génération pour chacune d’elles pour **AndroidResource**.

Ouvrez **Resources/Layout/Main.axml** et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Ouvrez `MainActivity.cs` et insérez le code suivant pour le [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) méthode :

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Créez une classe appelée `ImageAdapter` qui sous-classe [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

    public override Java.Lang.Object GetItem (int position)
    {
          return null;
    }

    public override long GetItemId (int position)
    {
          return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

Exécutez l'application. Il doit ressembler à la capture d’écran ci-dessous :

![Capture d’écran de HelloGallery afficher des exemples d’images](gallery-images/hellogallery3.png)



## <a name="references"></a>Références

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).


