---
title: Types de système de fichiers de Xamarin.Essentials
description: La classe de système de fichiers contient une série de programmes d’assistance pour trouver le cache de l’application et les répertoires de données et ouvrir des fichiers dans le package d’application.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 871d164df982d1d170e8ba5bffd3bd6600a4cdda
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>Types de système de fichiers de Xamarin.Essentials

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **FileSystem** classe contient une série de programmes d’assistance pour rechercher les répertoires de données et du cache de l’application et ouvrir des fichiers dans le package d’application.

## <a name="using-file-system-helpers"></a>À l’aide des types de système de fichiers

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour obtenir le répertoire de l’application pour stocker **mettre en cache des données**. Les données de cache peuvent être utilisées pour toutes les données qui doivent être conservés plus longtemps que les données temporaires, mais ne doivent pas être données requises pour fonctionner correctement.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Pour obtenir des diredctory de niveau supérieur de l’application pour tous les fichiers qui ne sont pas des fichiers de données utilisateur. Ces fichiers sont sauvegardés avec le système d’exploitation, la synchronisation de framework. Consultez les caractéristiques d’implémentation plateforme ci-dessous.

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

## <a name="platform-implementation-specifics"></a>Caractéristiques d’implémentation de plate-forme

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** : retourne la [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) du contexte actuel.
- **AppDataDirectory** : retourne la [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) du contexte actuel et sont sauvegardées à l’aide de [Autu sauvegarde](https://developer.android.com/guide/topics/data/autobackup.html) à partir des API 23 et versions ultérieures.

Ajouter tous les fichiers dans le **actifs** dossier dans le Android de projet et sélectionnez l’Action de génération en tant que **AndroidAsset** à utiliser avec `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** : retourne la [bibliothèque/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) active.
- **AppDataDirectory** : retourne la [bibliothèque](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) active qui est sauvegardé par iTunes et iCloud.

Ajouter tous les fichiers dans le **ressources** dossier dans le fichier iOS de projet et sélectionnez l’Action de génération en tant que **BundledResource** à utiliser avec `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** : retourne la [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) active...
- **AppDataDirectory** : retourne la [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) répertoire qui est sauvegardée dans le cloud.

Ajouter un fichier à la racine dans le projet UWP et marquer en tant que l’Action de génération **contenu** à utiliser avec `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Code de source de programmes d’assistance du système de fichiers](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Documentation sur les API de système de fichiers](xref:Xamarin.Essentials.FileSystem)
