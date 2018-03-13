---
title: Profil utilisateur
ms.topic: article
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: cf8230c5832104fd17b14532f1d32822a1fc0097
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="user-profile"></a>Profil utilisateur

Android prend en charge l’énumération des contacts avec le `ContactsContract` fournisseur depuis l’API de niveau 5. Par exemple, à la liste de contacts est aussi simple que d’utiliser la `ContactContracts.Contacts` classe, comme indiqué dans le code suivant :

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Avec 4 Android (API niveau 14), un nouveau `ContactsContact.Profile` classe n’est disponible via le fournisseur ContactsContract. Le `ContactsContact.Profile` fournit l’accès à un profil personnel pour le propriétaire d’un appareil, ce qui inclut des données telles que le propriétaire du périphérique nom et numéro de téléphone de contact.


## <a name="required-permissions"></a>Autorisations requises

Pour lire et écrire des données de contact, les applications doivent demander le `Read_Contacts` et `Write_Contacts` autorisations, respectivement. En outre, pour lire et modifier le profil utilisateur, les applications doivent demander le `Read_Profile` et `Write_Profile` autorisations.


## <a name="updating-profile-data"></a>Mise à jour des données de profil

Une fois que ces autorisations ont été définies, une application peut utiliser des techniques Android normales pour interagir avec les données du profil utilisateur. Par exemple, pour mettre à jour le nom du profil complet nous appelez `ContentResolver.Update` avec un `Uri` récupérées via les `ContactsContract.Profile.ContentRawContactsUri` propriété, comme indiqué ci-dessous :

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```


## <a name="reading-profile-data"></a>Lire des données de profil

Émission d’une requête à la `ContactsContact.Profile.ContentUri` relit les données de profil. Par exemple, le code suivant lira nom d’affichage du profil utilisateur :

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```


## <a name="navigating-to-the-people-app"></a>Navigation vers l’application contacts

Enfin, pour accéder au profil d’utilisateur dans la nouvelle application personnes qui est fourni avec Android 4, créez simplement une intention avec un `ActionView` action et un `ContactsContract.Profile.ContentUri`et le passer à la `StartActivity` méthode comme suit :

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Lorsque vous exécutez le code ci-dessus, l’application personnes chargera au profil d’utilisateur, comme illustré dans la capture d’écran suivante :

[![Application de capture d’écran de personnes affichant le profil d’utilisateur John Doe](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png#lightbox)

Utilisation avec le profil utilisateur est désormais semblable à l’interaction avec d’autres données dans Android et offre un niveau supplémentaire de personnalisation de l’appareil.



## <a name="related-links"></a>Liens associés

- [ContactsProviderDemo (sample)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
