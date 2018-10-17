---
title: 'Xamarin.Essentials : Système de fichiers'
description: La classe FileSystem contient un ensemble de fonctions facilitant l'accès et l'édition des répertoires de cache et de données de l'application ainsi que l'ouverture de fichiers embarqués dans du package d'application.
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
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials : Système de fichiers

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **FileSystem** contient un ensemble de fonctions facilitant l'accès et l'édition des répertoires de cache et de données de l'application ainsi que l'ouverture de fichiers embarqués dans du package d'application.

## <a name="using-file-system-helpers"></a>Utilisation de **FileSystem**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Le répertoire `CacheDirectory` contient les **données en cache**. Les données en cache sont les données devant être plus persistantes que les données temporaires mais qui ne sont pas indispensables au bon fonctionnement de l'application.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Le répertoire `AppDataDirectory` contient les **données embarquées avec l'application**. Ces fichiers sont sauvegardés dans le système d'application et seront synchronisé lorsque le système se synchronisera, voir les **Implémentations spécifiques par plateforme** ci-dessous.

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

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – retourne le [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) du contexte actuel.
- **AppDataDirectory** – retourne le [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) du contexte actuel qui est sauvegardé automatiquement lors de la [sauvegarde automatique](https://developer.android.com/guide/topics/data/autobackup.html) , disponible à partir de l'API 23.

Pour retrouver un fichier dans votre AppPackage, via `OpenAppPackageFileAsync`:

- Ajoutez vos fichiers dans le dossier **Assets** de votre solution Android.
- Marquez ces fichiers avec l'action de génération **AndroidAsset**

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – retourne la [bibliothèque/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html).
- **AppDataDirectory** – retourne la [bibliothèque](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) qui est sauvegardée par iTunes et iCloud.

Pour retrouver un fichier dans votre AppPackage, via `OpenAppPackageFileAsync`:

- Ajoutez vos fichiers dans le dossier **ressources** de votre solution iOS.
- Marquez ces fichiers avec l'action de génération **BundledResource**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – retourne le [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder)...
- **AppDataDirectory** – retourne le [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) qui est sauvegardée dans le cloud.

Pour retrouver un fichier dans votre AppPackage, via `OpenAppPackageFileAsync`:

- Ajoutez vos fichiers à la racine du projet UWP.
- Marquez ces fichiers avec l'action de génération **contenu**.

--------------

## <a name="api"></a>API

- [Code source du fichier assistances de système](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Documentation sur les API de système de fichiers](xref:Xamarin.Essentials.FileSystem)
