---
title: À l’aide de contentprovider des Contacts
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 95d11ef692ec8b43c128cb55a21d0973151cd24a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120435"
---
# <a name="using-the-contacts-contentprovider"></a>À l’aide de contentprovider des Contacts

Code qu’utilise accéder aux données exposées par un `ContentProvider` ne nécessite pas une référence à la `ContentProvider` classe du tout. Au lieu de cela, un Uri est utilisé pour créer un curseur sur les données exposées par le `ContentProvider`. Android utilise l’Uri pour rechercher le système pour l’application qui expose un `ContentProvider` avec cet identificateur. L’Uri est une chaîne, généralement dans un format DNS inversé comme `com.android.contacts/data`.

Plutôt que d’effectuer les développeurs n’oubliez pas de cette chaîne, l’Android *Contacts* fournisseur expose ses métadonnées dans la `android.provider.ContactsContract` classe. Cette classe est utilisée pour déterminer l’Uri de le `ContentProvider` , ainsi que les noms des tables et des colonnes qui peuvent être interrogées.

Certains types de données requièrent également spécial autorisé à y accéder. La liste de contacts intégrée nécessite la `android.permission.READ_CONTACTS` autorisation dans le **AndroidManifest.xml** fichier.

Il existe trois façons de créer un curseur à partir de l’Uri :

1. **ManagedQuery()** &ndash; la meilleure approche dans Android 2.3 (API niveau 10) et versions antérieures, un `ManagedQuery` retourne un curseur et gère également automatiquement l’actualisation des données et la fermeture du curseur. Cette méthode est déconseillée dans Android 3.0 (API niveau 11).

1. **ContentResolver.Query()** &ndash; retourne un curseur non managé, ce qui signifie qu’il doit être actualisé et fermé explicitement dans le code.

1. **CursorLoader(). LoadInBackground()** &ndash; introduit dans 3.0 Android API niveau 11, `CursorLoader` est désormais le meilleur moyen de consommer un `ContentProvider` . `CursorLoader` requêtes un `ContentResolver` sur un thread d’arrière-plan pour l’interface utilisateur n’est pas bloqué.
   Cette classe est accessible dans les versions antérieures d’Android à l’aide de la bibliothèque de compatibilité v4.


Chacune de ces méthodes a le même ensemble de base d’entrées :

-  **URI** &ndash; le nom qualifié complet de le `ContentProvider` .
-  **Projection** &ndash; spécification de colonnes à sélectionner pour le curseur.
-  **Sélection** &ndash; similaire à SQL `WHERE` clause.
-  **SelectionArgs** &ndash; paramètres devant être substitué dans la sélection.
-  **SortOrder** &ndash; colonnes à trier.



## <a name="creating-inputs-for-a-query"></a>Création d’entrées pour une requête

Le `ContactsProvider` exemple de code effectue une requête très simple dans le fournisseur de Contacts intégré d’Android. Vous n’avez pas besoin de connaître les noms de Uri ou de colonne réelles - toutes les informations nécessaires pour interroger les Contacts `ContentProvider` est disponible en tant que constantes exposées par le `ContactsContract` classe.

Quelle que soit la méthode qui permet de récupérer le curseur, ces mêmes objets sont utilisés comme paramètres, comme indiqué dans le *ContactsProvider/ContactsAdapter.cs* fichier :

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

Pour cet exemple, le `selection`, `selectionArgs` et `sortOrder` sera ignoré en les définissant sur `null`.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Création d’un curseur à partir d’un Uri de fournisseur de contenu

Une fois que les objets de paramètre ont été créés, elles peuvent servir de l’une des trois méthodes suivantes :



### <a name="using-a-managed-query"></a>À l’aide d’une requête gérée

Applications qui ciblent Android 2.3 (API niveau 10) ou antérieure doivent utiliser cette méthode :

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Ce curseur est géré par Android, vous n’avez pas besoin de le fermer.



### <a name="using-contentresolver"></a>À l’aide de ContentResolver

L’accès à `ContentResolver` directement pour obtenir un curseur par rapport à un `ContentProvider` peut être comme suit :

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Ce curseur étant pas géré, il doit être fermé n’est plus nécessaire.
Vérifiez que le code ferme un curseur est ouvert, sinon une erreur se produit.

```csharp
cursor.Close();
```

Vous pouvez également appeler `StartManagingCursor()` et `StopManagingCursor()` le curseur « gestion ». Géré les curseurs sont automatiquement désactivés et interrogées de nouveau lorsque les activités sont arrêtées puis redémarrées.



### <a name="using-cursorloader"></a>À l’aide de CursorLoader

Les applications générées pour Android 3.0 (API niveau 11) ou plus récente doit utiliser cette méthode :

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

Le `CursorLoader` garantit que toutes les opérations de curseur sont effectuées sur un thread d’arrière-plan et peuvent intelligemment réutiliser un curseur existant entre les instances d’activité quand une activité est redémarrée (par exemple, en raison d’une modification de configuration) plutôt que recharger les données à nouveau.

Les versions Android antérieures peuvent également utiliser le `CursorLoader` classe à l’aide de la [bibliothèques de prise en charge v4](http://developer.android.com/tools/support-library/index.html).



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Afficher les données de curseur avec un adaptateur personnalisé

Pour afficher l’image de contact, nous allons utiliser un adaptateur personnalisé, afin que nous puissions résoudre manuellement les `PhotoId` référence à un chemin d’accès du fichier image.

Pour afficher des données avec un adaptateur personnalisé, l’exemple utilise un `CursorLoader` pour récupérer toutes les données de Contact dans une collection locale dans le **FillContacts** méthode à partir de **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Implémentez ensuite les méthodes de l’adaptateur BaseAdapter à l’aide de la `contactList` collection. L’adaptateur est implémenté comme il le serait avec n’importe quelle autre collection &ndash; il n’existe aucun traitement spécial ici car les données en provenance d’un `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

L’image s’affiche (si elle existe) à l’aide de l’Uri dans le fichier image sur l’appareil. L’application se présente comme suit :

[![Capture d’écran de l’application de l’affichage de contacts dans un ListView ; une image est affichée à gauche d’une entrée](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

À l’aide d’un modèle de code similaire, votre application peut accéder à un large éventail de données système, y compris des photos, vidéos et musique de l’utilisateur.
Certains types de données requièrent des autorisations spéciales pour être demandées dans le projet **AndroidManifest.xml**.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Afficher les données de curseur avec un SimpleCursorAdapter

Le curseur pourrait également être affiché avec un `SimpleCursorAdapter` (bien que seul le nom s’affichera, pas la photo). Ce code montre comment utiliser un `ContentProvider` avec `SimpleCursorAdapter` (ce code n’apparaît pas dans l’exemple) :

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Reportez-vous à la [ListViews et Adapters](~/android/user-interface/layouts/list-view/index.md) pour plus d’informations sur l’implémentation `SimpleCursorAdapter`.


## <a name="related-links"></a>Liens associés

- [Démonstration de ContactsAdapter (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
