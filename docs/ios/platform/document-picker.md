---
title: Sélecteur de documents dans Xamarin.iOS
description: Ce document décrit le sélecteur de documents iOS et explique comment l’utiliser dans Xamarin.iOS. Il examine un iCloud, documents, code de programme d’installation commun, les extensions de fournisseur de document et bien plus encore.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: ac77bbc27784d1b836f4a1d26365acd65ac63ae7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111393"
---
# <a name="document-picker-in-xamarinios"></a>Sélecteur de documents dans Xamarin.iOS

Le sélecteur de documents permet aux documents être partagés entre les applications. Ces documents peuvent être stockées dans iCloud ou dans le répertoire d’une autre application. Les documents sont partagés par le biais de l’ensemble de [Extensions de fournisseur de Document](~/ios/platform/extensions.md) l’utilisateur a installé sur leur appareil. 

En raison de la difficulté de conserver les documents de synchronisation sur les applications et le cloud, ils introduisent une certaine quantité de complexité nécessaire.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour effectuer les étapes présentées dans cet article :

-  **Xcode 7 et iOS 8 ou version ultérieure** – du Apple Xcode 7 et iOS 8 ou API plus récentes doivent être installés et configurés sur l’ordinateur du développeur.
-  **Visual Studio ou Visual Studio pour Mac** – la dernière version de Visual Studio pour Mac doit être installée.
-  **APPAREIL iOS** : un appareil iOS exécutant iOS 8 ou version ultérieure.

## <a name="changes-to-icloud"></a>Modifications apportées à iCloud

Pour implémenter les nouvelles fonctionnalités du sélecteur de Document, les modifications suivantes ont été apportées à iCloud d’Apple Service :

-  Le démon iCloud a été entièrement réécrit à l’aide de CloudKit.
-  ICloud existant fonctionnalités ont été renommés iCloud Drive.
-  Prise en charge pour le système d’exploitation de Microsoft Windows a été ajoutée sur iCloud.
-  Un dossier iCloud a été ajouté dans le Finder Mac OS.
-  les appareils iOS peuvent accéder au contenu du dossier du système d’exploitation Mac iCloud.

> [!IMPORTANT]
> Apple [fournit des outils](https://developer.apple.com/support/allowing-users-to-manage-data/) pour aider les développeurs à gérer correctement le Règlement général sur la protection des données (RGPD) de l’Union européenne.

## <a name="what-is-a-document"></a>Qu’est un Document ?

Lorsque vous faites référence à un Document dans iCloud, il est une entité unique et autonome et doit être perçu comme tel par l’utilisateur. Un utilisateur peut souhaiter modifier le document ou le partager avec d’autres utilisateurs (par courrier électronique, par exemple).

Il existe plusieurs types de fichiers que l’utilisateur immédiatement reconnaîtra sous forme de Documents, telles que des Pages, fichiers discours d’ouverture ou de nombres. Toutefois, iCloud n’est pas limité à ce concept. Par exemple, l’état d’un jeu (par exemple, une correspondance Chess) peut être traité comme un document et stockée dans iCloud. Ce fichier peuvent être transmises entre les appareils d’un utilisateur et leur permettre de récupérer un jeu où il s’était arrêté sur un autre appareil.

## <a name="dealing-with-documents"></a>Gestion des Documents

Avant de plonger dans le code nécessaire pour utiliser le sélecteur de documents avec Xamarin, cet article va couvrent les meilleures pratiques pour l’utilisation des Documents iCloud et plusieurs des modifications apportées aux API existantes requis pour prendre en charge le sélecteur de documents.

### <a name="using-file-coordination"></a>À l’aide de la Coordination de fichier

Un fichier peut être modifié à partir de plusieurs emplacements, il est nécessaire de coordination afin d’éviter la perte de données.

 [![](document-picker-images/image1.png "À l’aide de la Coordination de fichier")](document-picker-images/image1.png#lightbox)

Jetons un œil à l’illustration ci-dessus :

1.  Un appareil iOS à l’aide de la coordination de fichier crée un nouveau Document et l’enregistre dans le dossier iCloud.
2.  iCloud enregistre le fichier modifié vers le cloud pour la distribution à chaque appareil.
3.  Un Mac attaché voit que le fichier modifié dans le dossier iCloud et utilise la Coordination de fichier à copier vers le bas les modifications apportées au fichier.
4.  Un appareil ne pas à l’aide de Coordination de fichier apporte une modification au fichier et l’enregistre dans le dossier iCloud. Ces modifications sont instantanément répliquées sur les autres appareils.

Supposons que l’original appareil iOS ou Mac a été modifiant le fichier, désormais leurs modifications sont perdues et remplacées par la version du fichier à partir de l’appareil non coordonnée. Pour éviter la perte de données, fichier Coordination est indispensable lorsque vous travaillez avec des Documents basés sur le cloud.

### <a name="using-uidocument"></a>À l’aide de UIDocument

 `UIDocument` rend les choses simples (ou `NSDocument` sur macOS) en faisant tout l’essentiel du travail pour le développeur. Il fournit intégré dans le fichier de Coordination avec les files d’attente en arrière-plan pour empêcher le blocage de l’interface utilisateur de l’application.

 `UIDocument` expose plusieurs, les API de haut niveau qui facilitent l’effort de développement d’une application Xamarin à quelque fin que le développeur a besoin.

Le code suivant crée une sous-classe de `UIDocument` pour implémenter un document texte générique qui peut être utilisé pour stocker et récupérer du texte iCloud :

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

Le `GenericTextDocument` classe présentée ci-dessus sera utilisée tout au long de cet article lorsque vous travaillez avec le sélecteur de documents et des Documents externes dans une application Xamarin.iOS 8.

## <a name="asynchronous-file-coordination"></a>Coordination de fichier asynchrones

iOS 8 fournit plusieurs nouvelles fonctionnalités de Coordination de fichier asynchrone via les nouvelles API de Coordination de fichier. Avant d’iOS 8, toutes les API de Coordination de fichier existants ont été totalement synchrones. Cela signifiait que le développeur était chargé d’implémenter leur propres arrière-plan queuing pour empêcher le blocage de l’interface utilisateur de l’application de Coordination du fichier.

La nouvelle `NSFileAccessIntent` classe contient une URL pointant vers le fichier et de plusieurs options pour contrôler le type de coordination nécessaire. Le code suivant illustre le déplacement d’un fichier à partir d’un emplacement vers un autre à l’aide d’intentions :

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Découverte et la liste de Documents

Pour le découvrir et de liste de Documents consiste à l’aide d’existant `NSMetadataQuery` API. Cette section décrit les nouvelles fonctionnalités ajoutées à `NSMetadataQuery` qui facilitent l’utilisation des Documents qu’avant.

### <a name="existing-behavior"></a>Comportement existant

Avant d’iOS 8, `NSMetadataQuery` était lent pour les modifications de collecte de fichiers local tel que : supprime, crée et renomme.

 [![](document-picker-images/image2.png "Vue d’ensemble de modifications de fichier local NSMetadataQuery")](document-picker-images/image2.png#lightbox)

Dans le diagramme ci-dessus :

1.  Pour les fichiers qui existent déjà dans le conteneur d’applications, `NSMetadataQuery` a existants `NSMetadata` enregistrements créés au préalable et mis en attente afin qu’ils soient immédiatement disponibles pour l’application.
1.  L’application crée un nouveau fichier dans le conteneur d’Application.
1.  Il existe un délai avant `NSMetadataQuery` voit la modification dans le conteneur d’Application et crée le requis `NSMetadata` enregistrement.


En raison du délai lors de la création de la `NSMetadata` enregistrement, l’application devait avoir deux ouvrir des sources de données : un pour les modifications de fichier local et un pour le cloud en fonction modifications.

### <a name="stitching"></a>Assemblage

Dans iOS 8, `NSMetadataQuery` est plus facile à utiliser directement avec une nouvelle fonctionnalité appelée assemblage :

 [![](document-picker-images/image3.png "NSMetadataQuery avec une nouvelle fonctionnalité appelée panoramas")](document-picker-images/image3.png#lightbox)

À l’aide d’assemblage dans le diagramme ci-dessus :

1.  Comme auparavant, pour les fichiers qui existent déjà dans le conteneur d’Application, `NSMetadataQuery` a existants `NSMetadata` enregistrements créés au préalable et mis en attente.
1.  L’application crée un nouveau fichier dans le conteneur d’Application à l’aide de la Coordination de fichier.
1.  Un crochet dans le conteneur Application voit la modification et les appels `NSMetadataQuery` pour créer le texte requis `NSMetadata` enregistrement.
1.  Le `NSMetadata` enregistrement est créé directement après le fichier et est accessible à l’application.


À l’aide de panoramas l’application n’a plus à ouvrir une source de données pour surveiller local et les modifications de fichier basé sur le cloud. À présent l’application peut reposer sur `NSMetadataQuery` directement.

> [!IMPORTANT]
> Assemblage fonctionne uniquement si l’Application est à l’aide de Coordination de fichier telle que présentée dans la section ci-dessus. Si la Coordination de fichier n’est pas utilisée, les API par défaut au comportement antérieur iOS 8.




### <a name="new-ios-8-metadata-features"></a>Nouvelles fonctionnalités de métadonnées iOS 8

Les nouvelles fonctionnalités suivantes ont été ajoutées à `NSMetadataQuery` dans iOS 8 :

-   `NSMetatadataQuery` maintenant de répertorier les documents non locaux stockés dans le cloud.
-  Nouvelles API ont été ajoutées pour accéder aux informations de métadonnées sur les documents de cloud. 
-  Il existe un nouveau `NSUrl_PromisedItems` API sera pour accéder aux attributs de fichier des fichiers qui peuvent ou non avoir leur contenu disponible localement.
-  Utiliser le `GetPromisedItemResourceValue` méthode pour obtenir des informations sur un fichier donné, ou utilisez le `GetPromisedItemResourceValues` méthode pour obtenir des informations sur plusieurs fichiers à la fois.


Deux nouveaux indicateurs de coordination de fichier ont été ajoutées pour la gestion des métadonnées :

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Avec les indicateurs ci-dessus, le contenu du fichier de Document n’avez pas besoin être disponible localement pour les utiliser.

Le segment de code suivant montre comment utiliser `NSMetadataQuery` pour rechercher l’existence d’un fichier spécifique et générer le fichier s’il n’existe :

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Miniatures de documents

Apple estime que la meilleure expérience utilisateur lors de l’énumération des documents d’une application consiste à utiliser des versions préliminaires. Cela donne le contexte des utilisateurs finaux, afin qu’ils peuvent identifier rapidement le document qu’ils souhaitent utiliser.

Avant d’iOS 8, montrant les aperçus de document requis une implémentation personnalisée. Nouveau dans iOS 8 sont des attributs de système de fichiers qui permettent au développeur de travailler rapidement avec des miniatures de documents.

#### <a name="retrieving-document-thumbnails"></a>Lors de la récupération des miniatures de documents 

En appelant le `GetPromisedItemResourceValue` ou `GetPromisedItemResourceValues` méthodes, `NSUrl_PromisedItems` API, un `NSUrlThumbnailDictionary`, est retournée. La seule clé actuellement dans ce dictionnaire est le `NSThumbnial1024X1024SizeKey` et sa correspondance `UIImage`.

#### <a name="saving-document-thumbnails"></a>Enregistrement des miniatures de documents

Le moyen le plus simple pour enregistrer une miniature est à l’aide de `UIDocument`. En appelant le `GetFileAttributesToWrite` méthode de la `UIDocument` et de la définition de la miniature, celles-ci sont automatiquement enregistrées lorsque le fichier de Document est. Le démon iCloud sera voit ce changement et propager vers iCloud. Sur Mac OS X, les miniatures sont automatiquement générés pour les développeurs par le plug-in de recherche rapide.

Les principes fondamentaux de travailler avec des Documents iCloud basé en place, ainsi que les modifications apportées à une API existante, nous sommes prêts à implémenter le contrôleur d’affichage de sélecteur de Document dans un kit de développement Xamarin iOS 8 Application Mobile.


## <a name="enabling-icloud-in-xamarin"></a>L’activation d’iCloud dans Xamarin

Avant de pouvoir utiliser le sélecteur de documents dans une Application Xamarin.iOS, prise en charge d’iCloud doit être activé à la fois dans votre application et via Apple. 

La procédure suivante suit le processus d’approvisionnement pour iCloud.

1. Créer un conteneur iCloud.
2. Créer un ID d’application qui contient le Service d’application d’iCloud.
3. Créer un profil d’approvisionnement qui inclut cet ID. application

Le [utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) guide présente en détail les deux premières étapes. Pour créer un profil de provisionnement, suivez les étapes décrites dans le [profil de provisionnement](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) guide.



La procédure suivante étapes du processus de configuration de votre application pour iCloud :

Effectuez ce qui suit :

1.  Ouvrez le projet dans Visual Studio pour Mac ou Visual Studio.
2.  Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez Options.
3.  Dans, sélectionnez la boîte de dialogue Options **iOS Application**, vérifiez que le **identificateur de Bundle** correspond à celui qui a été défini dans **ID d’application** créé ci-dessus pour l’application. 
4.  Sélectionnez **signature du Bundle iOS**, sélectionnez le **Developer Identity** et **profil de provisionnement** créé ci-dessus.
5.  Cliquez sur le **OK** bouton pour enregistrer les modifications et fermer la boîte de dialogue.
6.  Avec le bouton droit sur `Entitlements.plist` dans le **l’Explorateur de solutions** pour l’ouvrir dans l’éditeur.

    > [!IMPORTANT]
    > Dans Visual Studio vous devrez peut-être ouvrir l’éditeur de droits en cliquant dessus, en sélectionnant **ouvrir avec...** et en sélectionnant l’éditeur de liste de propriétés

7.  Vérifiez **activer iCloud** , **Documents iCloud** , **stockage clé-valeur** et **CloudKit** .
8.  Vérifiez le **conteneur** existe pour l’application (créé ci-dessus). Exemple : `iCloud.com.your-company.AppName`
9.  Enregistrez les modifications dans le fichier.

Pour plus d’informations sur les droits, reportez-vous à la [utilisation de droits](~/ios/deploy-test/provisioning/entitlements.md) guide.

Avec la configuration ci-dessus en place, l’application peut désormais utiliser des documents basés sur le cloud et le nouveau contrôleur d’affichage de sélecteur de Document.

## <a name="common-setup-code"></a>Code de programme d’installation commun

Avant la mise en route avec le contrôleur d’affichage de sélecteur de Document, il est du code de programme d’installation standard requis. Commencez par modifier l’application `AppDelegate.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");  

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {   
                    // Yes, inform caller and save location the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> Le code ci-dessus inclut le code à partir de la section de découverte et de la liste des Documents ci-dessus. Elle est présentée ici dans son intégralité, elle apparaît dans une application réelle. Par souci de simplicité, cet exemple fonctionne avec un fichier unique, codé en dur (`test.txt`) uniquement.

Le code ci-dessus expose plusieurs raccourcis de lecteur iCloud pour les rendre plus facile à utiliser dans le reste de l’application.

Ensuite, ajoutez le code suivant à la vue ou n’importe quel conteneur d’affichage à l’aide du sélecteur de Document ou utilisation de documents basée sur le cloud :

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Cette opération ajoute un raccourci pour accéder à la `AppDelegate` et accéder aux raccourcis iCloud créés ci-dessus.

Avec ce code en place, jetons un œil à mettre en oeuvre le contrôleur d’affichage de sélecteur de Document dans une application Xamarin iOS 8.

## <a name="using-the-document-picker-view-controller"></a>À l’aide du contrôleur d’affichage de Document sélecteur

Avant iOS 8, il était très difficile d’accéder aux Documents à partir d’une autre application, car il n’existait aucun moyen pour découvrir les documents en dehors de l’application à partir de l’application.

### <a name="existing-behavior"></a>Comportement existant

 [![](document-picker-images/image31.png "Vue d’ensemble de comportement existant")](document-picker-images/image31.png#lightbox)

Jetons un œil à l’accès à un document externe avant iOS 8 :

1.  Tout d’abord l’utilisateur aurait ouvrir l’application qui l’a créé le Document.
1.  Le Document est sélectionné et le `UIDocumentInteractionController` est utilisé pour envoyer le Document à la nouvelle application.
1.  Enfin, une copie du Document d’origine est placée dans conteneur de la nouvelle application.


À partir de là, le Document est disponible pour la deuxième application ouvrir et modifier.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Découverte des Documents en dehors conteneur d’une application

Dans iOS 8, une application est en mesure d’accéder aux Documents en dehors de son propre conteneur d’Application en toute simplicité :

 [![](document-picker-images/image32.png "Découverte des Documents en dehors conteneur d’une application")](document-picker-images/image32.png#lightbox)

À l’aide d’iCloud nouveau sélecteur de documents ( `UIDocumentPickerViewController`), une application iOS peut directement découvrir et accéder en dehors de son conteneur d’Application. Le `UIDocumentPickerViewController` fournit un mécanisme pour l’utilisateur accorder l’accès à et de modifier ceux découverts Documents via des autorisations.

Une application doit choisir d’avoir ses Documents s’affichent dans le sélecteur de documents iCloud et être disponible pour d’autres applications de découvrir et de les manipuler. Pour avoir une application Xamarin iOS 8 partager son conteneur d’Application, modifiez-le `Info.plist` fichier dans un éditeur de texte standard, puis ajoutez les deux lignes suivantes vers le bas du dictionnaire (entre le `<dict>...</dict>` balises) :

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

Le `UIDocumentPickerViewController` fournit une excellente nouvelle interface utilisateur qui permet à l’utilisateur de choisir des documents. Pour afficher le contrôleur d’affichage de sélecteur de Document dans une application Xamarin iOS 8, procédez comme suit :

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> Le développeur doit appeler le `StartAccessingSecurityScopedResource` méthode de le `NSUrl` avant un document externe sont accessibles. Le `StopAccessingSecurityScopedResource` méthode doit être appelée pour libérer le verrou de sécurité dès que le document a été chargé.

### <a name="sample-output"></a>Résultat de l'exemple

Voici un exemple de la façon dont le code ci-dessus afficherait un sélecteur de documents lorsque vous exécutez sur un appareil iPhone :

1.  L’utilisateur démarre l’application et l’interface principale s’affiche :   
 
    [![](document-picker-images/image33.png "L’interface principale s’affiche.")](document-picker-images/image33.png#lightbox)
1.  L’utilisateur appuie sur le **Action** bouton en haut de l’écran et est invité à sélectionner un **Document fournisseur** dans la liste des fournisseurs disponibles :   
 
    [![](document-picker-images/image34.png "Sélectionnez un fournisseur de Document dans la liste des fournisseurs disponibles")](document-picker-images/image34.png#lightbox)
1.  Le **contrôleur d’affichage de Document sélecteur** s’affiche pour le **Document fournisseur**:   
 
    [![](document-picker-images/image35.png "Le contrôleur d’affichage de Document sélecteur s’affiche.")](document-picker-images/image35.png#lightbox)
1.  L’utilisateur appuie sur un **dossier Document** pour afficher son contenu :   
 
    [![](document-picker-images/image36.png "Le contenu du dossier de Document")](document-picker-images/image36.png#lightbox)
1.  L’utilisateur sélectionne un **Document** et **sélecteur de documents** est fermé.
1.  L’interface principale s’affiche de nouveau la **Document** est chargé à partir du conteneur externe et son contenu affiché.


L’affichage de contrôleur d’affichage de Document sélecteur varie selon les fournisseurs de Document que l’utilisateur a installé sur l’appareil et le Mode de sélecteur de Document a été d’implémenter. L’exemple ci-dessus utilise le Mode d’ouverture, les autres types de mode seront abordés en détail ci-dessous.

## <a name="managing-external-documents"></a>La gestion des Documents externes

Comme indiqué ci-dessus, avant iOS 8, une application peut accéder aux documents qui faisaient partie de son conteneur d’Application. Dans iOS 8 une application peut accéder aux Documents provenant de sources externes :

 [![](document-picker-images/image37.png "Vue d’ensemble de la gestion des Documents externes")](document-picker-images/image37.png#lightbox)

Lorsque l’utilisateur sélectionne un Document à partir d’une source externe, un Document de référence est écrit dans le conteneur d’Application qui pointe vers le Document d’origine.

Pour vous aider à ajouter cette nouvelle capacité dans des applications existantes, plusieurs nouvelles fonctionnalités ont été ajoutées à la `NSMetadataQuery` API. En règle générale, une application utilise l’étendue de Document omniprésent à la liste des documents qui vivent dans son conteneur d’Application. À l’aide de cette étendue, seuls les documents au sein du conteneur de l’Application continue à afficher.

À l’aide de la nouvelle étendue de Document externe omniprésent retournera les Documents qui résident en dehors du conteneur d’Application et de retournent les métadonnées pour les. Le `NSMetadataItemUrlKey` pointera vers l’URL où se trouve réellement le Document.

Parfois, une application ne souhaite pas travailler avec les Documents qui est pointés par référence de th. Au lieu de cela, l’application souhaite travailler directement avec le Document de référence. Par exemple, l’application souhaite afficher le document dans le dossier l’Application dans l’interface utilisateur, ou pour autoriser l’utilisateur à déplacer les références à l’intérieur d’un dossier.

Dans iOS 8, un nouveau `NSMetadataItemUrlInLocalContainerKey` a été fourni pour accéder directement au Document de référence. Cette clé pointe vers la référence réelle au document externe dans un conteneur d’Application.

Le `NSMetadataUbiquitousItemIsExternalDocumentKey` est utilisé pour déterminer si un document est externe conteneur d’une Application ou non. Le `NSMetadataUbiquitousItemContainerDisplayNameKey` est utilisé pour accéder au nom du conteneur qui est qui héberge la copie d’origine d’un Document externe.

### <a name="why-document-references-are-required"></a>Pourquoi les références de Document sont requis

La principale raison qu’iOS 8 utilise des références pour accéder aux Documents externes est la sécurité. Aucune application n’est donnée accès conteneur de n’importe quel autre application. Uniquement le sélecteur de documents peuvent le faire, étant donné qu’est en cours d’exécution out-of-process et a accès au système de large.

La seule façon d’accéder à un document en dehors du conteneur de l’Application est à l’aide du sélecteur de documents, et si l’URL retournée par le sélecteur est étendue de sécurité. L’URL d’étendue de sécurité contient juste suffisamment d’informations pour sélectionné le document, ainsi que les droits d’étendue nécessaire à l’octroi d’une application l’accès au document.

Il est important de noter que si l’URL d’étendue de sécurité a été sérialisé dans une chaîne et puis désérialisé, les informations de sécurité seraient perdues et le fichier serait inaccessible à partir de l’URL. La fonctionnalité de référence de Document fournit un mécanisme pour revenir aux fichiers désignés par ces URL.

Par conséquent, si l’application acquiert un `NSUrl` parmi les Documents de référence, il déjà a l’étendue de sécurité attaché et peut être utilisé pour accéder au fichier. Pour cette raison, il est fortement recommandé que le développeur utiliser `UIDocument` , car il gère toutes ces informations et les processus pour eux.

### <a name="using-bookmarks"></a>Utilisation des signets

Il n’est pas toujours possible d’énumérer les Documents d’une application pour revenir à un Document spécifique, par exemple, lors de la restauration de l’état. iOS 8 fournit un mécanisme permettant de créer des signets qui ciblent directement un Document donné.

Le code suivant crée un signet à partir d’un `UIDocument`de `FileUrl` propriété :

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

L’API de signet existant est utilisé pour créer un signet à un existant `NSUrl` qui peuvent être enregistrés et chargés pour fournir un accès direct à un fichier externe. Le code suivant restaure un signet a été créé ci-dessus :

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Ouvrez Visual Studio. Mode d’importation et le sélecteur de documents

Le contrôleur d’affichage de sélecteur de Document comporte deux modes d’opération :

1.  **Ouvrez le Mode** : dans ce mode, lorsque l’utilisateur sélectionne et Document externe, le sélecteur de documents créera un signet d’étendue de sécurité dans le conteneur d’Application.   
 
    [![](document-picker-images/image37.png "Une sécurité étendue signet dans le conteneur d’Application")](document-picker-images/image37.png#lightbox)
1.  **En Mode importation** : dans ce mode, lorsque l’utilisateur sélectionne et Document externe, le sélecteur de documents ne sera pas créer un signet, mais au lieu de cela, copiez le fichier dans un emplacement temporaire et fournir l’application à accéder au Document à cet emplacement :   
 
    [![](document-picker-images/image38.png "Le sélecteur de Document seront copier le fichier dans un emplacement temporaire et fournir l’accès aux applications au Document à cet emplacement")](document-picker-images/image38.png#lightbox)   
 Une fois que l’application s’arrête pour une raison quelconque, l’emplacement temporaire est vidé et le fichier supprimé. Si l’application doit conserver l’accès au fichier, il doit faire une copie et placez-le dans son conteneur d’Application.


Le Mode d’ouverture est utile lorsque l’application souhaite collaborer avec une autre application et de partager toutes les modifications apportées au document avec cette application. Le Mode d’importation est utilisé lors de l’application ne souhaite pas partager ses modifications à un Document avec d’autres applications.

## <a name="making-a-document-external"></a>Rendre un Document externe

Comme indiqué ci-dessus, une application iOS 8 n’a pas accès aux conteneurs en dehors de son propre conteneur d’Application. L’application peut écrire dans son propre conteneur localement ou dans un emplacement temporaire, puis utiliser un mode de document spécifique pour déplacer le Document résultant en dehors du conteneur d’Application vers un emplacement de choisie par l’utilisateur.

Pour déplacer un Document vers un emplacement externe, procédez comme suit :

1.  Tout d’abord créer un nouveau Document dans un emplacement local ou temporaire.
1.  Créer un `NSUrl` qui pointe vers le nouveau Document.
1.  Ouvrez un nouveau contrôleur d’affichage de sélecteur de Document et passez-lui le `NSUrl` avec le Mode de `MoveToService` . 
1.  Une fois que l’utilisateur choisit un nouvel emplacement, le Document sera déplacé de son emplacement actuel vers le nouvel emplacement.
1.  Un Document de référence sera écrit à conteneur l’application d’Application afin que le fichier est toujours accessible par l’application de création.


Le code suivant peut être utilisé pour déplacer un Document vers un emplacement externe : `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Le Document de référence retourné par le processus ci-dessus est exactement le même que celui créé par le Mode d’ouverture du sélecteur de Document. Toutefois, il existe des fois où l’application peut souhaiter déplacer un Document sans conserver une référence à celui-ci.

Pour déplacer un Document sans générer une référence, utilisez la `ExportToService` Mode. Exemple : `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Lorsque vous utilisez la `ExportToService` mode, le Document est copié dans le conteneur externe et la copie existante reste dans son emplacement d’origine.

## <a name="document-provider-extensions"></a>Extensions de fournisseur de document

Avec iOS 8, Apple veut l’utilisateur final pour pouvoir accéder à tous leurs documents basée sur le cloud, quel que soit le s’ils existent. Pour atteindre cet objectif, iOS 8 fournit un nouveau mécanisme d’Extension de fournisseur de Document.

### <a name="what-is-a-document-provider-extension"></a>Qu’est une Extension de fournisseur de Document ?

En termes simples, une Extension de fournisseur de Document est un moyen pour un développeur ou un tiers, de fournir un stockage d’application autre document qui sont accessibles dans la même façon que l’emplacement de stockage iCloud existant.

L’utilisateur peut sélectionner un de ces emplacements de stockage alternatif dans le sélecteur de Document et ils peuvent utiliser les modes d’accès même exact (ouvrir, importer, de déplacement ou exportation) pour travailler avec des fichiers à cet emplacement.

Ceci est implémenté à l’aide de deux différentes extensions :

-  **Extension du sélecteur de documents** – fournit un `UIViewController` sous-classe qui fournit une interface graphique pour l’utilisateur de choisir un document à partir d’un autre emplacement de stockage. Cette sous-classe s’affichera en tant que partie du contrôleur d’affichage du sélecteur de Document.
-  **Fournir une Extension de fichier** – il s’agit d’une extension non IU qui traite réellement en fournissant le contenu de fichiers. Ces extensions sont fournies grâce à la Coordination de fichier ( `NSFileCoordinator` ). Il s’agit d’un autre cas important où la Coordination de fichier est requise.


Le diagramme suivant illustre le flux de données classiques lorsque vous travaillez avec des Extensions de fournisseur de Document :

 [![](document-picker-images/image39.png "Ce diagramme illustre le flux de données standard lorsque vous travaillez avec des Extensions de fournisseur de Document")](document-picker-images/image39.png#lightbox)

Le processus suivant se produit :

1.  L’application présente un contrôleur de sélecteur de Document pour autoriser l’utilisateur à sélectionner un fichier à utiliser.
1.  L’utilisateur sélectionne un autre emplacement et le fichier personnalisé `UIViewController` extension est appelée pour afficher l’interface utilisateur.
1.  L’utilisateur sélectionne un fichier à partir de cet emplacement et l’URL est transmise sur le sélecteur de documents.
1.  Le sélecteur de documents sélectionne l’URL du fichier et le renvoie à l’application pour l’utilisateur de travailler sur.
1.  L’URL est transmise au coordinateur de fichier pour retourner le contenu de fichiers à l’application.
1.  Le coordinateur de fichier appelle l’Extension de fournisseur de fichier personnalisé pour récupérer le fichier.
1.  Le contenu du fichier est retourné au coordinateur de fichier.
1.  Le contenu du fichier est retourné à l’application.


### <a name="security-and-bookmarks"></a>Sécurité et les signets

Cette section prendra un rapide coup de œil à la sécurité et le fichier persistant accéder via fonctionne de signets avec les Extensions de fournisseur de Document. Contrairement à iCloud fournisseur de Document, qui enregistre automatiquement sécurité et les signets dans le conteneur d’Application, les Extensions de fournisseur de Document ne, car ils ne font pas partie du système de référence de Document.

Par exemple : dans un environnement d’entreprise qui fournit son propre magasin de données sécurisé à toute la société, les administrateurs ne souhaitez pas que des informations confidentielles d’entreprise accessibles ou traités par les serveurs public iCloud. Par conséquent, le système de référence de Document intégrées ne peut pas être utilisé.

Le système de signet peut toujours être utilisé et il incombe à l’Extension de fichier fournisseur pour traiter correctement une URL en signet et de retourner le contenu du Document vers lequel pointé il.

Pour des raisons de sécurité, iOS 8 a une couche d’Isolation qui rend persistantes les informations sur les applications a accès à l’identificateur dans le fournisseur de fichier. Il convient de noter que tous les accès de fichier est contrôlé par cette couche d’Isolation.

Le diagramme suivant illustre le flux de données lorsque vous travaillez avec des signets et d’une Extension de fournisseur de Document :

 [![](document-picker-images/image40.png "Ce diagramme illustre le flux de données lorsque vous travaillez avec des signets et d’une Extension de fournisseur de Document")](document-picker-images/image40.png#lightbox)

Le processus suivant se produit :

1.  L’application est sur le point d’entrer l’arrière-plan et doit conserver son état. Il appelle `NSUrl` pour créer un signet dans un fichier dans un stockage alternatif.
1.  `NSUrl` appelle l’Extension de fournisseur pour obtenir une URL persistante au Document. 
1.  L’Extension de fournisseur de fichier retourne l’URL sous forme de chaîne pour le `NSUrl` .
1.  Le `NSUrl` regroupe l’URL dans un signet et le renvoie à l’application.
1.  Lorsque l’Application sort d’en cours en arrière-plan et il faut restaurer l’état, il passe le signet à `NSUrl` .
1.  `NSUrl` appelle l’Extension de fournisseur de fichier avec l’URL du fichier.
1.  Le fournisseur d’Extension de fichier accède au fichier et retourne l’emplacement du fichier à `NSUrl` .
1.  L’emplacement du fichier est fourni avec les informations de sécurité et retourné à l’application.


À ce stade, l’application peut accéder au fichier et de les utiliser comme d’habitude.

### <a name="writing-files"></a>Écriture de fichiers

Cette section sera examinons rapidement comment écrire des fichiers vers un autre emplacement avec une Extension de fournisseur de Document de works. L’application iOS utilisera la Coordination de fichier pour enregistrer les informations sur le disque à l’intérieur du conteneur d’Application. Peu de temps après que le fichier a été écrit correctement, l’Extension de fournisseur du fichier sera avertie de la modification.

À ce stade, l’Extension de fournisseur de fichier peut démarrer le téléchargement du fichier vers un autre emplacement (ou marquer le fichier en tant que téléchargement dirty et nécessitant une).

### <a name="creating-new-document-provider-extensions"></a>Création de nouvelles Extensions de fournisseur de Document

La création de nouvelles Extensions de fournisseur de Document est en dehors de la portée de cet article d’introduction. Ces informations sont fournies ici pour montrer que, selon les extensions de qu'un utilisateur a chargé dans leur appareil iOS, une application peut avoir accès à des emplacements de stockage de Document en dehors de l’Apple fourni iCloud emplacement.

Le développeur doit être conscient de ce fait, lorsque vous utilisez le sélecteur de documents et travailler avec des Documents externes. Ils ne doivent pas supposer ces documents sont hébergés dans iCloud.

Pour plus d’informations sur la création d’un fournisseur de stockage ou une Extension de sélecteur de Document, consultez la [Introduction aux Extensions d’application](~/ios/platform/extensions.md) document.

## <a name="migrating-to-icloud-drive"></a>Migration vers iCloud Drive

Sur iOS 8, les utilisateurs peuvent choisir de continuer à l’aide d’iCloud existant de système de Documents utilisés dans iOS 7 (et avec les systèmes), ou ils peuvent choisir de migrer des Documents existants vers le nouveau mécanisme de lecteur iCloud.

Sur Mac OS X Yosemite, Apple ne fournit pas la descendante compatibilité pour tous les documents doivent être migrés vers iCloud Drive ou qu’ils ne seront plus être mise à jour sur des appareils.

Une fois un compte d’utilisateur a été migré vers iCloud Drive, seuls les appareils à l’aide d’iCloud Drive sera en mesure de propager les modifications apportées aux Documents sur ces appareils.

> [!IMPORTANT]
> Les développeurs doivent être conscients que les nouvelles fonctionnalités abordées dans cet article sont disponibles uniquement si le compte d’utilisateur a été migré vers iCloud Drive. 

## <a name="summary"></a>Récapitulatif

Cet article a présenté les modifications apportées à iCloud existant API requis pour prendre en charge d’iCloud Drive et le nouveau contrôleur d’affichage de sélecteur de Document. Il est couvert Coordination de fichier et pourquoi il est important lorsque vous travaillez avec des documents basés sur le cloud. Il a décrit les étapes nécessaires pour activer des documents basés sur le cloud dans une Application Xamarin.iOS et étant donné un coup de œil Introduction à travailler avec des documents à l’extérieur conteneur d’une application d’Application à l’aide du contrôleur d’affichage de sélecteur de Document.

En outre, cet article a abordé brièvement les Extensions de fournisseur de Document et pourquoi le développeur doit être conscient lors de l’écriture d’applications qui peuvent gérer des documents basés sur le cloud.

## <a name="related-links"></a>Liens associés

- [DocPicker (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Introduction à iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Introduction aux Extensions d’application](~/ios/platform/extensions.md)
