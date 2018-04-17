---
title: iCloud
description: Apple a introduit l’iCloud dans iOS 5 en tant que service pour permettre aux applications de stocker des données sur les serveurs Apple et l’avez synchronisées sur tous les périphériques utilisés par la même personne (via leurs ID Apple). Il possède également un composant de sauvegarde, où les données sur vos appareils sont sauvegardé à des serveurs Apple. Ce document décrit comment utiliser certains iCloud API fournies par Apple pour stocker et récupérer des données à partir de leurs serveurs, avec les exemples c# pour stocker les paires clé-valeur données et pour stocker des documents. Elle explique également comment iCloud sauvegarde peut influencer la conception de votre application.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: a62d4621a8f3ace64401d64e35c806317a591c03
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="icloud"></a>iCloud

_Apple a introduit l’iCloud dans iOS 5 en tant que service pour permettre aux applications de stocker des données sur les serveurs Apple et l’avez synchronisées sur tous les périphériques utilisés par la même personne (via leurs ID Apple). Il possède également un composant de sauvegarde, où les données sur vos appareils sont sauvegardé à des serveurs Apple. Ce document décrit comment utiliser certains iCloud API fournies par Apple pour stocker et récupérer des données à partir de leurs serveurs, avec les exemples c# pour stocker les paires clé-valeur données et pour stocker des documents. Elle explique également comment iCloud sauvegarde peut influencer la conception de votre application._

L’API de stockage iCloud dans iOS 5 permet aux applications enregistrer des documents de l’utilisateur et les données spécifiques à l’application dans un emplacement central et accéder à ces éléments à partir de tous les périphériques l’utilisateur.

Il existe quatre types de stockage :

- **Valeur de clé de stockage** - pour le partage de petites quantités de données avec votre application sur d’autres dispositifs d’un utilisateur.

- **Stockage de UIDocument** - pour stocker des documents et autres données dans le compte d’utilisateur iCloud à l’aide d’une sous-classe de UIDocument.

- **CoreData** -stockage de base de données SQLite.

- **Les fichiers et répertoires** - pour la gestion d’un grand nombre de fichiers différents directement dans le système de fichiers.

Ce document décrit les deux premiers types - paires clé-valeur et les sous-classes UIDocument - et comment utiliser ces fonctionnalités dans Xamarin.iOS.

> [!IMPORTANT]
> Apple [fournit des outils](https://developer.apple.com/support/allowing-users-to-manage-data/) pour aider les développeurs de gérer correctement général données Protection règlement (PIBR l’Union européenne).

## <a name="requirements"></a>Spécifications

- La dernière version stable de Xamarin.iOS
- Xcode 8 ou version ultérieure
- Visual Studio pour Mac ou Visual Studio 2015 et versions ultérieures.

## <a name="preparing-for-icloud-development"></a>Préparation pour le développement d’iCloud

Applications doit être configuré pour utiliser iCloud dans les [portail de configuration Apple](https://developer.apple.com/account/ios/overview.action) et le projet lui-même. Avant de développer pour iCloud (ou essayer les exemples), procédez comme suit.

Pour configurer correctement une application à accéder iCloud :

-   **Trouver votre ID d’équipe** -connexion à [developer.apple.com](http://developer.apple.com) , visitez le **Member Center > votre compte > Résumé de compte de développeur** pour obtenir votre ID d’équipe (ou ID individuels pour les développeurs uniques ). Il sera une chaîne de 10 caractères ( **A93A5CM278** par exemple)-cela constitue une partie de l’identificateur du conteneur « ».

-   **Créer un nouvel ID d’application** - pour créer un ID d’application, suivez les étapes décrites dans le [de configuration pour la section de Technologies de stockage du guide de configuration du périphérique](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)et vérifiez **iCloud** comme un service autorisée :

 [![](introduction-to-icloud-images/icloud-sml.png "Recherche un service autorisé iCloud")](introduction-to-icloud-images/icloud.png#lightbox)

- **Créer un profil de configuration** - pour créer un profil de configuration, suivez les étapes décrites dans le [guide de configuration du périphérique](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) .

- **Ajoutez l’identificateur du conteneur à Entitlements.plist** -le format d’identificateur de conteneur est `TeamID.BundleID`. Pour plus d’informations, reportez-vous à la [utilisation des droits](~/ios/deploy-test/provisioning/entitlements.md) guide.

- **Configurer les propriétés du projet** - dans le fichier Info.plist fichier garantir la **identificateur de lot** correspond à la **ID d’offre groupée** définie lorsque [création d’un ID d’application ](~/ios/deploy-test/provisioning/capabilities/index.md); IOS signature d’offre groupée utilise un **profil de préparation** qui contiennent un ID d’application avec le Service d’application, iCloud et **personnalisé droits** fichier sélectionné. Tous les procéder dans Visual Studio sous le volet de propriétés du projet.

- **Activer iCloud sur votre appareil** - atteindre **Paramètres > iCloud** et assurez-vous que l’appareil est connecté.
Sélectionnez et activez la **Documents et données** option.

- **Vous devez utiliser un appareil pour tester iCloud** -il ne fonctionnera pas sur le simulateur.
En fait, vous avez vraiment besoin deux ou plusieurs appareils connectés avec le même ID Apple pour voir iCloud en action.


## <a name="key-value-storage"></a>Valeur de clé de stockage

Valeur de clé de stockage est conçu pour les petites quantités de données un utilisateur intéresser persistante entre les périphériques - telles que la dernière page consulté dans un livre ou un magazine. Stockage de la clé-valeur ne doit pas être utilisé pour les données de sauvegarde.

Il existe certaines limites à connaître lors de l’utilisation du stockage de la valeur de clé :

- **Taille maximale de la clé** -les noms de clé ne peut pas comporter plus de 64 octets.

- **Taille de la valeur maximale** -vous ne pouvez pas stocker plus de 64 kilo-octets dans une seule valeur.

- **Taille maximale magasin clé-valeur pour une application** -Applications ne peuvent stocker jusqu'à 64 Ko de données de la valeur de clé au total. Tente de définir les clés au-delà de cette limite échoue et la valeur précédente est conservés.

- **Types de données** : seuls les types de base telles que des chaînes, nombres et valeurs booléennes peuvent être stockées.

Le **iCloudKeyValue** illustre son fonctionnement. L’exemple de code crée une clé nommée pour chaque périphérique : vous pouvez définir cette clé sur un appareil et observez la valeur est propagée à d’autres personnes. Il crée également une clé appelée « Partagé », qui peut être modifié sur n’importe quel appareil - si vous modifiez à la fois sur de nombreux appareils, iCloud décidera dont la valeur « wins » (à l’aide d’un horodatage sur les modifications) et sont propagées.

Cette capture d’écran montre l’exemple en cours d’utilisation. Lors de la réception des notifications de modification à partir d’iCloud, ils sont imprimés dans la vue de texte de défilement au bas de l’écran et mis à jour dans les champs d’entrée.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Le flux de messages entre les appareils")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Définition et récupération de données

Ce code montre comment définir une valeur de chaîne.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Synchroniser l’appel garantit que la valeur est conservée dans le stockage de disque local uniquement. La synchronisation sur iCloud se produit en arrière-plan et ne peut pas être « forcée » par le code d’application. Avec la connectivité réseau fonctionne bien la synchronisation souvent se produira dans les 5 secondes, toutefois, si le réseau est faible (ou déconnecté) une mise à jour peut prendre beaucoup plus de temps.

Vous pouvez récupérer une valeur avec ce code :

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

La valeur est récupérée à partir du magasin de données local, cette méthode ne tente pas de contacter des serveurs iCloud pour obtenir la valeur « dernier ». iCloud met à jour le magasin de données local en fonction de son propre calendrier.

### <a name="deleting-data"></a>Suppression de données

Pour supprimer complètement une paire clé-valeur, utilisez la méthode Remove comme suit :

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Observer des modifications

Une application peut également recevoir des notifications lorsque les valeurs sont modifiées par iCloud en ajoutant un observateur à le `NSNotificationCenter.DefaultCenter`.
Le code suivant à partir de **KeyValueViewController.cs** `ViewWillAppear` méthode montre comment écouter les notifications et créer une liste dont les clés ont été modifiés :

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Votre code peut effectuer certaines actions sur la liste de clés modifiées, telles que la mise à jour d’une copie locale ou de mise à jour de l’interface utilisateur avec les nouvelles valeurs.

Les raisons des modifications possibles sont : ServerChange (0), InitialSyncChange (1) ou QuotaViolationChange (2). Vous pouvez accéder à la raison et effectuer un traitement différent si nécessaire (par exemple, vous devrez peut-être supprimer certaines clés à la suite d’un *QuotaViolationChange*).

## <a name="document-storage"></a>Stockage de documents

iCloud de stockage de documents est conçu pour gérer les données importantes à votre application (et à l’utilisateur). Il peut être utilisé pour gérer les fichiers et autres données que votre application doit s’exécuter, tout en fournissant la sauvegarde sur iCloud et sur les appareils du tous les utilisateurs partage.

Ce diagramme illustre comment tout s’ajuste. Chaque périphérique possède les données enregistrées sur le stockage local (la UbiquityContainer) et d’iCloud du système d’exploitation que démon prend en charge de l’envoi et réception de données dans le cloud. Tous les accès de fichier à la UbiquityContainer doivent être effectuée via FilePresenter/FileCoordinator pour empêcher l’accès simultané. La `UIDocument` classe implémente pour vous ; cet exemple montre comment utiliser UIDocument.

 [![](introduction-to-icloud-images/icloud-overview.png "La vue d’ensemble du stockage de document")](introduction-to-icloud-images/icloud-overview.png#lightbox)

L’exemple iCloudUIDoc implémente un simple `UIDocument` sous-classe qui contient un champ de texte unique. Le texte est rendu dans un `UITextView` et les modifications sont propagées par iCloud à d’autres périphériques avec un message de notification en rouge. L’exemple de code ne traite pas des fonctionnalités plus avancées telles que la résolution des conflits iCloud.

Cette capture d’écran montre l’exemple d’application - après la modification du texte et en appuyant sur **UpdateChangeCount** le document est synchronisé via iCloud à d’autres périphériques.

 [![](introduction-to-icloud-images/iclouduidoc.png "Cette capture d’écran montre l’exemple d’application après la modification du texte et en appuyant sur UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Il existe cinq parties à l’exemple iCloudUIDoc :

1. **L’accès à la UbiquityContainer** -déterminer si iCloud est activé et si tel est le cas le chemin d’accès à la zone de stockage iCloud de votre application.

1. **Création d’une sous-classe de UIDocument** -créer une classe comme intermédiaire entre le stockage d’iCloud et de vos objets de modèle.

1. **Rechercher et ouvrir des documents iCloud** -utilisez `NSFileManager` et `NSPredicate` pour rechercher des documents iCloud et de les ouvrir.

1. **Afficher des documents iCloud** -exposent des propriétés à partir de votre `UIDocument` afin que vous pouvez interagir avec les contrôles d’interface utilisateur.

1. **Enregistrer des documents d’iCloud** -Vérifiez que les modifications apportées dans l’interface utilisateur sont conservées sur disque et iCloud.

Toutes les opérations d’iCloud exécuter (ou doit s’exécuter) asynchrone afin que ne pas bloquer en attendant que quelque chose doit se produire. Vous verrez trois façons différentes d’accomplir cela dans l’exemple :

 **Threads** - dans `AppDelegate.FinishedLaunching` l’appel initial à `GetUrlForUbiquityContainer` est effectuée sur un autre thread pour empêcher le blocage du thread principal.

 **NotificationCenter** - l’enregistrement de notifications asynchrones lorsque les opérations telles que `NSMetadataQuery.StartQuery` terminée.

 **Gestionnaires d’achèvement** - en passant dans les méthodes pour exécuter à la fin des opérations asynchrones, telles que `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>L’accès à la UbiquityContainer

À l’aide d’iCloud de stockage de documents, la première étape consiste à déterminer si iCloud est activé et cas dans ce l’emplacement du conteneur omniprésence « » (le répertoire où se trouvent les fichiers iCloud sur l’appareil).

Ce code se trouve dans le `AppDelegate.FinishedLaunching` méthode de l’exemple.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

Bien que l’exemple ne le faites pas, Apple vous recommande d’appeler GetUrlForUbiquityContainer chaque fois qu’une application s’affiche au premier plan.

### <a name="creating-a-uidocument-subclass"></a>Création d’une sous-classe de UIDocument

Tous les répertoires et fichiers d’iCloud (ie. quoi que ce soit stocké dans le répertoire UbiquityContainer) doit être géré à l’aide des méthodes NSFileManager, implémentation du protocole NSFilePresenter et en écriture via un NSFileCoordinator.
Le plus simple pour faire tout cela est ne pas de l’écrire vous-même, mais sous-classe UIDocument qui le fait pour vous.

Il existe deux méthodes que vous devez implémenter dans une sous-classe UIDocument pour travailler avec iCloud :

- **LoadFromContents** -transmet le NSData du contenu du fichier pour vous permettre de décompresser dans votre classe de modèle/es.

- **ContentsForType** -demande vous permet de fournir la représentation sous forme de NSData de votre classe de modèle/es à enregistrer sur disque (et le Cloud).

Cet exemple de code à partir de **iCloudUIDoc\MonkeyDocument.cs** montre comment implémenter UIDocument.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

Dans ce cas le modèle de données est très simple : un champ de texte unique. Votre modèle de données peut être aussi complexe que nécessaire, tel qu’un document Xml ou les données binaires. Le rôle principal de l’implémentation de UIDocument consiste à traduire entre vos classes de modèle et une représentation sous forme de NSData qui peut être enregistrés et chargée sur le disque.

### <a name="finding-and-opening-icloud-documents"></a>Recherche et ouverture de Documents iCloud

L’exemple d’application ne traite qu’un seul fichier - test.txt - pour que le code dans **AppDelegate.cs** crée un `NSPredicate` et `NSMetadataQuery` pour rechercher ce nom de fichier en particulier. Le `NSMetadataQuery` s’exécute de façon asynchrone et envoie une notification lorsqu’elle est terminée. `DidFinishGathering` appelé par l’Observateur de notification, arrête la requête et appelle LoadDocument, qui utilise le `UIDocument.Open` méthode avec un gestionnaire d’achèvement pour tenter de charger le fichier et l’afficher dans un `MonkeyDocumentViewController`.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>Afficher des Documents iCloud

Affichage d’un UIDocument ne doivent pas être différente pour toute autre classe de modèle
- propriétés sont affichées dans les contrôles d’interface utilisateur, éventuellement modifié par l’utilisateur et puis écrit dans le modèle.

Dans l’exemple **iCloudUIDoc\MonkeyDocumentViewController.cs** affiche le texte MonkeyDocument dans un `UITextView`. `ViewDidLoad` écoute de la notification envoyée le `MonkeyDocument.LoadFromContents` (méthode). `LoadFromContents` est appelé le lorsque iCloud a des nouvelles données pour le fichier, afin que la notification indique que le document a été mis à jour.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

L’exemple de gestionnaire de notification de code appelle une méthode pour mettre à jour de l’interface utilisateur - dans ce cas la détection de conflit ou de résolution.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>L’enregistrement des Documents iCloud

Pour ajouter un UIDocument sur iCloud, vous pouvez appeler `UIDocument.Save` directement (pour les nouveaux documents uniquement) ou déplacer un fichier existant à l’aide `NSFileManager.DefaultManager.SetUbiquitious`. L’exemple de code crée un nouveau document directement dans le conteneur de l’omniprésence avec ce code (il existe deux achèvement gestionnaires ici, une pour le `Save` opération et l’autre pour l’ouvrir) :

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Les modifications ultérieures apportées au document ne sont pas « enregistrées » directement, au lieu de cela nous dire le `UIDocument` qui il a été modifié avec `UpdateChangeCount`, et elle planifiera automatiquement un enregistrement à l’opération de disque :

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Gestion des Documents iCloud

Les utilisateurs peuvent gérer des documents dans iCloud le **Documents** répertoire du « conteneur omniprésence » en dehors de votre application via des paramètres ; ils peuvent afficher la liste des fichiers et effectuez un balayage à supprimer. Code de l’application doit être en mesure de gérer la situation où les documents sont supprimés par l’utilisateur. Ne stockez pas de données d’application interne dans le **Documents** active.

 [![](introduction-to-icloud-images/icloudstorage.png "La gestion des flux de travail de Documents iCloud")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Les utilisateurs recevront également différents avertissements lorsqu’ils tentent de supprimer une application iCloud à partir de son appareil, pour les informer de l’état des documents d’iCloud liés à cette application.

 [![](introduction-to-icloud-images/icloud-delete1.png "Exemple de dialogue lorsque l’utilisateur tente de supprimer une application compatible iCloud leur appareil")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Exemple de dialogue lorsque l’utilisateur tente de supprimer une application compatible iCloud leur appareil")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud de sauvegarde

Lors de la sauvegarde sur iCloud n’est pas une fonctionnalité qui est directement accessible par les développeurs, la manière dont vous concevez votre application peut affecter l’expérience utilisateur.
Apple offre [iOS recommandations de stockage de données](http://developer.apple.com/icloud/documentation/data-storage/) pour les développeurs à suivre dans leurs applications iOS.

L’essentiel est que votre application stocke des fichiers volumineux qui ne sont pas générés par l’utilisateur (par exemple, une application de lecteur magazine qui stocke les mégaoctets hundred-plus de contenu par problème). Apple préfère que vous ne stockez pas de ce type de données pour lesquelles il sera être sauvegardé sur iCloud et inutilement remplir le quota d’utilisateur iCloud.

Les applications qui stockent des grandes quantités de données de cette doivent soit stocker dans un des répertoires de l’utilisateur qui n’est pas sauvegardé (par exemple). Les caches ou tmp) ou utilisez `NSFileManager.SetSkipBackupAttribute` pour appliquer un indicateur à ces fichiers afin qu’iCloud les ignore pendant les opérations de sauvegarde.

## <a name="summary"></a>Récapitulatif

Cet article a introduit la nouvelle fonctionnalité iCloud incluse dans iOS 5. Il a examiné les étapes requises pour configurer votre projet pour utiliser iCloud et puis fourni des exemples montrant comment implémenter les fonctionnalités iCloud.

L’exemple de valeur de clé de stockage a montré comment iCloud peut servir à stocker une petite quantité de données de la même manière que nsuserpreferences sont stockés. L’exemple UIDocument a montré comment des données complexes peuvent être stockées et synchronisées entre plusieurs appareils via iCloud.

Enfin, il inclus une brève présentation sur la façon dont l’ajout d’iCloud sauvegarde doit influencer la conception de votre application.



## <a name="related-links"></a>Liens associés

- [Introduction à iCloud (exemple)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [Exemple de Code séminaire d’iCloud](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud séminaire diapositives](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud stockage](http://support.apple.com/kb/HT4847)
