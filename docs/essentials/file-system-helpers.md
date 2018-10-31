---
title: "Xamarin.Essentials : Programmes d'assistance du système de fichiers"
description: La classe **FileSystem** contient une série de programmes d'assistance pour trouver les répertoires de données et de cache de l'application et ouvrir les fichiers du package de l'application.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815616"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials : Programmes d'assistance du système de fichiers

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **FileSystem** contient un ensemble de fonctions facilitant l'accès et l'édition des répertoires de cache et de données de l'application ainsi que l'ouverture de fichiers embarqués dans le package d'application.

## <a name="using-file-system-helpers"></a>Utilisation de **FileSystem**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour que le répertoire de l'application stocke les données du cache. Ces dernières peuvent être utilisées pour toutes les données qui doivent persister plus longtemps que des données temporaires, mais ne doivent pas être des données requises pour un fonctionnement correct.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Pour que le répertoire de niveau supérieur de l'application stocke tous les fichiers qui ne sont pas des fichiers de données utilisateur. Ces fichiers sont sauvegardés avec le système d'exploitation. Voir Spécificités de mise en oeuvre par plateforme.

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

## <a name="platform-implementation-specifics"></a>Informations d'implémentation de la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – retourne le [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) du contexte actuel.
- **AppDataDirectory** : retourne le [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) du contexte actuel qui est sauvegardé lors de la [sauvegarde automatique](https://developer.android.com/guide/topics/data/autobackup.html) , disponible à partir de l'API 23.

Ajoutez vos fichiers dans le dossier **Assets** de votre solution Android. Marquez ces fichiers avec l'action de génération **AndroidAsset**à utiliser avec `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – retourne le répertoire [LocalCacheFolder](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)...
- **AppDataDirectory** – retourne le répertoire [LocalFolder](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) qui est sauvegardée dans le cloud.

Ajoutez vos fichiers dans le dossier **ressources** de votre solution iOS. Marquez ces fichiers avec l'action de génération **BundledResource**à utiliser avec `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – retourne le [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) répertoire...
- **AppDataDirectory** – retourne le [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) répertoire qui est sauvegardée dans le cloud.

Ajoutez vos fichiers à la racine du projet UWP. Marquez ces fichiers avec l'option **Générer action en tant que contenu**à utiliser avec `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Code source du fichier assistances de système](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Documentation sur les API de système de fichiers](xref:Xamarin.Essentials.FileSystem)
