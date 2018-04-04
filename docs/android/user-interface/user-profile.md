---
title: Profil utilisateur
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/22/2018
ms.openlocfilehash: 1eaae86ab9eacf007eca792d96e889db6f367922
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="user-profile"></a>Profil utilisateur

Android prend en charge l’énumération des contacts dont le [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) fournisseur depuis l’API de niveau 5. Par exemple, la liste de contacts est aussi simple que d’utiliser le [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) classe, comme indiqué dans l’exemple de code suivant :

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Android 4 (API niveau 14), à partir de la [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) classe est disponible via le `ContactsContract` fournisseur. Le `ContactsContact.Profile` fournit l’accès au profil personnel pour le propriétaire d’un appareil, ce qui inclut des données telles que le propriétaire du périphérique nom et numéro de téléphone de contact.


## <a name="required-permissions"></a>Autorisations requises

Pour lire et écrire des données de contact, les applications doivent demander le `READ_CONTACTS` et `WRITE_CONTACTS` autorisations, respectivement.
En outre, pour lire et modifier le profil utilisateur, les applications doivent demander le `READ_PROFILE` et `WRITE_PROFILE` autorisations.


## <a name="updating-profile-data"></a>Mise à jour des données de profil

Une fois que ces autorisations ont été définies, une application peut utiliser des techniques Android normales pour interagir avec les données du profil utilisateur. Par exemple, pour mettre à jour le nom du profil complet, appelez [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) avec un `Uri` récupérées via les [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) propriété, comme indiqué ci-dessous :

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Lire des données de profil

Émission d’une requête à la [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) relit les données de profil. Par exemple, le code suivant lira nom d’affichage du profil utilisateur :

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Navigation dans le profil utilisateur

Enfin, pour accéder au profil d’utilisateur, créez une intention avec un `ActionView` action et un `ContactsContract.Profile.ContentUri` puis transmettez-le à la `StartActivity` méthode comme suit :

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Lorsque vous exécutez le code ci-dessus, le profil de l’utilisateur s’affiche comme illustré dans la capture d’écran suivante :

[![Capture d’écran de profil à afficher le profil d’utilisateur John Doe](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Utilisation avec le profil utilisateur est similaire à l’interaction avec d’autres données dans Android, et propose un niveau supplémentaire de personnalisation de l’appareil.



## <a name="related-links"></a>Liens associés

- [ContactsProviderDemo (sample)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
