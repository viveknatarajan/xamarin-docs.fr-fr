---
title: Contacts et ContactsUI
description: Cet article décrit l’utilisation avec l’interface utilisateur de Contacts et les Contacts de nouvelles infrastructures dans une application Xamarin.iOS. Ces infrastructures remplacent le carnet d’adresses existant et l’interface utilisateur du carnet d’adresse utilisé dans les versions antérieures d’iOS.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: ca4235f7fb67c26ade6171d91870e74407aedbd3
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="contacts-and-contactsui"></a>Contacts et ContactsUI

_Cet article décrit l’utilisation avec l’interface utilisateur de Contacts et les Contacts de nouvelles infrastructures dans une application Xamarin.iOS. Ces infrastructures remplacent le carnet d’adresses existant et l’interface utilisateur du carnet d’adresse utilisé dans les versions antérieures d’iOS._

Avec l’introduction d’iOS 9, Apple a publié les deux infrastructures de nouveau, `Contacts` et `ContactsUI`, que le remplacement l’existant carnet d’adresses et les infrastructures d’interface utilisateur de livre adresse utilisés par iOS 8 et versions antérieures.

Les deux infrastructures nouvelle contiennent les fonctionnalités suivantes :

- [**Contacts** ](#contacts) -permet d’accéder aux données de liste de contacts de l’utilisateur.
    Étant donné que la plupart des applications nécessitent uniquement un accès en lecture seule, cette infrastructure a été optimisée pour un accès thread-safe en lecture seule.

- [**ContactsUI** ](#contactsui) -éléments de l’interface utilisateur de Xamarin.iOS fournit à afficher, modifier et sélectionnez Créer des contacts sur les appareils iOS.

[![](contacts-images/add01.png "Un exemple de feuille de Contact sur un appareil iOS")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Existants `AddressBook` et `AddressBookUI` utilisés par iOS 8 infrastructures (et versions antérieures) ont été déconseillés dans iOS 9 et doit être remplacés par la nouvelle `Contacts` et `ContactsUI` infrastructures dès que possible pour n’importe quelle application Xamarin.iOS existante. Nouvelles applications doivent être écrits sur les infrastructures de nouveau.




Dans les sections suivantes, nous allons examiner ces nouvelles infrastructures et comment les implémenter dans une application Xamarin.iOS.

<a name="contacts" />

## <a name="the-contacts-framework"></a>L’infrastructure de Contacts

L’infrastructure de Contacts fournit l’accès de Xamarin.iOS pour les informations de contact de l’utilisateur. Étant donné que la plupart des applications nécessitent uniquement un accès en lecture seule, cette infrastructure a été optimisée pour un accès thread-safe en lecture seule.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Objets de contact

La `CNContact` classe fournit un accès aux propriétés d’un contact telles que nom, adresse ou les numéros de téléphone thread-safe en lecture seule. `CNContact` fonctionne comme un `NSDictionary` et contient plusieurs collections en lecture seule des propriétés (telles que les adresses ou les numéros de téléphone) :

[![](contacts-images/contactobjects.png "Vue d’ensemble de l’objet de contact")](contacts-images/contactobjects.png#lightbox)

Pour toute propriété qui peut avoir plusieurs valeurs (par exemple, les numéros d’adresse ou par courrier électronique), elles sont représentées comme un tableau de `NSLabeledValue` objets. `NSLabeledValue` est un tuple safe thread constitué d’un jeu en lecture seule des étiquettes et valeurs où l’étiquette définit la valeur à l’utilisateur (par exemple les e-mails d’accueil ou de travail). L’infrastructure de Contacts fournit une sélection d’étiquettes prédéfinies (via la `CNLabelKey` et `CNLabelPhoneNumberKey` les classes static) que vous pouvez utiliser dans votre application ou que vous avez la possibilité de définir des étiquettes personnalisées pour vos besoins.

Pour n’importe quelle application Xamarin.iOS qui ont besoin d’ajuster les valeurs d’un contact existant (ou créer de nouveaux), utilisez la `NSMutableContact` version de la classe et ses classes de sub (tel que `CNMutablePostalAddress`).

Par exemple, le code suivant sera créer un nouveau contact et l’ajouter à la collection de l’utilisateur de contacts :

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

Si ce code est exécuté sur un appareil iOS 9, un nouveau contact sera ajouté à la collection de l’utilisateur. Par exemple :

[![](contacts-images/add01.png "Un nouveau contact ajouté à la collection de l’utilisateur")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Mise en forme de contact et la localisation

L’infrastructure de Contacts contient plusieurs objets et les méthodes qui peuvent vous aider à mettre en forme et localiser le contenu à afficher pour l’utilisateur. Par exemple, le code suivant serait mettre correctement en forme un nom de contacts et votre adresse pour l’affichage :

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Pour les étiquettes de propriété que vous devez afficher dans l’interface utilisateur de votre application, l’infrastructure du Contact comporte des méthodes pour la localisation de ces chaînes ainsi. Là encore, cela dépend des paramètres régionaux actuels de l’appareil iOS qu'est en cours d’exécution sur l’application. Par exemple :

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>L’extraction des Contacts existants

À l’aide d’une instance de la `CNContactStore` (classe), vous pouvez extraire les informations de contact à partir de la base de données contacts de l’utilisateur. Le `CNContactStore` contient toutes les méthodes requises pour l’extraction ou de mise à jour des contacts et des groupes à partir de la base de données. Étant donné que ces méthodes sont synchrones, il est recommandé de les exécuter sur un thread d’arrière-plan pour empêcher le blocage de l’interface utilisateur.

À l’aide des prédicats (construit à partir du `CNContact` classe), vous pouvez filtrer les résultats retournés lors de la récupération des contacts à partir de la base de données. Pour extraire uniquement les contacts qui contiennent la chaîne `Appleseed`, utilisez le code suivant :

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Les prédicats composés et génériques ne sont pas pris en charge par l’infrastructure de Contacts.

Par exemple, pour limiter l’extraction aux seuls le **GivenName** et **FamilyName** propriétés du contact, utilisez le code suivant :

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

Si ce code a été exécuté après que l’exemple que nous avons créés dans le **objet Contacts** section ci-dessus, il retourne le contact de « John Appleseed » que nous venons de créer.

### <a name="contact-access-privacy"></a>Accès contact confidentialité

Étant donné que les utilisateurs finaux peuvent accorder ou refuser l’accès à leurs informations de contact sur chaque application, la première fois que vous effectuez un appel à la `CNContactStore`, une boîte de dialogue s’affiche pour leur demandant d’autoriser l’accès pour votre application.

La demande d’autorisation s’affiche uniquement une seule fois, la première fois que l’application est l’exécution, ainsi que les suivantes s’exécute ou les appels à la `CNContactStore` utilisera l’autorisation de l’utilisateur sélectionné à ce moment-là.

Vous devez concevoir votre application afin qu’il gère correctement l’utilisateur de refuser l’accès à leur base de données de contact.

#### <a name="fetching-partial-contacts"></a>L’extraction des Contacts partielles

A _contactez partielle_ est uniquement certaines des propriétés disponibles qui ont été extraites du magasin de contacts pour un contact. Si vous essayez d’accéder à une propriété qui n’a pas été lue précédemment, il entraîne une exception.

Vous pouvez facilement vérifier si un contact donné a la propriété souhaitée à l’aide du `IsKeyAvailable` ou `AreKeysAvailable` méthodes de la `CNContact` instance. Par exemple :

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
> Le `GetUnifiedContact` et `GetUnifiedContacts` méthodes de la `CNContactStore` classe _uniquement_ renvoyer un Contact partielle limité pour les propriétés demandées à partir des clés de récupération fournis.

### <a name="unified-contacts"></a>Contacts unifiées

Un utilisateur peut avoir différentes sources d’informations de contact pour une seule personne dans leur base de données de contact (comme iCloud, Facebook ou Google Mail). Dans iOS et OS X applications, ces informations seront automatiquement liées entre elles et affichées à l’utilisateur en tant qu’un seul, _unifiée contactez_:

[![](contacts-images/unified01.png "Vue d’ensemble des Contacts unifiée")](contacts-images/unified01.png#lightbox)

Ce Contact unifiée est une vue temporaire en mémoire des informations de contact de lien qui a son propre identificateur unique (qui doit être utilisé pour extraire le contact si nécessaire). Par défaut, l’infrastructure de Contacts retournera un Contact unifiée autant que possible.

### <a name="creating-and-updating-contacts"></a>Création et mise à jour des Contacts

Comme nous l’avons vu dans la [objets Contact](#Contact_Objects) section ci-dessus, vous utilisez un `CNContactStore` et une instance d’un `CNMutableContact` pour créer des contacts qui sont ensuite écrites à l’utilisateur contacter à l’aide de la base de données un `CNSaveRequest`:

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

A `CNSaveRequest` peut également servir à mettre en cache plusieurs modifications de contact et de groupe en une seule opération et le lot de ces modifications à la `CNContactStore`.

Pour mettre à jour un contact non mutable obtenu à partir d’une opération d’extraction, vous devez d’abord demander une copie mutable qui vous modifiez et enregistrez dans le magasin de contacts. Par exemple :

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

Chaque fois qu’un contact est modifié, le Contact de magasin valide un `CNContactStoreDidChangeNotification` au centre de Notification par défaut. Si vous avez mis en cache ou affichez actuellement les contacts, vous devez actualiser ces objets à partir du magasin de Contact (`CNContactStore`).

### <a name="containers-and-groups"></a>Conteneurs et les groupes

Les contacts d’un utilisateur peuvent exister ou localement sur l’appareil de l’utilisateur en tant que contacts synchronisés à l’appareil à partir d’un ou plusieurs comptes de serveur (tels que Facebook ou Google). Chaque pool de contacts possède son propre _conteneur_ et un Contact donné peut uniquement exister dans un seul conteneur.

[![](contacts-images/containers01.png "Vue d’ensemble des conteneurs et des groupes")](contacts-images/containers01.png#lightbox)

Certains conteneurs permettant d’être organisées en un ou plusieurs Contacts _groupes_ ou _sous-groupes_. Ce comportement dépend du magasin de stockage pour un conteneur donné. Par exemple, iCloud a un seul conteneur, mais peut avoir plusieurs groupes (mais pas sous-groupes). Microsoft Exchange ne prend pas en charge les groupes quant à eux, mais peuvent avoir plusieurs conteneurs (un pour chaque dossier Exchange).

[![](contacts-images/containers02.png "Se chevaucher dans les conteneurs et les groupes")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Le Framework ContactsUI

Dans les situations où votre application n’a pas besoin pour présenter une interface utilisateur personnalisée, vous pouvez utiliser l’infrastructure ContactsUI pour présenter les éléments d’interface utilisateur pour afficher, modifier, sélectionnez et créer des contacts dans votre application Xamarin.iOS.

À l’aide des contrôles intégrés d’Apple vous non seulement de réduire la quantité de code que vous devez créer pour prendre en charge des Contacts dans votre application Xamarin.iOS, mais vous présenter une interface cohérente pour les utilisateurs de l’application.

### <a name="the-contact-picker-view-controller"></a>Le contrôleur de vue du sélecteur de Contact

Le contrôleur de vue du sélecteur de Contact (`CNContactPickerViewController`) gère l’affichage de sélecteur de Contact standard qui permet à l’utilisateur de sélectionner un Contact ou une propriété de Contact à partir de la base de données de Contact de l’utilisateur. L’utilisateur peut sélectionner un ou plusieurs contacts (basé sur son utilisation) et le contrôleur de vue de sélecteur de Contact n’affiche aucune invite d’autorisation avant d’afficher le sélecteur.

Avant d’appeler le `CNContactPickerViewController` (classe), vous définissez les propriétés de l’utilisateur peut sélectionner et définir des prédicats de façon à contrôler l’affichage et la sélection de propriétés de Contact.

Utiliser une instance de la classe qui hérite de `CNContactPickerDelegate` pour répondre à une interaction de l’utilisateur avec le sélecteur. Par exemple :

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

Pour autoriser l’utilisateur à sélectionner une adresse de messagerie à partir de contacts dans leur base de données, vous pouvez utiliser le code suivant :

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

### <a name="the-contact-view-controller"></a>Le contrôleur de la vue des contacts

Le contrôleur de vue du Contact (`CNContactViewController`) classe fournit un contrôleur pour présenter une vue de Contact standard à l’utilisateur final. L’affichage de Contact peut présenter de nouveaux contacts de nouveau, inconnu ou existant et le type doit être spécifié avant l’affichage de la vue en appelant le constructeur statique correct (`FromNewContact`, `FromUnknownContact`, `FromContact`). Par exemple :

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée de travailler avec les infrastructures de Contact et l’interface utilisateur de Contact dans une application Xamarin.iOS. Tout d’abord, il couvert les différents types d’objets qui fournit de l’infrastructure de Contact et comment les utiliser pour créer ou accéder aux contacts existants. Elle examinait également l’infrastructure de Contact de l’interface utilisateur pour sélectionner les contacts existants et afficher les informations de contact.


## <a name="related-links"></a>Liens associés

- [Exemple de QuickContacts](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Nouveautés dans iOS 9](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Référence de Framework contacts](https://developer.apple.com/documentation/contacts?language=objc)
- [Référence ContactsUI Framework](https://developer.apple.com/documentation/contactsui?language=objc)
