---
title: Contacts et ContactsUI dans Xamarin.iOS
description: Cet article aborde l’utilisation de l’interface utilisateur de Contacts et les Contacts de nouvelles infrastructures dans une application Xamarin.iOS. Ces infrastructures remplacement le carnet d’adresses existante et l’interface utilisateur du carnet d’adresse utilisé dans les versions précédentes d’e/s.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e3f1533605d08df58d8d257714dd8135690c0e5d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388983"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>Contacts et ContactsUI dans Xamarin.iOS

_Cet article aborde l’utilisation de l’interface utilisateur de Contacts et les Contacts de nouvelles infrastructures dans une application Xamarin.iOS. Ces infrastructures remplacement le carnet d’adresses existante et l’interface utilisateur du carnet d’adresse utilisé dans les versions précédentes d’e/s._

Avec l’introduction d’iOS 9, Apple a publié deux nouvelles infrastructures, `Contacts` et `ContactsUI`, que le remplacement l’existant carnet d’adresses et les infrastructures d’interface utilisateur du livre adresse utilisés par iOS 8 et versions antérieures.

Les deux nouvelles infrastructures comprennent les fonctionnalités suivantes :

- [**Contacts** ](#contacts) -fournit l’accès aux données de liste de contacts de l’utilisateur.
    Étant donné que la plupart des applications requièrent uniquement un accès en lecture seule, cette infrastructure a été optimisée pour l’accès thread-safe en lecture seule.

- [**ContactsUI** ](#contactsui) -éléments de l’interface utilisateur de Xamarin.iOS fournit à afficher, modifier et sélectionnez Créer des contacts sur les appareils iOS.

[![](contacts-images/add01.png "Un exemple de feuille de Contact sur un appareil iOS")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Existant `AddressBook` et `AddressBookUI` utilisé par iOS 8 infrastructures (et les versions antérieures) ont été déconseillés dans iOS 9 et doit être remplacés par la nouvelle `Contacts` et `ContactsUI` infrastructures dès que possible pour n’importe quelle application Xamarin.iOS existante. Nouvelles applications doivent être écrits sur les nouvelles infrastructures.




Dans les sections suivantes, nous allons examiner ces nouvelles infrastructures et comment les implémenter dans une application Xamarin.iOS.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Le Framework de Contacts

Le Framework de Contacts fournit l’accès de Xamarin.iOS pour les informations de contact de l’utilisateur. Étant donné que la plupart des applications requièrent uniquement un accès en lecture seule, cette infrastructure a été optimisée pour l’accès thread-safe en lecture seule.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Objets de contact

Le `CNContact` classe fournit un accès aux propriétés d’un contact telles que les numéros de téléphone, adresse ou nom de thread-safe en lecture seule. `CNContact` fonctionne comme un `NSDictionary` et contient plusieurs collections en lecture seule des propriétés (telles que les adresses ou les numéros de téléphone) :

[![](contacts-images/contactobjects.png "Vue d’ensemble des objets contact")](contacts-images/contactobjects.png#lightbox)

Pour toute propriété qui peut avoir plusieurs valeurs (par exemple, les numéros d’adresse ou le téléphone e-mail), elles sont représentées comme un tableau de `NSLabeledValue` objets. `NSLabeledValue` est un tuple sécurisée de thread consistant en un jeu en lecture seule d’étiquettes et valeurs où l’étiquette définit la valeur à l’utilisateur (par exemple e-mail domestique ou d’entreprise). Le framework de Contacts fournit une sélection des étiquettes prédéfinies (via le `CNLabelKey` et `CNLabelPhoneNumberKey` les classes static) que vous pouvez utiliser dans votre application ou que vous avez la possibilité de définir des étiquettes personnalisées pour vos besoins.

Pour n’importe quelle application Xamarin.iOS à ajuster les valeurs d’un contact existant (ou créer de nouveaux), utilisez le `NSMutableContact` version de la classe et ses classes de sub (tel que `CNMutablePostalAddress`).

Par exemple, le code de suivi sera créer un contact et l’ajouter à la collection de l’utilisateur de contacts :

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Si ce code est exécuté sur un appareil iOS 9, un nouveau contact sera ajouté à la collection de l’utilisateur. Exemple :

[![](contacts-images/add01.png "Un nouveau contact ajouté à la collection de l’utilisateur")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Mise en forme de contact et de localisation

Le framework de Contacts contient plusieurs objets et méthodes qui peuvent vous aider à mettre en forme et localiser le contenu à afficher pour l’utilisateur. Par exemple, le code suivant serait correctement mettre en forme un nom de contacts et votre adresse pour l’affichage :

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Pour les étiquettes de propriété que vous devez afficher dans l’interface utilisateur de votre application, le framework de Contact a des méthodes pour la localisation également ces chaînes. Là encore, cela est basé sur les paramètres régionaux de l’appareil iOS qu'est en cours d’exécution sur l’application. Exemple :

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Récupération des Contacts existants

À l’aide d’une instance de la `CNContactStore` (classe), vous pouvez récupérer des informations de contact de base de données de contacts de l’utilisateur. Le `CNContactStore` contient toutes les méthodes requises pour l’extraction ou la mise à jour des contacts et des groupes à partir de la base de données. Étant donné que ces méthodes sont synchrones, il est recommandé de les exécuter sur un thread d’arrière-plan pour empêcher le blocage de l’interface utilisateur.

À l’aide des prédicats (construit à partir de la `CNContact` classe), vous pouvez filtrer les résultats retournés lors de la récupération des contacts à partir de la base de données. Pour extraire uniquement les contacts qui contiennent la chaîne `Appleseed`, utilisez le code suivant :

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Prédicats génériques et composées ne sont pas pris en charge par l’infrastructure de Contacts.

Par exemple, pour limiter l’extraction seulement le **GivenName** et **FamilyName** propriétés du contact, utilisez le code suivant :

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Enfin, pour rechercher la base de données et retourner les résultats, utilisez le code suivant :

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Si ce code a été exécuté après l’exemple que nous avons créé dans le **objet Contacts** section ci-dessus, il retourne le contact « John Appleseed » que nous venons de créer.

### <a name="contact-access-privacy"></a>Confidentialité de l’accès aux contacts

Étant donné que les utilisateurs finaux peuvent accorder ou refuser l’accès à leurs informations de contact sur chaque application, la première fois que vous effectuez un appel à la `CNContactStore`, une boîte de dialogue s’affiche pour lui demander d’autoriser l’accès pour votre application.

La demande d’autorisation s’affiche uniquement une fois, la première fois que l’application est l’exécution, ainsi que les suivantes s’exécute ou les appels à la `CNContactStore` utilisera l’autorisation sélectionné par l’utilisateur à ce moment-là.

Vous devez concevoir votre application afin qu’il gère correctement l’utilisateur refusant l’accès à leur base de données de contact.

#### <a name="fetching-partial-contacts"></a>Récupération des Contacts partielles

Un _contacter partielle_ est un contact uniquement certaines des propriétés disponibles qui ont été extraites du magasin de contacts pour. Si vous essayez d’accéder à une propriété qui n’a pas été lue précédemment, il entraîne une exception.

Vous pouvez facilement vérifier si un contact donné a la propriété souhaitée à l’aide du `IsKeyAvailable` ou `AreKeysAvailable` méthodes de la `CNContact` instance. Exemple :

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> Le `GetUnifiedContact` et `GetUnifiedContacts` méthodes de la `CNContactStore` classe _uniquement_ retourner un Contact partielle limité pour les propriétés demandées dans les clés de récupération fournies.

### <a name="unified-contacts"></a>Contacts unifiées

Un utilisateur peut avoir différentes sources d’informations de contact pour une seule personne dans leur base de données de contact (par exemple, iCloud, Facebook ou Google Mail). Dans les applications d’OS X et iOS, ces informations seront automatiquement liées entre elles et affichées à l’utilisateur comme une seule _unifiée contactez_:

[![](contacts-images/unified01.png "Vue d’ensemble de Contacts unifiée")](contacts-images/unified01.png#lightbox)

Ce Contact unifiée est une vue temporaire en mémoire, les lien informations de contact qui aura son propre identificateur unique (qui doit être utilisé pour récupérer à nouveau le contact, si nécessaire). Par défaut, le framework de Contacts retournera un Contact unifiée autant que possible.

### <a name="creating-and-updating-contacts"></a>Création et la mise à jour des Contacts

Comme nous l’avons vu dans la [objets Contact](#Contact_Objects) section ci-dessus, vous utilisez un `CNContactStore` et une instance d’un `CNMutableContact` pour créer les nouveaux contacts qui sont ensuite écrites dans l’utilisateur contacter à l’aide de la base de données un `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

Un `CNSaveRequest` peut également être utilisé pour mettre en cache plusieurs modifications de contact et de groupe en une seule opération et commandes ces modifications à la `CNContactStore`.

Pour mettre à jour un contact non mutable obtenu à partir d’une opération d’extraction, vous devez tout d’abord demander une copie mutable qui vous puis de modifiez et enregistrez dans le magasin de contacts. Exemple :

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Notifications de modification de contact

Chaque fois qu’un contact est modifié, le Store de Contact publie un `CNContactStoreDidChangeNotification` au centre de Notification par défaut. Si vous avez mis en cache ou affichez actuellement les contacts, vous devez actualiser ces objets à partir du Store de Contact (`CNContactStore`).

### <a name="containers-and-groups"></a>Conteneurs et les groupes

Les contacts d’un utilisateur peuvent exister en local sur l’appareil de l’utilisateur ou en tant que contacts synchronisés à l’appareil à partir d’un ou plusieurs comptes de serveur (tels que Facebook ou Google). Chaque pool de contacts possède son propre _conteneur_ et un Contact donné ne peut exister dans un conteneur.

[![](contacts-images/containers01.png "Vue d’ensemble des conteneurs et des groupes")](contacts-images/containers01.png#lightbox)

Certains conteneurs autoriser pour les Contacts être organisées en un ou plusieurs _groupes_ ou _sous-groupes_. Ce comportement dépend du magasin de stockage pour un conteneur donné. Par exemple, iCloud a un seul conteneur, mais peut avoir plusieurs groupes (mais aucun sous-groupes). Microsoft Exchange ne prend pas en charge les groupes quant à eux, mais peut avoir plusieurs conteneurs (un pour chaque dossier Exchange).

[![](contacts-images/containers02.png "Se chevaucher dans les conteneurs et les groupes")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Le Framework ContactsUI

Dans les situations où votre application sans devoir présenter une interface utilisateur personnalisée, vous pouvez utiliser le framework ContactsUI pour présenter les éléments d’interface utilisateur pour afficher, modifier, sélectionner et créer des contacts dans votre application Xamarin.iOS.

À l’aide de contrôles intégrés d’Apple vous non seulement réduire la quantité de code que vous devez créer pour prendre en charge des Contacts dans votre application Xamarin.iOS, mais vous présenter une interface cohérente aux utilisateurs de l’application.

### <a name="the-contact-picker-view-controller"></a>Le contrôleur d’affichage de sélecteur de Contact

Le contrôleur d’affichage de sélecteur de Contact (`CNContactPickerViewController`) gère l’affichage de sélecteur de Contact standard qui permet à l’utilisateur sélectionner un Contact ou une propriété de Contact à partir de la base de données de Contact de l’utilisateur. L’utilisateur peut sélectionner un ou plusieurs contacts (basé sur son utilisation) et le contrôleur d’affichage du sélecteur de Contact ne demande pas d’autorisation avant d’afficher le sélecteur.

Avant d’appeler le `CNContactPickerViewController` (classe), vous définissez les propriétés de l’utilisateur peut sélectionner et définir des prédicats de façon à contrôler l’affichage et la sélection de propriétés de Contact.

Utiliser une instance de la classe qui hérite de `CNContactPickerDelegate` pour répondre à l’interaction utilisateur avec le sélecteur. Exemple :

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Pour autoriser l’utilisateur à sélectionner une adresse de messagerie à partir des contacts dans leur base de données, vous pouvez utiliser le code suivant :

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Le contrôleur d’affichage de Contact

Le contrôleur d’affichage de Contact (`CNContactViewController`) classe fournit un contrôleur pour présenter une vue de Contact standard à l’utilisateur final. L’affichage de Contact permet d’afficher les contacts, inconnu existant ou nouveau et le type doit être spécifié avant l’affichage de la vue en appelant le constructeur statique correct (`FromNewContact`, `FromUnknownContact`, `FromContact`). Par exemple :

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation avec les infrastructures de Contact et de l’interface utilisateur de Contact dans une application Xamarin.iOS. Tout d’abord, elle couvrait les différents types d’objets qui fournit l’infrastructure de Contact et comment vous servir pour créer ou accéder aux contacts existants. Il a également examiné le framework de Contact de l’interface utilisateur pour sélectionner des contacts existants et afficher des informations de contact.


## <a name="related-links"></a>Liens associés

- [Exemple de QuickContacts](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Quelles sont les nouveautés dans iOS 9](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Référence de Framework de contacts](https://developer.apple.com/documentation/contacts?language=objc)
- [Référence de l’infrastructure de ContactsUI](https://developer.apple.com/documentation/contactsui?language=objc)
