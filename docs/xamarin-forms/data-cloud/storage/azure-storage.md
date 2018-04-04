---
title: Le stockage et l’accès aux données dans le stockage Azure
description: Stockage Azure est une solution de stockage cloud évolutives qui peut être utilisée pour stocker des données structurées et non structurées. Cet article explique comment utiliser Xamarin.Forms pour stocker les données texte et binaires dans le stockage Azure et comment accéder aux données.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 63afeec81eff350b034e8dd3a13da52801937826
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Le stockage et l’accès aux données dans le stockage Azure

_Stockage Azure est une solution de stockage cloud évolutives qui peut être utilisée pour stocker des données structurées et non structurées. Cet article explique comment utiliser Xamarin.Forms pour stocker les données texte et binaires dans le stockage Azure et comment accéder aux données._

## <a name="overview"></a>Vue d'ensemble

Le stockage Azure fournit quatre services de stockage :

- Stockage d’objets BLOB. Un objet blob peut être des données texte ou binaire, tels que les sauvegardes, les ordinateurs virtuels, les fichiers multimédias ou les documents.
- Stockage de table est un magasin d’attribut de clé NoSQL.
- Stockage de file d’attente est un service de messagerie pour le traitement de flux de travail et la communication entre les services de cloud computing.
- Stockage de fichiers fournit un stockage partagé en utilisant le protocole SMB.

Il existe deux types de comptes de stockage :

- Un compte de stockage à usage général fournit l’accès aux services de stockage Azure à partir d’un seul compte.
- Un compte de stockage d’objets Blob est un compte de stockage spécialisé pour le stockage BLOB. Ce type de compte est recommandé lorsque vous avez besoin uniquement stocker des données blob.

Cet article et qui l’accompagne, exemple d’application illustre le téléchargement de fichiers image et texte pour le stockage d’objets blob et les télécharger. En outre, il montre également la récupération d’une liste de fichiers depuis le stockage blob et suppression de fichiers.

Pour plus d’informations sur le stockage Azure, consultez [Introduction au stockage](https://azure.microsoft.com/documentation/articles/storage-introduction/).

## <a name="introduction-to-blob-storage"></a>Introduction au stockage d’objets Blob

Stockage d’objets BLOB se compose de trois composants, qui sont affichés dans le diagramme suivant :

![](azure-storage-images/blob-storage.png "Concepts liés au stockage BLOB")

Tous les accès au stockage Azure sont via un compte de stockage. Un compte de stockage peut contenir un nombre illimité de conteneurs, et un conteneur peut stocker un nombre illimité d’objets BLOB, jusqu'à la limite de capacité du compte de stockage.

Un objet blob est un fichier de tout type et la taille. Le stockage Azure prend en charge trois types d’objets blob différents :

- Objets BLOB de blocs est optimisés pour la diffusion en continu et le stockage d’objets de cloud et constitue un bon choix pour le stockage des sauvegardes, des fichiers multimédias, etc. de documents. Objets BLOB de blocs peut être jusqu'à 195 go.
- Ajouter les objets BLOB sont semblables aux objets BLOB de blocs, mais sont optimisés pour ajouter des opérations, telles que la journalisation. Ajouter des objets BLOB peuvent être jusqu'à 195 go.
- Objets BLOB de pages est optimisés pour les opérations de lecture/écriture fréquentes et est généralement utilisés pour stocker des ordinateurs virtuels et leurs disques. Objets BLOB de pages peut être jusqu'à 1 To.

> [!NOTE]
> Notez que les comptes de stockage d’objets blob prennent en charge de bloc et ajouter des objets BLOB, mais pas objets BLOB de pages.

Et un objet blob est téléchargé vers Azure Storage, téléchargé depuis le stockage Azure, sous la forme d’un flux d’octets. Par conséquent, les fichiers doivent être convertis en un flux d’octets avant le téléchargement et l’arrière converti en leur représentation sous forme d’origine après le téléchargement.

Chaque objet qui est stocké dans le stockage Azure a une adresse URL unique. Le nom de compte de stockage constitue le sous-domaine de cette adresse et la combinaison des formes de nom de sous-domaine et le domaine une *point de terminaison* pour le compte de stockage. Par exemple, si votre compte de stockage nommé *mystorageaccount*, le point de terminaison du blob par défaut pour le compte de stockage `https://mystorageaccount.blob.core.windows.net`.

L’URL pour accéder à un objet dans un compte de stockage est créé en ajoutant l’emplacement de l’objet dans le compte de stockage pour le point de terminaison. Par exemple, une adresse de l’objet blob aura le format `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`.

## <a name="setup"></a>Installation

Le processus d’intégration d’un compte de stockage Azure dans une application de Xamarin.Forms est la suivante :

1. Créer un compte de stockage. Pour plus d’informations, consultez [créer un compte de stockage](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Ajouter le [bibliothèque cliente de stockage Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) à l’application de Xamarin.Forms.
1. Configurer la chaîne de connexion de stockage. Pour plus d’informations, consultez [connexion au stockage Azure](#connecting).
1. Ajouter `using` des directives pour la `Microsoft.WindowsAzure.Storage` et `Microsoft.WindowsAzure.Storage.Blob` espaces de noms pour les classes qui accéderont à Azure Storage.

> [!NOTE]
> Bien que cet exemple utilise un projet d’accès partagé, la bibliothèque cliente de stockage Azure maintenant prend également en charge consommé à partir d’un projet de bibliothèque de classes Portable (PCL).

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Connexion au stockage Azure

Chaque demande adressée aux ressources de compte de stockage doit être authentifiée. Alors que les objets BLOB peut être configuré pour prendre en charge l’authentification anonyme, il existe deux approches principales, qu'une application peut utiliser pour s’authentifier avec un compte de stockage :

- Clé partagée. Cette approche utilise le stockage Azure compte et le nom de clé de compte pour accéder aux services de stockage. Un compte de stockage est affecté de deux clés privées lors de la création qui peut être utilisé pour l’authentification de clé partagée.
- Signature d’accès partagé. Il s’agit d’un jeton qui peut être ajouté à une URL qui permet l’accès délégué à une ressource de stockage, avec les autorisations qu’il indique, pour la période de temps qu’il est valide.

Chaînes de connexion peuvent être spécifiées qui incluent les informations d’authentification requises pour accéder aux ressources de stockage Azure à partir d’une application. En outre, une chaîne de connexion peut être configurée pour se connecter à l’émulateur de stockage Azure à partir de Visual Studio.

> [!NOTE]
> Le stockage Azure prend en charge HTTP et HTTPS dans une chaîne de connexion. Toutefois, il est recommandé d’utiliser le protocole HTTPS.

### <a name="connecting-to-the-azure-storage-emulator"></a>Connexion à l’émulateur de stockage Azure

L’émulateur de stockage Azure fournit un environnement local qui émule les objets blob Azure, file d’attente et les services de table à des fins de développement.

La chaîne de connexion doit être utilisée pour se connecter à l’émulateur de stockage Azure :

```csharp
UseDevelopmentStorage=true
```

Pour plus d’informations sur l’émulateur de stockage Azure, consultez [utiliser l’émulateur de stockage Azure pour le développement et test](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Connexion au stockage Azure à l’aide d’une clé partagée

Le format de chaîne de connexion suivant doit être utilisé pour se connecter au stockage Azure avec une clé partagée :

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` doit être remplacé par le nom de votre compte de stockage et `myAccountKey` doit être remplacé par un de vos deux clés d’accès de compte.

> [!NOTE]
> À l’aide de lorsque partagé clé d’authentification, votre nom de compte et votre clé de compte sera distribué à chaque personne qui utilise votre application, qui fournit l’accès complet en lecture/écriture au compte de stockage. Par conséquent, utilisez l’authentification par clé partagée uniquement à des fins de test et jamais les distribuer à d’autres utilisateurs.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Connexion au stockage Azure à l’aide d’une Signature d’accès partagé

Le format de chaîne de connexion suivante permet de se connecter au stockage Azure avec un SAS :

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` doit être remplacé par l’URL de votre point de terminaison d’objet blob, et `mySharedAccessSignature` doit être remplacé par votre SAS. Les associations de sécurité fournit le protocole, le point de terminaison de service et les informations d’identification pour accéder à la ressource.

> [!NOTE]
> L’authentification SAP est recommandée pour les applications de production. Toutefois, dans une application de production, les associations de sécurité doivent être récupérées à partir d’une serveur principal service à la demande, au lieu de sera fourni avec l’application.

Pour plus d’informations sur les Signatures d’accès partagé, consultez [à l’aide de Signatures SAP (accès partagé)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Création d’un conteneur

Le `GetContainer` méthode est utilisée pour récupérer une référence à un conteneur nommé, qui peut ensuite être utilisée pour récupérer des objets BLOB du conteneur ou pour ajouter des objets BLOB au conteneur. Le code suivant exemple illustre la `GetContainer` méthode :

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

Le `CloudStorageAccount.Parse` méthode analyse une chaîne de connexion et retourne un `CloudStorageAccount` instance qui représente le compte de stockage. A `CloudBlobClient` instance, ce qui permet de récupérer les conteneurs et objets BLOB, est ensuite créé par le `CreateCloudBlobClient` (méthode). Le `GetContainerReference` méthode récupère le conteneur spécifié comme un `CloudBlobContainer` de l’instance, avant d’être retournée à la méthode appelante. Dans cet exemple, le nom du conteneur est le `ContainerType` valeur d’énumération, converti en une chaîne en minuscules.

> [!NOTE]
> Les noms de conteneur doivent être en minuscules et doivent commencer par une lettre ou un chiffre. En outre, ils ne peuvent contenir que des lettres, des chiffres et des tirets et doivent être comprise entre 3 et 63 caractères.

Le `GetContainer` méthode est appelée comme suit :

```csharp
var container = GetContainer(containerType);
```

Le `CloudBlobContainer` instance peut ensuite être utilisée pour créer un conteneur, s’il n’existe pas :

```csharp
await container.CreateIfNotExistsAsync();
```

Par défaut, un conteneur nouvellement créé est privé. Cela signifie qu’une clé d’accès de stockage doit être spécifiée pour récupérer des objets BLOB du conteneur. Pour plus d’informations sur les objets BLOB dans un conteneur de public, consultez [créer un conteneur](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Chargement des données dans un conteneur

Le `UploadFileAsync` méthode permet de télécharger un flux de données de type octet pour le stockage d’objets blob et est indiqué dans l’exemple de code suivant :

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

Après avoir récupéré une référence de conteneur, la méthode crée le conteneur s’il n’existe pas. Un nouveau `Guid` est ensuite créé pour agir en tant que nom d’objet blob unique, et une référence de bloc d’objets blob est récupérée sous la forme un `CloudBlockBlob` instance. Le flux de données est ensuite téléchargé sur l’objet blob à l’aide de la `UploadFromStreamAsync` (méthode), ce qui crée l’objet blob s’il n’existe pas déjà, ou le remplace s’il existe.

Avant de peut télécharger un fichier à l’aide de cette méthode de stockage d’objets BLOB, il doit tout d’abord être converti en un flux d’octets. Cela est illustré dans l’exemple de code suivant :

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

Le `text` données sont converties en un tableau d’octets, qui est ensuite encapsulé sous forme de flux qui est transmise à la `UploadFileAsync` (méthode).

## <a name="downloading-data-from-a-container"></a>Téléchargement des données à partir d’un conteneur

Le `GetFileAsync` (méthode) est utilisée pour transférer des données blob depuis le stockage Azure et est indiquée dans l’exemple de code suivant :

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

Après avoir récupéré une référence de conteneur, la méthode récupère une référence d’objet blob pour les données stockées. Si l’objet blob existe, ses propriétés sont récupérées par le `FetchAttributesAsync` (méthode). Un tableau d’octets de la taille correcte est créé, et l’objet blob est téléchargé sous la forme d’un tableau d’octets qui obtient retourné à la méthode d’appel.

Après avoir téléchargé les données blob d’octets, il doit être converti en sa représentation sous forme d’origine. Cela est illustré dans l’exemple de code suivant :

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Le tableau d’octets est récupéré à partir du stockage Azure par le `GetFileAsync` chaîne encodée de méthode, avant qu’il est converti en un UTF-8.

## <a name="listing-data-in-a-container"></a>Affichage des données dans un conteneur

Le `GetFilesListAsync` méthode est utilisée pour récupérer une liste d’objets BLOB stockés dans un conteneur et est indiqué dans l’exemple de code suivant :

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

Après avoir récupéré une référence de conteneur, la méthode utilise du conteneur `ListBlobsSegmentedAsync` pour récupérer les références aux objets BLOB du conteneur. Les résultats retournés par la `ListBlobsSegmentedAsync` méthode sont énumérées pendant que le `BlobContinuationToken` instance n’est pas `null`. Chaque objet blob est effectué à partir de retourné `IListBlobItem` à un `CloudBlockBlob` dans l’accès ordre la `Name` propriété de l’objet blob, avant qu’il soit la valeur est ajoutée à la `allBlobsList` collection. Une fois la `BlobContinuationToken` instance est `null`, le nom de l’objet blob a été renvoyé et l’exécution quitte la boucle.

## <a name="deleting-data-from-a-container"></a>Suppression de données dans un conteneur

Le `DeleteFileAsync` méthode est utilisée pour supprimer un objet blob à partir d’un conteneur et est indiqué dans l’exemple de code suivant :

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Après avoir récupéré une référence de conteneur, la méthode récupère une référence d’objet blob pour l’objet blob spécifié. L’objet blob est ensuite supprimé avec le `DeleteIfExistsAsync` (méthode).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser Xamarin.Forms pour stocker les données texte et binaires dans le stockage Azure et comment accéder aux données. Stockage Azure est une solution de stockage cloud évolutives qui peut être utilisée pour le stockage des données structurées et non structurées.


## <a name="related-links"></a>Liens associés

- [Azure Storage (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Introduction au stockage](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Comment utiliser le stockage d’objets Blob à partir de Xamarin](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [À l’aide de Signatures d’accès partagé (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/)
