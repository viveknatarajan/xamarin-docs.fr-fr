---
title: 'Xamarin.Essentials : Assistances de système de fichier'
description: La classe FileSystem dans Xamarin.Essentials contient une série de programmes d’assistance pour rechercher le cache d’application et des répertoires de données et ouvrir des fichiers à l’intérieur du package d’application.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815616"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials : Assistances de système de fichier

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **FileSystem** contient une série de programmes d’assistance pour trouver les répertoires de cache et les données de l’application et ouvrir des fichiers à l’intérieur du package d’application.

## <a name="using-file-system-helpers"></a>Utiliser le système de fichiers

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour obtenir le répertoire de l’application pour stocker les **mettre en cache données**. Les données de cache peuvent être utilisées pour toutes les données qui devant conserver plus longtemps que les données temporaires, mais ne doivent pas être données requises pour fonctionner correctement.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Pour obtenir le répertoire de niveau supérieur de l’application pour tous les fichiers qui ne sont pas des fichiers de données utilisateur. Ces fichiers sont sauvegardés avec le système d’exploitation que la synchronisation de framework. Consultez les caractéristiques de mise en œuvre la plateforme ci-dessous.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Pour ouvrir un fichier qui est fourni dans le package d’application :

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – retourne le [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) du contexte actuel.
- **AppDataDirectory** – retourne le [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) du contexte actuel et sont sauvegardées à l’aide [sauvegarde automatique](https://developer.android.com/guide/topics/data/autobackup.html) démarrage sur les API 23 et versions ultérieures.

Ajouter n’importe quel fichier dans le dossier **actifs** Android du projet et marquer l’Action de génération comme **AndroidAsset** à utiliser avec `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – retourne le [bibliothèque/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) directory.
- **AppDataDirectory** – retourne le [bibliothèque](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) directory est sauvegardée par iTunes et iCloud.

Ajouter n’importe quel fichier dans le dossier **ressources** sur le site iOS de projet et marquer l’Action de génération comme **BundledResource** à utiliser avec `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – retourne le [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) répertoire...
- **AppDataDirectory** – retourne le [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) répertoire qui est sauvegardée dans le cloud.

Ajouter n’importe quel fichier dans la racine du projet UWP et marquer l’Action de génération comme **contenu** à utiliser avec `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Code source du fichier assistances de système](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Documentation sur les API de système de fichiers](xref:Xamarin.Essentials.FileSystem)
