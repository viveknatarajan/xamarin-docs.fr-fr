---
title: CloudKit
description: "iCloud API permettre aux applications iOS 8 stocker des données dans iCloud, avec prise en charge pour la synchronisation automatique entre un compte d’utilisateur. À l’aide de CloudKit fournit aux utilisateurs une expérience cohérente et transparente entre les périphériques iCloud. Cet article traite de l’activation de CloudKit dans une application iOS 8 à l’aide de l’API de commodité."
ms.topic: article
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: 2c48ae5386fda63aed43c24c09c10cfb87b93637
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="cloudkit"></a>CloudKit

_iCloud API permettre aux applications iOS 8 stocker des données dans iCloud, avec prise en charge pour la synchronisation automatique entre un compte d’utilisateur. À l’aide de CloudKit fournit aux utilisateurs une expérience cohérente et transparente entre les périphériques iCloud. Cet article traite de l’activation de CloudKit dans une application iOS 8 à l’aide de l’API de commodité._

Le framework CloudKit simplifie le développement d’applications qui iCloud de l’accès. Cela inclut la récupération des données d’application et droits de l’élément multimédia ainsi que la possibilité de stocker en toute sécurité les informations de l’application. Ce kit fournit aux utilisateurs une couche d’anonymat en autorisant l’accès aux applications avec leur ID iCloud sans partager des informations personnelles.

Les développeurs peuvent se concentrer sur leurs applications côté client et laisser iCloud élimine la nécessité d’écrire une logique d’application côté serveur. CloudKit fournit l’authentification, les bases de données privés et publics et des données structurées et les services de stockage actif.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont nécessaire pour terminer les étapes présentées dans cet article :

-  **Xcode et le Kit de développement logiciel iOS** – d’Apple Xcode et iOS 8 API doivent être installés et configurés sur l’ordinateur du développeur.
-  **Visual Studio pour Mac** – la version la plus récente de Visual Studio pour Mac doit être installée et configurée sur l’appareil de l’utilisateur.
-  **iOS 8 appareil** – un appareil iOS exécutent la dernière version d’iOS 8 pour le test.

## <a name="what-is-cloudkit"></a>Qu’est CloudKit ?

CloudKit consiste à donner au développeur l’accès sur le serveurs iCloud. Il fournit la base d’iCloud lecteur et iCloud bibliothèque de photos. CloudKit est pris en charge sur les appareils Mac OS X et Apple iOS.

 [![](intro-to-cloudkit-images/image1.png "Comment CloudKit est pris en charge sur Mac OS X et des périphériques Apple iOS")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit utilise l’infrastructure de compte iCloud. En l’absence d’un utilisateur connecté à un compte sur le périphérique iCloud, CloudKit utilisera leur ID pour identifier l’utilisateur. Si aucun compte n’est disponible, un accès limité en lecture seule doivent être fourni.

CloudKit prend en charge les deux le concept de bases de données publiques et privées. Bases de données publiques fournissent a « z » de l’utilisateur a accès à toutes les données. Bases de données privées sont conçus pour stocker les données privées liées à un utilisateur spécifique.

CloudKit prend en charge les données structurées et en bloc. Il est capable de gérer les transferts de fichiers volumineux en toute transparence. CloudKit prend en charge du transfert efficacement des fichiers volumineux vers et depuis le serveurs iCloud en arrière-plan, le développeur pour vous concentrer sur d’autres tâches.

> [!NOTE]
> **Remarque :** il est important de noter que CloudKit est un _Transport technologie_. Il ne fournit pas les persistances ; Elle permet uniquement à une application envoyer et recevoir des informations à partir des serveurs efficacement.

À ce jour, Apple initialement fournit CloudKit gratuitement avec une limite supérieure sur la capacité de la bande passante et de stockage. Pour des projets ou des applications avec une base de l’utilisateur de grande taille plus grande, Apple a suggéré qu’une échelle de tarification abordable sera fournie.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>L’activation de CloudKit dans une Application Xamarin

Une application Xamarin peut utiliser l’infrastructure CloudKit, l’application doit être configurée correctement comme détaillé dans le [utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) et [utilisation des droits](~/ios/deploy-test/provisioning/entitlements.md) guides

1.  Ouvrez le projet dans Visual Studio pour Mac ou Visual Studio.
2.  Dans le **l’Explorateur de solutions**, ouvrez le **Info.plist** de fichier et vérifiez que le **identificateur de lot** correspond à celle qui a été définie dans **ID d’application**créé comme partie de l’approvisionnement configurer :
 
    [![](intro-to-cloudkit-images/image26a.png "Entrez l’identificateur de lot")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  Faites défiler jusqu’en bas de la **Info.plist** fichier et sélectionnez **Modes d’arrière-plan activé**, **mises à jour de l’emplacement** et **des Notifications à distance**:

    [![](intro-to-cloudkit-images/image27a.png "Sélectionnez les Modes d’arrière-plan activé, les mises à jour de l’emplacement et les Notifications à distance")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  Cliquez sur le projet iOS dans la solution et sélectionnez **Options**.
5.  Sélectionnez **signature d’offre groupée iOS**, sélectionnez le **développeur identité** et **profil de préparation** créé ci-dessus.
6.  Vérifiez le **Entitlements.plist** inclut **activer iCloud** , **stockage de clé-valeur** et **CloudKit** .
7.  Vérifiez le **omniprésence conteneur** existe pour l’application (créé ci-dessus). Exemple : `iCloud.com.your-company.CloudKitAtlas`
8.  Enregistrez les modifications dans le fichier.


Avec ces paramètres en place, l’application est maintenant prête à accéder aux API CloudKit Framework.

## <a name="cloudkit-api-overview"></a>Présentation de l’API CloudKit

Avant d’implémenter CloudKit dans une application iOS de Xamarin, cet article va couvrent les principes fondamentaux de l’infrastructure CloudKit, qui inclut les rubriques suivantes :

1.  **Conteneurs** – isolé silos de communications d’iCloud.
2.  **Bases de données** – publics et privés sont accessibles à l’application.
3.  **Enregistrements** – le mécanisme dans laquelle les données structurées sont déplacées vers et depuis CloudKit.
4.  **Zones d’enregistrement** – sont des groupes d’enregistrements.
5.  **Enregistre les identificateurs** – sont entièrement normalisées et représenter l’emplacement spécifique de l’enregistrement.
6.  **Référence** : fournir des relations entre les enregistrements connexes dans une base de données parent-enfant.
7.  **Ressources** – autoriser pour le fichier de données non structurées volumineuses pour être téléchargé sur iCloud et associé à un enregistrement donné.


### <a name="containers"></a>Conteneurs

Une application donnée est en cours d’exécution sur un appareil iOS est toujours en cours d’exécution côté ainsi que les autres applications et services sur l’appareil. Sur l’appareil client, l’application va être isolées ou bac à sable d’une certaine façon. Dans certains cas, il s’agit d’un bac à sable littéral, et dans d’autres, l’application est simplement en cours d’exécution qu’il est propre espace de mémoire.

Le concept consistant à prendre une application cliente et séparée des autres clients en cours d’exécution est très puissant et offre les avantages suivants :

1.  **Sécurité** – une application ne peut pas interférer avec d’autres applications client ou le système d’exploitation lui-même.
1.  **Stabilité** : si l’application cliente bloque il ne peut pas prendre les autres applications, le système d’exploitation.
1.  **Confidentialité** – chaque application cliente a un accès limité aux données personnelles stockées dans l’appareil.


CloudKit a été conçu pour fournir les mêmes avantages comme indiqué ci-dessus et les appliquer à l’utilisation des informations sur le cloud :

 [![](intro-to-cloudkit-images/image31.png "Les applications CloudKit communiquent à l’aide de conteneurs")](intro-to-cloudkit-images/image31.png#lightbox)

Tout comme l’application en cours de-plusieurs en cours d’exécution sur l’appareil, est donc les communications de l’application avec iCloud de-plusieurs. Chacun de ces silos de communication différentes sont appelés des conteneurs.

Les conteneurs sont exposées dans le CloudKit Framework via la `CKContainer` classe. Par défaut, les entretiens d’une application à un conteneur et de ce conteneur sépare les données pour cette application. Cela signifie que plusieurs applications peuvent stocker des informations sur le même compte iCloud, mais ces informations ne seront jamais être mélangées.

Le conteneurs sont des données d’iCloud permet également de CloudKit encapsuler les informations utilisateur. De cette façon, l’application aura un accès limité au compte iCloud et les informations utilisateur sont stockées dans tous les tout en protégeant la confidentialité et la sécurité de l’utilisateur.

Les conteneurs sont entièrement gérés par le développeur de l’application via le portail WWDR. L’espace de noms du conteneur est global sur tous les développeurs d’Apple, pour le conteneur de ne doit pas être unique pour les applications d’un développeur donné, mais pour toutes les applications et les développeurs d’Apple.

Apple suggère à l’aide de la notation DNS inverse lors de la création de l’espace de noms pour les conteneurs d’applications. Exemple :

```csharp
iCloud.com.company-name.application-name
```

Alors que les conteneurs sont, par défaut, lié univoque à une application donnée, ils peuvent être partagées entre les applications. Par conséquent, plusieurs applications peuvent coordonner sur un seul conteneur. Une seule application peut également communiquer avec plusieurs conteneurs.

### <a name="databases"></a>Bases de données

Une des principales fonctions de CloudKit consiste à mettre les modèle de données d’une application et la réplication ce modèle sur les serveurs iCloud. Certaines informations sont destinées à l’utilisateur qui l’a créé, autres informations sont les données publiques qui peuvent être créées par un utilisateur pour une utilisation publique (comme critique d’un restaurant) ou il peut s’agir des informations que le développeur a publié pour l’application. Dans les deux cas, l’audience n’est pas simplement un seul utilisateur, mais est une Communauté d’utilisateurs.

 [![](intro-to-cloudkit-images/image32.png "Diagramme du conteneur CloudKit")](intro-to-cloudkit-images/image32.png#lightbox)

À l’intérieur d’un conteneur, tout d’abord est la base de données public. Il s’agit où toutes les informations publiques réside et même emplacement mingles. En outre, il existe de plusieurs bases de données privées individuelles pour chaque utilisateur de l’application.

Lors de l’exécution sur un appareil iOS, l’application aura uniquement accès aux informations de l’utilisateur actuellement connecté sur iCloud. Vue de l’application du conteneur est donc comme suit :

 [![](intro-to-cloudkit-images/image33.png "La vue applications de conteneur")](intro-to-cloudkit-images/image33.png#lightbox)

Il peut voir uniquement les bases de données public et privé associé à l’utilisateur actuellement connecté sur iCloud.

Bases de données sont exposées dans le CloudKit Framework via la `CKDatabase` classe. Chaque application a accès à deux bases de données : la base de données publique et privée celui.

Le conteneur est un point d’entrée initiale dans CloudKit. Le code suivant peut être utilisé pour accéder à la base de données publique et privée à partir de la valeur par défaut de l’application conteneur :

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

Voici les différences entre les types de base de données :

<table cellpadding="1" cellspacing="1" border="1" width="100%">
<thead>
<tr>
<td></td>
<td>Base de données public</td>
<td>Base de données privée</td>
</tr>
</thead>
<tbody>
<tr>
<td>Type de données</td>
<td>Données partagées</td>
<td>Données de l’utilisateur actuel</td>
</tr>

<tr>
<td>Quota</td>
<td>Prise en compte dans le Quota de développeur</td>
<td>Prise en compte dans le Quota de l’utilisateur</td>
</tr>

<tr>
<td>Autorisations par défaut</td>
<td>World lisible</td>
<td>Utilisateur lisible</td>
</tr>

<tr>
<td>Autorisations de modification</td>
<td>iCloud rôles du tableau de bord via un niveau de la classe d’enregistrement</td>
<td>N/A</td>
</tr>
</tbody>
</table>

### <a name="records"></a>Enregistrements

Les conteneurs renferment des bases de données, et à l’intérieur des bases de données sont des enregistrements. Les enregistrements sont le mécanisme dans laquelle les données structurées sont déplacées vers et à partir de CloudKit :

 [![](intro-to-cloudkit-images/image34.png "Les conteneurs renferment des bases de données, et à l’intérieur des bases de données sont des enregistrements")](intro-to-cloudkit-images/image34.png#lightbox)

Les enregistrements sont exposées dans le CloudKit Framework via la `CKRecord` classe qui encapsule des paires clé-valeur. Une instance d’un objet dans une application est équivalente à une `CKRecord` dans CloudKit. En outre, chaque `CKRecord` possède un type d’enregistrement, qui est équivalent à la classe d’un objet.

Enregistrements ont un schéma juste-à-temps, donc les données sont décrite à CloudKit avant d’être transmises pour le traitement. À partir de là, CloudKit interprétera les informations et gérer la logistique de stockage et la récupération de l’enregistrement.

La `CKRecord` classe prend également en charge un large éventail de métadonnées. Par exemple, un enregistrement contient des informations sur sa date de création et de l’utilisateur qui l’a créé. Un enregistrement contient également des informations sur quand il a été modifié et l’utilisateur qui l’a modifié.

Les enregistrements contiennent la notion d’une balise de modification. Il s’agit d’une version précédente d’une révision d’un enregistrement donné. La balise de modification est utilisée comme un moyen léger permettant de déterminer si le client et le serveur ont la même version d’un enregistrement donné.

Comme indiqué précédemment, `CKRecords` encapsuler des paires clé-valeur et par conséquent, les types de données suivants peuvent être stockées dans un enregistrement :

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


Outre les types de valeur unique, un enregistrement peut contenir un tableau homogène d’un des types répertoriés ci-dessus.

Le code suivant peut être utilisé pour créer un nouvel enregistrement et le stocker dans une base de données :

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>Zones de l’enregistrement

Enregistrements n’existent pas directement dans une base de données : groupes d’enregistrements sont à l’intérieur d’une Zone d’enregistrement. Zones de l’enregistrement peut être considérés comme des Tables dans une base de données relationnelle classique :

 [![](intro-to-cloudkit-images/image35.png "Groupes d’enregistrements sont à l’intérieur d’une Zone d’enregistrement")](intro-to-cloudkit-images/image35.png#lightbox)

Il peut y avoir plusieurs enregistrements dans une Zone d’enregistrement donnée et plusieurs Zones d’enregistrement au sein d’une base de données. Chaque base de données contient une Zone d’enregistrement par défaut :

 [![](intro-to-cloudkit-images/image36.png "Chaque base de données contient une Zone d’enregistrement par défaut et une Zone personnalisée")](intro-to-cloudkit-images/image36.png#lightbox)

Il s’agit dans lequel les enregistrements sont stockés par défaut. En outre, les Zones d’enregistrement personnalisés peuvent être créés. Représentent des Zones d’enregistrement la granularité de base auxquelles les validations atomique et le suivi des modifications est effectuée.

## <a name="record-identifiers"></a>Identificateurs d’enregistrement

Identificateurs d’enregistrement sont représentées sous forme de tuple, qui contient à la fois un client fourni nom de l’enregistrement et la zone dans laquelle se trouve l’enregistrement. Identificateurs d’enregistrement présentent les caractéristiques suivantes :

-  Ils sont créés par l’application cliente.
-  Ils sont entièrement normalisées et représentent l’emplacement spécifique de l’enregistrement.
-  En affectant l’ID Unique d’un enregistrement dans une base de données externe pour le nom de l’enregistrement, elles peuvent servir à relier des bases de données locales qui ne sont pas stockées dans CloudKit.


Lorsque les développeurs créent de nouveaux enregistrements, ils peuvent choisir de passer un identificateur d’enregistrement. Si un identificateur d’enregistrement n’est pas spécifié, un UUID est automatiquement créé et affecté à l’enregistrement.

Lorsque les développeurs créent de nouveaux identificateurs d’enregistrement, ils peuvent choisir de spécifier la Zone d’enregistrement qui appartiennent à chaque enregistrement. Si aucun n’est spécifié, la Zone d’enregistrement par défaut sera utilisée.

Identificateurs d’enregistrement sont exposées dans le CloudKit Framework via la `CKRecordID` classe. Le code suivant peut être utilisé pour créer un nouvel identificateur d’enregistrement :

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Références

Références fournissent des relations entre les enregistrements connexes dans une base de données :

 [![](intro-to-cloudkit-images/image37.png "Références fournissent des relations entre les enregistrements connexes dans une base de données")](intro-to-cloudkit-images/image37.png#lightbox)

Dans l’exemple ci-dessus, Parents possèdent des enfants afin que l’enfant est un enregistrement enfant de l’enregistrement parent. La relation passe de l’enregistrement enfant à l’enregistrement parent et qu’il est appelée un *référence arrière*.

Les références sont exposées dans le CloudKit Framework via la `CKReference` classe. Ils sont un moyen de permettre le serveur iCloud à comprendre la relation entre les enregistrements.

Références fournissent le mécanisme derrière les suppressions en cascade. Si un enregistrement parent est supprimé de la base de données, tous les enregistrements enfants (comme spécifié dans une relation) sont automatiquement supprimés de la base de données.

> [!NOTE]
> **Remarque**: non résolu les pointeurs sont possibles lorsque vous utilisez CloudKit. Par exemple, au moment où l’application a extrait une liste de pointeurs d’enregistrement, sélectionné un enregistrement et demandé pour l’enregistrement, l’enregistrement n’existe plus dans la base de données. Une application doit être programmée pour gérer cette situation en douceur.

Bien que non obligatoire, références arrière sont préférables lorsque vous travaillez avec l’infrastructure CloudKit. Apple a affiné le système pour rendre le type le plus efficace de référence.

Lorsque vous créez une référence, le développeur peut soit fournissent un enregistrement qui est déjà en mémoire, ou créer une référence à un identificateur d’enregistrement. Si à l’aide d’un identificateur d’enregistrement et la référence spécifiée n’existe pas dans la base de données, un pointeur non résolu est créé.

Voici un exemple de création d’une référence à un enregistrement connu :

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Ressources

Autoriser les ressources pour un fichier de données non structurées volumineux pour être téléchargé sur iCloud et associé à un enregistrement donné :

 [![](intro-to-cloudkit-images/image38.png "Autoriser les ressources pour un fichier de données non structurées volumineux pour être téléchargé sur iCloud et associé à un enregistrement donné")](intro-to-cloudkit-images/image38.png#lightbox)

Sur le client, un `CKRecord` est créé qui décrit le fichier qui vont être téléchargées sur le serveur iCloud. A `CKAsset` est créé pour contenir le fichier et est lié à l’enregistrement de description.

Lorsque le fichier est téléchargé vers le serveur, l’enregistrement est placé dans la base de données et le fichier est copié dans une base de données en bloc stockage spécial. Un lien est créé entre l’enregistrement de pointeur et le fichier téléchargé.

Sont exposées dans le CloudKit Framework via la `CKAsset` classe et sont utilisés pour stocker des données non structurées importants. Étant donné que le développeur souhaite jamais ont des données non structurées importants dans la mémoire, actifs sont implémentées à l’aide de fichiers sur le disque.

Actifs appartiennent à des enregistrements, ce qui permet les ressources à récupérer à partir d’iCloud à l’aide de l’enregistrement en tant que pointeur. De cette façon, le serveur peut actifs de collecter Garbage lors de l’enregistrement qui possède l’élément multimédia est supprimé.

Étant donné que `CKAssets` visent gérer les fichiers de données de grande taille, Apple conçu CloudKit pour charger et télécharger les ressources efficacement.

Le code suivant peut être utilisé pour créer un élément multimédia et l’associer à l’enregistrement :

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Nous avons maintenant couvert tous les objets fondamentaux dans CloudKit. Conteneurs sont associés aux applications et bases de données. Bases de données contiennent des enregistrements qui sont regroupées dans des Zones de l’enregistrement, désignés par des identificateurs d’enregistrement. Relations parent-enfant sont définies entre les enregistrements à l’aide de références. Enfin, les fichiers volumineux peuvent être téléchargés et associés à des enregistrements à l’aide des ressources.

## <a name="cloudkit-convenience-api"></a>API pratique de CloudKit

Apple offre deux différents ensembles d’API pour travailler avec CloudKit :

-  **API Operational** – offre toutes les fonctionnalités de CloudKit. Pour les applications plus complexes, cette API fournit un contrôle affiné sur CloudKit.
-  **API pratique** – offre un sous-ensemble commun et préconfiguré de CloudKit fonctionnalités. Fournit une solution d’accès simple et pratique pour inclure des fonctionnalités de CloudKit dans une application iOS.


L’API de commodité est généralement le meilleur choix pour la plupart des applications iOS et Apple suggère de démarrer avec lui. Le reste de cette section couvre les rubriques suivantes de la commodité API :

-  Sauvegarde d’un enregistrement.
-  Extraction d’un enregistrement.
-  Mise à jour un enregistrement.


### <a name="common-setup-code"></a>Le programme d’installation de Code

Mise en route avec l’API de commodité CloudKit, il est du code de programme d’installation standard requis. Démarrez en modifiant l’application `AppDelegate.cs` de fichier et le rendre l’aspect suivant :

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

Le code ci-dessus expose les bases de données CloudKit publics et privés en tant que raccourcis pour les rendre plus facile de travailler avec le reste de l’application.

Ensuite, ajoutez le code suivant à toute vue ou un conteneur d’affichage qui utiliseront CloudKit :

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Cela ajoute un raccourci pour accéder à la `AppDelegate` et accéder aux raccourcis de base de données publiques et privées créés ci-dessus.

Avec ce code en place, examinons une implémentation de l’API de commodité CloudKit dans une application iOS 8 de Xamarin.

### <a name="saving-a-record"></a>Sauvegarde d’un enregistrement

Utilisant le modèle présenté ci-dessus lorsque vous présentez des enregistrements, le code suivant sera créer un nouvel enregistrement et utiliser l’API de commodité à le pour enregistrer dans la base de données publique :

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Trois choses à noter concernant le code ci-dessus :

1.  En appelant le `SaveRecord` méthode de la `PublicDatabase`, le développeur n’a pas spécifier la façon dont les données sont envoyées, quel fuseau, il est écrit, etc. L’API de commodité est en prenant soin de tous ces détails lui-même.
1.  L’appel est asynchrone et fournit une routine de rappel lorsque l’appel se termine avec succès ou l’échec. Si l’appel échoue, un message d’erreur sera fourni.
1.  CloudKit ne fournit pas de stockage local/persistance ; Il est uniquement un support de transfert. Par conséquent, lorsqu’une demande est faite pour enregistrer un enregistrement, il est immédiatement envoyé aux serveurs iCloud.


> [!NOTE]
> **Remarque**: en raison de la nature de communications réseau mobile, où les connexions sont abandonnées en permanence ou interrompue, une des considérations premier, le développeur doit faire lors de l’utilisation de CloudKit est la gestion des erreurs « perte ».

### <a name="fetching-a-record"></a>Extraction d’un enregistrement

Avec un enregistrement est créé et stocké avec succès sur le serveur iCloud, utilisez le code suivant pour récupérer l’enregistrement :

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Comme dans l’enregistrement, le code ci-dessus est asynchrone, simple et nécessite la gestion des erreurs très.

### <a name="updating-a-record"></a>Mise à jour un enregistrement

Après qu’un enregistrement ont été récupéré à partir des serveurs iCloud, le code suivant peut être utilisé pour modifier l’enregistrement et enregistrer les modifications dans la base de données :

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

Le `FetchRecord` méthode de la `PublicDatabase` retourne un `CKRecord` si l’appel a réussi. L’application modifie l’enregistrement et les appels `SaveRecord` pour réécrire les modifications dans la base de données.

Cette section a montré le cycle typique qui utilise une application lorsque vous travaillez avec l’API de commodité CloudKit. L’application enregistrer dans iCloud, extraire ces enregistrements iCloud, modifier les enregistrements et enregistrer ces modifications dans iCloud.

## <a name="designing-for-scalability"></a>Conception de l’extensibilité

Cet article a étudié jusqu'à présent de stocker et extraire le modèle d’objet complet d’une application à partir des serveurs iCloud, chaque fois qu’il va être manipulées. Alors que cette approche fonctionne bien avec une petite quantité de données et un peu d’utilisateurs, il n’évolue pas bien lorsque la quantité d’informations ou pour un utilisateur de base augmente.

### <a name="big-data-tiny-device"></a>Données volumineuses, appareil minuscule

Devient le plus populaire une application, mais plus de données dans la base de données et il est le moins possible d’avoir un cache de données entiers sur l’appareil. Les techniques suivantes peuvent être utilisées pour résoudre ce problème :

-  **Conserver les données de grande taille dans le Cloud** – CloudKit a été conçu pour gérer efficacement les données de grande taille.
-  **Client doit uniquement afficher une tranche de données** – réduire au minimum de données nécessaires pour gérer n’importe quelle tâche à un moment donné.
-  **Peuvent modifier les vues client** – étant donné que chaque utilisateur possède diverses préférences, la tranche de données affichées permettre modifier l’utilisateur et de l’utilisateur individuel, vue d’un secteur donné peut être différent.
-  **Client utilise des requêtes pour vous concentrer le point de vue** – requêtes permettent à l’utilisateur d’afficher un petit sous-ensemble d’un plus grand jeu de données qui existe dans le Cloud.


### <a name="queries"></a>Requêtes

Comme indiqué ci-dessus, les requêtes permettent au développeur de sélectionner un sous-ensemble du jeu de données plus volumineux qui existe dans le Cloud. Les requêtes sont exposées dans le CloudKit Framework via la `CKQuery` classe.

Une requête combine trois éléments différents : un Type d’enregistrement ( `RecordType`), un prédicat ( `NSPredicate`) et, éventuellement, un descripteur de tri ( `NSSortDescriptors`). CloudKit prend en charge la plupart des `NSPredicate`.

#### <a name="supported-predicates"></a>Prédicats pris en charge

CloudKit prend en charge les types suivants de `NSPredicates` lorsque vous travaillez avec des requêtes :


1. Mise en correspondance les enregistrements dont le nom est égal à une valeur stockée dans une variable :
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. Permet la mise en correspondance s’appuie sur une valeur de clé dynamique, afin que la clé ne doit pas nécessairement être connaître au moment de la compilation :
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. Correspondance des enregistrements où la valeur de l’enregistrement est supérieure à la valeur donnée :
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Correspondance des enregistrements où emplacement de l’enregistrement est de 100 mètres de l’emplacement donné :
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit prend en charge une recherche sous forme de jeton. Cet appel crée deux jetons, un pour `after` et l’autre pour `session`. Elle retourne un enregistrement qui contient ces deux jetons :
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. Prend en charge CloudKit composés prédicats joints à l’aide de la `AND` opérateur.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>Création de requêtes

Le code suivant peut être utilisé pour créer un `CKQuery` dans une application iOS 8 de Xamarin :

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

Tout d’abord, il crée un prédicat pour sélectionner uniquement les enregistrements qui correspondent à un nom donné. Il crée ensuite une requête qui sélectionne les enregistrements du Type d’enregistrement donné qui correspondent au prédicat.

#### <a name="performing-a-query"></a>Exécution d’une requête

Une fois qu’une requête a été créée, utilisez le code suivant pour exécuter la requête et de traiter les enregistrements retournés :

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

Le code ci-dessus prend la requête créée ci-dessus et l’exécute sur la base de données Public. Dans la mesure où aucune Zone d’enregistrement n’est spécifié, toutes les zones de recherche sont effectuées. Si aucune erreur ne s’est produite, un tableau de `CKRecords` sera retourné correspondant aux paramètres de la requête.

La méthode pour réfléchir aux requêtes est qu’ils sont des sondages et sont une bonne solution pour le découpage des jeux de données volumineux. Les requêtes, toutefois, ne sont pas adaptés pour les jeux de données volumineux, généralement statiques pour les raisons suivantes :

-  Ils sont incorrects pour l’autonomie de l’appareil.
-  Ils sont incorrects pour le trafic réseau.
-  Ils sont incorrects pour une expérience utilisateur, car les informations qu’ils voient sont limitées par la fréquence à laquelle l’application est interrogation de la base de données. Aujourd'hui, les utilisateurs attendent des notifications push lorsque quelque chose change.


### <a name="subscriptions"></a>Abonnements

Lorsque vous traitez des jeux de données volumineux, généralement statique, la requête ne doit pas être effectuée sur le périphérique client, il doit s’exécuter sur le serveur sur le client. La requête doit s’exécuter en arrière-plan et doit être exécutée après chaque enregistrement est enregistré, par l’appareil en cours ou un autre périphérique toucher la même base de données.

Enfin, une notification push doit être envoyée à tous les périphériques attachés à la base de données lors de l’exécution de la requête côté serveur.

Les abonnements sont exposées dans le CloudKit Framework via la `CKSubscription` classe. Ils combinent un Type d’enregistrement ( `RecordType`), un prédicat ( `NSPredicate`) et une Notification Push d’Apple ( `Push`).

> [!NOTE]
> **Remarque**: CloudKit push est augmentées légèrement car elles contiennent une charge utile contenant des informations spécifiques CloudKit ce qui a provoqué le push de se produire.

#### <a name="how-subscriptions-work"></a>Fonctionnement des abonnements

Avant d’implémenter l’abonnement dans le code c#, prenons une rapide vue d’ensemble du fonctionnement des abonnements :

 [![](intro-to-cloudkit-images/image39.png "Une vue d’ensemble du fonctionnement des abonnements")](intro-to-cloudkit-images/image39.png#lightbox)

Le graphique ci-dessus illustre le processus d’inscription standard comme suit :

1.  L’appareil client crée un nouvel abonnement contenant le jeu de conditions qui déclencheront l’abonnement et une Notification Push qui sera envoyée lors de l’exécution du déclencheur.
2.  L’abonnement est envoyé à la base de données dans laquelle il est ajouté à la collection des abonnements existants.
3.  Un deuxième appareil crée un nouvel enregistrement et enregistre cet enregistrement à la base de données.
4.  Recherche de la base de données dans la liste des abonnements pour identifier si le nouvel enregistrement correspond à une de leurs conditions.
5.  Si une correspondance est trouvée, la Notification Push est envoyée à l’appareil qui inscrit l’abonnement avec des informations sur l’enregistrement qui a provoqué son déclenchement.


Avec cette base de connaissances en place, nous allons examiner à la création d’abonnements dans un Xamarin iOS 8 application.

#### <a name="creating-subscriptions"></a>Création d’abonnements

Le code suivant peut être utilisé pour créer un abonnement :

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

Tout d’abord, il crée un prédicat qui fournit la condition de déclenchement de l’abonnement. Ensuite, il crée l’abonnement par rapport à un Type d’enregistrement spécifique et définit l’option de lorsque le déclencheur est testé. Enfin, il définit le type de notification qui se produit lorsque l’abonnement est déclenchée et l’attache l’abonnement.

### <a name="saving-subscriptions"></a>L’enregistrement des abonnements

Avec l’abonnement créé, le code suivant l’enregistrerez sur la base de données :

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

À l’aide de l’API de commodité, l’appel est asynchrone, simple et fournit la gestion des erreurs facile.

#### <a name="handling-push-notifications"></a>Gestion des Notifications de Push

Si le développeur a utilisé précédemment Apple Push Notifications (APS), le processus de gestion des Notifications générées par CloudKit doit connaître.

Dans le `AppDelegate.cs`, remplacer la `ReceivedRemoteNotification` classe comme suit :

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

Le code ci-dessus demande CloudKit pour analyser l’userInfo dans une Notification CloudKit. Ensuite, les informations sont extraites sur l’alerte. Enfin, le type de notification est testé et la notification est gérée en conséquence.

Cette section a montré comment répondre le Big Data, des problèmes de périphérique minuscule présentées ci-dessus à l’aide de requêtes et des abonnements. L’application laisser ses données de grande taille dans le cloud et utiliser ces technologies pour proposer des vues dans ce jeu de données.

## <a name="cloudkit-user-accounts"></a>Comptes d’utilisateur CloudKit

Comme mentionné au début de cet article, CloudKit repose sur l’infrastructure iCloud. La section suivante décrit, en détail, comment les comptes sont exposés à un développeur à l’aide de l’API CloudKit.

### <a name="authentication"></a>Authentification

Lorsque vous traitez des comptes d’utilisateur, le premier point important est l’authentification. CloudKit prend en charge l’authentification via l’utilisateur actuellement connecté dans iCloud sur l’appareil. L’authentification a lieu en arrière-plan et est gérée par iOS. De cette façon, les développeurs n’ont jamais à vous soucier des détails d’implémentation de l’authentification. Ils uniquement testent pour voir si un utilisateur est connecté.

### <a name="user-account-information"></a>Informations de compte d’utilisateur

CloudKit fournit les informations utilisateur suivantes au développeur :

-  **Identité** – un moyen d’identifier l’utilisateur.
-  **Métadonnées** – la possibilité d’enregistrer et récupérer des informations sur les utilisateurs.
-  **Confidentialité** – toutes les informations sont gérées dans un mode consciente de confidentialité. Rien n’est exposé à moins que l’utilisateur a consenti à ce dernier.
-  **Découverte** – fournit aux utilisateurs la possibilité de découvrir ses amis qui utilisent la même application.


Ensuite, nous allons nous intéresser à ces rubriques en détail.

#### <a name="identity"></a>identité

Comme indiqué ci-dessus, CloudKit fournit un moyen de l’application identifier de façon unique un utilisateur donné :

 [![](intro-to-cloudkit-images/image40.png "Identifie de façon unique identifier un utilisateur donné")](intro-to-cloudkit-images/image40.png#lightbox)

Il existe une application cliente en cours d’exécution sur les périphériques d’un utilisateur et toutes les bases de données utilisateur privées spécifiques à l’intérieur du conteneur CloudKit. L’application cliente va être lié à un de ces utilisateurs spécifiques. Il est basé sur l’utilisateur qui est connecté à iCloud localement sur l’appareil.

Car il provient d’iCloud, il existe une riche de stocker les informations d’utilisateur de sauvegarde. Et car iCloud réellement héberge le conteneur, il peut faire correspondre des utilisateurs. Dans le graphique ci-dessus, l’utilisateur dont le compte iCloud `user@icloud.com` est lié au client en cours.

Chaque conteneur en conteneur, un ID d’utilisateur unique, généré de manière aléatoire est créé et associé au compte iCloud de l’utilisateur (adresse de messagerie). Cet ID utilisateur est renvoyé à l’application et peut être utilisé dans le développeur convient.

> [!NOTE]
> **Remarque**: différentes applications en cours d’exécution sur le même périphérique pour le même utilisateur iCloud aura des ID utilisateur différents, car ils sont connectés à différents conteneurs CloudKit.

Le suivant code obtient l’ID d’utilisateur CloudKit pour actuellement connecté dans utilisateur iCloud sur l’appareil :

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

Le code ci-dessus est demandant le conteneur CloudKit pour fournir l’ID de l’utilisateur actuellement connecté. Étant donné que cette information provient d’iCloud serveur, l’appel est asynchrone et la gestion des erreurs est requise.

#### <a name="metadata"></a>Métadonnées

Chaque utilisateur CloudKit possède des métadonnées spécifiques qui les décrit. Ces métadonnées sont représentée comme un enregistrement CloudKit :

 [![](intro-to-cloudkit-images/image41.png "Chaque utilisateur CloudKit possède des métadonnées spécifiques qui les décrit")](intro-to-cloudkit-images/image41.png#lightbox)

À l’intérieur de la base de données privée pour un utilisateur spécifique d’un conteneur, il est un enregistrement qui définit cet utilisateur. Il existe plusieurs enregistrements d’utilisateur à l’intérieur de la base de données publique, une pour chaque utilisateur du conteneur. Un de ces aura un ID d’enregistrement qui correspond à l’ID d’enregistrement de l’utilisateur actuellement connecté

Enregistrements d’utilisateur dans la base de données publics sont world lisible. Ils sont traités par la plupart des cas, comme un enregistrement ordinaire et avoir un type de `CKRecordTypeUserRecord`. Ces enregistrements sont réservés par le système et ne sont pas disponibles pour les requêtes.

Le code suivant permet d’accéder à un enregistrement d’utilisateur :

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

Le code ci-dessus est demandant de retourner l’enregistrement d’utilisateur pour l’utilisateur qui est l’ID nous accessible au-dessus de la base de données Public. Étant donné que cette information provient d’iCloud serveur, l’appel est asynchrone et la gestion des erreurs est requise.

#### <a name="privacy"></a>Confidentialité

CloudKit a été de conception, par défaut, pour protéger la confidentialité de l’utilisateur actuellement connecté. Aucune information d’identification personnelle sur l’utilisateur n’est exposée par défaut. Il existe certains cas où l’application a besoin des informations limitées sur l’utilisateur.

Dans ce cas, l’application peut demander que l’utilisateur divulguer ces informations. Une boîte de dialogue s’affiche à l’utilisateur lui demandant de participer à l’exposition de leurs informations de compte.

#### <a name="discovery"></a>découverte,

En supposant que l’utilisateur en tant que choisi d’autoriser l’application un accès limité à leurs informations de compte d’utilisateur, ils peuvent être détectables à d’autres utilisateurs de l’application :

 [![](intro-to-cloudkit-images/image42.png "Un utilisateur peut être détectable à d’autres utilisateurs de l’application")](intro-to-cloudkit-images/image42.png#lightbox)

L’application cliente communique avec un conteneur, et iCloud pour accéder aux informations d’utilisateur communique avec le conteneur. L’utilisateur peut fournir une adresse de messagerie et de découverte peut être utilisée pour obtenir des informations relatives à l’utilisateur précédent. Si vous le souhaitez, l’ID d’utilisateur peut également servir à découvrir des informations sur l’utilisateur.

CloudKit fournit également un moyen de découvrir des informations sur tous les utilisateurs peuvent être des amis d’actuellement connecté sur utilisateur iCloud en interrogeant le carnet d’adresses entier. Le processus de CloudKit extraire dans le livre de Contact de l’utilisateur et utiliser les adresses de messagerie pour voir s’il peut trouver d’autres utilisateurs de l’application qui correspondent à ces adresses.

Cela permet à l’application tirer parti du livre de Contact de l’utilisateur sans fournir l’accès à ce dernier ou invite l’utilisateur à approuver l’accès aux contacts. À aucun moment les informations de contact sera disponibles à l’application, seul le processus CloudKit a accès.

Pour résumer, il existe trois différents types d’entrées disponibles pour la découverte d’utilisateur :

-  **ID d’enregistrement utilisateur** – découverte peut être effectuée par rapport à l’ID d’utilisateur d’actuellement connecté dans CloudKit utilisateur.
-  **Adresse de messagerie utilisateur** : l’utilisateur peut fournir une adresse de messagerie, et il peut être utilisé pour la découverte.
-  **Contactez Book** – carnet d’adresses de l’utilisateur peut servir à rechercher des utilisateurs de l’application qui ont la même adresse de messagerie, comme indiqué dans leurs contacts.


Découverte d’utilisateurs retournera les informations suivantes :

-  **ID d’enregistrement utilisateur** -l’ID unique d’un utilisateur dans la base de données Public.
-  **Premier et le dernier nom** - tel que stocké dans la base de données Public.


Ces informations seront affichera uniquement pour les utilisateurs qui ont choisi dans la découverte.

Le code suivant permet de découvrir plus d’informations sur l’utilisateur actuellement connecté dans iCloud sur l’appareil :

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

Utilisez le code suivant pour interroger tous les utilisateurs dans le livre de Contact :

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

Dans cette section, nous avons couvert les quatre zones principales de l’accès à un compte d’utilisateur qui peut fournir des CloudKit à une application. À partir de la mise en route d’identité et les métadonnées de l’utilisateur, les stratégies de confidentialité qui sont intégrées à CloudKit et enfin, la possibilité de découvrir d’autres utilisateurs de l’application.

## <a name="the-development-and-production-environments"></a>Le développement et les environnements de Production

CloudKit fournit des environnements de développement et de Production distincts pour les types d’enregistrements et les données d’une application. L’environnement de développement est un environnement plus flexible qui est disponible uniquement pour les membres d’une équipe de développement. Lorsqu’une application ajoute un nouveau champ à un enregistrement et l’enregistre dans l’environnement de développement, le serveur met automatiquement à jour les informations de schéma.

Le développeur peut utiliser cette fonctionnalité pour apporter des modifications à un schéma pendant le développement, ce qui fait gagner du temps. L’inconvénient est qu’après l’ajout d’un champ à un enregistrement, le type de données associé à ce champ ne peut pas être modifié par programmation. Pour modifier un type de champ, le développeur doit supprimer le champ dans [CloudKit le tableau de bord](https://icloud.developer.apple.com/dashboard/) et ajoutez-le de nouveau avec le nouveau type.

Avant de déployer l’application, le développeur peut migrer leur schéma et les données dans l’environnement de production avec **CloudKit le tableau de bord**. Lors de l’exécution par rapport à l’environnement de Production, le serveur empêche une application à partir de la modification du schéma par programme. Le développeur peut toujours modifier avec **CloudKit le tableau de bord** mais tente d’ajouter des champs à un enregistrement dans le résultat de l’environnement de Production des erreurs.

> [!NOTE]
> **Remarque :** iOS Simulator fonctionne uniquement avec les **environnement de développement**. Lorsque le développeur est prêt à tester une application dans un **environnement de Production**, un appareil iOS physiques est nécessaire.


## <a name="shipping-a-cloudkit-enabled-app"></a>Copie un CloudKit activé l’application

Avant de livrer une application qui utilise CloudKit, elle devra être configuré pour cibler le **environnement de Production CloudKit** ou l’application est rejetée par Apple.

Effectuez ce qui suit :

1. Dans Visual Studio pour l’agent de gestion, compilez l’application pour **version** > **appareil iOS**: 

    [![](intro-to-cloudkit-images/shipping01.png "Compilez l’application pour la mise en production")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. À partir de la **générer** menu, sélectionnez **Archive**: 

    [![](intro-to-cloudkit-images/shipping02.png "Sélectionnez l’Archive")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. Le **Archive** est créé et affiché dans Visual Studio pour Mac : 

    [![](intro-to-cloudkit-images/shipping03.png "L’Archive est créée et affichée")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Démarrez **Xcode**.
5. À partir de la **fenêtre** menu, sélectionnez **organisateur**: 

    [![](intro-to-cloudkit-images/shipping04.png "Sélectionnez la bibliothèque multimédia")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Sélectionnez l’archive de l’application et cliquez sur le **exporter...**  bouton : 

    [![](intro-to-cloudkit-images/shipping05.png "Archive de l’application")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. Sélectionnez une méthode pour l’exportation, cliquez sur le **suivant** bouton : 

    [![](intro-to-cloudkit-images/shipping06.png "Sélectionnez une méthode d’exportation")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Sélectionnez le **équipe de développement** dans la liste déroulante et cliquez sur le **choisir** bouton : 

    [![](intro-to-cloudkit-images/shipping07.png "Sélectionnez l’équipe de développement dans la liste déroulante")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Sélectionnez **Production** dans la liste déroulante et cliquez sur le **suivant** bouton : 

    [![](intro-to-cloudkit-images/shipping08.png "Sélectionnez Production dans la liste déroulante")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. Passez en revue le paramètre et cliquez sur le **exporter** bouton : 

    [![](intro-to-cloudkit-images/shipping09.png "Passez en revue les paramètres")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Choisissez un emplacement pour générer l’application résultante `.ipa` fichier.

Le processus est similaire pour l’envoi de l’application directement dans iTunes Connect, cliquez simplement sur le **envoyer...**  bouton au lieu de l’exportation... après la sélection d’une Archive dans la fenêtre de la bibliothèque multimédia.

## <a name="when-to-use-cloudkit"></a>Quand utiliser CloudKit

Comme nous l’avons vu dans cet article, CloudKit fournit un moyen simple pour une application stocker et récupérer des informations à partir des serveurs iCloud. Cela étant dit, CloudKit ne pas obsolète ou désapprouver un des outils existants ou des infrastructures.

### <a name="use-cases"></a>Cas d’usage

Les cas d’usage suivant doivent permettre au développeur de déterminer quand utiliser une infrastructure d’iCloud spécifique ou de la technologie :

-  **iCloud magasin clé-valeur** – de façon asynchrone maintient une petite quantité de données à jour et est très utile pour travailler avec les préférences de l’application. Toutefois, il est limité pour une petite quantité d’informations.
-  **iCloud lecteur** : iCloud existant conçues pour les API de Documents et fournit une API simple pour synchroniser des données non structurées à partir du système de fichiers. Il fournit un cache hors connexion complet sur Mac OS X et est idéal pour les applications basées sur les documents.
-  **Données de base d’iCloud** – permet aux données de réplication entre tous les périphériques de l’utilisateur. Les données sont utilisateur unique et idéal pour garder privé données structurées synchronisées.
-  **CloudKit** : fournit des données publiques à la fois structure et en bloc et est capable de gérer le jeu de données volumineux et des fichiers volumineux non structurées. Son liée à l’utilisateur compte iCloud et fournit client dirigé de transfert de données.


Conservation de ces cas d’usage à l’esprit, le développeur doit choisir la technologie iCloud correct pour fournir à la fois la fonctionnalité actuelle de l’application requise et de fournir une bonne évolutivité pour une croissance future.

## <a name="summary"></a>Récapitulatif

Cet article a couvert une introduction rapide à l’API CloudKit. Il a montré comment approvisionner et configurer une application iOS de Xamarin à utiliser CloudKit. Il a couvert les fonctionnalités de l’API de commodité CloudKit. Il a montré comment concevoir un CloudKit activé l’application pour l’évolutivité à l’aide de requêtes et des abonnements. Et enfin, il a affiché les informations de compte d’utilisateur qui sont exposées à une application par CloudKit.

## <a name="related-links"></a>Liens associés

- [CloudKitAtlas (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [Introduction à iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Création d’un profil de configuration](~/ios/get-started/installation/device-provisioning/index.md)
